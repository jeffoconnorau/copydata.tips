---
id: 37
title: 'Actifio Windows Connector Silent Install Tips'
date: '2014-04-15T05:50:25+10:00'
author: joconnor
layout: post
guid: 'http://ruste.net/copydata.tips/?p=37'
permalink: /2014/04/actifio-windows-connector-silent-install-tips/
dsq_thread_id:
    - '2612913615'
tags:
    - Connector
---

When you want to deploy the connector initially, you may want to install the filter driver or not. Below are the values you can use to achieve this:

/NORESTART – This will avoid a restart, which is normally the case when installing the agent above versions 6.0.3, but is normally a good idea just in case.

/SILENT – to install without prompting

/VERYSILENT – this will suppress any pop-ups, and for remote installs, or packaged installs this is likely the option you want.

If a restart is necessary and the ‘/NORESTART’ command isn’t used (see below) and Setup is silent, it will display a *Reboot now?* message box. If it’s very silent it will reboot without asking.

/TYPE=full or compact – for full install or compact to not install the filter driver – you can also elect to not enter a type, and you will get the installation without the change tracking filter driver.

/LOG=”logfile.log” – the name and location of where you would like the install log stored. Use full path if required i.e. C:\\TEMP\\Logfile.log

/SUPPRESSMSGBOXES – Instructs Setup to suppress message boxes. Only has an effect when combined with ‘/SILENT’ or ‘/VERYSILENT’.

So the actual command line would like the following if you wanted the filter driver installed:

```
connector-win32.exe /SUPPRESSMSGBOXES /NORESTART /VERYSILENT /TYPE=full /LOG="Actifio_agent_install.log"
```

or if you don’t want the filter driver running (such as inside a VMware Virtual Machine) run the following:

```
connector-win32.exe /SUPPRESSMSGBOXES /NORESTART /VERYSILENT /LOG="Actifio_agent_install.log"
```

And for Uninstall you can either goto Add/Remove Programs and choose to uninstall the Actifio Connector, or run the following commands:

1\. Open a command prompt and go to C:\\Program Files\\Actifio\\

2\. Run the command:

```
unins000.exe /SILENT
```

You should be good to go now.
