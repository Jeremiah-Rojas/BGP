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

These images show BGP fully functional and the configuration details:

R1 BGP details:
Output of “show ip bgp summary”
<img width="1311" height="75" alt="image" src="https://github.com/user-attachments/assets/dcb99713-0ebb-4c75-846a-7407ba2484fd" />

Output of “show ip route bgp”
<img width="870" height="163" alt="image" src="https://github.com/user-attachments/assets/c293e293-907f-4308-b385-11e413d3e283" />

Output of “show ip bgp”
<img width="1303" height="146" alt="image" src="https://github.com/user-attachments/assets/54960b84-01d9-4429-aef8-8503db872249" />


R2 BGP details:
Output of “show ip bgp summary”
<img width="1307" height="157" alt="image" src="https://github.com/user-attachments/assets/b559d96f-ffcc-4aec-966c-c640b32be284" />

Output of “show ip route bgp”
<img width="1306" height="129" alt="image" src="https://github.com/user-attachments/assets/033396b9-c209-4167-9afe-448aac70b324" />

Output of “show ip bgp”
<img width="1306" height="129" alt="image" src="https://github.com/user-attachments/assets/ed6b5b73-b438-4891-b61e-c0075808904d" />



R3 BGP details:
Output of “show ip bgp summary”

Output of “show ip route bgp”

Output of “show ip bgp”
