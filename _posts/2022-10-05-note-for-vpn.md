# OpenVPN on a server
This article shows how to install OpenVPN on a server. 

## Install OpenVPN server
`wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`

After running this command, we can get a `.OVPN` file. Add this file to  OpenVPN Connect App. You can run this line again to remove and add users or uninstall this OpenVPN server. 

> https://github.com/Nyr/openvpn-install

## tips for OpenVPN server
+ If having the port problem, try to change the iptable rules with `sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 1194 -j ACCEPT`.


## tips for connecting OpenVPN server

### Linux client

1. Install client:
```bash
sudo apt install openvpn
```

2. copy ovpn files to the linux client.

3. connect to OpenVPN server.
```bash
sudo openvpn --config xxx.ovpn
```

4. If you SSH to the linux client, the SSH connection will be closed. You should change the route of the linux server.

```bash
# Find the IP address of your local machine on the Linux server. The peer address is your IP.
sudo ss -Hnt sport = :22

# Find the nexthop of your route.
ip route show default

# Add route for linux server
sudo ip route add {your_local_ip}/32 via {nexthop_ip} dev eth0

# Del route
sudo ip route del {your_local_ip}/32
```
