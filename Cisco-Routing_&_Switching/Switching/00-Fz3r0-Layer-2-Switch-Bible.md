## Cisco IOS-XE: `Layer 2 Switch (10)` - `IOS-SWL2-10`

````py
! #####################################################################################
! # WIPE SWITCH TO INIT SETTINGS                                                      #
! #####################################################################################

! # Completely erases the startup configuration
!write erase

! # Completely erases the VLAN database
!delete vlan.dat

! # Completely erases any stack provisioning
!no switch <X> provision

! # Reloads the device for a full factory‑default reset.
!reload

! #####################################################################################
! # ENABLE + CONFIG (Expand CLI terminal width)                                       #
! #####################################################################################

! # Enters privileged EXEC mode to allow configuration and full device control.
enable

! # Sets terminal line width to 500 characters to prevent line wrapping on long outputs.
terminal width 500

! # Enters global configuration mode to make changes to the running configuration.
configure terminal

! #####################################################################################
! # BANNER (MOTD, LOGIN, EXEC, INCOMING, SLIP-PPP)                                    #
! #####################################################################################

! # banner motd     - Shown before the login prompt (to everyone who connects)
! # banner login    - Shown right before the login prompt, after MOTD
! # banner exec     - Displayed after a successful login, before the user gets the prompt
! # banner incoming - Only for reverse Telnet sessions (console ports through an access server)
! # banner slip-ppp - (rare) SLIP or PPP connections, almost obsolete today

banner motd $


#######################################################################################
#                                                                                     #
#                              - Fz3r0 :: Cisco IOS-XE -                              #
#                                                                                     #
#   * Github    : Fz3r0                                                               #
#   * X Twitter : @fz3r0_OPs                                                          #
#   * Youtube   : @fz3r0_OPs                                                          #
#                                                                                     #
#######################################################################################
#                                                                                     #
#    SWITCH     : IOS-SWL2-10                                                         #
#    ROLE       : ACCESS/DISTRIBUTION SWITCH - LAYER 2                                #
#                                                                                     #
#    MGMT       : 10.10.66.10/23 (V66 MGMT)                                           #
#    LOGIN      : admin / admin.cisco                                                 #
#    ENABLE     : admin.cisco                                                         #
#                                                                                     #
#    GATEWAY    : 10.10.66.1/23 (255.255.255.0)                                       #
#    DNS1       : 8.8.8.8                                                             #
#    DNS2       : 8.8.4.4                                                             #
#                                                                                     #
#    VLAN       : VLAN 10     : BLUE        (HOST DATA)                               #
#               : VLAN 20     : RED         (HOST DATA)                               #
#               : VLAN 30     : GREEN       (HOST DATA)                               #
#               : VLAN 50     : WI-FI       (802.11 WIRELESS / WI-FI)                 #
#               : VLAN 66     : MANAGEMENT  (MANAGEMENT / ADMIN)                      #
#               : VLAN 99     : NATIVE      (TRUNK NATIVE VLAN)                       #
#               : VLAN 1      : DEFAULT     (OFF / UNUSED)                            #
#                                                                                     #
#    TRUNK      : Po1 - Eth0/0-1 (Uplink 1) - Allowed 10,20,30,50,66 - Native 99      #
#               : Po2 - Eth0/2-3 (Uplink 2) - Allowed 10,20,30,50,66 - Native 99      #
#                                                                                     #
#    ACCESS     : Eth1/0-1    : Access VLAN 10 (BLUE)       SVI - (no ip address)     #
#               : Eth1/2-3    : Access VLAN 20 (RED)        SVI - (no ip address)     #
#               : Eth2/0-1    : Access VLAN 30 (GREEN)      SVI - (no ip address)     #
#               : Eth2/2      : Access VLAN 50 (WI-FI)      SVI - (no ip address)     #
#               : Eth2/3      : Access VLAN 66 (MANAGEMEN)  SVI - 10.10.66.10/23      #
#                                                                                     #
#    STP        : STP MODE    : PVST+ (Per-VLAN Spanning Tree Plus)                   #
#               : VLANS       : 1-99                                                  #
#               : DEVICE ROLE : NON-ROOT BRIDGE                                       #
#               : PRIORITY    : 4096 (HIGH NUMBER = LEAST PREFERENCE)                 #
#                                                                                     #
#    AAA        : AUTHENTICATION     :  RADIUS (PORT 1645)                            #
#               : ACCOUNTING         :  RADIUS (PORT 1646)                            #
#               : RADIUS SERVER      :  Fz3r0_RADIUS_SERVER                           #
#               : RADIUS SERVER IP   :  10.10.66.201                                  #
#               : SHARED SECRET KEY  :  Fz3r0_RADIUS_Pa$$w0rD                         #
#                                                                                     #
#    SNMP       : SNMP VERSION       :  SNMPv3                                        #
#               : MONITOR SERVER     :  SOLARWINDS                                    #
#               : SOLARWINDS SERVER  :  10.10.66.202     



