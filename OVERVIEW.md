<img width="676" height="325" alt="image" src="https://github.com/user-attachments/assets/6fb8be08-d970-4902-81e5-ee0992ab3aa7" />

The interfaces of P and PE routers are enabled for frame mode MPLS with the mpls 
ip interface subcommand and all P and PE routers use a common IGP (EIGRP with AS 
200)

Confirming ldp neighborship 

P#sh mpls ldp neighbor


<img width="460" height="260" alt="image" src="https://github.com/user-attachments/assets/ea607e17-8371-413d-b393-b48b1bbf41f9" />


PE1#sh mpls ldp neighbor

<img width="420" height="142" alt="image" src="https://github.com/user-attachments/assets/e401256a-89ad-4866-a9ba-b8c209300fac" />

PE2#sh mpls ldp neighbor
    
    Peer LDP Ident: 3.3.3.3:0; Local LDP Ident 2.2.2.2:0
        
        TCP connection: 192.168.2.2.21390 - 192.168.2.1.646
        
        State: Oper; Msgs sent/rcvd: 80/82; Downstream
       
        Up time: 01:06:11
        
        LDP discovery sources:
          
          GigabitEthernet2/0, Src IP addr: 192.168.2.2
        
        Addresses bound to peer LDP Ident:
          
          192.168.1.2     192.168.2.2     3.3.3.3


PE1#show mpls forwarding-table

    Local      Outgoing   Prefix           Bytes Label   Outgoing   Next Hop

    Label      Label      or Tunnel Id     Switched      interface

    16         No Label   172.16.1.1/32[V] 0             Gi2/0      10.1.1.2

    17         No Label   172.16.1.1/32[V] 0             Gi3/0      10.3.3.2

VRF CONFIGURATION EACH WITH RD AND RD , ASSOCIATING CUSTOMER FACING INTERFACES

PE1#

    ip vrf CUST-A
    rd 1:111
    route-target export 1:100
    route-target import 1:100
  !
PE1#    

    ip vrf CUST-B
    rd 2:222
    route-target export 2:200
    route-target import 2:200
!
      
    interface GigabitEthernet2/0
    ip vrf forwarding CUST-A
    ip address 10.1.1.1 255.255.255.0
!
    interface GigabitEthernet3/0
    ip vrf forwarding CUST-B
    ip address 10.3.3.1 255.255.255.0

PE2#
    ip vrf CUST-A
    rd 1:111
    route-target export 1:100
    route-target import 1:100
    
!

     ip vrf CUST-B
     rd 2:222
     route-target export 2:200
     route-target import 2:200
     
!

      interface GigabitEthernet3/0
      ip vrf forwarding CUST-A
      ip address 10.2.2.1 255.255.255.0
      
! 

     interface GigabitEthernet1/0
     ip vrf forwarding CUST-B
     ip address 10.4.4.1 255.255.255.0

**Configuring the IGP Between PE and CE routers**

CE-A1:

   interface Loopback0
   ospfv3 1 ipv4 area 0
!

   interface GigabitEthernet2/0
   ipv6 enable
   ospfv3 1 ipv4 area 0
!

   router ospfv3 1
   
!

   address-family ipv4 unicast
   exit-address-family
   
CE-A2:

    interface Loopback0
    ospfv3 1 ipv4 area 0
!

   interface GigabitEthernet3/0
   ipv6 enable
   ospfv3 1 ipv4 area 0
   
!

router ospfv3 1

!

   address-family ipv4 unicast
   exit-address-family
   
PE1:

   interface GigabitEthernet2/0
   ipv6 enable
   ospfv3 1 ipv4 area 0
   
!

   interface GigabitEthernet3/0
   ipv6 enable
   ospfv3 1 ipv4 area 0
   
!

   router ospfv3 1
!

   address-family ipv4 unicast vrf CUST-B
   exit-address-family
   
!

   address-family ipv4 unicast vrf CUST-A
   exit-address-family
   
PE2:

   interface GigabitEthernet3/0
   ipv6 enable
   ospfv3 1 ipv4 area 0
   
