# Note for proxy server Squid

## Installation

Install squild and check the status.
```bash
sudo apt install squid
sudo systemctl status squid.service
```

## Secure

Create a passwd file for the squid.
```bash
sudo apt install apache2-utils
sudo htpasswd -c /etc/squid/passwords your_squid_username
```


## Configure
Configure squid.
```bash
sudo vim /etc/squid/squid.d/lephix.conf
```

Add following content to this file.
```
auth_param basic program /usr/lib/squid3/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED

http_access allow authenticated
```

Restart
```bash
sudo systemctl restart squid.service
```

## Test
Test for the proxy.
```bash
curl -v -x http://username:password@155.248.193.206:80 https://www.google.com/
```