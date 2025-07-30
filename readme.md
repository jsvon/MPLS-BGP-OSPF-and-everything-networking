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