!

    interface GigabitEthernet1/0
    ipv6 enable
    ospfv3 1 ipv4 area 0
    
!

   router ospfv3 1
!
   address-family ipv4 unicast vrf CUST-B
   exit-address-family
   
!
   address-family ipv4 unicast vrf CUST-A
   exit-address-family

**Verifying OSPFV3 neighborship**

PE1#sh ospfv3 vrf CUST-A neighbor

          OSPFv3 1 address-family ipv4 vrf CUST-A (router-id 10.1.1.1)

Neighbor ID     Pri   State           Dead Time   Interface ID    Interface

172.16.1.1        1   FULL/DR         00:00:34    5               GigabitEthernet2/0

PE2#
sh ip route vrf CUST-A ospfv3

Routing Table: CUST-A
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP

       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       
       E1 - OSPF external type 1, E2 - OSPF external type 2
       
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       
       + - replicated route, % - next hop override

      Gateway of last resort is not set

      172.16.0.0/32 is subnetted, 1 subnets
      
      O        172.16.2.1 [110/1] via 10.2.2.2, 02:14:29, GigabitEthernet3/0

PE1#sh ip route vrf CUST-A ospfv3

   Routing Table: CUST-A
   
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
   
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       
       E1 - OSPF external type 1, E2 - OSPF external type 2
       
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       
       + - replicated route, % - next hop override

     Gateway of last resort is not set

      172.16.0.0/32 is subnetted, 1 subnets
    O        172.16.1.1 [110/1] via 10.1.1.2, 02:21:30, GigabitEthernet2/0

PE1#sh ip route vrf CUST-B ospfv3

   Routing Table: CUST-B
   
   Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
   
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       
       E1 - OSPF external type 1, E2 - OSPF external type 2
       
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       
       + - replicated route, % - next hop override

     Gateway of last resort is not set

      172.16.0.0/32 is subnetted, 1 subnets
      
      O        172.16.1.1 [110/1] via 10.3.3.2, 02:22:00, GigabitEthernet3/0


**Redistribution Between PE-CE routers (between OSPFv3 and MP-BGP):**

   PE1(config)#router bgp 65000
   
   PE1(config-router)#address-family ipv4 vrf CUST-A
   
   PE1(config-router-af)#redistribute ospfv3 1
   
   PE1(config-router-af)#address-family ipv4 vrf CUST-B
   
   PE1(config-router-af)#redistribute ospfv3 1
   
   PE1(config-router-af)#router ospfv3 1
   
   PE1(config-router)#address-family ipv4 vrf CUST-A
   
   PE1(config-router-af)#redistribute bgp 65000

   PE1(config-router-af)#address-family ipv4 vrf CUST-B
   
   PE1(config-router-af)#redistribute bgp 65000
   
   PE2(config)#router bgp 65000
   
   PE2(config-router)#address-family ipv4 vrf CUST-A
   
   PE2(config-router-af)#redistribute ospfv3 1
   
   PE2(config-router-af)#address-family ipv4 vrf CUST-B
   
   PE2(config-router-af)#redistribute ospfv3 1
   
   PE2(config-router-af)#router ospfv3 1
   
   PE2(config-router)#address-family ipv4 vrf CUST-A
   
   PE2(config-router-af)#redistribute bgp 65000
   
   PE2(config-router-af)#address-family ipv4 vrf CUST-B
   
   PE2(config-router-af)#redistribute bgp 65000


**Configuration MP-BGP Between PEs routers:**

   PE1(config)#router bgp 65000
   
   PE1(config-router)#neighbor 2.2.2.2 remote-as 65000
   
   PE1(config-router)#neighbor 2.2.2.2 update-source loop0
   
   PE1(config-router)#address-family vpnv4
   
   PE1(config-router-af)#neighbor 2.2.2.2 activate
   
   PE1(config-router-af)#neighbor 2.2.2.2 send-community

   PE2(config)#router bgp 65000
   
   PE2(config-router)#neighbor 1.1.1.1 remote-as 65000
   
   PE2(config-router)#neighbor 1.1.1.1 update-source loop0
   
   PE2(config-router)#address-family vpnv4
   
   PE2(config-router-af)#neighbor 1.1.1.1 activate
   
   PE2(config-router-af)#neighbor 1.1.1.1 send-community

