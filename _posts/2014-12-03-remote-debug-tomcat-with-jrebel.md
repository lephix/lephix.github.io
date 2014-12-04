---
layout: post
title: Remote debug for tomcat with JRebel
---

### Tomcat

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

### IDE

Create a new remote debug, and set the port to 8000. It is better to install a JRebel plugin, it help you to redeploy latest changes to Tomcat.

Register JRebel at [My JRebel](https://my.jrebel.com).

