---
vid: '10013980'
title: News from Sustaining Members
volume: '16'
issue: '3'
page: '7'
source: trad/v163/susnews.htm
converter: vecrescue 0.1.0
issue_tab: n3-january-2000
tags:
- implementations
- APL community
---

## Soliton Associates Ltd.

Soliton is pleased to announce the availability of a beta release of the SHARP APL Java Interface (SJI).

The SJI enables SHARP APL to call Java and vice versa. SHARP APL is available for OS/390, AIX, Solaris and Linux. The SHARP APL versions for AIX, Solaris and Linux are commonly called SAX.

The beta release of the SJI for Linux can be downloaded from www.soliton.com/ linux. The SJI for Linux, like SAX for Linux, is royalty free for personal use licenses.

### GUI and non-GUI gateway

The SJI enables an APL programmer to use all of the available Java GUI classes. However, the SJI is not limited to Java’s GUI classes but can access non-GUI classes as well, such as JDBC and Servlets.

### A Simple and Timeless Architecture

The architecture of the SJI is simple, consisting of two components: an APL toolkit and a SHARP APL Java Engine. The communication between the toolkit and the Java Engine is socket-based. The Java Engine is a Java Program that can be executed in any Java environment, for example those available for Windows, NT, Unix, Linux and Mac/OS. The Java Engine is small and can also be executed as part of an applet within a browser (Netscape or MS-IE). This architecture is consistent with the server nature of SAX. We expect that many users will have a SAX server under Linux and a LAN-connected GUI client on a Microsoft Windows platform.

The Java Engine listens to a TCP/IP socket port or connects to SAX via the SHARP APL Browser Interface (SBI). Both mechanisms are supported. The SBI is planned for beta distribution in early 2000. The Java engine, being a Java program, can create objects of any Java class, and invoke any method on such objects. This is exactly its purpose. The APL toolkit instructs the Java Engine to create forms, button, toolbars, JDBC objects, etc. When the Java Engine receives information, from a user clicking something or from another process, like JDBC, it forwards this information to the APL application.

The Java Engine and the toolkit are unaware of the available Java classes. This enables the APL application programmer to refer to Java classes and methods using their Java names. However, it does take care to convert APL data structures into Java data structures, and vice versa. That simplicity makes the SJI timeless. As Java evolves, new classes and methods will be available, and these, in turn, will be available to the APL programmers. Moreover, as long as Java supports the classes and methods on the targeted platforms, they will be available to the SJI.

### GUI Geometry

Writing applications with a graphical user interface can be tedious, in particular, dealing with how the different GUI elements (buttons, text boxes, labels) are placed and how they move when the user resizes the form. Most application writers have to count pixels and compute the placement of the graphic elements based on the total space that the form has after a resize. Java, being device independent, handles the geometry of GUI elements very well by layering frames in a similar way to HTML forms. In most cases, the form has the expected appearance and behaves as expected during resize.

### Distributed processing

With the SJI, you can offload some of the processing from the server platform to the client platform by creating Java expressions to be executed by the Java Engine. A typical example would be data validation by the Java client before a form is submitted to the APL server.

### Application as URL

Soliton recognises the need to make APL more available to the Internet. By coupling the SBI (SHARP APL’s Web server) with the SJI, APL-Java applications can be accessed through their Internet address or URL. By specifying the URL associated to SJI application, the SBI will send the Java engine to your browser as an applet if not already cached and the application will communicate with the applet to create forms. An application is simply a URL, no need to have DLLs or executables installed.

### JAVA calls APL

Not only can APL call Java, but a Java application can also call APL, thanks to some specific classes that are part of the SJI. For example, you may use a J-Application Builder to design a user interface and call APL for data processing and retrieval. The conversion of data structures between Java and APL is automatic. A ‘bean’ to facilitate this type of usage is under consideration.

### Performance and efficiency

Offering a GUI via a client-server, device and operating system independent mechanism may seem like a ‘slow’ generic solution. The SJI isn’t slow, because most Java commands are buffered and sent as a group to the Java Engine. For example, to create a form, 10 buttons, a menu bar with options and set ‘listeners’ or callback functions when the buttons are pressed, is done in a single transaction. The TCP connection between the server and the client can introduce a small delay in processing user input. However, in most cases, only the ‘Submit’ button will cause a TCP/IP transfer to the server, while the Java client will process buttons and fields input locally. To facilitate the work of GUI developers, the SJI can work in diagnostic mode where commands are not buffered but executed at once by the Java Engine.

### Beta Limitation