#    CLOCK      : INTERNAL   :  CST -6 (summer-time CDT) : 12:45:00 July 21 2025      #
                : NTP SERVER :  216.239.35.0 (time.google.com)

#                                                                                     #
#######################################################################################


$

! #####################################################################################
! # HOST NAME                                                                         #
! #####################################################################################

! # Sets the device hostname. Shown in CLI, SNMP, syslog, remote access, etc.
hostname IOS-SWL2-10

! #####################################################################################
! # ACTIVE SERVICES                                                                   #
! #####################################################################################

! # Disables the legacy X.25 PAD service, which is unnecessary on modern networks.
no service pad

! # Adds detailed timestamps (with milliseconds) to all debug output.
service timestamps debug datetime msec
! # Adds local time timestamps to log messages for accurate event tracking.
service timestamps log datetime localtime
! # Cisco’s weak Type‑7 password encryption, hide plain-text passwords in configs (basic obfuscation, not strong security).
service password-encryption

! #####################################################################################
! # DOMAIN & DNS                                                                      #
! #####################################################################################

! # DOMAIN = (fz3r0.com) - Device’s default DNS domain name (used to build its FQDN) / required for generating SSH keys / appended when resolving unqualified hostnames  | ping server1 (server1.fz3r0.com)
ip domain-name fz3r0.com

! # DNS 1 - Google DNS 
ip name-server 8.8.8.8
! # DNS 2 - Google DNS 2
ip name-server 8.8.4.4

! # Disable DNS lookup (For labs: Avoid delays when typing invalid commands)
no ip domain-lookup

! #####################################################################################
! # DEFAULT GATEWAY & IP ROUTE                                                        #
! #####################################################################################

! # L2 : Default Gateway @ GW - Pure Layer 2 (no routing enabled) send traffic to local LAN GW.
ip default-gateway 10.10.66.1

! # L3 : Default Route @ GW->WAN - Default route to a Layer‑3 Router, sending all non‑local traffic to 10.10.66.1 (Gateway) so it can reach ANY OUTSIDE networks.
ip route 0.0.0.0 0.0.0.0 10.10.66.1

! #####################################################################################
! #  ENABLE IP ROUTING                                                                #
! #####################################################################################

!# (For L3 Switches) Enables the router to forward packets BETWEEN DIFFERENT NETWORKS (turns on Layer 3 routing between management & default VRFs) - InterVLAN Routing
!ip routing

! #####################################################################################
! # REMOTE ACCESS & SECURITY                                                          #
! #####################################################################################

! # MINIMUM PASSWORDS LENGHT
security passwords min-length 10

! # ADMIN LOGIN
username admin privilege 15 secret admin.cisco

! # LOW PRIV LOGIN (Custom 5)
username noob privilege 5 secret admin.cisco

! # PRIVILEGE 5 (CUSTOM) ALLOW SHOW AND PING
privilege exec level 5 ping
privilege exec level 5 show running-config view full
privilege exec level 5 show running-config view
privilege exec level 5 show running-config all
privilege exec level 5 show running-config
privilege exec level 5 show

! # ENABLE LOGIN
enable secret admin.cisco

! # LOGIN LOG & LOGIN ATTEMPS
login block-for 60 attempts 3 within 60
login on-failure log
login on-success log

! # RSA KEY - you must set a hostname and domain name before (to define device’s FQDN); once the keys are created, they enable SSH and other cryptographic functions.
crypto key generate rsa modulus 2048

! # SSHv2 enable (Virtual Lab Unavailable)
!ip ssh version 2

! # CONSOLE LINE LOGIN (SERIAL/CONSOLE) 
line con 0
   password admin.cisco
   logging synchronous
   transport preferred none
   stopbits 1
exit

! # VTY LINES LOGIN (SSH/Telnet) - ACL for VTY Access Control | Deny = default Implicit (best practice to write it out) 
ip access-list standard Fz3r0-VTY-ACCESS
  ! # Networks
  permit 10.10.0.0 0.0.255.255        
  permit 10.10.66.0 0.0.0.255
  ! # Hosts    
  permit host 10.10.10.101            
  permit host 10.10.20.101
  ! # Deny (implicit)  
  deny any                         
