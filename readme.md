**STEPS TO BE FOLLOWED FOR THIS LAB**

Step 1 – IP addressing of MPLS Core and OSPF

Step 2 – Configure LDP on all the interfaces in the MPLS Core

Step 3 – MPLS BGP Configuration 

Step 4 – create VRFs

Step 5 - redistribute ospf to bgp and bgp to ospf

**TOPOLOGY**

<img width="751" height="387" alt="image" src="https://github.com/user-attachments/assets/88779544-701f-481d-8e3d-8a7c82399232" />

**Step 1**

**IP addressing of MPLS Core and OSPF**

**R1**

    hostname R1
    int lo0 
    ip add 1.1.1.1 255.255.255.255
    ip ospf 1 area 0 

    interface GigabitEthernet1/0
    ip add 10.0.0.1 255.255.255.0
    no shut
    ip ospf 1 area 0

    interface GigabitEthernet3/0
    ip address 192.168.1.1 255.255.255.0
    no shut
    ip ospf 2 area 2


**R2**

    hostname R2
    int lo0
    ip add 2.2.2.2 255.255.255.255
    ip ospf 1 are 0 

    interface GigabitEthernet1/0
    ip add 10.0.0.2 255.255.255.0
    no shut
    ip ospf 1 area 0

    interface GigabitEthernet2/0
    ip add 10.0.1.2 255.255.255.0 
    no shut 
    ip ospf 1 area 0 


**R3**

    hostname R3
    int lo0 
    ip add 3.3.3.3 255.255.255.255
    ip ospf 1 are 0 

    interface GigabitEthernet2/0 
    ip add 10.0.1.3 255.255.255.0 
    no shut 
    ip ospf 1 area 0 

    interface GigabitEthernet3/0
    ip address 192.168.2.1 255.255.255.0
    ip ospf 2 area 2

**R4**

    hostname R4
    int lo0 
    ip add 4.4.4.4 255.255.255.255
    ip ospf 2 area 2
    
    interface GigabitEthernet3/0
    ip address 192.168.1.4 255.255.255.0
    ip ospf 2 area 2

**R5**

    hostname R5
    int lo0 
    ip add 6.6.6.6 255.255.255.255
    ip ospf 2 area 2

    interface GigabitEthernet3/0
    ip address 192.168.2.6 255.255.255.0
    ip ospf 2 area 2

**Step 2**

**Configure LDP on all the interfaces in the MPLS Core**

**R1**

    router ospf 1
    mpls ldp autoconfig

**R2**

    router ospf 1
    mpls ldp autoconfig

**R3**

    router ospf 1
    mpls ldp autoconfig

**Verifying MPLS**

R2#sh mpls interfaces

<img width="497" height="78" alt="image" src="https://github.com/user-attachments/assets/fbe9dea8-517c-42b6-81f8-94f634bbcef4" />

R2#sh mpls ldp neighbor

<img width="423" height="270" alt="image" src="https://github.com/user-attachments/assets/2a631e71-4d96-4554-a846-c1a760236bd6" />

trace commamnd to show mpls label

<img width="434" height="91" alt="image" src="https://github.com/user-attachments/assets/5be81911-00e3-4969-8a31-f47c00277ed6" />

**Step 3 **

**MPLS BGP Configuration**

**MP-BGP session between R1 and R3 using the vpnv4  address family**

**R1#**

    router bgp 1
     neighbor 3.3.3.3 remote-as 1
     neighbor 3.3.3.3 update-source Loopback0
     no auto-summary
 !
 
     address-family vpnv4
      neighbor 3.3.3.3 activate

**R3#**

    router bgp 1
     neighbor 1.1.1.1 remote-as 1
     neighbor 1.1.1.1 update-source Loopback0
     no auto-summary
 !
 
     address-family vpnv4
      neighbor 1.1.1.1 activate

**BGP verification**

sh bgp vpnv4 unicast all summary

<img width="617" height="229" alt="image" src="https://github.com/user-attachments/assets/b012c256-f9ba-482b-b60a-6824f9ef8968" />

**Step 4**

**create VRFs**

**R1**

    ip vrf RED 
    rd 4:4
    route-target both 4:4

**R3**

    ip vrf RED
    rd 4:4
    route-target both 4:4

**Attaching interface GigabitEthernet3/0 to VRF RED**

**R1**

    interface GigabitEthernet3/0
    ip vrf forwarding RED
    ip address 192.168.1.1 255.255.255.0  # Reapply the IP to the interface as it is removed on application of vrf

**R3**

    interface GigabitEthernet3/0
    ip vrf forwarding RED
    ip address 192.168.2.1 255.255.255.0  # Reapply the IP to the interface as it is removed on application of vrf

<img width="377" height="175" alt="image" src="https://github.com/user-attachments/assets/c241f142-6cd5-4cdf-87cd-b734038fe1ec" />


**The Global Routing Table**

<img width="564" height="346" alt="image" src="https://github.com/user-attachments/assets/c800b1be-133d-4394-9421-57c3dc4da1f9" />




**The Routing Table for VRF RED**

R1#sh ip route vrf RED

<img width="557" height="339" alt="image" src="https://github.com/user-attachments/assets/c6a4d1a7-3946-4aff-8bd4-9c7c6dd86639" />

R3#sh ip route vrf RED

<img width="545" height="339" alt="image" src="https://github.com/user-attachments/assets/86a259af-e756-4c4d-b6c8-438a9e27c956" />


**VERIFICATIONS**

<img width="581" height="334" alt="image" src="https://github.com/user-attachments/assets/dbfd108a-cb65-43c6-99ea-bc5e1b44ab3b" />

<img width="560" height="314" alt="image" src="https://github.com/user-attachments/assets/b5f31029-26c8-4e3f-a453-e77b6dc542dc" />


**Step 5**  

**redistribute ospf to bgp and bgp to ospf**

**Redistribute OSPF into MP-BGP on R1**

**R1**

    router bgp 1
    address-family ipv4 vrf RED 
    redistribute ospf 2

**Redistribute OSPF into MP-BGP on R3**

**R3**

    router bgp 1
    address-family ipv4 vrf RED 
    redistribute ospf 2

<img width="571" height="231" alt="image" src="https://github.com/user-attachments/assets/7699a32f-5772-4e52-9df2-ca4fd1576769" />

Here we can see that 4.4.4.4 is now in the BGP table in VRF RED on R1 with a next hop of 192.168.1.4 (R4) and also 6.6.6.6 is in there as 

well with a next hop of 3.3.3.3 (which is the loopback of R3 – showing that it is going over the MPLS and R1 is not in the picture)

The same should be true on R3

<img width="557" height="216" alt="image" src="https://github.com/user-attachments/assets/1287b8c0-d60a-46e1-9033-dbccb38c5fca" />

**Redistribute BGP into OSPF**


**R1**

    router ospf 2 
    redistribute bgp 1 subnets 

**R3**

    router ospf 2 
    redistribute bgp 1 subnets



