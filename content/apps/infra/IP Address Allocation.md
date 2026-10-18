This page contains the "source of truth" for IPs in the infrastructure, including apps and hardware.

# Networking 
Local networking settings for general infrastructure follows as below. Note the range does not cover the entire subnet, the rest of the range is used for DHCP and other existing machines on the network.

> It's not generally worth the effort to seperate the subnets here

Gateway|192.168.1.1
Subnet|192.168.1.0/24
Range|192.168.1.160-192.168.1.254
DNS|192.168.1.5

# Assignment Guide
Roughly follow this guide for assigning new IPs in the network.

.16x|Hypervisors
.17x|K3S Nodes
.18x|Management servers
.20x|App servers
.24x|Infrastructure 

# Allocations
Current allocations for servers

fqdn|ip|purpose/type
node0.pve.jordanhome.dev|192.168.1.160|thinkcentre
node1.pve.jordanhome.dev|192.168.1.161|thinkcentre
node2.pve.jordanhome.dev|192.168.1.162|nuc
k3s-node0|192.168.1.170|k3s
k3s-node1|192.168.1.171|k3s
k3s-node2|192.168.1.172|k3s
dns|192.168.1.5|dns
nas.jordanhome.dev|192.168.1.111|nas
App VIP|192.168.1.205|k3s


