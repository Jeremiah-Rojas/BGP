# BGP

## Network Topology
<img width="1107" height="495" alt="image" src="https://github.com/user-attachments/assets/b04eb27e-87b6-4ed1-a812-32e8f396c880" />

<br>This lab demonstrates how to configure BGP (border gateway protocol). BGP is an exterior gateway protocol meaning it operates between autonomous systems (AS); an AS is a collection of networks under one administrative domain like company networks. BGP allows for different entities across the internet, such as an ISP or cloud provider, to communicate and transfer traffic back and forth.__That is why the topology has: Company A, Company B, etc.__

## Configurations

| Router 1 | Router 2 | Router 3 |
|---|---|---|
| ```text
enable
conf t

hostname R1

interface g0/0
ip address 192.168.12.1 255.255.255.252
no shutdown

interface loopback0
ip address 10.1.1.1 255.255.255.0

router bgp 65001
bgp log-neighbor-changes

neighbor 192.168.12.2 remote-as 65002

network 10.1.1.0 mask 255.255.255.0

end
wr
``` | ```text
enable
conf t

hostname R2

interface g0/0
ip address 192.168.12.2 255.255.255.252
no shutdown

interface g0/1
ip address 192.168.23.1 255.255.255.252
no shutdown

interface loopback0
ip address 10.2.2.1 255.255.255.0

router bgp 65002
bgp log-neighbor-changes

neighbor 192.168.12.1 remote-as 65001
neighbor 192.168.23.2 remote-as 65003

network 10.2.2.0 mask 255.255.255.0

end
wr
``` | ```text
enable
conf t

hostname R3

interface g0/1
ip address 192.168.23.2 255.255.255.252
no shutdown

interface loopback0
ip address 10.3.3.1 255.255.255.0

router bgp 65003
bgp log-neighbor-changes

neighbor 192.168.23.1 remote-as 65002

network 10.3.3.0 mask 255.255.255.0

end
wr
``` |
