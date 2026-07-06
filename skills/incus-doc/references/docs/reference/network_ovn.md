[]{#network-ovn}

# OVN network

OVN is a software-defined networking system that supports virtual network abstraction.
You can use it to build your own private cloud.
See [`www.ovn.org`](https://www.ovn.org/) for more information.

The [`ovn`] network type allows to create logical networks using the OVN SDN.
This kind of network can be useful for labs and multi-tenant environments where the same logical subnets are used in multiple discrete networks.

An Incus OVN network can be connected to an existing managed [Bridge network](../network_bridge/#network-bridge) or [Physical network](../network_physical/#network-physical) to gain access to the wider network.
By default, all connections from the OVN logical networks are NATed to an IP allocated from the uplink network.

See [How to set up OVN with Incus](../../howto/network_ovn_setup/#network-ovn-setup) for basic instructions for setting up an OVN network.

**Note**

Static DHCP assignments depend on the client using its MAC address as the DHCP identifier.
This method prevents conflicting leases when copying an instance, and thus makes statically assigned leases work properly.

[]{#network-ovn-options}

## Configuration options

The following configuration key namespaces are currently supported for the [`ovn`] network type:

-   [`bridge`] (L2 interface configuration)

-   [`dns`] (DNS server and resolution configuration)

-   [`ipv4`] (L3 IPv4 configuration)

-   [`ipv6`] (L3 IPv6 configuration)

-   [`security`] (network ACL configuration)

-   [`user`] (free-form key/value for user metadata)

**Note**

Incus uses the [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) where network subnet information is required, for example, [`192.0.2.0/24`] or [`2001:db8::/32`]. This does not apply to cases where a single address is required, for example, local/remote addresses of tunnels, NAT addresses or specific addresses to apply to an instance.

The following configuration options are available for the [`ovn`] network type:

[[`bridge.external_interfaces`]][]

Comma-separated list of unconfigured network interfaces to include in the bridge

[[*!*](#network_ovn-common:bridge.external_interfaces)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`brid                            |
|                                   | ge.external_interfaces`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

[[`bridge.hwaddr`]][]

MAC address for the virtual bridge interface

[[*!*](#network_ovn-common:bridge.hwaddr)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bridge.hwaddr`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`bridge.mtu`]][]

Bridge MTU (default allows host to host Geneve tunnels)

[[*!*](#network_ovn-common:bridge.mtu)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bridge.mtu`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`1442`]              |
+-----------------------------------+-----------------------------------+

[[`dns.domain`]][]

Domain to advertise to DHCP clients and use for DNS resolution

[[*!*](#network_ovn-common:dns.domain)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.domain`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`incus`]              |
+-----------------------------------+-----------------------------------+

[[`dns.mode`]][]

DNS registration mode: none for no DNS record, managed for OVN managed records

[[*!*](#network_ovn-common:dns.mode)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.mode`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`managed`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`dns.nameservers`]][]

DNS server IPs to advertise to DHCP clients and via Router Advertisements. Both IPv4 and IPv6 addresses get pushed via DHCP, and the first IPv6 address is also advertised as RDNSS via RA.

[[*!*](#network_ovn-common:dns.nameservers)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.nameservers`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | Uplink DNS servers (IPv4 and IPv6 |
|                                   | address if no uplink is           |
|                                   | configured)                       |
+-----------------------------------+-----------------------------------+

[[`dns.search`]][]

Full comma-separated domain search list, defaulting to [`dns.domain`] value

[[*!*](#network_ovn-common:dns.search)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.search`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`dns.zone.forward`]][]

Comma-separated list of DNS zone names for forward DNS records

[[*!*](#network_ovn-common:dns.zone.forward)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.zone.forward`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`dns.zone.reverse.ipv4`]][]

DNS zone name for IPv4 reverse DNS records

[[*!*](#network_ovn-common:dns.zone.reverse.ipv4)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `dns.zone.reverse.ipv4`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`dns.zone.reverse.ipv6`]][]

DNS zone name for IPv6 reverse DNS records

[[*!*](#network_ovn-common:dns.zone.reverse.ipv6)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `dns.zone.reverse.ipv6`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`ipv4.address`]][]

IPv4 address for the bridge (use [`none`] to turn off IPv4 or [`auto`] to generate a new random unused subnet) (CIDR)

[[*!*](#network_ovn-common:ipv4.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | (initial value on creation:       |
|                                   | [`auto`])             |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | standard mode                     |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp`]][]

Whether to allocate addresses using DHCP

[[*!*](#network_ovn-common:ipv4.dhcp)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`true`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp.expiry`]][]

When to expire DHCP leases

[[*!*](#network_ovn-common:ipv4.dhcp.expiry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp.expiry`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`1h`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp.gateway`]][]

Address of the gateway for the subnet (use [`none`] to turn off gateway announcement)

[[*!*](#network_ovn-common:ipv4.dhcp.gateway)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp.gateway`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp.ranges`]][]

Comma-separated list of IP ranges to use for DHCP (FIRST-LAST format)

[[*!*](#network_ovn-common:ipv4.dhcp.ranges)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp.ranges`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | all addresses                     |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp.routes`]][]

Static routes to provide via DHCP option 121, as a comma-separated list of alternating subnets (CIDR) and gateway addresses (same syntax as dnsmasq and OVN)

[[*!*](#network_ovn-common:ipv4.dhcp.routes)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp.routes`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv4.l3only`]][]

Whether to enable layer 3 only mode.

[[*!*](#network_ovn-common:ipv4.l3only)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.l3only`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.nat`]][]

Whether to NAT

[[*!*](#network_ovn-common:ipv4.nat)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.nat`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`] initial      |
|                                   | value on creation if              |
|                                   | [`ipv4.address`] is  |
|                                   | set to [`auto:`]` `[`true`])    |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.nat.address`]][]

The source address used for outbound traffic from the network (requires uplink [`ovn.ingress_mode=routed`])

[[*!*](#network_ovn-common:ipv4.nat.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.nat.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.address`]][]

IPv6 address for the bridge (use [`none`] to turn off IPv6 or [`auto`] to generate a new random unused subnet) (CIDR)

[[*!*](#network_ovn-common:ipv6.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | (initial value on creation:       |
|                                   | [`auto`])             |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | standard mode                     |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp`]][]

Whether to provide additional network configuration over DHCP

[[*!*](#network_ovn-common:ipv6.dhcp)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.dhcp`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`true`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp.stateful`]][]

Whether to allocate addresses using DHCP

[[*!*](#network_ovn-common:ipv6.dhcp.stateful)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.dhcp.stateful`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv6.l3only`]][]

Whether to enable layer 3 only mode.

[[*!*](#network_ovn-common:ipv6.l3only)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.l3only`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 DHCP stateful                |
+-----------------------------------+-----------------------------------+

[[`ipv6.nat`]][]

Whether to NAT

[[*!*](#network_ovn-common:ipv6.nat)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.nat`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`] (initial     |
|                                   | value on creation if              |
|                                   | [`ipv6.address`] is  |
|                                   | set to [`auto:`]` `[`true`])    |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.nat.address`]][]

The source address used for outbound traffic from the network (requires uplink [`ovn.ingress_mode=routed`])

[[*!*](#network_ovn-common:ipv6.nat.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.nat.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`network`]][]

Uplink network to use for external network access or [`none`] to keep isolated

[[*!*](#network_ovn-common:network)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`network`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`security.acls`]][]

Comma-separated list of Network ACLs to apply to NICs connected to this network

[[*!*](#network_ovn-common:security.acls)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`security.acls.default.egress.action`]][]

Action to use for egress traffic that doesn't match any ACL rule

[[*!*](#network_ovn-common:security.acls.default.egress.action)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls                   |
|                                   | .default.egress.action`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`reject`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`security.acls`]     |
+-----------------------------------+-----------------------------------+

[[`security.acls.default.egress.logged`]][]

Whether to log egress traffic that doesn't match any ACL rule

[[*!*](#network_ovn-common:security.acls.default.egress.logged)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls                   |
|                                   | .default.egress.logged`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`security.acls`]     |
+-----------------------------------+-----------------------------------+

[[`security.acls.default.ingress.action`]][]

Action to use for ingress traffic that doesn't match any ACL rule

[[*!*](#network_ovn-common:security.acls.default.ingress.action)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls.                  |
|                                   | default.ingress.action`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`reject`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`security.acls`]     |
+-----------------------------------+-----------------------------------+

[[`security.acls.default.ingress.logged`]][]

Whether to log ingress traffic that doesn't match any ACL rule

[[*!*](#network_ovn-common:security.acls.default.ingress.logged)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls.                  |
|                                   | default.ingress.logged`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`security.acls`]     |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.group`]][]

Multicast address for [`vxlan`]

[[*!*](#network_ovn-common:tunnel.NAME.group)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.group`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`239.0.0.1`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.id`]][]

Specific tunnel ID to use for the [`vxlan`] tunnel

[[*!*](#network_ovn-common:tunnel.NAME.id)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.id`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`0`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.interface`]][]

Specific host interface to use for the tunnel

[[*!*](#network_ovn-common:tunnel.NAME.interface)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `tunnel.NAME.interface`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.local`]][]

Local address for the tunnel

[[*!*](#network_ovn-common:tunnel.NAME.local)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.local`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`gre`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.port`]][]

Specific port to use for the [`vxlan`] tunnel

[[*!*](#network_ovn-common:tunnel.NAME.port)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.port`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`0`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.protocol`]][]

Tunneling protocol: [`vxlan`] or [`gre`]

[[*!*](#network_ovn-common:tunnel.NAME.protocol)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.protocol`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | standard mode                     |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.remote`]][]

Remote address for the tunnel

[[*!*](#network_ovn-common:tunnel.NAME.remote)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.remote`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`gre`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.ttl`]][]

Specific TTL to use for multicast routing topologies

[[*!*](#network_ovn-common:tunnel.NAME.ttl)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`tunnel.NAME.ttl`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`1`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`user.*`]][]

User-provided free-form key/value pairs

[[*!*](#network_ovn-common:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

**Note**

The [`bridge.external_interfaces`] option supports an extended format allowing the creation of missing VLAN interfaces.
The extended format is [`<interfaceName>/<parentInterfaceName>/<vlanId>`].
When the external interface is added to the list with the extended format, the system will automatically create the interface upon the network's creation and subsequently delete it when the network is terminated. The system verifies that the [`<interfaceName>`] does not already exist. If the interface name is in use with a different parent or VLAN ID, or if the creation of the interface is unsuccessful, the system will revert with an error message.

[]{#network-ovn-features}

## Supported features

The following features are supported for the [`ovn`] network type:

-   [How to configure network ACLs](../../howto/network_acls/#network-acls)

-   [How to configure network forwards](../../howto/network_forwards/#network-forwards)

-   [How to configure network integrations](../../howto/network_integrations/#network-integrations)

-   [How to configure network zones](../../howto/network_zones/#network-zones)

-   [How to create peer routing relationships](../../howto/network_ovn_peers/#network-ovn-peers)

-   [How to configure network load balancers](../../howto/network_load_balancers/#network-load-balancers)
