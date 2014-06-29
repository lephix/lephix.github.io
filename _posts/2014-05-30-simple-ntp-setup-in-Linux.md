---
layout: post
title: Simple NTP setup in Linux
---

Network Time Protocol (NTP) is a networking protocol for clock synchronization between computer systems over packet-switched, variable-latency data networks.

This post tells how to simplely setup a NTP server in your local network.

In Server file /etc/ntp.conf

~~~config
server 127.127.1.0 # local clock
fudge 127.127.1.0 stratum 10
driftfile /var/lib/ntp/drift
~~~

In Client file /etc/ntp.conf

~~~
server 153.65.197.214 # local clock
driftfile /var/lib/ntp/drift
~~~
Client synchronize to server manully(may need stop ntpd service in client first):

~~~sh
ntpd -gq
~~~

or

~~~sh
ntpdate -s server
~~~
