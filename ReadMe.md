# Testing VPN Clients for leaks (LocalNet/ServerIP)

In this project I tested multiple VPN clients on iOS, Android, Mac OS and Windows against two attacks from the paper cited bellow.
```
@inproceedings {291223,
author = {Nian Xue and Yashaswi Malla and Zihang Xia and Christina P{\"o}pper and Mathy Vanhoef},
title = {Bypassing Tunnels: Leaking {VPN} Client Traffic by Abusing Routing Tables},
booktitle = {32nd USENIX Security Symposium (USENIX Security 23)},
year = {2023},
isbn = {978-1-939133-37-3},
address = {Anaheim, CA},
pages = {5719--5736},
url = {https://www.usenix.org/conference/usenixsecurity23/presentation/xue},
publisher = {USENIX Association},
month = aug
}
```
These two attacks basically abused routing exceptions added by VPN client. Goal of these attack is to leak traffic to a target website outside of VPN tunnel.

## LocalNet Attack
This attack abuse the routing exception added for access to local network when using VPN.
### Attack Scenario
1. Client device is connected to a rogue ap which hands out an IP address from the subnet of the target website.
2. When connected to VPN, VPN client creates an routing exception so that traffic to local network does not go through encrypted VPN tunnel.
3. But rouge ap gave the device an IP address from the subnet of the target website.
4. Now when user visits the target website, device thinks the target website is in local network, hence it sends the traffic through an unencrypted channel.

## ServerIP Attack:
This attack abuses the routing exception for traffic to VPN server.

### Attack Scenario
1. Client device is connected to a rogue ap.
2. When connected to VPN, VPN client creates an routing exception so that traffic to VPN servers IP address does not go through encrypted VPN tunnel.
3. During VPN connection process VPN client will send a DNS qwery for the IP address of the VPN server.
4. This time rouge access point will send a spoofed DNS response which says the IP address of the VPN server is the IP address of the target website. 
4. Now when user visits the target website, device thinks the target website is the VPN server, hence it sends the traffic through an unencrypted channel.


Originally repo of authors: https://github.com/vanhoefm/vpnleaks