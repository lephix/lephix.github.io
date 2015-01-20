---
layout: post
title: Tomcat tricks and debugging with JRebel
---

### Tomcat Tricks

#### Resolve character encoding
Adding `URIEncoding` in `Connector` configuration like following.

~~~
<Connector executor="tomcatThreadPool"
   port="8080" protocol="HTTP/1.1"
   connectionTimeout="20000"
   URIEncoding="UTF-8"
   redirectPort="8443" />
~~~

### Using Tomcat with JRebel

#### Get Tomcat Ready

Use following commands to start or stop tomcat, Tomcat will open a 8000 port for remote debuging.

* tomcat-start.sh

~~~bash
#!/bin/bash
export JAVA_OPTS="-javaagent:/Library/jrebel/jrebel.jar -Drebel.remoting_plugin=true $JAVA_OPTS"
/Library/Tomcat/apache-tomcat-7.0.56/bin/catalina.sh jpda start
~~~

* tomcat-stop.sh

~~~bash
#!/bin/sh
/Library/Tomcat/apache-tomcat-7.0.56/bin/catalina.sh stop
~~~

#### Get IDE Ready

Create a new remote debug, and set the port to 8000. It is better to install a JRebel plugin, it help you to redeploy latest changes to Tomcat.

Register JRebel at [My JRebel](https://my.jrebel.com).

