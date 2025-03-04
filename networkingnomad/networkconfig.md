Network Interfaces
A network interface is how the kernel links up the software side of networking to the hardware side. We've already seen an example of this:


pete@icebox:~$ ifconfig -a

eth0      Link encap:Ethernet  HWaddr 1d:3a:32:24:4d:ce  

          inet addr:192.168.1.129  Bcast:192.168.1.255  Mask:255.255.255.0

          inet6 addr: fd60::21c:29ff:fe63:5cdc/64 Scope:Link

The ifconfig command

The ifconfig tool allows us to configure our network interfaces, if we don't have any network interfaces set up, the kernel's device drivers and the network won't know how to talk to each other. Ifconfig runs on bootup and configures our interfaces through config files, but we can also manually modify them. The output of ifconfig shows the interface name on the left side and the right side shows detailed information. You'll most commonly see interfaces named eth0 (first Ethernet card in the machine), wlan0 (wireless interface), lo (loopback interface). The loopback interface is used to represent your computer, it just loops you back to yourself. This is good for debugging or connecting to servers running locally.

The status of interfaces, can be up or down, as you can guess if you wanted to "turn off" an interface you can set it to go down. The fields you'll probably look at the most in the ifconfig output is the HWaddr (MAC address of the interface), inet address (IPv4 address) and inet6 (IPv6 address). Of course you can see that the subnet mask and broadcast address are there as well. You can also view interface information at /etc/network/interfaces.

To create an interface and bring it up

$ ifconfig eth0 192.168.2.1 netmask 255.255.255.0 up
This assigns an IP address and netmask to the eth0 interface and also turns it up.

To bring up or down an interface


$ ifup eth0

$ ifdown eth0

The ip command

The ip command also allows us to manipulate the networking stack of a system. Depending on the distribution you are using it may be the preferred method of manipulating your network settings.

Here are some examples of its use:

To show interface information for all interfaces


$ ip link show

To show the statistics of an interface


$ ip -s link show eth0

To show ip addresses allocated to interfaces


$ ip address show

To bring interfaces up and down


$ ip link set eth0 up

$ ip link set eth0 down

To add an IP address to an interface


$ ip address add 192.168.1.1/24 dev eth0

route
We've already discussed viewing our routing tables with the route command, if you wanted to add or remove routes you can do so manually.

Add a new route


$ sudo route add -net 192.168.2.1/23 gw 10.11.12.3

Delete a route


$ sudo route del -net 192.168.2.1/23 

You can also perform these changes with the ip command:

To add a route


$ ip route add 192.168.2.1/23 via 10.11.12.3

To delete a route


$ ip route delete 192.168.2.1/23 via 10.11.12.3

or

$ ip route delete 192.168.2.1/23

dhclient
We've discussed DHCP before and most often you will never need to statically set your IP addresses, subnet masks, etc. Instead you'll be using DHCP! The dhclient starts up on boot and gets a list of network interfaces from the dhclient.conf file. For each interface listed it tries to configure the interface using the DHCP protocol.

In the dhclient.leases file, dhclient keeps track of a list of leases across system reboots, after reading dhclient.conf, the dhclient.leases file is read to let it know what leases it's already assigned.

To obtain a fresh IP

$ sudo dhclient

Network Manager
Of course if you wanted to have your system's networking up and running automatically there is something already in place for that. Most distributions utilize the NetworkManager daemon to configure their networks automatically.

You'll notice NetworkManager in the form of an applet somewhere on your desktop taskbar if you are using a GUI. As you can see it manages your network's hardware and connection information. For instance on startup, NetworkManager will gather network hardware information, search for connections to wireless, wired, etc. and then activates it.

There are also command-line tools to interact with NetworkManager:

nm-tool

nm-tools reports NetworkManager's state and it's devices


pete@icebox:/$ nm-tool

NetworkManager Tool


State: connected (global)



- Device: eth0  [Wired connection 1] -------------------------------------------

  Type:              Wired

  Driver:            pcnet32

  State:             connected

  Default:           yes

  HW Address:        12:3D:45:56:7D:CC



  Capabilities:

    Carrier Detect:  yes



  Wired Properties

    Carrier:         on



  IPv4 Settings:

    Address:         192.168.22.1

    Prefix:          24 (255.255.255.0)

    Gateway:         192.168.22.2



    DNS:             192.168.22.2


nmcli

The nmcli command allows you to control and modify NetworkManager, see the manpage for more details.

 arp
Remember when we lookup a MAC address with ARP, it first checks the locally stored ARP cache on our system, you can actually view this cache:


pete@icebox:~$ arp

Address                  HWtype  HWaddress           Flags Mask            Iface

192.168.22.1            ether   00:12:24:fc:12:cc   C                     eth0

192.168.22.254          ether   00:12:45:f2:84:64   C                     eth0

The ARP cache is actually empty when a machine boots up, it gets populated as packets are being sent to other hosts. If we send a packet to a destination that isn't in the ARP cache, the following happens:

The source host creates the Ethernet frame with an ARP request packet
The source host broadcasts this frame to the entire network
If one of the hosts on the network knows the correct MAC address, it will send a reply packet and frame containing the MAC address
The source host adds the IP to MAC address mapping to the ARP cache and then proceeds with sending the packet
You can also view your arp cache via the ip command:


$ ip neighbour show
