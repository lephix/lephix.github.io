# OpenVPN on a server
This article shows how to install OpenVPN on a server. 

## Install OpenVPN server
`wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`

After running this command, we can get a `.OVPN` file. Add this file to  OpenVPN Connect App. You can run this line again to remove and add users or uninstall this OpenVPN server. 

> https://github.com/Nyr/openvpn-install

## tips
+ If having the port problem, try to change the iptable rules with `sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 1194 -j ACCEPT`.
