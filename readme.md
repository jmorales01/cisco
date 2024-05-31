# REDES PC2 

### SWITCH 0
```c
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown
interface range gi0/1 - 2
shutdown

// Configurar DHCP | port fa0/23
ip dhcp snooping
ip arp inspection vlan 1
ip arp inspection vlan 1
interface fastEthernet 0/23
no shutdown
ip dhcp snooping trust
ip arp inspection trust
exit
interface range fastEthernet 0/1 -22
ip dhcp snooping limit rate 6
exit
ip arp inspection validate src-mac dst-mac ip
do show run | include validate 

// Etherchannel Protocolo LACP | port 1,2,3,4,5,6
interface range fastEthernet 0/1 - 2
channel-protocol lacp
channel-group 1 mode active
no shutdown
exit
interface port-channel 1
switchport mode trunk

interface range fastEthernet 0/3 - 4
channel-protocol lacp
channel-group 2 mode active
no shutdown
exit
interface port-channel 2
switchport mode trunk

interface range fastEthernet 0/5 - 6
channel-protocol lacp
channel-group 4 mode active
no shutdown
exit
interface port-channel 4
switchport mode trunk
```

### SWITCH 1
```c
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown 

// Etherchannel Protocolo LACP | port 1,2,3,4,5,6
interface range fastEthernet 0/1 - 2
channel-protocol lacp
channel-group 3 mode active
no shutdown
exit
interface port-channel 3
switchport mode trunk

interface range fastEthernet 0/3 - 4
channel-protocol lacp
channel-group 2 mode active
no shutdown
exit
interface port-channel 2
switchport mode trunk

interface range fastEthernet 0/5 - 6
channel-protocol lacp
channel-group 5 mode active
no shutdown
exit
interface port-channel 5
switchport mode trunk

// STP secondary
spanning-tree vlan 1 root secondary

// Configuracion pc | port Fa0/24
interface fastEthernet 0/24
no shutdown
switchport mode access                          // Mitigar los ataques de la tabla de direcciones MAC
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 0090.0CDC.DC31
switchport port-security mac-address sticky
switchport port-security violation restrict

spanning-tree portfast
spanning-tree bpduguard enable

// Configuracion wireless | port Fa0/23
interface fastEthernet 0/23
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 3
switchport port-security mac-address sticky
switchport port-security violation restrict


```

### SWITCH 2
```c
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown 

// Etherchannel Protocolo LACP | port 1,2,5,6
interface range fastEthernet 0/1 - 2
channel-protocol lacp
channel-group 3 mode active
no shutdown
exit
interface port-channel 3
switchport mode trunk

interface range fastEthernet 0/5 - 6
channel-protocol lacp
channel-group 4 mode active
no shutdown
exit
interface port-channel 4
switchport mode trunk
spanning-tree portfast

// root bridge STP
spanning-tree vlan 1 root primary

// Puerto del router | port Fa0/3
interface fastEthernet 0/3
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 3
switchport port-security violation restrict
switchport port-security mac-address sticky
spanning-tree portfast
spanning-tree bpduguard enable
exit
ip arp inspection vlan 1
interface fa0/1
ip arp inspection trust
```

### SWITCH 3
```c
// Apagar todo los switchs
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown

// Poner contraseña al switch
line console 0
password cisco123               // contraseña de la consola
login
exit
line vty 0 4
password cisco123               // contraseña de VTY (Telnet/SSH)
login
exit
enable secret cisco123          // contraseña enable

// Etherchannel Protocolo LACP | port 1,2,5,6
interface range fastEthernet 0/1 - 2 
channel-protocol lacp
channel-group 1 mode active
no shutdown
exit
interface port-channel 1
switchport mode trunk

interface range fastEthernet 0/5 - 6
channel-protocol lacp
channel-group 5 mode active
no shutdown
exit
interface port-channel 5
switchport mode trunk
```

### DHCP
IPv4: `192.168.1.2`
Mask: `255.255.255.0`
Default Gateway: `192.168.1.1`

Pool Name: `serverPool`
Default Gateway: `192.168.1.1`
Start IP Address: `192.168.1.20`
Subnet Mask: `255.255.255.0`
Max User: `100`

Service: On

### SMTP

### Wireless Router 1
SSID: `admin`
WPA2-PSK: `cisco123`

### Router 1
```c
enable
configure terminal

// Acceso a switch | port Gi0/0
interface gi0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
// Acceso a router3 | port Gi0/1
interface gi0/1
ip address 192.168.10.2 255.255.255.0
no shutdown
// Acceso a router2 | port Gi0/2
interface gi0/2
ip address 192.168.10.3 255.255.255.0
no shutdown

exit
enable secret cisco123
line console 0
password cisco123
login
exit
line vty 0 4
password cisco123
login
exit
ip domain-name google.com
crypto key generate rsa
username admin secret cisco123
hostname Router01
line vty 0 4
transport input ssh
login local
exit
ip ssh version 2
```
### Router 2
### Router 3