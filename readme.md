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
switchport mode trunk
channel-protocol lacp
channel-group 1 mode active
no shutdown
exit


interface range fastEthernet 0/3 - 4
switchport mode trunk
channel-protocol lacp
channel-group 2 mode active
no shutdown
exit


interface range fastEthernet 0/5 - 6
switchport mode trunk
channel-protocol lacp
channel-group 4 mode active
no shutdown
exit


// Configuracion pc | port Fa0/23
interface fastEthernet 0/23
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 0007.EC25.D3BB
switchport port-security mac-address sticky
switchport port-security violation restrict

// Configuracion pc | port Fa0/24 SMTP
interface fastEthernet 0/24
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 000A.4169.E45E
switchport port-security mac-address sticky
switchport port-security violation restrict
exit

//bpdu
spanning-tree portfast default
spanning-tree portfast bpduguard default

//snooping
interface range f0/7-22
ip dhcp snooping limit rate 6

//contraseña
line console 0
password cisco123 
login
exit
line vty 0 4
password cisco123 
Slogin
exit
enable secret cisco123 

```


### SWITCH 1
```c
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown 

// Etherchannel Protocolo LACP | port 1,2,3,4,5,6
interface range fastEthernet 0/1 - 2
switchport mode trunk
channel-protocol lacp
channel-group 3 mode active
no shutdown
exit


interface range fastEthernet 0/3 - 4
switchport mode trunk
channel-protocol lacp
channel-group 2 mode active
no shutdown
exit


interface range fastEthernet 0/5 - 6
switchport mode trunk
channel-protocol lacp
channel-group 5 mode active
no shutdown
exit


// STP secondary
spanning-tree vlan 1 root secondary

// Configuracion pc | port Fa0/24
interface fastEthernet 0/24
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 0060.5CE9.7B00
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

//bpdu
spanning-tree portfast default
spanning-tree portfast bpduguard default

//contraseña
line console 0
password cisco123 
login
exit
line vty 0 4
password cisco123 
Slogin
exit
enable secret cisco123 
```




### SWITCH 2
```c
enable
confgure terminal
interface range fastEthernet 0/1 - 24
shutdown 

// Etherchannel Protocolo LACP | port 1,2,5,6
interface range fastEthernet 0/1 - 2
switchport mode trunk
channel-protocol lacp
channel-group 3 mode active
no shutdown
exit


interface range fastEthernet 0/5 - 6
switchport mode trunk
channel-protocol lacp
channel-group 4 mode active
no shutdown
exit

//spanning-tree portfast

// root bridge STP
spanning-tree vlan 1 root primary

// Puerto del router | port Fa0/3
// interface fastEthernet 0/3
// no shutdown
// switchport mode access
// switchport port-security
// switchport port-security maximum 3
// switchport port-security violation restrict
// switchport port-security mac-address sticky
// spanning-tree portfast
// spanning-tree bpduguard enable
// exit
// ip arp inspection vlan 1
// interface fa0/1
// ip arp inspection trust

// Configuracion pc | port Fa0/24
interface fastEthernet 0/24
no shutdown
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 0060.3EE3.9602
switchport port-security mac-address sticky
switchport port-security violation restrict

//bpdu
spanning-tree portfast default
spanning-tree portfast bpduguard default

//snooping
interface range f0/7-22
ip dhcp snooping limit rate 6

//contraseña
line console 0
password cisco123 
login
exit
line vty 0 4
password cisco123 
Slogin
exit
enable secret cisco123 
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
password cisco123 
login
exit
line vty 0 4
password cisco123 
login
exit
enable secret cisco123 

// Etherchannel Protocolo LACP | port 1,2,5,6
interface range fastEthernet 0/1 - 2 
switchport mode trunk
channel-protocol lacp
channel-group 1 mode active
no shutdown
exit


interface range fastEthernet 0/5 - 6
switchport mode trunk
channel-protocol lacp
channel-group 5 mode active
no shutdown
exit


//bpdu
spanning-tree portfast default
spanning-tree portfast bpduguard default

//snooping
interface range f0/7-22
ip dhcp snooping limit rate 6

//contraseña
line console 0
password cisco123 
login
exit
line vty 0 4
password cisco123 
Slogin
exit
enable secret cisco123 
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


//contraseña
enable secret cisco123
line console 0
password cisco123
login
exit
line vty 0 4
password cisco123
login
exit
crypto key generate rsa
username admin secret cisco123
hostname Router01
line vty 0 4
login local


```
### Router 2
//contraseña
enable secret cisco123
line console 0
password cisco123
login
exit
line vty 0 4
password cisco123
login
exit
crypto key generate rsa
username admin secret cisco123
hostname Router02
line vty 0 4
login local


### Router 3
//contraseña
enable secret cisco123
line console 0
password cisco123
login
exit
line vty 0 4
password cisco123
login
exit
crypto key generate rsa
username admin secret cisco123
hostname Router03
line vty 0 4
login local




![image](https://github.com/jmorales01/cisco/assets/91076395/45e4f5d2-388e-41b1-923f-d413db60e8e3)
