


## ACL

- Use allways Extended ACLs so names and order can be edited, even the ACL is a basic IP block/allow, also config is cleaner, example:

### 📋 ACL: `NAT Overload (PAT)` (Standard Version)

- ACL Type: `Standard`
- Purpose: `Classification`
- Use Case: Select which IPs or subnets will be translated to the router’s public IP using Port Address Translation (PAT).

````py
!# Step 1 – Create the ACL that defines which internal IPs are allowed to be NATed / using: 10.10.x.x/16 (Mask: 255.255.0.0)
access-list 10 permit 10.10.0.0 0.0.255.255  #! <<<<<<--- STANDARD 
access-list 10 permit 10.30.0.0 0.0.255.255  #! <<<<<<--- STANDARD 
access-list 10 permit 10.50.0.0 0.0.255.255  #! <<<<<<--- STANDARD 

!# Step 2 – Create the NAT rule that applies overload using the ACL
ip nat inside source list 10 interface GigabitEthernet0/0 overload

!# Step 3 – Mark interfaces as NAT inside or outside
interface GigabitEthernet0/1
   description ** LAN (Inside) **
   ip address 10.10.0.1 255.255.0.0
   ip nat inside
exit

interface GigabitEthernet0/0
   description ** WAN (Outside) **
   ip address 123.1.1.2 255.255.255.252
   ip nat outside
exit
````

### 📋 ACL: `NAT Overload (PAT)` (Extended Version)

- ACL Type: `Extended`
- Purpose: `Classification`
- Use Case: Select which IPs or subnets will be translated to the router’s public IP using Port Address Translation (PAT).

````py
!# Step 1 – Create the ACL that defines which internal IPs are allowed to be NATed / using: 10.10.x.x/16 (Mask: 255.255.0.0)
ip access-list extended NAT_INSIDE       #! <<<<<<--- EXTENDED
   permit ip 10.10.0.0 0.0.255.255 any   #! <<<<<<--- EXTENDED
   permit ip 10.30.0.0 0.0.255.255 any   #! <<<<<<--- EXTENDED
   permit ip 10.50.0.0 0.0.255.255 any   #! <<<<<<--- EXTENDED
exit                                     #! <<<<<<--- EXTENDED

!# Step 2 – Create the NAT rule that applies overload using the ACL
ip nat inside source list NAT_INSIDE interface GigabitEthernet0/0 overload

!# Step 3 – Mark interfaces as NAT inside or outside
interface GigabitEthernet0/1
   description ** LAN (Inside) **
   ip address 10.10.0.1 255.255.0.0
   ip nat inside
exit

interface GigabitEthernet0/0
   description ** WAN (Outside) **
   ip address 123.1.1.2 255.255.255.252
   ip nat outside
exit
````
