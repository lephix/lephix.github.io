---
layout: post
---
Server ntp.conf
```config
server 127.127.1.0 # local clock
fudge 127.127.1.0 stratum 10
driftfile /var/lib/ntp/drift
```

Client ntp.conf
```
server 153.65.197.214 # local clock
driftfile /var/lib/ntp/drift
```
Client synchronize to server manully(may need stop ntpd service in client first):
```sh
ntpd -gq
```
or
```sh
ntpdate -s server
```
