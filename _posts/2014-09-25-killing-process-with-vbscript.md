---
layout: post
title: 'Killing Windows process with VBScript and WMI'
tags: windows services vbscript
comments: true
permalink:
share: true
---

For some info about `VBScript` and `WMI` chek the previous post.

So lets kill some specific windows process filtering their params.

In this case i want to kill the Domain2 thats running on Glassfish.

So i will run it like this:

```vbnet
KillProcess "java.exe", "domain2"
```

Why i want to do this? Well there is a time when i really want to stop glassfish services but they ignore me xD and raise some timeout :(

Here is the `KillProcess` snippet:

```vbnet

Sub KillProcess(Process,CommandLine)
    strComputer = "."
    Set objWMIService = GetObject("winmgmts://./root/cimv2")

    Set colProcessbyName = objWMIService.ExecQuery("Select * from Win32_Process Where Name like '%" & Process & "%' and CommandLine LIKE '%"& CommandLine &"%'")

    If (NOT IsNull(colProcessbyName)) AND ( getCount(colProcessbyName) > 0) Then
      For Each objProcess in colProcessbyName
          strProcessID = objProcess.ProcessId
          Wscript.Echo "Process ID: " & strProcessID
      Next
    End If

    Set colProcessList = objWMIService.ExecQuery("Select * from Win32_Process where ProcessId =" & strProcessID)
    If (NOT IsNull(colProcessList)) AND (getCount(colProcessList) > 0) Then
      For Each objProcess in colProcessList
          objProcess.Terminate()
      Next
    End If

End Sub

```


That's all folks