CCNP 
Network Assurance: 


PING!
Different ping sizes 
Send 1500-byte packet with the DF (Do Not Fragment) bit set to make sure there are no MTU issues (especially helpful when used with tunneling) or test different QOS policies that restrict certain packet sizes. 


TOS (type of service) 184 = Expedited Forwarding PHB

Data Pattern
helpful when troubleshooting framing errors, line coding, or clock signaling issues on serial interfaces. 

Traceroute
By Default, traceroute tries up to 30 times/hops before completing. 
!H = host unreachable. 

timeout
default is 3 seconds

port number
destination port number of probes, default is 33434


Debugging


OSPF example: 
commone issues
* mtu
* incorrect interface type
* improperly configured network mask

MTU
show ip ospf neighbor
cycles between INIT/EXCHANGE/INIT

debug ip ospf adj
*Nbr 4.4.4.4 has smaller interface MTU
you can see the actual mtu in the preceding 'OSPF-1 ADJ" message


network type mismatch
debug ip ospf hello
'mismatched hello parameters'

see mismatched hello interval, which you can view with 
show ip ospf interface ethernet0/1
which will reveal that the interface is correctly configured as 'point-to-multipoint'

fix by changing back to default type of broadcast
ip ospf network broadcast
* do show ip ospf neighbor



Improper configuration of ip address and subnet mask on an ospf interface


state is stuck in INIT

use both debug ip ospf hello and debug ip ospf adj

"no more immediate hello for nbr 4.4.4.4"

on the neighbor if you apply the same commands, you will see mismatched hello parameters

"Mask R 255.255.255.0 C 255.255.255.248"


Conditional Debugging

debug ip packet
* <1-199> - standard access list
* <1300-2699> Access list with expanded range
* detail - more debugging detail

conditional debugging ip packet for 192.168.14.0/24
access-list 100 permit ip any 192.168.14.0 0.0.0.255
access-list 100 permit ip 192.168.14.0 0.0.0.255 any
debug ip packet 100

you can also debug on a specific interface:
debug interface loopback0

undebug all
* you need to do this below because the conditions for debugging will still persist. 
undebug interface loopback0

SNMP: 

some measures involved in setting up Device
* Define the SNMP host or the NMS to send traps to
* Create an access list to restric access via SNMP
* Define the read-only community string
* Define the read/write community string
* Define the SNMP locaiotn
* Define the SNMP contact

lets say a network monitoring system has an IP of 192.168.14.100 
access-list 99 permit 192.168.14.100 0.0.0.0
snmp-server community READONLY ro 99
snmp-server community READONLY rw 99

to send traps, first enable them
snmp-server enable traps

to be selective, limit with
snmp-server enable traps ?
Example
snmp-server enable traps eigrp
snmp-server host 192.168.14.100 traps READONLY


syslog:

enable logging buffer
* Enable logging to the buffer
* set the severity of syslog messages to send to the buffer
* set the logging buffer to a larger size

logging buffer ?
logging buffer 10000
logging buffer debugging

 syslog default port of 514

logging host 192.168.14.100
logging trap 7


Netflow and Flexible Netflow

2 components
netflow data capture and netflow data export

A flow is a unidirectional traffic stream that contains a combo of 
source ip address
destination ip address
source port number
destination port number
layer 3 protocol type
type of service (Tos)
input logical interface

Example configuration
ip flow-export version 9
ip flow-export destination 192.168.14.100 9999
interface ethernet0/1
ip flow ingress
ip flow egress
end


Verify
show ip flow interface
	view configured interfaces
show ip flow export
	shows the destination for netflow data as well as statistics, including errors
show ip flow cache
	view the traffic flows that netflow is capturing

ip flow-top-talkers
top 10
sort-by bytes
end
show ip flow top-talkers

flexible netflow allows for the use of multiple flow monitors on the same traffic at the same time, meaning multiple different flow policies can be applied to the same traffic as it flows through a device

match - select key fields
collect - select non-key fields

steps to create a custom flow record
1. define the flow record name
2. set a useful description of the flow record
3. set match criteria for key fields
4. define non-key fields to be collected


