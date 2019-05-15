# Ansible Role:cnos-bgp-evpn – BGP EVPN Configuration on Lenovo CNOS
---
<add role description below>

This role pertains to configuring BGP EVPN for Data Center Interconnect 
(DCI) on switches running CNOS.
The Lenovo DCI BGP EVPN Configuration starts with:
•             vLAG Configuration on Leaf Switches
•             Underlying Transport Network Configuration on Leaf Switches
•             Underlying Transport Network Configuration on Route Reflectors
•             MP-BGP EVPN Configuration on Leaf Switches
•             MP-BGP EVPN Configuration on Route Reflectors 

When deploying a distributed solution for a VXLAN network, Control Plane 
information is exchanged between VXLAN Tunnel End Points (VTEPs) by using
Multi-protocol Border Gateway Protocol (MP-BGP) Ethernet Virtual Private
Network (EVPN). MP-BGP EVPN offers a control plane through which to discover
protocol based VTEP peers, and also presents a feature set that allows for 
optimal forwarding of both west-east traffic and south-north traffic and 
south-north traffic in the VXLAN network

VTEPs running MP-BGP EVPN support Control Plane functions which allows them to
initiate MP-BGP EVPN routes advertisement from their local hosts, and receive
updates from their peers and install EVPN routes in their tables. They also
support data plane functions which allows them to encapsulate network traffic
using VXLAN and then send the encapsulated traffic over the underlaying I
network. When receiving VXLAN encapsulated packets from other VTEPs, they
decapsulate the packets, encapsulate them using native Ethernet, and then 
forward them to the host.

Ansible role for DCI MP-BGP EVPN Configuration for CNOS Switches is a template
driven playbook.

The role configures all the Lenovo CNOS swithes in the MP-BGP EVPN Configuration

MP-BGP EVPN role on Lenovo switches running CNOS support the following:
•             Configure MP-BGP EVPN on Leaf Switches 1 and 2
•             Configure MP-BGP EVPN on Route Reflector 1
•             Configure MP-BGP EVPN on Route Reflector 2
•             Check the MP-BGP EVPN Configuration
•             Check the information on Route Distinguishers (RDs) and Route 
              Targets (RTs) for each VNI

For more details, see [MP-BGP EVPN Configuration](
https://systemx.lenovofiles.com/help/topic/com.lenovo.switchmgt.ansible.doc/Ansible_User_Guide_2-8_rev1.pdf).


## Requirements
---
<add role requirements information below>

- Ansible version 2.8 or later ([Ansible installation documentation](http://docs.ansible.com/ansible/intro_installation.html))
- Lenovo switches running CNOS version 10.10.1.0 or later
- an SSH connection to the Lenovo switch (SSH must be enabled on the network device)


## Role Variables
---
<add role variables information below>
Available variables are listed below, along with description.

The following are mandatory inventory variables:

Variable | Description
--- | ---
`ansible_connection` | Has to be `network_cli`
`ansible_network_os` | Has to be `cnos`
`ansible_ssh_user` | Specifies the username used to log into the switch
`ansible_ssh_pass` | Specifies the password used to log into the switch
`deviceType` | Specifies the type of the CNOS switch
`rolename` | Specifies the role name of the CNOS switch. Values are LEAF1, LEAF2, ROUTE_REFLECTOR_1 and ROUTE_REFLECTOR_2

The values of the variables used need to be modified to fit the specific scenario in which you are deploying the solution. To change the values of the variables, you need to visit the *vars* directory of this role and edit the *main.yml* file located there. The values stored in this file will be used by Ansible when the template is executed.

The syntax of *main.yml* file for variables is the following:

```
<template variable>:<value>
```

You will need to replace the `<value>` field with the value that suits your topology. The `<template variable>` fields are taken from the template and it is recommended that you leave them unchanged.

Available variables are listed below, along with description:

Variable | Description
--- | ---
`bgp_as_number` | Configure AS Number for BGP
`router_id_leaf` | Configure router identifier of Leaf Switch
`route_reflector_1` | Configure IP Address of Router Reflector 1.
`interface_type` | Type of Interface viz loopback or ethernet.
`interface_type_value` | Index value of loopback or ethernet interface.
`route_reflector_2` | Configure IP Address of Router Reflector 2.
`router_id_spine_1` | Configure router identifier of Router Reflector 1 
`router_id_spine_2` | Configure router identifier of Router Reflector 2
`neighbor_leaf_1` | Configure IP Address of leaf 1 
`neighbor_leaf_2` | Configure IP Address of leaf 2
`neighbor_leaf_3` | Configure IP Address of leaf 3
`neighbor_leaf_4` | Configure IP Address of leaf 4


## Dependencies
---
<add dependencies information below>

- username.iptables - Configures the firewall and blocks all ports except those needed for web server and SSH access.
- username.common - Performs common server configuration.
- /etc/ansible/hosts - You must edit the */etc/ansible/hosts* file with the device information of the switches designated as spine switches. You may refer to *cnos-nsxgateway-hosts* for a sample configuration.

Ansible keeps track of all network elements that it manages through a hosts file. Before the execution of a playbook, the hosts file must be set up.

Open the */etc/ansible/hosts* file with root privileges. Most of the file is commented out by using **#**. You can also comment out the entries you will be adding by using **#**. You need to copy the content of the hosts file for the role into the */etc/ansible/hosts* file. The sample hosts file for the role is located in the main directory.
```
[cnos-bgp-evpn]
10.241.107.36   ansible_network_os=cnos ansible_ssh_user=<username> ansible_ssh_pass=<password> deviceType=G8272 rolename=LEAF1
10.241.107.37   ansible_network_os=cnos ansible_ssh_user=<username> ansible_ssh_pass=<password> deviceType=G8272 rolename=LEAF2
10.241.107.38   ansible_network_os=cnos ansible_ssh_user=<username> ansible_ssh_pass=<password> deviceType=G8272 rolename=ROUTE_REFLECTOR_1
10.241.107.39   ansible_network_os=cnos ansible_ssh_user=<username> ansible_ssh_pass=<password> deviceType=G8272 rolename=ROUTE_REFLECTOR_2
```
**Note:** You need to change the IP addresses, to fit your specific topology. You also need to change the `<username>` and `<password>` to the appropriate values used to log into the specific Lenovo network devices.


## Example Playbook
---
<add playbook samples below>

To execute an Ansible playbook, use the following command:

```
ansible-playbook cnos-bgp-evpn.yml -vvv
```

`-vvv` is an optional verbos command that helps identify what is happening during playbook execution. The playbook for each role of the NSX Gateway configuration solution is located in the main directory of the solution.
```
- hosts: cnos-bgp-evpn
  roles:
    - cnos-bgp-evpn
```

## License
---
<add license information below>
Copyright (C) 2019 Lenovo, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
