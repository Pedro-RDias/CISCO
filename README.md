# Cisco

In this file I'll be documenting some cisco commands for configuring and troubleshooting.

## Modes

The cisco IOS has different modes, each one with different commands available.

### User EXEC mode

This is the first mode you get when you connect to a cisco device. It's the most basic mode, and it's used to run basic commands like `show` and `ping`.
This mode is identified by the `>` symbol at the end of the prompt.

### Privileged EXEC mode

This mode is used to run more advanced commands, like `configure` and `debug`. This mode is identified by the `#` symbol at the end of the prompt.
To enter this mode, you need to run the `enable` (or `en`) command in the user EXEC mode.

### Global configuration mode

This mode is used to configure the device. It's identified by the `(config)#` symbol at the end of the prompt.
To enter this mode, you need to run the `configure terminal` (or `conf t`) command in the privileged EXEC mode.

### Interface configuration mode

This mode is used to configure a specific interface. It's identified by the `(config-if)#` symbol at the end of the prompt.
To enter this mode, you need to run the `interface <interface>` command in the global configuration mode.

To see the available interfaces, you can run the `show ip interface brief` command in the privileged EXEC mode.

### Line configuration mode

This mode is used to configure a specific line. It's identified by the `(config-line)#` symbol at the end of the prompt.
To enter this mode, you need to run the `line <line>` command in the global configuration mode.

To see the available lines, you can run the `show line` command in the privileged EXEC mode.

### Router configuration mode

This mode is used to configure a specific router. It's identified by the `(config-router)#` symbol at the end of the prompt.
To enter this mode, you need to run the `router <router>` command in the global configuration mode.

To see the available routers, you can run the `show ip protocols` command in the privileged EXEC mode.

## Configuring

### enable synchronous logging

To enable synchronous logging, you need to run the `logging synchronous` command in the line configuration mode.

This is useful when you are configuring a device, because it prevents the output from being interrupted by log messages.

Example:

```
Router(config)# line console 0
Router(config-line)# logging synchronous
Router(config-line)# exit
```

### Configuring a hostname

To configure a hostname, you need to run the `hostname <hostname>` command in the global configuration mode.

### Configuring a banner

To configure a banner, you need to run the `banner motd <delimiter> <message> <delimiter>` command in the global configuration mode.

Example:

```
Router(config)# banner motd ^ Unauthorized access is prohibited! ^
```

Be careful with the delimiter you choose, because if you use a character that is present in the message, the message will end prematurely.

If you want to show end of line characters in the message, you need to use the `^` character before the character you want to show.

### Configuring a password

A password can be configured in different ways, depending on the type of password you want to configure.

#### Console password

To configure a console password, you need to run the `line console 0` command in the global configuration mode, and then run the `password <password>` command in the line configuration mode.

Example:

```
Router(config)# line console 0
Router(config-line)# password cisco
Router(config-line)# login
Router(config-line)# exit
```

#### Enable password

To configure an enable password, you need to run the `enable password <password>` command in the global configuration mode.

Example:

```
Router(config)# enable password cisco
```

#### Enable secret password

To configure an enable secret password, you need to run the `enable secret <password>` command in the global configuration mode.


Example:

```
Router(config)# enable secret cisco
```

A secret password is more secure than a normal password, because it's encrypted.

#### Encrypting passwords

To encrypt passwords, you need to run the `service password-encryption` command in the global configuration mode.

Example:

```
Router(config)# service password-encryption
```

### Configuring a static IP address

To configure a static IP address, you need to run the `interface <interface>` command in the global configuration mode, and then run the `ip address <ip> <mask>` command in the interface configuration mode.

Example:

```
Router(config)# interface GigabitEthernet 0/0/0
Router(config-if)# ip address <network ip> <mask>
Router(config-if)# no shutdown
Router(config-if)# exit
```

In the case of a switch, you need to run the `interface vlan <vlan>` command in the global configuration mode, and then run the `ip address <ip> <mask>` command in the interface configuration mode.
The vlan number must be the same as the vlan number of the switch. You can see the vlan number of the switch by running the `show vlan` command in the privileged EXEC mode.

To show the IP address of an interface, you can run the `show ip interface <interface>` command in the privileged EXEC mode. To list all the interfaces, you can run the `show ip interface brief` command in the privileged EXEC mode.


### Configuring a default gateway

To configure a default gateway, you need to run the `ip default-gateway <ip>` command in the global configuration mode.

Example:

```
Router(config)# ip default-gateway
```

### Configuring a static route

To configure a static route, you need to run the `ip route` followed by the destination network (ip network and mask) and the 
next hop address.


Example:

```
Router(config)# ip route <network> <mask> <next hop>
```

#### Configuring a backup route

To configure a backup route, you need to run the `ip route` followed by the destination network (ip network and mask) and the weight (the lower the weight, the higher the priority). The default weight is 1.

Example:

```
Router(config)# ip route <network> <mask> <next hop> <weight>
```

### Configuring a dhcp server

Before configuring a dhcp server, you need to exclude the ip addresses that you don't want to be assigned by the dhcp server. To do this, you need to run the `ip dhcp excluded-address <ip>` command in the global configuration mode.

Then you need to configure the dhcp pool. To do this, you need to run the `ip dhcp pool <name>` command in the global configuration mode, and then run the `network <ip> <mask>` command in the dhcp pool configuration mode.

Example:

```
Router(config)# ip dhcp excluded-address
Router(config)# ip dhcp pool LAN
Router(dhcp-config)# network <network> <mask>
```

It's also possible to configure a default gateway and dns servers. To do this, you need to run the `default-router <ip>` and `dns-server <ip>` commands in the dhcp pool configuration mode.

Example:

```
Router(dhcp-config)# default-router <ip>
Router(dhcp-config)# dns-server <ip>
```

### Configuring a dhcp relay

To configure a dhcp relay, you need to run the `interface <interface>` command in the global configuration mode, and then run the `ip helper-address <ip>` command in the interface configuration mode.

Example:

```
Router(config)# interface GigabitEthernet 0/0/0
Router(config-if)# ip helper-address <ip>
Router(config-if)# exit
```

This is useful when you have a dhcp server in a different network.

### Configuring ssh

To configure ssh, you need to have a hostname and a domain name configured. To configure a hostname, you need to run the `hostname <hostname>` command in the global configuration mode. To configure a domain name, you need to run the `ip domain-name <domain name>` command in the global configuration mode.

Example:

```
Router(config)# hostname Router
Router(config)# ip domain-name example.com
```

Then you need to generate a rsa key. To do this, you need to run the `crypto key generate rsa` command in the global configuration mode. You can also specify the size of the key during the generation. The default size is 512 bits.
Example:

```
Router(config)# crypto key generate rsa
```

Then you need to configure the vty lines. To do this, you need to run the `line vty 0 4` command in the global configuration mode, and then run the `transport input ssh` command in the line configuration mode.


Example:

```
Router(config)# line vty 0 4
Router(config-line)# login local

Router(config-line)# transport input ssh
Router(config-line)# exit
```

To be able to login with ssh, you need to have a user configured. To configure a user, you need to run the `username <username> secret <password>` command in the global configuration mode.

Example:

```
Router(config)# username admin secret cisco
```

To be able to enter the global configuration mode, you need to have a password configured. To configure a password, you need to run the `enable secret <password>` command in the global configuration mode.



