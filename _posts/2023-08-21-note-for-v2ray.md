# Note for v2ray

## Server installation
Install on the server.
```bash
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

Check the status and the config.json file path.
```bash
sudo systemctl status v2ray
```

Update `/usr/local/etc/v2ray/config.json` content, you can change the `port` and `id` settings. `id` could be regenerated at `https://www.uuidgenerator.net/` with version 4.
```json
{
  "inbounds": [{
    "port": 10086,
    "protocol": "vmess",
    "settings": {
      "clients": [{ "id": "56a4a6c0-b60e-4225-9738-da0a9eb37bd7" }]
    }
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  }]
}
```

Restart v2ray.
```bash
sudo systemctl restart v2ray
```

## Client installation
Download client at https://github.com/v2ray/v2ray-core/releases.

Update `config.json` content.
```json
{
  "inbounds": [{
    "port": 1080,  // local proxy port
    "listen": "127.0.0.1",
    "protocol": "socks",
    "settings": {
      "udp": true
    }
  }],
  "outbounds": [{
    "protocol": "vmess",
    "settings": {
      "vnext": [{
        "address": "server", // server address
        "port": 10086,  // remote server port
        "users": [{ "id": "56a4a6c0-b60e-4225-9738-da0a9eb37bd7" }]
      }]
    }
  },{
    "protocol": "freedom",
    "tag": "direct",
    "settings": {}
  }],
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [{
      "type": "field",
      "ip": ["geoip:private"],
      "outboundTag": "direct"
    }]
  }
}
```

Start the v2ray client.
```bash
./v2ray
```

## Tips
+ Change the firewall policy if the port is not reachable with command `sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT`.
