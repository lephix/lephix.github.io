---
# layout: post
---

## SSH tunning proxy

Dynamic forward: <ssh_server> will using dynamic port to access the target accroding the request of the ssh client on <local_port>.

~~~bash
ssh -D <local_port> <user>@<ssh_server>
~~~

Local forward: All the request to the <local_port> will be forwarded to the <remote_address>:<remote_port>. And the <local_port> cannot be accessed from other machines with a 'connection refused' message, add a `-g` can resolve it. 

~~~bash
ssh -L <local_port>:<remote_address>:<remote_port> <user>@<ssh_server> 
~~~

Remote forward: All the request to the <local_port> on the ssh server will be forwarded to the <remote_address>:<remote_port>. 

~~~bash
ssh -R <local_port>:<remote_address>:<remote_port> <user>@<ssh_server>
~~~

## Polipo HTTP proxy

SSH tunning will only support for socks5 protocol, and some of the software only support HTTP proxy, so Polipo could be used as a child HTTP proxy server with SSH tunning as parent proxy service.  
Simplely download Polipo and run `make` command, and then run following command to using it.

~~~bash
./polipo socksParentProxy=localhost:7070
~~~

> 7070 is the port for SSH tunning proxy.