exit
! # VTY LINES LOGIN (INTERFACES - VIRTUAL TELETYPE (SSH/Telnet) - Use telnet ONLY in lab enviorments / Use SSHv2 in real scenarios
line vty 0 4
   access-class Fz3r0-VTY-ACCESS in
       exec-timeout 0 0
       password admin.cisco
       transport preferred none
           transport input telnet
           !transport input ssh
exit

! # NO HTTP SERVER 
no ip http server
no ip http secure-server

! # Avoid port blocking if MAC (e.g. user laptop) change to other switch interface
authentication mac-move permit

! #####################################################################################
! # RADIUS & AAA                                                                      #
! #####################################################################################

! # AAA
aaa new-model
aaa authentication login default local group radius
aaa authorization exec default local group radius 
aaa session-id common

! # RADIUS
radius server Fz3r0_RADIUS_SERVER
   address ipv4 10.10.66.201 auth-port 1645 acct-port 1646
   key Fz3r0_RADIUS_Pa$$w0rD
exit

! #####################################################################################
! # CLOCK & TIME                                                                      #
! #####################################################################################

! # INTERNAL CLOCK
clock timezone CST -6 0
clock summer-time CDT recurring
do clock set 12:45:00 July 21 2025

! # NTP SERVER - Google (manual clock will be overwirted automatically)
ntp server 216.239.35.0

! #####################################################################################
! # PV-STP (non root)                                                                 #
! #####################################################################################

spanning-tree mode pvst
spanning-tree extend system-id
spanning-tree vlan 1-99 priority 4096

! #####################################################################################
! # VLANs                                                                             #
! #####################################################################################

!# VLAN
vlan 10
   name V10-BLUE
exit
vlan 20
   name V10-RED
exit
vlan 30
   name V10-GREEN
exit
!
vlan 40 
   name V40-VOIP
exit
!
vlan 50 
   name V50-WIRELESS-TUNNEL
exit
!
vlan 66 
   name V66-MANAGEMENT
exit
!
vlan 99
   name VLAN99-TRUNK-NATIVE
   vlan dot1q tag native
! # <Auto-Exit>

! # VTP OFF (FOR SECURITY - CVE-2018-0197)
vtp domain _VTP_DISABLED_CVE-2018-0197_
vtp mode off

! #####################################################################################
! # SWITCH VIRTUAL INTERFACES (SVI)                                                   #
! #####################################################################################

! # SVI - VLAN 1 DEFAULT SHUTDOWN (for security)
interface Vlan1
   no ip address
   shutdown
exit

! # SVI - VLAN 10 (/23 = 510 hosts)
interface Vlan10
   description *** V10-BLUE ***
   no ip address
   no shutdown
exit
!
! # SVI - VLAN 20 (/23 = 510 hosts)
interface Vlan20
   description *** V20-RED ***
   no ip address
   no shutdown
exit
!
! # SVI - VLAN 30 (/23 = 510 hosts)
interface Vlan30
   description *** V30-GREEN ***
   no ip address
   no shutdown
exit

! # SVI - VLAN 50 (WIRELESS) (/23 = 510 hosts)
interface Vlan50
   description *** V50-WIFI-WIRELESS ***
   no ip address
   no shutdown
exit

! # SVI - VLAN 66 (MANAGEMENT) (/23 = 510 hosts)
interface Vlan66
   description *** V66-MANAGEMENT ***
   ip address 10.10.66.10 255.255.254.0
   no shutdown
exit

! #####################################################################################
! # SWITCH PHYSICAL INTERFACES (ETHERNET)                                             #
! #####################################################################################

! # <<< TRUNK INTERFACES >>>

Per Cisco:  If you have EtherChannel ports configured on your switch, you must configure QoS classification, policing, mapping, and queueing on the individual physical ports that comprise the EtherChannel.

! # PORT CHANNEL 1 - TRUNK UPLINK TO SWITCH (OTHER SWITCH) # 1
interface Port-channel 1
! # ALL trunk-related configs here, the logical interface. (Po1 members stay minimal)
   description ** Po1 UPLINK - TRUNK - L2 AUTOQOS **
      switchport trunk encapsulation dot1q
      switchport mode trunk
      switchport trunk native vlan 99
      switchport trunk allow vlan 10,20,30,50,66
