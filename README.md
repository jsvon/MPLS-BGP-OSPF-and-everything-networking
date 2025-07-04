
# Loopback ips /32
10.0.1.1

10.0.2.2

10.0.3.3

10.0.4.4

10.0.5.5

10.0.6.6

10.0.7.7

10.0.8.8

## AREA O IP'S

R1   R2   172.16.255.49, 172.16.255.50   ip address 172.16.255.49 255.255.255.252

R1   R3   172.16.255.53, 172.16.255.54

R3   R4   172.16.255.57, 172.16.255.58

R2   R4   172.16.255.61, 172.16.255.62

## AREA 1 IP'S

R2   R6   172.16.255.65, 172.16.255.66

R6   R5   172.16.255.69, 172.16.255.70

R2   R5   172.16.255.73, 172.16.255.74

## AREA 2 IP'S /30 

R3   R7   172.16.255.77, 172.16.255.78

R3   R8   172.16.255.81, 172.16.255.82

R7   R8   172.16.255.85 ,172.16.255.86







# OSPF INTER AREA ROUTING

# TOPOLOGY

![image](https://github.com/user-attachments/assets/6c5c5e33-8d4c-492a-bf39-e5edcdd2dbbd)


For ospf, all areas must have a link to area 0 which acts as a BB area.

R1#

interface GigabitEthernet1/0

 ip address 172.16.255.49 255.255.255.252
	
 ip ospf 1 area 0
	
 negotiation auto
	
 mpls label protocol ldp
	
 mpls ip
	
end

R1#

interface GigabitEthernet2/0

 ip address 172.16.255.53 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R1#


R2#

interface GigabitEthernet1/0

 ip address 172.16.255.50 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R2#

interface GigabitEthernet2/0

 ip address 172.16.255.61 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R2#

interface GigabitEthernet3/0

 ip address 172.16.255.65 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R2#

interface GigabitEthernet4/0

 ip address 172.16.255.73 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R3#

interface GigabitEthernet1/0

 ip address 172.16.255.57 255.255.255.252
 
 ip ospf 1 area 0
 
 negotiation auto
 
 mpls ip
 
end

R3#

interface GigabitEthernet2/0

 ip address 172.16.255.54 255.255.255.252
 
 ip ospf 1 area 0
 
 negotiation auto
 
 mpls ip
 
end

R3#

interface GigabitEthernet3/0

 ip address 172.16.255.77 255.255.255.252
 
 ip ospf 1 area 2
 
 negotiation auto
 
end

R3#

interface GigabitEthernet4/0

 ip address 172.16.255.81 255.255.255.252
 
 ip ospf 1 area 2
 
 negotiation auto
 
end

R4#

interface GigabitEthernet1/0

 ip address 172.16.255.58 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R4#

interface GigabitEthernet2/0

 ip address 172.16.255.62 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R4#

R5#

interface GigabitEthernet2/0

 ip address 172.16.255.70 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R5#

interface GigabitEthernet4/0

 ip address 172.16.255.74 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R6#

interface GigabitEthernet2/0

 ip address 172.16.255.69 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R6#

interface GigabitEthernet3/0

 ip address 172.16.255.66 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R6#

R7#

interface GigabitEthernet1/0

 ip address 172.16.255.85 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R7#

interface GigabitEthernet3/0

 ip address 172.16.255.78 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R8#

interface GigabitEthernet1/0

 ip address 172.16.255.86 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end

R8#

interface GigabitEthernet4/0

 ip address 172.16.255.82 255.255.255.252
 
 negotiation auto
 
 mpls ip
 
end


# Verification of OSPF

# R1#sh ip ospf neighbor

Neighbor ID     Pri   State           Dead Time   Address         Interface

10.0.3.3          1   FULL/BDR        00:00:35    172.16.255.54   GigabitEthernet2/0

10.0.2.2          1   FULL/BDR        00:00:33    172.16.255.50   GigabitEthernet1/0


# R1#sh ip ospf rib


![image](https://github.com/user-attachments/assets/64a419a0-5f42-448c-83cd-df4e2286b3db)


![image](https://github.com/user-attachments/assets/b5ed4a00-53ce-4889-8ba9-0e160549d83f)


![image](https://github.com/user-attachments/assets/31543c98-dee6-464b-89a3-7d239925f0e8)

# AREA 1 ABR RIB


![image](https://github.com/user-attachments/assets/04e02af8-0e2c-4d91-bb8a-d5224a93c18f)


# AREA 2 ABR RIB


![image](https://github.com/user-attachments/assets/f1aa1766-78de-441d-bed9-38b1f86e7c59)