A beta release of the SBI is packaged with the beta release of the SJI for Linux. Only connecting out to the Java Engine is supported: the ‘Application as URL’ requires the SBI and is not supported yet.

However, with the current beta, you can create GUI or JDBC applications in which APL runs on a Linux server and Java executes on a Linux, Windows or MAC client.

### Examples

To illustrate the ease of use of the SJI, here are simple examples:

1. Create a frame, give it a name  
    `frame←Anew 'Frame' ⊃ 'My Application'`  
    Create an object of type ‘Frame’ and give it a title ‘My Application’. Create the object at once (Anew) and assign the handle to the variable ‘frame’.
2. Create a button ‘Click Me’  
    `btn←Anew 'Button' ⊃ 'Click Me'` Same as first example but object type is ‘Button’ and title (label) is ‘Click Me’. ‘btn’ receives the handle for the button.
3. Put the button on the frame `Ninvoke frame ⊃ 'add' ⊃ <btn` Add ‘btn’, the button, to the frame ‘frame’ as defined in the first example. The ‘Ninvoke’ means that the operation will be buffered.
4. Create a callback, when the Button ‘btn’ is pressed, call the APL function ‘clickme’ `Ninvoke btn⊃'addActionListener'⊃<NEW⊃'SJActionListener'⊃'clickme'` For button ‘btn’, add a Java listener called ‘SJActionListener’. SJActionListener is a generic listener for the SJI and in turn, it will tell the APL application to execute the function ‘clickme’.

### Conclusion

Soliton believes that SJI applications will enable an organisation to distribute APL processing among different TCP connected platforms. It will ease the work of the APL developer by offering a solution that does not need to be rewritten or recompiled every time a desktop operating system is upgraded or changed. Moreover, it will enable users to access APL applications without any need to install additional desktop software as long as a browser is installed.

***

## Insight Systems ApS

Insight Systems is back! After 4 years as part of the Adaytum Group, it was time to return to our roots and become a small company again. On November 1st 1999, Gitte Christensen, Helene Boesen and Morten Kromberg extracted the APL and Statgraphics business from Adaytum, moved about 100 metres and set up our own shop again (with a Linux server this time, rather than NT).

### APL Sales and Support

Insight Systems remains the “Array Language Supermarket” for Denmark. We distribute and support APL and J interpreters from Cognos, Dyadic, IBM and Iverson Software.

### Products

Most of the products and technologies that we developed in the first half of the nineties are now either in the public domain or OEM’d by vendors (and a few are obsolete, let’s face it). Our ODBC interfaces remain the “Industry Standard” under the name SQAPL or APL Link, depending on which vendor you use. We expect the vendors to provide suitable replacements using ADO or other technologies, but until this happens we will continue to support the products and adapt them to new environments if required. The platform-independent array format we developed for SQAPL and our “APL Pipes” products have been adopted by a group of APL vendors working on a common TCP-based array transfer protocol. We’re entering the new millennium without products of our own, but watch this space!

### APL Management Consultancy

If you have decisions to take about adapting your APL application to take advantage of emerging technologies, or would like your strategy reviewed, give us a call. We have extensive experience in all areas of APL development, from legacy systems, up, down and sideways migrations, to the development and support of shrink-wrapped solutions based on APL.

### APL Development and Project Management

Even if we don’t have time to do the work ourselves, we will know where to find someone who is an expert in your version of APL and your application area, on your continent.

### Contact Details

Please see the [Product Guide](../prodguid/insight.htm) for our new address and phone numbers.

***

## Causeway Graphical Systems Ltd

The main focus of work since Orlando has been getting the APL2000 releases of both *RainPro* and *NewLeaf* completely back into line with the Dyalog versions. The chart viewer has been modernised with the new CommandBars, and takes advantage of the additional printer properties in APL+Win 3.5 to allow printer selection and paper orientation to be set by the viewer. We now have a single source for the documentation for both platforms, which allows the manuals, help-files and web-based tutorials to be maintained and re-built with minimal effort. From now on, it stays this way!

Also completed over the Christmas break is our Java-applet viewer for RainPro graphs and NewLeaf tables. This provides a useful alternative to the VML technology (vector graphics support is now solidly available in IE5 and Windows 2000) for non-Microsoft platforms, and it is an essential component of any SAX/Linux implementation of Rain graphics. Examples of both formats are on the web-site, where you can also keep up to date with the latest features and fixes. We will be set up for secure web trading (VISA/MasterCard) in the very near future, so purchasing the latest update will get even easier.

***
