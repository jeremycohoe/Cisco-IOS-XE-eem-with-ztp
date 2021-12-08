# Example for ZTP + EEM + GuestShell

# Configure Guest Shell via EEM applet

This applet is used to send the CLI commands to configure guest shell. Note the wait times to allow for day0guestshell to shut down and remove some conflicting CLI's before the CLI's are applied by this applet.

# Enable Guest Shell via EEM applet

This applet is used to enable Guest Shell and run a script automatically. Note the wait time to allow for Guest Shell to start after the day0guestshell is destroyed and conflicting CLI's removed.

# Run the EEM applets and copy commands
Copy some files to the guest-share folder so that the EEM applet can execute them. Now the EEM applets are run. Again note the wait times, first the config CLI's are added, then Guest Shell is enabled and command is run.



ztp.py:
```
# Configure Guest Shell via EEM applet
print ("*** Configure eem_gs_config ... ***")
eem_gs_commands = ['no event manager applet eem_gs_config',
                'event manager applet eem_gs_config',
                'event none maxrun 900',
                'action 1003 wait 150',
                'action 1004 cli command "enable"',
                'action 1005 cli command "conf t"',
                'action 1006 cli command "iox"',
                'action 1007 cli command "ip nat inside source list NAT_ACL interface vlan 1 overload"',
                'action 1008 cli command "ip access-list standard NAT_ACL"',
                'action 1009 cli command "permit 192.168.0.0 0.0.255.255"',
                'action 1010 cli command "exit"',
                'action 1011 cli command "vlan 4094"',
                'action 1012 cli command "exit"',
                'action 1013 cli command "int vlan 4094"',
                'action 1014 cli command "ip address 192.168.2.1 255.255.255.0"',
                'action 1015 cli command "ip nat inside"',
                'action 1016 cli command "ip routing"',
                'action 1017 cli command "ip route 0.0.0.0 0.0.0.0 10.1.1.3"',
                'action 1101 cli command "app-hosting appid guestshell"',
                'action 1102 cli command "app-vnic AppGigabitEthernet trunk"',
                'action 1103 cli command "vlan 4094 guest-interface 0"',
                'action 1104 cli command "guest-ipaddress 192.168.2.2 netmask 255.255.255.0"',
                'action 1105 cli command "exit"',
                'action 1106 cli command "exit"',
                'action 1107 cli command "app-default-gateway 192.168.2.1 guest-interface 0"',
                'action 1108 cli command "name-server0 128.107.212.175"',
                'action 1109 cli command "name-server1 64.102.6.247"',
                'action 1110 cli command "exit"',
                'action 1111 cli command "interface AppGigabitEthernet1/0/1"',
                'action 1112 cli command "switchport mode trunk"',
                'action 1113 cli command "exit"',
                'action 1114 cli command "end"'
                ]
results = configure(eem_gs_commands)

# Enable Guest Shell via EEM applet
print ("*** Configure cli2yang examples on device... ***")
eem_enable_gs_commands = ['no event manager applet enable_gs',
                'event manager applet enable_gs',
                'event none maxrun 600',
                'action 0001 cli command "enable"',
                'action 0002 wait 250',
                'action 0003 cli command "guestshell enable"',
                'action 0004 wait 60',
                'action 0005 cli command "guestshell run /usr/bin/bash /bootflash/guest-share/cli2yang.sh"'
                ]
results = configure(eem_enable_gs_commands)
print ("*** Successfully configured cli2yang on device! ***")

# Run the cli2yang Guest Shell commands twice for good luck:
# Copy the files for the cli2yang
cli_command = "copy tftp://10.1.1.3/cli2yang.tgz flash:guest-share/"
cli.executep(cli_command)
cli_command = "copy tftp://10.1.1.3/cli2yang.sh  flash:guest-share/"
cli.executep(cli_command)
cli_command = "event manager run eem_gs_config"
cli.executep(cli_command)
cli_command = "event manager run enable_gs"
cli.executep(cli_command)
```
