# REDES PC2 

```javascript
function test(){
	console.log("Hello world!");
}
 
(function(){
    var box = function(){
        return box.fn.init();
    };

    box.prototype = box.fn = {
        init : function(){
            console.log('box.init()');

			return this;
        },

		add : function(str){
			alert("add", str);

			return this;
		},

		remove : function(str){
			alert("remove", str);

			return this;
		}
    };
    
    box.fn.init.prototype = box.fn;
    
    window.box =box;
})();

var testBox = box();
testBox.add("jQuery").remove("jQuery");
```

### SWITCH 0
```c
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
```

### SWITCH 3
```c
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
```

### DHCP

### SMTP
