# BGP

## Network Topology
<img width="1107" height="495" alt="image" src="https://github.com/user-attachments/assets/b04eb27e-87b6-4ed1-a812-32e8f396c880" />

<br>This lab demonstrates how to configure BGP (border gateway protocol). BGP is an exterior gateway protocol meaning it operates between autonomous systems (AS); an AS is a collection of networks under one administrative domain like company networks. BGP allows for different entities across the internet, such as an ISP or cloud provider, to communicate and transfer traffic back and forth.__That is why the topology has: Company A, Company B, etc.__

Devices Used:
- Cisco IOSvL2 15.2.1 switch
- GNS3 Software
- Ubuntu Container (running on VMware machine)

## Configurations

| R1 Configuration | R2 Configuration | R3 Configuration |
|---|---|---|
| "enable<br>conf t<br><br>hostname R1<br><br>interface g0/0<br>ip address 192.168.12.1 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.1.1.1 255.255.255.0<br><br>router bgp 65001<br>bgp log-neighbor-changes<br><br>neighbor 192.168.12.2 remote-as 65002<br><br>network 10.1.1.0 mask 255.255.255.0<br><br>end<br>wr" | "enable<br>conf t<br><br>hostname R2<br><br>interface g0/0<br>ip address 192.168.12.2 255.255.255.252<br>no shutdown<br><br>interface g0/1<br>ip address 192.168.23.1 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.2.2.1 255.255.255.0<br><br>router bgp 65002<br>bgp log-neighbor-changes<br><br>neighbor 192.168.12.1 remote-as 65001<br>neighbor 192.168.23.2 remote-as 65003<br><br>network 10.2.2.0 mask 255.255.255.0<br><br>end<br>wr" | "enable<br>conf t<br><br>hostname R3<br><br>interface g0/1<br>ip address 192.168.23.2 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.3.3.1 255.255.255.0<br><br>router bgp 65003<br>bgp log-neighbor-changes<br><br>neighbor 192.168.23.1 remote-as 65002<br><br>network 10.3.3.0 mask 255.255.255.0<br><br>end<br>wr" |
