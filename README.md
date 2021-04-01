# wireguard
Manage/Install WireGuard on applicable ASUS routers

see https://www.snbforums.com/threads/experimental-wireguard-for-rt-ac86u-gt-ac2900-rt-ax88u-rt-ax86u.46164/

## Installation ##

###NOTE: Entware is assumed to be installed###

Enable SSH on router, then use your preferred SSH Client e.g. Xshell6,MobaXterm, PuTTY etc.

(TIP: Triple-click the install command below) to copy'n'paste into your router's SSH session:
    
    curl --retry 3 "https://raw.githubusercontent.com/MartineauUK/wireguard/dev/wg_manager.sh" --create-dirs -o "/jffs/addons/wireguard/wg_manager.sh" && chmod 755 "/jffs/addons/wireguard/wg_manager.sh" && /jffs/addons/wireguard/wg_manager.sh install
    
Example successful install.....

	Downloading scripts
	wg_client downloaded successfully 
	wg_server downloaded successfully 
	UDP_Updater.sh downloaded successfully 

    Package column (2.36.1-2) installed in root is up to date.
    Package coreutils-mkfifo (8.32-6) installed in root is up to date.
	Downloading Wireguard Kernel module for RT-AC86U (v386.2)

	Downloading WireGuard Kernel module 'wireguard-kernel_1.0.20210219-k27_aarch64-3.10.ipk' for RT-AC86U (v386.2)...

      ##################################################################################################################################################################################### 100.0%##################################################################################################################################################################################### 100.0%

	Downloading WireGuard User space Tool 'wireguard-tools_1.0.20210223-1_aarch64-3.10.ipk' for RT-AC86U (v386.2)

    ##################################################################################################################################################################################### 100.0%##################################################################################################################################################################################### 100.0%

	Loading WireGuard Kernel module and Userspace Tool for RT-AC86U (v386.2)
    Package wireguard-kernel (1.0.20210219-k27) installed in root is up to date.
    Package wireguard-tools (1.0.20210223-1) installed in root is up to date.
	wireguard: WireGuard 1.0.20210219 loaded. See www.wireguard.com for information.
	wireguard: Copyright (C) 2015-2019 Jason A. Donenfeld <Jason@zx2c4.com>. All Rights Reserved.


	Creating WireGuard configuration file '/jffs/addons/wireguard/WireguardVPN.conf'

	No Peer entries to auto-migrate from '/jffs/addons/wireguard/WireguardVPN.conf', but you will need to manually import the 'device' Peer '*.conf' files:



	[✔] WireGuard Peer SQL Database initialised OK

	Creating WireGuard 'Server' Peer (wg21)'
	Creating WireGuard Private/Public key-pairs for RT-AC86U (v386.2)
	Initialising WireGuard VPN 'server' Peer

	Requesting WireGuard VPN Peer start (wg21)

	wireguard-server1: Initialising Wireguard VPN 'Server' Peer (wg21) on 10.88.8.1:51820 (# RT-AC86U Server #1)
	wireguard-server1: Initialisation complete.

	[✔] Statistics gathering is ENABLED

	nat-start updated to protect WireGuard firewall rules
	Restarting DNSmasq to add 'wg*' interfaces

    Done.

	Creating 'wg_manager' alias for 'wg_manager.sh'

	Event scripts

	Adding Peer Auto-start @BOOT
	Installing QR rendering module
    Package qrencode (4.1.1-1) installed in root is up to date.
	Do you want to create a 'device' Peer for 'server' Peer (wg21) ?
	Press y to create 'device' Peer or press [Enter] to skip
    y
    Enter the device name e.g. iPhone
    iPhone

	Creating Wireguard Private/Public key pair for device 'iPhone'
	Device 'iPhone' Public key=zHI1BIxOsF9nKwfsPGoGPFa9ffMt83jRWz+ipPexWjM=

	Using Public key for 'server' Peer 'wg21'


	WireGuard config for Peer device 'iPhone' created (Allowed IP's 0.0.0.0/0 # ALL Traffic)

	Press y to Display QR Code for Scanning into WireGuard App on device 'iPhone' or press [Enter] to SKIP.
    y
	Press y to ADD device 'iPhone' to 'server' Peer (wg21) or press [Enter] to SKIP.
    y

	Adding device Peer 'iPhone' 10.50.1.2/32 to RT-AC86U 'server' (wg21) and WireGuard config


	WireGuard 'server' Peer needs to be restarted to listen for 'client' Peer iPhone "Device"
	Press y to restart 'server' Peer (wg21) or press [Enter] to SKIP.
    y

	Requesting WireGuard VPN Peer restart (wg21)

	Restarting Wireguard 'server' Peer (wg21)
	wireguard-server1: Wireguard VPN '' Peer (wg21) on 10.88.8.1:51820 (# RT-AC86U Server #1) Terminated

	wireguard-server1: Initialising Wireguard VPN 'Server' Peer (wg21) on 10.88.8.1:51820 (# RT-AC86U Server #1)
	wireguard-server1: Initialisation complete.


	interface: wg21 	Port:51820	10.50.1.1/24 		VPN Tunnel Network	# RT-AC86U Server #1
		peer: zHI1BIxOsF9nKwfsPGoGPFa9ffMt83jRWz+ipPexWjM= 	10.50.1.2/32		# iPhone "Device"	

	v4.08 WireGuard Session Manager install COMPLETED.


 	WireGuard ACTIVE Peer Status: Clients 0, Servers 1


In lieu of the NVRAM variables that can retain OpenVPN Client/Server configurations across reboots, this script uses 

'/jffs/configs/WireguardVPN_map' for the WireGuard directives.

As this is a beta, the layout of the file includes placeholders, but currently, the first column is significant and is used as a primary lookup key and only the 'Auto' and 'Annotation Comment' fields are extracted/used to determine the actions taken by the script.

e.g.

    wg13    P      xxx.xxx.xxx.xxx/32    103.231.88.18:51820    193.138.218.74    # Mullvad Oz, Melbourne

is used to auto-start WireGuard VPN 'client' Peer 3 ('wg13')' in Policy mode, where the associated Policy rules are defined as

    rp13    <Dummy VPN 3>172.16.1.3>>VPN<Plex>172.16.1.123>1.1.1.1>VPN<Router>172.16.1.1>>WAN<All LAN>172.16.1.0/24>>VPN

which happens to be in the same format as the Policy rules created by the GUI for OpenVPN clients i.e.

Use the GUI to generate the rules using a spare VPN Client and simply copy'n'paste the resulting NVRAM variable

    vpn_client?_clientlist etc.
    
The contents of the WireGuard configuration file will be used when 'wg13.conf' is activated - assuming that you have used say the appropriate WireGuard Web configurator such as Mullvads' to create the Local IP address and Public/Private key-pair for the remote Peer.
 e.g
 
    S50wireguard start client 3
    
 The script supports several commands:
    
    S50wireguard   {start|stop|restart|check|install} [ [client [policy] |server]} [wg_instance] ]
    S50wireguard   start 0
                   Initialises remote peer 'client' 'wg0' solely to remain backwards compatibilty with original
    S50wireguard   start client 0
                   Initialises remote peer 'client' 'wg0'
    S50wireguard   start 1
                   Initialises local peer 'server' 'wg1' solely to remain backwards compatibilty with original
    S50wireguard   start server 1
                   Initialises local peer 'server' 'wg21' uses interface naming convention as per OpenVPN e.g. tun21
    S50wireguard   start client 1
                   Initialises remote peer 'client' 'wg11' uses interface naming convention as per OpenVPN e.g. tun11
    S50wireguard   start client 1 policy
                   Initialises remote peer 'client' 'wg11' in 'policy' Selective Routing mode
    S50wireguard   stop client 3
                   Terminates remote peer 'client' 'wg13'
    S50wireguard   stop 1
                   Terminates local peer 'server' 'wg21'
    S50wireguard   stop
                   Terminates ALL ACTIVE peers (wg1* and wg2*)
    S50wireguard   start
                   Initialises ALL peers (wg1* and wg2*) defined in the configuration file where Auto=Y or Auto=P
                 
and if the install is successful, there should now be simple aliases

e.g.

    wgstart
    wgstop
    wgr
    wgd
    
where the top two aliases allow quickly Starting/Stopping all of the Defined/Active WireGuard Peers, and the bottom two generate a report of active Peers (either with or without DEBUG iptables/RPDB rules)

An example of the enhanced WireGuard Peer Status report showing the names of the Peers rather than just their cryptic Public Keys

    wgr

    (S50wireguard): 15024 v1.01b4 WireGuard VPN Peer Status check.....

	interface: wg21 	(# Martineau Host Peer 1)
		 public key: j+aNKC0yA7+hFyH7cA9gISJ9+Ms05G3q4kYG/JkBwAU=
		 private key: (hidden)
		 listening port: 1151
		
		peer: wML+L6hN7D4wx+E1SA0K4/5x1cMjlpYzeTOPYww2WSM= 	(# Samsung Galaxy S8)
		 allowed ips: 10.50.1.88/32
		
		peer: LK5/fu1iX1puR7+I/njj6W88Cr6/tDZhuaKp3XKM/R4= 	(# Device iPhone12)
		 allowed ips: 10.50.1.90/32
 
NOTE: Currently, if you start say three WireGuard remote Peers concurrently and none of which are designated as Policy Peers, ALL traffic will be forced via the most recent connection, so if you then terminate that Peer, then the least oldest of the previous Peers will then have ALL traffic directed through it.
Very crude fall-over configuration but may be useful. 

For hosting a 'server' Peer (wg21) you can use the following command to generate a Private/Public key-pair and auto add it to the 'wg21.conf' and to the WireGuard config '/jffs/configs/WireGuardVPN_map'

    S50wireguard genkeys GoldstrikeriPhone3GSSupreme24K

	Creating Wireguard Private/Public key pair for device 'GoldstrikeriPhone3GSSupreme24K'

	Device 'GoldstrikeriPhone3GSSupreme24K' Public key=uAMVeM6DNsj9rEsz9rjDJ7WZEiJjEp98CDfDhSFL0W0=

	Press y to ADD device 'GoldstrikeriPhone3GSSupreme24K' to 'server' Peer (wg21) or press [Enter] to SKIP.
    y
	Adding device Peer 'GoldstrikeriPhone3GSSupreme24K' to RT-AC86U 'server' (wg21) and WireGuard config
and the resulting entry in the WireGuard 'server' Peer config 'wg21.conf' - where 10.50.1.125 is derived from the DHCP pool for the 'server' Peer

e.g. WireGuard configuration 'WireguardVPN_map' contains

    wg21    Y      10.50.1.1/24                                                 # Martineau Host Peer 1

and the next avaiable IP with DHCP pool prefix '10.60.1' .125 is chosen as .124 is aleady assigned when the Peer is appended to 'wg21.conf'

    #GoldstrikeriPhone3GSSupreme24K
    [Peer]
    PublicKey = uAMVeM6DNsj9rEsz9rjDJ7WZEiJjEp98CDfDhSFL0W0=
    AllowedIPs = 10.50.1.125/32
  
and the cosmetic Annotation identification for the device '# Device GoldstrikeriPhone3GSSupreme24K' is appended to the WireGuard configuration 'WireguardVPN_map'  

    # Optionally define the 'server' Peer 'clients' so they can be identified by name in the enhanced WireGuard Peer status report
    # Public Key                                      DHCP IP             Annotation Comment
    <snip>
    xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=      10.50.1.124         # A Cell phone
    snip>
    
    uAMVeM6DNsj9rEsz9rjDJ7WZEiJjEp98CDfDhSFL0W0=      10.50.1.125         # Device GoldstrikeriPhone3GSSupreme24K

    




    
    
     