exit
!
interface range Ethernet 0/0-1
! # Minimal trunk config on member ports (All logic applied on Port-Channel 1)
   description ** UPLINK - TRUNK - Po1 MEMBER *
      switchport trunk encapsulation dot1q
      switchport mode trunk
         ! # Auto QoS for trust trunk - Config it only in Physical Eth (In Po the command doesn't even exists)
         mls qos trust dscp
             channel-group 1 mode active
exit

! # PORT CHANNEL 2 - TRUNK UPLINK TO SWITCH (OTHER SWITCH) # 2
interface Port-channel 2
! # ALL trunk-related configs here, the logical interface. (Po2 members stay minimal)
   description ** Po2 UPLINK - TRUNK - L2 AUTOQOS **
      switchport trunk encapsulation dot1q
      switchport mode trunk
      switchport trunk native vlan 99
      switchport trunk allow vlan 10,20,30,50,66
exit
!
interface range Ethernet 0/2-3
! # Minimal trunk config on member ports (All logic applied on Port-Channel 2)
   description ** UPLINK - TRUNK - Po2 MEMBER *
      switchport trunk encapsulation dot1q
      switchport mode trunk
         ! # Auto QoS: Trust trunk for QoS - Config it only in Physical Eth (In Po the command doesn't even exists)
         mls qos trust dscp
             channel-group 2 mode active
exit

! # <<< ACCESS INTERFACES >>>

! # ACCESS VLAN 10 (BLUE) + VLAN 40 (VOICE) - BLUE HOST
interface range Ethernet 1/0-1
   description ** ACCESS V10-BLUE + VOIP **
      switchport mode access
      switchport access vlan 10
         switchport voice vlan 40
         ! # Auto QoS: (Access Eth Host - Cisco Phone) AutoQoS macro for IP phones. Voice QoS on port + generates global QoS commands (mls qos, auto qos srnd4, class‑maps, and policy‑maps) across the switch.
         auto qos voip cisco-phone
               spanning-tree portfast edge
                  ! # Port Security: (Access Eth Host) Locks 2 static MAC + 8 sticky MACs | Violation opts: shutdown/protect/restrict | delete inactive sticky after 60 min
                  switchport port-security
                  switchport port-security maximum 10
                  switchport port-security mac-address 0000.1111.2222    ! # PC 1 fija
                  switchport port-security mac-address 0000.3333.4444    ! # PC 2 fija
                  switchport port-security mac-address sticky
                  switchport port-security violation shutdown 
                  switchport port-security aging time 60                 ! # 60 min
                  switchport port-security aging type inactivity         ! # solo borra sticky inactivas
exit

! # ACCESS VLAN 20 (RED) + VLAN 40 (VOICE) - RED HOST
interface range Ethernet 1/2-3
   description ** ACCESS V20-RED + VOIP **
      switchport mode access
      switchport access vlan 20
         switchport voice vlan 40
         ! # Auto QoS for access host (Cisco Phone)
         auto qos voip cisco-phone
               spanning-tree portfast edge
exit

! # ACCESS VLAN 30 (GREEN) + VLAN 40 (VOICE) - GREEN HOST
interface range Ethernet 2/0-1
   description ** ACCESS V30-GREEN + VOIP **
      switchport mode access
      switchport access vlan 30
         switchport voice vlan 40
         ! # Auto QoS for access host (Cisco Phone)
         auto qos voip cisco-phone
               spanning-tree portfast edge
exit

! # ACCESS VLAN 50 (WIRELESS)
interface Ethernet 2/2
   description ** ACCESS V50 - 802.11 - WI-FI AP **
      switchport mode access
      switchport access vlan 50
               spanning-tree portfast edge
exit

! # ACCESS VLAN 66 (MANAGEMENT)
interface Ethernet 2/3
   description ** ACCESS V66 - MANAGEMENT **
      switchport mode access
      switchport access vlan 66
               spanning-tree portfast edge
exit

! # CLEAR PORT-SECURITY STICKY MAC LEARNED (IF NEEDED)
!clear port-security sticky interface range Ethernet1/0-3,Ethernet2/0-3,Ethernet3/0-3

! #####################################################################################
! # SNMP (MONITORING: SolarWinds, Grafana, Data Dog, Zabbix, etc)                     #
! #####################################################################################

