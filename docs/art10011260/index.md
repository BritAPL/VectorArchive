---
vid: '10011260'
title: 'Letter: SAGE: a new web server for APLers'
authors:
- Woody Butler
volume: '22'
issue: '4'
page: '10'
source: trad/v224/letters.htm
converter: vecrescue 0.1.0
issue_tab: n4-december-2006
tags:
- system interfaces
- implementations
---

Greetings, APLers.

Over the past 3 years, Harold Ward from Netwings and I have developed a very robust Web Server engine called SAGE.

SAGE is written in APL2000 (runnnig on Windows) and is designed to interface with any language on the Windows server that supports standard COM methods.

Once an APL workspace is “registered” as a COM object on the server, SAGE can easily communicate with it... sending and receiving GET and POST strings from Internet users browsers and applications.

SAGE uses a back-end SQL SERVER DB for authentication and security control. So, by enrolling users in to tables (Members, Groups, and Web URL Resources) you can easily protect specific URLs. SAGE URL Resources can also be given full Public Access, as well.. so user login is NOT required.

I use SAGE to call DyalogAPL through COM. My DAPL WS gets called upon a URL GET or POST to SAGE, and SAGE passes my <span class="APLU">⎕LX</span> autostart program a nested vector with parameter names and values. I run my desired back-end APL program and then return a simple HTML string back to SAGE through COM, and SAGE passes the HTML page back to the user.

We’ve been running SAGE in production for 2 years, and it is rock solid. We would be interested in working with other APLers who would like to write Web Applications using DAPL, APL2000 or any other “COM” supporting language that runs on MS Windows.

Please reply if you are interested, and we can discuss. Since we own a small ISP (GDEX.net), we have 100% control over the servers, network, firewall, DNS, database and development environment.

Woody Butler (woody@teambase.com)  
President, TeamBase Corp.

***
