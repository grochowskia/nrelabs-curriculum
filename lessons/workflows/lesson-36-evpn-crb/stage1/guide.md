## Examine an EVPN-VXLAN fabric

---

### Part 1 - Introduction

#### EVPN VXLAN Background

Ethernet VPNs (EVPNs) enable you to connect groups of dispersed customer sites using Layer 2 virtual bridges, and Virtual Extensible LANs (VXLANs) allow you to stretch Layer 2 connectivity over an intervening Layer 3 network, while providing network segmentation like a VLAN, but without the scaling limitation of traditional VLANs. EVPN with VXLAN encapsulation handles Layer 2 connectivity at scale and replaces limiting protocols like Spanning Tree Protocol (STP), freeing up your Layer 3 network to use more robust routing protocols. 

#### Lesson Information

In this guide, we will gain some basic familiarity with an EVPN controlled VXLAN fabric, explore configuration options, and create an IRB to bridge two networks together.
Scenario:

Firstly, we will run a script to return the results of 'show bgp summary', so that we can make sure that all BGP peering relationships, both for underlay and overlay, are in an established state.

Please refer to the lesson diagram to examine the lab layout.

#### Starting out - Examine the existing fabric

Click the below link to display a show bgp summary on the spine switch. 
```
show bgp summary

```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('spine', this)">Run this snippet</button>



As you can see, the bgp sessions between the spine and the leaf switches are established.  192.168.100.x addresses are loopback addresses that enabled the EVPN VXLAN overlay, and the 172.16.1.x addresses are fabric link addresses, creating underlay peering associations. 

#### Hosts and Networks

Now let's turn our attention to the linux hosts in the diagram.  As you can see, host1 is in the subnet 10.1.1.0/24, and host 2, 10.1.2.0/24.  Each are in a different virtual network.  If we attempt to ping between the hosts, the ping will therefor fail.

```
ping host2 -c 5

```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('host1', this)">Run this snippet</button>

#### Interface configuration

Click the following link to examine the port configuration for each hosts respective switch port.

```
./viewportconfig.sh
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('linux1', this)">Run this snippet</button>

You can see from the returned config that host1 is in VLAN 10.  Further, VLAN 10 is mapped to VXLAN Network Identifier 5010.  Host2 is in VLAN 20, which is in turn mapped to VNI 5020.   The hosts are isolated to their own networks, and as we saw, can not see each other.


#### Up Next

This section shows some basic functionality of the fabric, and allows you to explore the relevant config.  Feel free to examine the config in detail on each of the vQFXs in the environment.  In the next section, we will configuration for Integrated Routing and Bridging (IRB) that will bridge VNI 5010, and VNI 5020, thereby allowing Host1 and Host2 to communicate.