! # SNMP ACL - SNMP Allow IPs [Other Branch Networks / Single Hosts] - (Deny everything else) 
ip access-list standard Fz3r0-SNMP-ACL
   ! # INTERNAL SITE NETWORK:
   permit 10.10.0.0
   ! # EXTERNAL NETWORKS (Other Servers):
   permit 10.11.0.0
   permit 10.12.0.0
   permit 10.13.0.0
   ! # EXTERNAL HOSTS (External Support Monitoring):
   permit 10.20.0.10
   permit 10.20.0.11
   permit 10.20.0.12
exit

! # SNMP LOCAL CHASSIS-ID - Monitoring tools (Solarwinds) display this identifier for the device.
snmp-server chassis-id IOS-SWL2-10

! # SNMP GROUP - “priv” security level (auth + encryption). binds to ACL Fz3r0-SNMP-ACL to restrict which IPs can access SNMP through this group.
snmp-server group Fz3r0-SNMP-GROUPNAME v3 priv access Fz3r0-SNMP-ACL

! # SNMP USER - with SHA authentication and AES‑256 privacy, assigned to the specified SNMP group.
! # Real Catalyst IOS-XE example
!snmp-server user Fz3r0-SNMP-USERNAME Fz3r0-SNMP-GROUPNAME v3 auth sha Fz3r0-SNMP-Auth_Pa$$ priv aes 256 Fz3r0-SNMP-Priv_Pa$$
! # Virtual IOS (Eve-NG / GNS3)
snmp-server user Fz3r0-SNMP-USERNAME Fz3r0-SNMP-GROUPNAME v3 auth sha Fz3r0-SNMP-Auth_Pa$$ access Fz3r0-SNMP-Priv_Pa$$

! # SNMP MANAGER & SNMPv3 TRAPS DST (SolarWinds Server) - Where to send SNMP traps and informs it which credentials to use
!snmp-server host <SolarWinds IP> version 3 priv <SNMP Username>
snmp-server host 10.10.0.201 version 3 priv Fz3r0-SNMP-USERNAME 

! # SNMP MANAGER(SERVER) LOGGING - MAIN (SOLARWINDS)
logging host 10.10.0.201
! # SNMP MANAGER(SERVER) LOGGING - SECONDARY (External Support Monitoring Hosts)
logging host 10.20.0.10
logging host 10.20.0.11
logging host 10.20.0.12

! # Disables SYSLOG messages forwarding; SNMP traps (UDP/162) are completely unaffected by this command
no logging trap

! # Enables linkUp/linkDown traps follow the industry-standard IETF MIB‑II format (RFC 1215) for interface status changes.
snmp-server trap link ietf

! # EnergyWise domain & shared secret: authenticate with EnergyWise and report power usage/events via SNMP traps.
energywise domain fz3r0_energy.com security shared-secret 0 3n3rgy.Pa$$w0rD

! # Globally enable various SNMP trap generation, ensuring the device notifies SNMP managers (SolarWinds)
snmp-server enable traps snmp
snmp-server enable traps snmp authentication linkdown linkup coldstart warmstart
snmp-server enable traps snmp linkdown
snmp-server enable traps snmp linkup
snmp-server enable traps ospf
snmp-server enable traps pim
snmp-server enable traps auth-framework
snmp-server enable traps auth-framework sec-violation
snmp-server enable traps ethernet cfm
snmp-server enable traps event-manager
snmp-server enable traps hsrp
snmp-server enable traps syslog
snmp-server enable traps tty
snmp-server enable traps vtp
snmp-server enable traps vlancreate
snmp-server enable traps vlandelete
snmp-server enable traps energywise

! #####################################################################################
! # SAVE CONFIGURATION                                                                #
! #####################################################################################

! # End exits all config modes to privileged EXEC
end

! # Legacy & Alias version of "copy running-config startup-config" 
!write memory
! # Modern: Saves active configuration from RAM (running-config) into NVRAM as the startup-config.
copy running-config startup-config


!
!


````











## Resources

- https://github.com/Fz3r0/Fz3r0/tree/1914a8727ec9f0f2eca5c63c789c5b37f99bdad7/Networking/Labs
- https://github.com/Fz3r0/Fz3r0/blob/1914a8727ec9f0f2eca5c63c789c5b37f99bdad7/Networking/Labs/EtherChannel_All-in-One.md
- https://github.com/Fz3r0/Fz3r0/blob/1914a8727ec9f0f2eca5c63c789c5b37f99bdad7/Networking/Labs/Security_%26_Best-Practices_VS_Layer2_Attack_FULL_PRO_CONFIG.md
