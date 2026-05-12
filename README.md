# BGP

## Network Topology
<img width="1107" height="495" alt="image" src="https://github.com/user-attachments/assets/b04eb27e-87b6-4ed1-a812-32e8f396c880" />

<br>This lab demonstrates how to configure BGP (border gateway protocol). BGP is an exterior gateway protocol meaning it operates between autonomous systems (AS); an AS is a collection of networks under one administrative domain like company networks. BGP allows for different entities across the internet, such as an ISP or cloud provider, to communicate and transfer traffic back and forth.__That is why the topology has: Company A, Company B, etc.__

Devices Used:
- Cisco IOSvL2 15.2.1 switch
- GNS3 Software
- Ubuntu Container (running on VMware machine)

## Configurations

__Note: It is recommended to copy and paste these commands into the routers. Because of the limited width of the table, some commands, which should be treated as one line, are broken into two lines and entering these commands as two lines will throw an error.__
| R1 Configuration | R2 Configuration | R3 Configuration |
|---|---|---|
|enable<br>conf t<br><br>hostname R1<br><br>interface g0/0<br>ip address 192.168.12.1 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.1.1.1 255.255.255.0<br><br>router bgp 65001<br>bgp log-neighbor-changes<br><br>neighbor 192.168.12.2 remote-as 65002<br><br>network 10.1.1.0 mask 255.255.255.0<br><br>end<br>wr|enable<br>conf t<br><br>hostname R2<br><br>interface g0/0<br>ip address 192.168.12.2 255.255.255.252<br>no shutdown<br><br>interface g0/1<br>ip address 192.168.23.1 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.2.2.1 255.255.255.0<br><br>router bgp 65002<br>bgp log-neighbor-changes<br><br>neighbor 192.168.12.1 remote-as 65001<br>neighbor 192.168.23.2 remote-as 65003<br><br>network 10.2.2.0 mask 255.255.255.0<br><br>end<br>wr|enable<br>conf t<br><br>hostname R3<br><br>interface g0/1<br>ip address 192.168.23.2 255.255.255.252<br>no shutdown<br><br>interface loopback0<br>ip address 10.3.3.1 255.255.255.0<br><br>router bgp 65003<br>bgp log-neighbor-changes<br><br>neighbor 192.168.23.1 remote-as 65002<br><br>network 10.3.3.0 mask 255.255.255.0<br><br>end<br>wr|

Understanding the output:
- Different AS numbers are used (65001/65002/65003) because they represent ISPs since BGP is an exterior gateway protocol.
- “bgp log-neighbor-changes” allows log message to be printed to the screen (optional).
- “neighbor 192.168.12.2 remote-as 65002” tell the router who is their neighbor and what AS they are on.

<br><br>These images show BGP fully functional and the configuration details:

R1 BGP details:
Output of “show ip bgp summary”
<img width="1311" height="75" alt="image" src="https://github.com/user-attachments/assets/dcb99713-0ebb-4c75-846a-7407ba2484fd" />

Output of “show ip route bgp”
<img width="870" height="163" alt="image" src="https://github.com/user-attachments/assets/c293e293-907f-4308-b385-11e413d3e283" />

Output of “show ip bgp”
<img width="1303" height="146" alt="image" src="https://github.com/user-attachments/assets/54960b84-01d9-4429-aef8-8503db872249" />


<br>R2 BGP details:
Output of “show ip bgp summary”
<img width="1305" height="96" alt="image" src="https://github.com/user-attachments/assets/087d8f7b-9f49-4025-8908-a756581c8e46" />

Output of “show ip route bgp”
<img width="1307" height="157" alt="image" src="https://github.com/user-attachments/assets/b559d96f-ffcc-4aec-966c-c640b32be284" />

Output of “show ip bgp”
<img width="1306" height="129" alt="image" src="https://github.com/user-attachments/assets/ed6b5b73-b438-4891-b61e-c0075808904d" />


<br>R3 BGP details:
Output of “show ip bgp summary”
<img width="1308" height="66" alt="image" src="https://github.com/user-attachments/assets/e5bbb42c-6d83-4e28-a018-66496ca7ef7d" />

Output of “show ip route bgp”
<img width="1308" height="100" alt="image" src="https://github.com/user-attachments/assets/00e100be-ede3-4e58-8966-f3b874caae8e" />

Output of “show ip bgp”
<img width="1304" height="125" alt="image" src="https://github.com/user-attachments/assets/fffd8456-328d-4941-bba1-959900bfcf6f" />

<br>Understanding the output:
show ip bgp summary
- Understanding the output of this command:
- Neighbor: IP address of BGP peer
- V: BGP version in use
- AS: Neighbor’s Autonomous System
- MsgRcvd: Number of BGP messages received
- MsgSent: Number of BGP messages sent
- TblVer: Version of BGP routing table
- InQ: Input queue showing how many packets are waiting to be received
- OutQ: Output queue showing how many packets are waiting to be sent out
- Up/Down: How long the session has been up in HH:MM:SS
- State/PfxRcd: Session state or prefixes received; this shows whether BGP is working or not. A prefix is a block of IP addresses telling the router where to send traffic destined for a certain IP.
show ip route bgp (for learned routes). This shows routes learned through BGP and is self explanatory.
show ip bgp (for BGP table). This shows the BGP table and below are the meaning of certain aspects of the output:
- /* = valid route
- /> = best route
- Network: Destination prefix
- Next Hop: Where packets are sent next
- Metric: Not applicable
- LocPrf: Not applicable
- Weight: Cisco-specific best path
- Path: path to specified AS. If the AS is a direct neighbor than only on entry will be shown, but if not, than it will show how what ASes must be used to get to the destination AS.

## Conclusion

<br>In this lab, I may not have broken any link as in some of my other demonstrations of routing protocols, simulating a break in this topology would not be useful for educational purposes since the topology is so small and would simply cause BGP output to stop on at least one router. In the real-world, many links exist between these routers and so a break in a link would automatically be addressed by re-routing traffic accordingly as a result of BGP being enforced.
