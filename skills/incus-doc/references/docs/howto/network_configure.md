[]{#network-configure}

# How to configure a network

To configure an existing network, use either the [[[`incus`]` `[`network`]` `[`set`]]](../../reference/manpages/incus/network/set/#incus-network-set-md) and [[[`incus`]` `[`network`]` `[`unset`]]](../../reference/manpages/incus/network/unset/#incus-network-unset-md) commands (to configure single settings) or the [`incus`]` `[`network`]` `[`edit`] command (to edit the full configuration).
To configure settings for specific cluster members, add the [`--target`] flag.

For example, the following command configures a DNS server for a physical network:

    incus network set UPLINK dns.nameservers=8.8.8.8

The available configuration options differ depending on the network type.
See [Network types](../network_create/#network-types) for links to the configuration options for each network type.

There are separate commands to configure advanced networking features.
See the following documentation:

-   [How to configure network ACLs](../network_acls/)

-   [How to configure network forwards](../network_forwards/)

-   [How to configure network integrations](../network_integrations/)

-   [How to configure network load balancers](../network_load_balancers/)

-   [How to configure network zones](../network_zones/)

-   [How to create peer routing relationships](../network_ovn_peers/) (OVN only)
