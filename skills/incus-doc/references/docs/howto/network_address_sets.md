[]{#network-address-sets}

# How to use network address sets

**Note**

Network address sets are working with [ACLs](../network_acls/#network-acls) and work only with [OVN network](../../reference/network_ovn/#network-ovn) or with [bridged networks](../network_bridge_firewalld/#network-bridge-firewall) using [`nftables`] only.

Network address sets are a list of either IPv4, IPv6 addresses with or without CIDR suffix. IP ranges (for example [`10.0.0.120-10.0.0.130`]) are also supported, though a single range may expand to at most 256 addresses. Larger sets of addresses should be expressed using CIDR notation instead. Address sets can be used in source or destination fields of [ACLs](../network_acls/#network-acls-rules-properties).

## Address set properties

Address sets have the following properties:

  Property                                                 Type          Required   Description
  -------------------------------------------------------- ------------- ---------- ----------------------------------------
  [`name`]          string        yes        Name of the network address set
  [`description`]   string        no         Description of the network address set
  [`addresses`]     string list   no         Ingress traffic rules

## Address set configuration options

The following configuration options are available for all network address sets:

[[`user.*`]][]

Free form user key/value storage

[[*!*](#network_address_set-common:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

User keys can be used in search.

## Creating an address set

Use the following command to create an address set.

    incus network address-set create <name> [configuration_options...]

This will create an address set without any addresses, after this you can [add addresses](#manage-addresses-in-set).

[]{#manage-addresses-in-set}

## Add or remove addresses

Adding addresses is pretty straightforward:

    incus network address-set add <name> <address1> <address2>

There is no restriction about the kind of address you are appending in your set. You can use any mix of IPv4, IPv6, CIDR subnets and IP ranges.

To remove addresses, the same [`remove`] command can be used instead.

    incus network address-set remove <name> <address1> <address2>

## Use of address sets in ACL rules

In order to use an address set in an [ACL](../network_acls/#network-acls-address-sets), we need to prepend [`name`] with [`$`] (you need to escape the dollar in command line). Then we can refer the address set in [`source`] or [`destination`] fields of an ACL rule.
