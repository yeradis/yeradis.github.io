---
layout: post
title: Hello Red Hat OpenShift !!! Bye bye Google App Engine!
date: 2012-11-13
tags: 
comments: true
permalink:
share: true
---




![overview-paas.png][1]

**
** **OpenShift** is an enterprise-class Platform-as-a-Service (**PaaS**).

But, unlike Google App Engine....

OpenShift provides enterprise developers with a wide selection of programming languages and frameworks including Java, Ruby, PHP, Perl, Python, and Node.js.

OpenShift uses an open source ecosystem to provide key platform services for mobile applications (Appcelerator), NoSQL services (MongoDB), SQL services (Postgres, MySQL), and provides an enterprise-class middleware platform for Java applications with JBoss (JBoss Enterprise Application Platform,JBoss Application Server), providing support for Java EE6 and integrated services such as transactions and messaging, which are critical for enterprise applications.

It also provides integrated developer tools to support the application lifecycle, including Eclipse integration, JBoss Developer Studio, Jenkins, Maven, and GIT.

OpenShift enables you to create, deploy and manage applications within the cloud. It provides disk space, CPU resources, memory, network connectivity, and an Apache or JBoss server. Depending on the type of application you are building, you also have access to a template file system layout for that type (for example, PHP, Python, and Ruby/Rails). OpenShift also generates a limited DNS for you.

Compared to Google AppEngine solution, OpenShift is closer to a VPS rather than traditional PaaS solutions.

So, why i love OpenShift over AppEngine ?

Because OpenShift takes a **No-Lock-In approach to PaaS** and you can code "anything" on there.

We don't have to develop for a specific box, using specific libraries, avoiding locking our code to that PaaS solution.

You can run on OpenShift a simple code or something bigger like Wordpress,Magento,Joomla,Zend Server, etc.

Here a simple example:


```
    //Create App
    rhc app create –a twt -t python-2.6

    //Add MongoDB NoSQL Database
    rhc app cartridge add -a twt -c mongodb-2.0

    //Add Upstream Repo
    cd twt
    git remote add upstream -m master git://github.com/openshift/openshift-twt-mongo-demo.git
    git pull -s recursive -X theirs upstream master

    //Push Repo
    git push

    //Enjoy!
    http://twt-$yourlogin.rhcloud.com
```


So, lets give it a try ;)

[1]: https://openshift.redhat.com/app/images/overview-paas.png
