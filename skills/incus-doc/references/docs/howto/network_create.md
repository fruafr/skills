# How to create a network

To create a managed network, use the [[[`incus`]` `[`network`]]](../../reference/manpages/incus/network/#incus-network-md) command and its subcommands.
Append [`--help`] to any command to see more information about its usage and available flags.

[]{#id1}

## Network types

The following network types are available:

  Network type                                          Documentation                                                                                                   Configuration options
  ----------------------------------------------------- --------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------
  [`bridge`]     [Bridge network](../../reference/network_bridge/#network-bridge)         [Configuration options](../../reference/network_bridge/#network-bridge-options)
  [`ovn`]        [OVN network](../../reference/network_ovn/#network-ovn)                  [Configuration options](../../reference/network_ovn/#network-ovn-options)
  [`macvlan`]    [Macvlan network](../../reference/network_macvlan/#network-macvlan)      [Configuration options](../../reference/network_macvlan/#network-macvlan-options)
  [`sriov`]      [SR-IOV network](../../reference/network_sriov/#network-sriov)           [Configuration options](../../reference/network_sriov/#network-sriov-options)
  [`physical`]   [Physical network](../../reference/network_physical/#network-physical)   [Configuration options](../../reference/network_physical/#network-physical-options)

## Create a network

Use the following command to create a network:

    incus network create <name> --type=<network_type> [configuration_options...]

See [Network types](#network-types) for a list of available network types and links to their configuration options.

If you do not specify a [`--type`] argument, the default type of [`bridge`] is used.

[]{#network-create-cluster}

### Create a network in a cluster

If you are running an Incus cluster and want to create a network, you must create the network for each cluster member separately.
The reason for this is that the network configuration, for example, the name of the parent network interface, might be different between cluster members.

Therefore, you must first create a pending network on each member with the [`--target=<cluster_member>`] flag and the appropriate configuration for the member.
Make sure to use the same network name for all members.
Then create the network without specifying the [`--target`] flag to actually set it up.

For example, the following series of commands sets up a physical network with the name [`UPLINK`] on three cluster members:

[`~$`]` `[`incus`]` `[`network`]` `[`create`]` `[`UPLINK`]` `[`--type=physical`]` `[`parent=br0`]` `[`--target=vm01`]

[`Network`]` `[`UPLINK`]` `[`pending`]` `[`on`]` `[`member`]` `[`vm01`]

[`~$`]` `[`incus`]` `[`network`]` `[`create`]` `[`UPLINK`]` `[`--type=physical`]` `[`parent=br0`]` `[`--target=vm02`]

[`Network`]` `[`UPLINK`]` `[`pending`]` `[`on`]` `[`member`]` `[`vm02`]

[`~$`]` `[`incus`]` `[`network`]` `[`create`]` `[`UPLINK`]` `[`--type=physical`]` `[`parent=br0`]` `[`--target=vm03`]

[`Network`]` `[`UPLINK`]` `[`pending`]` `[`on`]` `[`member`]` `[`vm03`]

[`~$`]` `[`incus`]` `[`network`]` `[`create`]` `[`UPLINK`]` `[`--type=physical`]

[`Network`]` `[`UPLINK`]` `[`created`]

Also see [How to configure networks for a cluster](../cluster_config_networks/#cluster-config-networks).

[]{#network-attach}

## Attach a network to an instance

After creating a managed network, you can attach it to an instance as a [NIC device](../../reference/devices_nic/#devices-nic).

To do so, use the following command:

    incus network attach <network_name> <instance_name> [<device_name>] [<interface_name>]

The device name and the interface name are optional, but we recommend specifying at least the device name.
If not specified, Incus uses the network name as the device name, which might be confusing and cause problems.
For example, Incus images perform IP auto-configuration on the [`eth0`] interface, which does not work if the interface is called differently.

For example, to attach the network [`my-network`] to the instance [`my-instance`] as [`eth0`] device, enter the following command:

    incus network attach my-network my-instance eth0

### Attach the network as a device

The [[[`incus`]` `[`network`]` `[`attach`]]](../../reference/manpages/incus/network/attach/#incus-network-attach-md) command is a shortcut for adding a NIC device to an instance.
Alternatively, you can add a NIC device based on the network configuration in the usual way:

    incus config device add <instance_name> <device_name> nic network=<network_name>

When using this way, you can add further configuration to the command to override the default settings for the network if needed.
See [NIC device](../../reference/devices_nic/#devices-nic) for all available device options.