**Confirming BGP**
   PE1#sh ip bgp summary

   BGP router identifier 1.1.1.1, local AS number 65000

   BGP table version is 1, main routing table version 1

   Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
   
   2.2.2.2         4        65000       0       0        1    0    0 never    Idle



PE1#sh ip bgp vpnv4 all
   BGP table version is 5, local router ID is 1.1.1.1
   
   Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              
              x best-external, a additional-path, c RIB-compressed,
              
   Origin codes: i - IGP, e - EGP, ? - incomplete
   
   RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
     
Route Distinguisher: 1:111 (default for vrf CUST-A)

 *>  10.1.1.0/24      0.0.0.0                  0         32768 ?
 
 *>  172.16.1.1/32    10.1.1.2                 1         32768 ?

Route Distinguisher: 2:222 (default for vrf CUST-B)

 *>  10.3.3.0/24      0.0.0.0                  0         32768 ?
 
 *>  172.16.1.1/32    10.3.3.2                 1         32768 ?

**VERIFICATION**

Verify that the customer routers have learned the routes from each customer router in the same VRF:

By definition when the PE router redistributes the VPNv4 routes to the OSPF domain, it checks the Domain ID to decide whether the routes should be redistributed as inter-area (same Domain ID) or external (different Domain ID) routes to the CE router.

Each OSPF instance must be assigned a unique Domain ID.

On Cisco routers, the OSPF Domain ID is the OSPF Process ID by default. When BGP distributes VPNv4 routes to other PE routers, the Domain ID is carried with the routes as extended community.

Since the Domain IDs on both PE routers match , PE routers redistributes the subnets 172.16.1.1/32 and 172.16.2.1/32 to CE routers as inter-area routes (LSA Type 3) as shown by the routing tables of the CE routers:

Verify the connectivity between the customers:

CE-A1#ping 172.16.2.1 sou 172.16.1.1


prefixes for each VRF to PE1 using BGP. PE2 has included all OSPF related BGP Extended communities. 

The route-type is set as LSA Type-2 (intra-area) route as shown by the line: OSPF RT:0.0.0.0:2:0. The Router ID is the router-id of PE2 
router set for that VRF instance (ROUTER ID:10.2.2.1:0 for VRF CUST-A and ROUTER ID:10.4.4.1:0 for VRF CUST-B).

PE1#show ip bgp vpnv4 all 172.16.2.1

BGP routing table entry for 1:111:172.16.2.1/32, version 11

Paths: (1 available, best #1, table CUST-A)
 
 Not advertised to any peer
 
 Refresh Epoch 1
 
 Local
 
 2.2.2.2 (metric 2809856) from 2.2.2.2 (2.2.2.2)
 
 Origin incomplete, metric 1, localpref 100, valid, internal, best
 
 Extended Community: RT:1:100 OSPF ROUTER ID:10.2.2.1:0
 
 OSPF RT:0.0.0.0:2:0
 
 mpls labels in/out nolabel/21
 
 rx pathid: 0, tx pathid: 0x0

BGP routing table entry for 2:222:172.16.2.1/32, version 13

Paths: (1 available, best #1, table CUST-B)
 
 Not advertised to any peer
 
 Refresh Epoch 1
 
 Local
 
 2.2.2.2 (metric 2809856) from 2.2.2.2 (2.2.2.2)
 
 Origin incomplete, metric 1, localpref 100, valid, internal, best
 
 Extended Community: RT:2:200 OSPF ROUTER ID:10.4.4.1:0
 
 OSPF RT:0.0.0.0:2:0
 
 mpls labels in/out nolabel/20
 
 rx pathid: 0, tx pathid: 0x0

PE1

And we can verify that CE-A1 router received an external route for the prefix 172.16.2.1/32 as shown by its routing table:

CE-A1#show ip route ospfv3 | beg Gate

PE2#show ospfv3 vrf CUST-A database