flow record CUSTOM1
description
match ipv4 destination address
collect counter bytes
collect counter packets
exit


do show flow record CUSTOM1

then, create a custom flow exporter
1. define the flow exporter name 
2. set a useful description of the flow exporter
3. specify the destination of the flow exporter to be used
4. specify netflow version to export
5. specify the UDP port


flow exporter CUSTOM1
description EXPORT-TO-NETFLOW-COLLECTOR
destination 192.168.14.100
export-protocol netflow-v9
transport UDP 9999
exit
show run flow exporter
show flow exporter CUSTOM1


Now, you need to create a custom flow monitor
1. define the flow monitor name
2. set a useful description of the flow monitor
3. specify the flow record to be used
4. specify a cache timeout of 60 for active connections
5. assign the exporter to the monitor


flow monitor CUSTOM1
description uses custom flow record CUSTOM1 for ipv5
record CUSTOM1
cache active timeout 60
exit
show run flow monitor

show flow monitor CUSTOM1


now, you need to map the flow exporter to the flow monitor. 

flow monitor CUSTOM1
exporter CUSTOM1
show run flow monitor
show flow monitor CUSTOM1

final step, apply the flow monitor to the interfaces

interface eth0/1
ip flow monitor CUSTOM1 input

show flow monitor CUSTOM1 cache


SPAN
local switched port analyzer
	capture local traffic on a switch and send a copy of the network traffic to a local port attached to some sort of traffic analyzer

remote switched port analyzer (RSPAN)
	capture traffic on a remote switch and send a copy of the network traffic to the local switch through layer 2 toward a local port attached to some soft of traffic analyzer

Encapsulated Remote Switched Port Analyzer (ERSPAN)
	capture network traffic on a remote device, send the traffic to the local system through layer 3 toward a local port 

local span
most basic option
destionat can be
	one or more specific switch ports
	port channel
	vlan (not svi interface)

	*destination cannot be reused between two different span sessions

specify source ports
	monitor session (sessionid) source
	span normally does not include STP, 802.1q tags, bpdus, cdp, vtp, DTP, PAgP, or LACP 
	to include, use 
	encapsulation replicate
		monitor session 10 destinate interface etherent1/0/1 encapsulation replicate

	if you need connectivity to a traffic analyzer (destination of the span session) the port needs to also be able to send/receive traffic 
	(ex, you may need to RDP to the traffic analyzer)

	monitor session 10 destination interface ethernet1/0/1 ingress 

	verify 
		show monitor session span local

	monitor session 1 destination interface eth1/0/1 ingress untagged vlan 123

Remote SPAN
	RSPAN
	mirror traffic is placed on a special vlan,designated for span traffic only

		mac addresses are not learned on ports associated w/ the rspan vlan
		traffic is flooded out all ports associated to the rspan vlan

	configuration
		vlan 99
		name rspan_vlan
		remote-span
		monitor session 1 source interface gi1/0/3
		monitor session 1 destination remote vlan 99
	verify 
		show monitor session 1

	destination switch configuration
		vlan 99
		name rspan_vlan
		remote-span
		monitor session 1 source remote vlan 99
		monitor session 1 destination interface gi1/0/9
	verify
		show monitor session remote

ERSPAN
	COME BACK TO THIS!
	route traffic for analysis 
	configuration 
		monitor session 1 type erspan-source
	
	monitor session 1 type erspan-source
	d

	verify 
		show monitor session erspan-source session

IP SLA
	configure IP SLA ICMP ECHO
	ip sla 1
		icmp-echo 192.168.14.100 source-interface Loopback0
		frequency 300
		end

	schedule and activate
		ip sla schedule 1 life forever start-time now
		show ip slap configuration
	
	HTTP GET SLA configuration
		ip sla 2
		http get http://192.168.14.100
		frequency 90
		end
	ip sla schedule 2 start-time now life forever
	
	monitor using the CISCO-RTTMON-MIB file with snmp

Cisco DNA center assurance


	
		
	