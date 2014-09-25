---
layout: post
title: 'Restarting Windows services with VBScript and WMI'
tags: windows services vbscript
comments: true
permalink:
share: true
---

For those who don't know, `VBScript` (Visual Basic Scripting Edition) is an Active Scripting language developed by Microsoft that is modeled on Visual Basic. 

It is designed as a "lightweight" language with a fast interpreter for use in a wide variety of Microsoft environments. VBScript uses the Component Object Model to access elements of the environment within which it is running.

Also i will be using `WMI` (Windows Management Instrumentation). 

WMI is the infrastructure for management data and operations on Windows-based operating systems. You can write WMI scripts or applications to automate administrative tasks on remote computers but WMI also supplies management data to other parts of the operating system and products, for example System Center Operations Manager, formerly Microsoft Operations Manager (MOM), or Windows Remote Management (WinRM).

Situation: I need to stop services and start it again when the process finished.

Problem: Services starts/stops in and asyncronous way and i dont want to use `sc` or `net stop/start` because i need to check the current status of the service in a programatically way.

Here is my snippet:

```vbscript
Sub WaitUntil(Query)
   Set wmi = GetObject("winmgmts://./root/cimv2")
   Do Until wmi.ExecQuery(Query).Count > 0
      WScript.Sleep 100
   Loop
End Sub

Sub StartOrStopService(Action, ServiceName)
  Set wmi = GetObject("winmgmts://./root/cimv2")

  Wscript.Echo "Trying to "+ Action +" service: " & ServiceName

  qry = "SELECT * FROM Win32_Service WHERE Name='" & ServiceName & "'"
  For Each s In wmi.ExecQuery(qry)
    Select Case Action
      Case "start": If s.State = "Stopped" Then 
                                s.StartService
                                Wscript.Echo "Wating for service to "& Action &": " & ServiceName
                                WaitUntil qry & " AND State='Running'"
                    End If

      Case "stop" : If s.State = "Running" Then 
                                s.StopService
                                Wscript.Echo "Wating for service to "& Action &": " & ServiceName
                                WaitUntil qry & " AND State='Stopped'"
                    End If

      Case "restart" : 
            StartOrStopService "stop", ServiceName
            StartOrStopService "start", ServiceName
      Case Else   : WScript.Echo "Invalid action: " & action
    End Select
  Next
End Sub
```

And i use it in this way:

```vbscript
    StartOrStopService "restart", "AppServer9PE-2"
```

Of course `StartOrStopService` is the main function, but there is a trick.

The `WaitUntil` is just a delay to avoid problemas when starting or stoping, becasue they run asyncronouslly.

>> yes i'm playing with a glassfish service

For more info about VBScript go to: http://msdn.microsoft.com/en-us/library/t0aew7h6(v=vs.84).aspx 

and for WMI go to: http://msdn.microsoft.com/en-us/library/aa394582(v=vs.85).aspx

And that's all folks