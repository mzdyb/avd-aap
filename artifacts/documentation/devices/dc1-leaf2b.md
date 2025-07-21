# dc1-leaf2b

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Switchport Default](#switchport-default)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | OOB_MANAGEMENT | oob | MGMT | 172.20.1.6/16 | 172.20.0.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | OOB_MANAGEMENT | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description OOB_MANAGEMENT
   no shutdown
   vrf MGMT
   ip address 172.20.1.6/16
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 8.8.8.8 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **Europe/Warsaw**.

#### Clock Device Configuration

```eos
!
clock timezone Europe/Warsaw
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management0 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 0.pool.ntp.org | MGMT | True | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management0
ntp server vrf MGMT 0.pool.ntp.org prefer
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| ansible | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username ansible privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| LEAF2 | Vlan4094 | 10.255.1.68 | Port-Channel55 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id LEAF2
   local-interface Vlan4094
   peer-address 10.255.1.68
   peer-link Port-Channel55
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 12 | VLAN12 | - |
| 22 | VLAN22 | - |
| 3009 | MLAG_L3_VRF_VRF10 | MLAG |
| 3019 | MLAG_L3_VRF_VRF20 | MLAG |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 12
   name VLAN12
!
vlan 22
   name VLAN22
!
vlan 3009
   name MLAG_L3_VRF_VRF10
   trunk group MLAG
!
vlan 3019
   name MLAG_L3_VRF_VRF20
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## Interfaces

### Switchport Default

#### Switchport Defaults Summary

- Default Switchport Mode: routed

#### Switchport Default Device Configuration

```eos
!
switchport default mode routed
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | SERVER_server2_eth2 | *trunk | *12,22 | *4092 | *- | 1 |
| Ethernet55 | MLAG_dc1-leaf2a_Ethernet55 | *trunk | *- | *- | *MLAG | 55 |
| Ethernet56 | MLAG_dc1-leaf2a_Ethernet56 | *trunk | *- | *- | *MLAG | 55 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49 | P2P_dc1-spine1_Ethernet4 | - | 10.255.255.13/31 | default | 1500 | False | - | - |
| Ethernet50 | P2P_dc1-spine2_Ethernet4 | - | 10.255.255.15/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description SERVER_server2_eth2
   no shutdown
   channel-group 1 mode active
!
interface Ethernet49
   description P2P_dc1-spine1_Ethernet4
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.255.13/31
!
interface Ethernet50
   description P2P_dc1-spine2_Ethernet4
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.255.15/31
!
interface Ethernet55
   description MLAG_dc1-leaf2a_Ethernet55
   no shutdown
   channel-group 55 mode active
!
interface Ethernet56
   description MLAG_dc1-leaf2a_Ethernet56
   no shutdown
   channel-group 55 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | SERVER_server2 | trunk | 12,22 | 4092 | - | - | - | 1 | - |
| Port-Channel55 | MLAG_dc1-leaf2a_Port-Channel55 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description SERVER_server2
   no shutdown
   switchport trunk native vlan 4092
   switchport trunk allowed vlan 12,22
   switchport mode trunk
   switchport
   mlag 1
   spanning-tree portfast
!
interface Port-Channel55
   description MLAG_dc1-leaf2a_Port-Channel55
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.255.0.6/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.255.1.5/32 |
| Loopback10 | DIAG_VRF_VRF10 | VRF10 | 10.255.10.6/32 |
| Loopback20 | DIAG_VRF_VRF20 | VRF20 | 10.255.20.6/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback10 | DIAG_VRF_VRF10 | VRF10 | - |
| Loopback20 | DIAG_VRF_VRF20 | VRF20 | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.255.0.6/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.255.1.5/32
!
interface Loopback10
   description DIAG_VRF_VRF10
   no shutdown
   vrf VRF10
   ip address 10.255.10.6/32
!
interface Loopback20
   description DIAG_VRF_VRF20
   no shutdown
   vrf VRF20
   ip address 10.255.20.6/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan12 | VLAN12 | VRF10 | - | False |
| Vlan22 | VLAN22 | VRF20 | - | False |
| Vlan3009 | MLAG_L3_VRF_VRF10 | VRF10 | 1500 | False |
| Vlan3019 | MLAG_L3_VRF_VRF20 | VRF20 | 1500 | False |
| Vlan4093 | MLAG_L3 | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan12 |  VRF10  |  -  |  10.10.12.1/24  |  -  |  -  |  -  |
| Vlan22 |  VRF20  |  -  |  10.10.22.1/24  |  -  |  -  |  -  |
| Vlan3009 |  VRF10  |  10.255.1.101/31  |  -  |  -  |  -  |  -  |
| Vlan3019 |  VRF20  |  10.255.1.101/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.1.101/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.1.69/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan12
   description VLAN12
   no shutdown
   vrf VRF10
   ip address virtual 10.10.12.1/24
!
interface Vlan22
   description VLAN22
   no shutdown
   vrf VRF20
   ip address virtual 10.10.22.1/24
!
interface Vlan3009
   description MLAG_L3_VRF_VRF10
   no shutdown
   mtu 1500
   vrf VRF10
   ip address 10.255.1.101/31
!
interface Vlan3019
   description MLAG_L3_VRF_VRF20
   no shutdown
   mtu 1500
   vrf VRF20
   ip address 10.255.1.101/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 1500
   ip address 10.255.1.101/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 10.255.1.69/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 12 | 10012 | - | - |
| 22 | 10022 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| VRF10 | 30010 | - |
| VRF20 | 30020 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc1-leaf2b_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 12 vni 10012
   vxlan vlan 22 vni 10022
   vxlan vrf VRF10 vni 30010
   vxlan vrf VRF20 vni 30020
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:00:99

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:99
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |
| VRF10 | True |
| VRF20 | True |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf VRF10
ip routing vrf VRF20
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |
| VRF10 | false |
| VRF20 | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 172.20.0.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 172.20.0.1
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102 | 10.255.0.6 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65102 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.255.0.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.0.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.1.100 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.255.12 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.255.14 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.100 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF10 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.255.1.100 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | VRF20 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True |  - | - | default | - |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 12 | 10.255.0.6:10012 | 10012:10012 | - | - | learned |
| 22 | 10.255.0.6:10022 | 10022:10022 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| VRF10 | 10.255.0.6:10 | connected | - |
| VRF20 | 10.255.0.6:20 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 10.255.0.6
   no bgp default ipv4-unicast
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65102
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc1-leaf2a
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.255.0.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.1 remote-as 65100
   neighbor 10.255.0.1 description dc1-spine1_Loopback0
   neighbor 10.255.0.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.0.2 remote-as 65100
   neighbor 10.255.0.2 description dc1-spine2_Loopback0
   neighbor 10.255.1.100 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.1.100 description dc1-leaf2a_Vlan4093
   neighbor 10.255.255.12 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.12 remote-as 65100
   neighbor 10.255.255.12 description dc1-spine1_Ethernet4
   neighbor 10.255.255.14 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.255.14 remote-as 65100
   neighbor 10.255.255.14 description dc1-spine2_Ethernet4
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 12
      rd 10.255.0.6:10012
      route-target both 10012:10012
      redistribute learned
   !
   vlan 22
      rd 10.255.0.6:10022
      route-target both 10022:10022
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf VRF10
      rd 10.255.0.6:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.255.0.6
      neighbor 10.255.1.100 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.100 description dc1-leaf2a_Vlan3009
      redistribute connected route-map RM-CONN-2-BGP-VRFS
   !
   vrf VRF20
      rd 10.255.0.6:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 10.255.0.6
      neighbor 10.255.1.100 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.255.1.100 description dc1-leaf2a_Vlan3019
      redistribute connected route-map RM-CONN-2-BGP-VRFS
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.0.0/27 eq 32 |
| 20 | permit 10.255.1.0/27 eq 32 |

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.1.100/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.255.0.0/27 eq 32
   seq 20 permit 10.255.1.0/27 eq 32
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.255.1.100/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| VRF10 | enabled |
| VRF20 | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance VRF10
!
vrf instance VRF20
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| VRF10 | 10.255.10.6 | - |
| VRF20 | 10.255.20.6 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf VRF10 address 10.255.10.6
ip address virtual source-nat vrf VRF20 address 10.255.20.6
```
