# REDES PC2 

### SWITCH 0
`
    enable
    confgure terminal
    interface range fastEthernet 0/1 - 24
    shutdown 

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
`

### SWITCH 1
`
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
`

### SWITCH 2
`
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
`

### SWITCH 3
`
    // Apagar todo los switchs
    enable
    confgure terminal
    interface range fastEthernet 0/1 - 24
    shutdown 

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
`

### DHCP

### SMTP