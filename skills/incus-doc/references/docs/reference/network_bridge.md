[]{#network-bridge}

# Bridge network

As one of the possible network configuration types under Incus, Incus supports creating and managing network bridges.

A network bridge creates a virtual L2 Ethernet switch that instance NICs can connect to, making it possible for them to communicate with each other and the host.
Incus bridges can leverage underlying native Linux bridges and Open vSwitch.

The [`bridge`] network type allows to create an L2 bridge that connects the instances that use it together into a single network L2 segment.
Bridges created by Incus are managed, which means that in addition to creating the bridge interface itself, Incus also sets up a local [`dnsmasq`] process to provide DHCP, IPv6 route announcements and DNS services to the network.
By default, it also performs NAT for the bridge.

See [How to configure your firewall](../../howto/network_bridge_firewalld/#network-bridge-firewall) for instructions on how to configure your firewall to work with Incus bridge networks.

**Note**

Static DHCP assignments depend on the client using its MAC address as the DHCP identifier.
This method prevents conflicting leases when copying an instance, and thus makes statically assigned leases work properly.

## IPv6 prefix size

If you're using IPv6 for your bridge network, you should use a prefix size of 64.

Larger subnets (i.e., using a prefix smaller than 64) should work properly too, but they aren't typically that useful for SLAAC.

Smaller subnets are in theory possible (when using stateful DHCPv6 for IPv6 allocation), but they aren't properly supported by [`dnsmasq`] and might cause problems.
If you must create a smaller subnet, use static allocation or another standalone router advertisement daemon.

[]{#network-bridge-options}

## Configuration options

The following configuration key namespaces are currently supported for the [`bridge`] network type:

-   [`bgp`] (BGP peer configuration)

-   [`bridge`] (L2 interface configuration)

-   [`dns`] (DNS server and resolution configuration)

-   [`ipv4`] (L3 IPv4 configuration)

-   [`ipv6`] (L3 IPv6 configuration)

-   [`security`] (network ACL configuration)

-   [`raw`] (raw configuration file content)

-   [`tunnel`] (cross-host tunneling configuration)

-   [`user`] (free-form key/value for user metadata)

**Note**

Incus uses the [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) where network subnet information is required, for example, [`192.0.2.0/24`] or [`2001:db8::/32`]. This does not apply to cases where a single address is required, for example, local/remote addresses of tunnels, NAT addresses or specific addresses to apply to an instance.

The following configuration options are available for the [`bridge`] network type:

[[`bgp.ipv4.instances`]][]

Whether to advertise a /32 route for the IPv4 address of each running instance

[[*!*](#network_bridge-common:bgp.ipv4.instances)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bgp.ipv4.instances`]     |
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
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

[[`bgp.ipv4.nexthop`]][]

Override the next-hop for advertised prefixes

[[*!*](#network_bridge-common:bgp.ipv4.nexthop)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bgp.ipv4.nexthop`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | local address                     |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

[[`bgp.ipv6.instances`]][]

Whether to advertise a /128 route for the IPv6 address of each running instance

[[*!*](#network_bridge-common:bgp.ipv6.instances)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bgp.ipv6.instances`]     |
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
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

[[`bgp.ipv6.nexthop`]][]

Override the next-hop for advertised prefixes

[[*!*](#network_bridge-common:bgp.ipv6.nexthop)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bgp.ipv6.nexthop`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | local address                     |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

[[`bridge.driver`]][]

Bridge driver: [`native`] or [`openvswitch`]

[[*!*](#network_bridge-common:bridge.driver)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bridge.driver`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`native`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`bridge.external_interfaces`]][]

Comma-separated list of unconfigured network interfaces to include in the bridge

[[*!*](#network_bridge-common:bridge.external_interfaces)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`brid                            |
|                                   | ge.external_interfaces`]     |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`bridge.hwaddr`]][]

MAC address for the bridge

[[*!*](#network_bridge-common:bridge.hwaddr)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bridge.hwaddr`]     |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`bridge.mtu`]][]

Bridge MTU (default varies if tunnel in use)

[[*!*](#network_bridge-common:bridge.mtu)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bridge.mtu`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`1500`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`bridge.multicast_snooping`]][]

Whether to enable multicast snooping on the bridge

[[*!*](#network_bridge-common:bridge.multicast_snooping)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bri                             |
|                                   | dge.multicast_snooping`]     |
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
|                                   | native bridge                     |
+-----------------------------------+-----------------------------------+

[[`dns.domain`]][]

Domain to advertise to DHCP clients and use for DNS resolution

[[*!*](#network_bridge-common:dns.domain)]

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
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`dns.mode`]][]

DNS registration mode: none for no DNS record, managed for Incus-generated static records or dynamic for client-generated records

[[*!*](#network_bridge-common:dns.mode)]

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

DNS server IPs to advertise to DHCP clients and via Router Advertisements. Both IPv4 and IPv6 addresses get pushed via DHCP, and IPv6 addresses are also advertised as RDNSS via RA.

[[*!*](#network_bridge-common:dns.nameservers)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.nameservers`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | IPv4 and IPv6 address             |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`dns.search`]][]

Full comma-separated domain search list, defaulting to [`dns.domain`] value

[[*!*](#network_bridge-common:dns.search)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.search`]              |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`dns.zone.forward`]][]

Comma-separated list of DNS zone names for forward DNS records

[[*!*](#network_bridge-common:dns.zone.forward)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`dns.zone.forward`]     |
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

[[`dns.zone.reverse.ipv4`]][]

DNS zone name for IPv4 reverse DNS records

[[*!*](#network_bridge-common:dns.zone.reverse.ipv4)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `dns.zone.reverse.ipv4`]     |
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

[[`dns.zone.reverse.ipv6`]][]

DNS zone name for IPv6 reverse DNS records

[[*!*](#network_bridge-common:dns.zone.reverse.ipv6)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `dns.zone.reverse.ipv6`]     |
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

[[`ipv4.address`]][]

IPv4 address for the bridge (use [`none`] to turn off IPv4 or [`auto`] to generate a new random unused subnet) (CIDR)

[[*!*](#network_bridge-common:ipv4.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -   (initial value on creation:   |
|                                   |     [`auto`])         |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | standard mode                     |
+-----------------------------------+-----------------------------------+

[[`ipv4.dhcp`]][]

Whether to allocate addresses using DHCP

[[*!*](#network_bridge-common:ipv4.dhcp)]

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

[[*!*](#network_bridge-common:ipv4.dhcp.expiry)]

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

[[*!*](#network_bridge-common:ipv4.dhcp.gateway)]

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

[[*!*](#network_bridge-common:ipv4.dhcp.ranges)]

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

Static routes to provide via DHCP option 121, as a comma-separated list of alternating subnets (CIDR) and gateway addresses (same syntax as dnsmasq)

[[*!*](#network_bridge-common:ipv4.dhcp.routes)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.dhcp.routes`]     |
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
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv4.firewall`]][]

Whether to generate filtering firewall rules for this network

[[*!*](#network_bridge-common:ipv4.firewall)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.firewall`]     |
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

[[`ipv4.nat`]][]

Whether to NAT

[[*!*](#network_bridge-common:ipv4.nat)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.nat`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`](initial      |
|                                   | value on creation if              |
|                                   | [`ipv4.address`] is  |
|                                   | set to [`auto`]:             |
|                                   | [`true`])             |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.nat.address`]][]

The source address used for outbound traffic from the bridge

[[*!*](#network_bridge-common:ipv4.nat.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.nat.address`]     |
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
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.nat.order`]][]

Whether to add the required NAT rules before or after any pre-existing rules (no effect with the [`nftables`] firewall driver)

[[*!*](#network_bridge-common:ipv4.nat.order)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.nat.order`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`before`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.ovn.ranges`]][]

Comma-separated list of IPv4 ranges to use for child OVN network routers (FIRST-LAST format)

[[*!*](#network_bridge-common:ipv4.ovn.ranges)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.ovn.ranges`]     |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`ipv4.routes`]][]

Comma-separated list of additional IPv4 CIDR subnets to route to the bridge

[[*!*](#network_bridge-common:ipv4.routes)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.routes`]              |
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
|                                   | IPv4 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv4.routing`]][]

Whether to route traffic in and out of the bridge

[[*!*](#network_bridge-common:ipv4.routing)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv4.routing`]     |
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
|                                   | IPv4 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv6.address`]][]

IPv6 address for the bridge (use [`none`] to turn off IPv6 or [`auto`] to generate a new random unused subnet) (CIDR)

[[*!*](#network_bridge-common:ipv6.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -   (initial value on creation:   |
|                                   |     [`auto`])         |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | standard mode                     |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp`]][]

Whether to provide additional network configuration over DHCP

[[*!*](#network_bridge-common:ipv6.dhcp)]

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
|                                   | IPv6 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp.expiry`]][]

When to expire DHCP leases

[[*!*](#network_bridge-common:ipv6.dhcp.expiry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.dhcp.expiry`]     |
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
|                                   | IPv6 DHCP                         |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp.ranges`]][]

Comma-separated list of IPv6 ranges to use for DHCP (FIRST-LAST format)

[[*!*](#network_bridge-common:ipv6.dhcp.ranges)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.dhcp.ranges`]     |
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
|                                   | IPv6 stateful DHCP                |
+-----------------------------------+-----------------------------------+

[[`ipv6.dhcp.stateful`]][]

Whether to allocate addresses using DHCP

[[*!*](#network_bridge-common:ipv6.dhcp.stateful)]

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

[[`ipv6.firewall`]][]

Whether to generate filtering firewall rules for this network

[[*!*](#network_bridge-common:ipv6.firewall)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.firewall`]     |
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

[[`ipv6.nat`]][]

Whether to NAT

[[*!*](#network_bridge-common:ipv6.nat)]

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
|                                   | set to [`auto`]:             |
|                                   | [`true`])             |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.nat.address`]][]

The source address used for outbound traffic from the bridge

[[*!*](#network_bridge-common:ipv6.nat.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.nat.address`]     |
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
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.nat.order`]][]

Whether to add the required NAT rules before or after any pre-existing rules (no effect with the [`nftables`] firewall driver)

[[*!*](#network_bridge-common:ipv6.nat.order)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.nat.order`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`before`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.ovn.ranges`]][]

Comma-separated list of IPv6 ranges to use for child OVN network routers (FIRST-LAST format)

[[*!*](#network_bridge-common:ipv6.ovn.ranges)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.ovn.ranges`]     |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`ipv6.routes`]][]

Comma-separated list of additional IPv6 CIDR subnets to route to the bridge

[[*!*](#network_bridge-common:ipv6.routes)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.routes`]              |
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
|                                   | IPv6 address                      |
+-----------------------------------+-----------------------------------+

[[`ipv6.routing`]][]

Whether to route traffic in and out of the bridge

[[*!*](#network_bridge-common:ipv6.routing)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ipv6.routing`]     |
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

[[`raw.dnsmasq`]][]

Additional dnsmasq configuration to append to the configuration file

[[*!*](#network_bridge-common:raw.dnsmasq)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`raw.dnsmasq`]              |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`security.acls`]][]

Comma-separated list of Network ACLs to apply to NICs connected to this network (see [Bridge limitations](../../howto/network_acls/#network-acls-bridge-limitations))

[[*!*](#network_bridge-common:security.acls)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`security.acls`]     |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

[[`security.acls.default.egress.action`]][]

Action to use for egress traffic that doesn't match any ACL rule

[[*!*](#network_bridge-common:security.acls.default.egress.action)]

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

[[*!*](#network_bridge-common:security.acls.default.egress.logged)]

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

[[*!*](#network_bridge-common:security.acls.default.ingress.action)]

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

[[*!*](#network_bridge-common:security.acls.default.ingress.logged)]

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

Multicast address for [`vxlan`] (used if local and remote aren't set)

[[*!*](#network_bridge-common:tunnel.NAME.group)]

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

[[*!*](#network_bridge-common:tunnel.NAME.id)]

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

[[*!*](#network_bridge-common:tunnel.NAME.interface)]

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

Local address for the tunnel (not necessary for multicast [`vxlan`])

[[*!*](#network_bridge-common:tunnel.NAME.local)]

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
|                                   | [`gre`] or           |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.port`]][]

Specific port to use for the [`vxlan`] tunnel

[[*!*](#network_bridge-common:tunnel.NAME.port)]

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

[[*!*](#network_bridge-common:tunnel.NAME.protocol)]

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

Remote address for the tunnel (not necessary for multicast [`vxlan`])

[[*!*](#network_bridge-common:tunnel.NAME.remote)]

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
|                                   | [`gre`] or           |
|                                   | [`vxlan`]              |
+-----------------------------------+-----------------------------------+

[[`tunnel.NAME.ttl`]][]

Specific TTL to use for multicast routing topologies

[[*!*](#network_bridge-common:tunnel.NAME.ttl)]

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

[[*!*](#network_bridge-common:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
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
|                                   | -                                 |
+-----------------------------------+-----------------------------------+

## BGP options

These options configure BGP peering for OVN downstream networks:

[[`bgp.peers.NAME.address`]][]

Peer address (IPv4 or IPv6) for use by [`ovn`] downstream networks

[[*!*](#network_bridge-bgp:bgp.peers.NAME.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`                                |
|                                   | bgp.peers.NAME.address`]     |
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
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

[[`bgp.peers.NAME.asn`]][]

Peer AS number for use by [`ovn`] downstream networks

[[*!*](#network_bridge-bgp:bgp.peers.NAME.asn)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`bgp.peers.NAME.asn`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -                                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

[[`bgp.peers.NAME.holdtime`]][]

Peer session hold time (in seconds; optional)

[[*!*](#network_bridge-bgp:bgp.peers.NAME.holdtime)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`b                               |
|                                   | gp.peers.NAME.holdtime`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`180`]              |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

[[`bgp.peers.NAME.password`]][]

Peer session password (optional) for use by [`ovn`] downstream networks

[[*!*](#network_bridge-bgp:bgp.peers.NAME.password)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`b                               |
|                                   | gp.peers.NAME.password`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | -   (no password)                 |
+-----------------------------------+-----------------------------------+
| **Condition:**                    | []                      |
|                                   |                                   |
|                                   | BGP server                        |
+-----------------------------------+-----------------------------------+

**Note**

The [`bridge.external_interfaces`] option supports an extended format allowing the creation of missing VLAN interfaces.
The extended format is [`<interfaceName>/<parentInterfaceName>/<vlanId>`].
When the external interface is added to the list with the extended format, the system will automatically create the interface upon the network's creation and subsequently delete it when the network is terminated. The system verifies that the [`<interfaceName>`] does not already exist. If the interface name is in use with a different parent or VLAN ID, or if the creation of the interface is unsuccessful, the system will revert with an error message.

[]{#network-bridge-features}

## Supported features

The following features are supported for the [`bridge`] network type:

-   [How to configure network ACLs](../../howto/network_acls/#network-acls)

-   [How to configure network forwards](../../howto/network_forwards/#network-forwards)

-   [How to configure network zones](../../howto/network_zones/#network-zones)

-   [How to configure Incus as a BGP server](../../howto/network_bgp/#network-bgp)

-   [[How to integrate with [`systemd-resolved`]]](../../howto/network_bridge_resolved/#network-bridge-resolved)
