[]{#networks}

# About networking

There are different ways to connect your instances to the Internet. The easiest method is to have Incus create a network bridge during initialization and use this bridge for all instances, but Incus supports many different and advanced setups for networking.

## Network devices

To grant direct network access to an instance, you must assign it at least one network device, also called NIC.
You can configure the network device in one of the following ways:

-   Use the default network bridge that you set up during the Incus initialization.
    Check the default profile to see the default configuration:

    ::: 
    ::: highlight
          incus profile show default
    :::
    :::

    This method is used if you do not specify a network device for your instance.

-   Use an existing network interface by adding it as a network device to your instance.
    This network interface is outside of Incus control.
    Therefore, you must specify all information that Incus needs to use the network interface.

    Use a command similar to the following:

    ::: 
    ::: highlight
          incus config device add <instance_name> <device_name> nic nictype=<nic_type> ...
    :::
    :::

    See [[Type: [`nic`]]](../../reference/devices_nic/#devices-nic) for a list of available NIC types and their configuration properties.

    For example, you could add a pre-existing Linux bridge ([`br0`]) with the following command:

    ::: 
    ::: highlight
          incus config device add <instance_name> eth0 nic nictype=bridged parent=br0
    :::
    :::

-   [Create a managed network](../../howto/network_create/) and add it as a network device to your instance.
    With this method, Incus has all required information about the configured network, and you can directly attach it to your instance as a device:

    ::: 
    ::: highlight
          incus network attach <network_name> <instance_name> <device_name>
    :::
    :::

    See [Attach a network to an instance](../../howto/network_create/#network-attach) for more information.

[]{#id1}

## Managed networks

Managed networks in Incus are created and configured with the [`incus`]` `[`network`]` `[`[create|edit|set]`] command.

Depending on the network type, Incus either fully controls the network or just manages an external network interface.

Note that not all [NIC types](../../reference/devices_nic/#devices-nic) are supported as network types.
Incus can only set up some of the types as managed networks.

### Fully controlled networks

Fully controlled networks create network interfaces and provide most functionality, including, for example, the ability to do IP management.

Incus supports the following network types:

[Bridge network](../../reference/network_bridge/#network-bridge)

:   A network bridge creates a virtual L2 Ethernet switch that instance NICs can connect to, making it possible for them to communicate with each other and the host.
    Incus bridges can leverage underlying native Linux bridges and Open vSwitch.

    In Incus context, the [`bridge`] network type creates an L2 bridge that connects the instances that use it together into a single network L2 segment.
    This makes it possible to pass traffic between the instances.
    The bridge can also provide local DHCP and DNS.

    This is the default network type.

[OVN network](../../reference/network_ovn/#network-ovn)

:   OVN is a software-defined networking system that supports virtual network abstraction.
    You can use it to build your own private cloud.
    See [`www.ovn.org`](https://www.ovn.org/) for more information.

    In Incus context, the [`ovn`] network type creates a logical network.
    To set it up, you must install and configure the OVN tools.
    In addition, you must create an uplink network that provides the network connection for OVN.
    As the uplink network, you should use one of the external network types or a managed Incus bridge.

    ::: 
    Tip

    Unlike the other network types, you can create and manage an OVN network inside a [project](../../projects/#projects).
    This means that you can create your own OVN network as a non-admin user, even in a restricted project.
    :::

### External networks

External networks use network interfaces that already exist.
Therefore, Incus has limited possibility to control them, and Incus features like network ACLs, network forwards and network zones are not supported.

The main purpose for using external networks is to provide an uplink network through a parent interface.
This external network specifies the presets to use when connecting instances or other networks to a parent interface.

Incus supports the following external network types:

[Macvlan network](../../reference/network_macvlan/#network-macvlan)

:   Macvlan is a virtual LAN that you can use if you want to assign several IP addresses to the same network interface, basically splitting up the network interface into several sub-interfaces with their own IP addresses.
    You can then assign IP addresses based on the randomly generated MAC addresses.

    In Incus context, the [`macvlan`] network type provides a preset configuration to use when connecting instances to a parent macvlan interface.

[SR-IOV network](../../reference/network_sriov/#network-sriov)

:   SR-IOV is a hardware standard that allows a single network card port to appear as several virtual network interfaces in a virtualized environment.

    In Incus context, the [`sriov`] network type provides a preset configuration to use when connecting instances to a parent SR-IOV interface.

[Physical network](../../reference/network_physical/#network-physical)

:   The [`physical`] network type connects to an existing physical network, which can be a network interface or a bridge, and serves as an uplink network for OVN.

    It provides a preset configuration to use when connecting OVN networks to a parent interface.

## Recommendations

In general, if you can use a managed network, you should do so because networks are easy to configure and you can reuse the same network for several instances without repeating the configuration.

Which network type to choose depends on your specific use case.
If you choose a fully controlled network, it provides more functionality than using a network device.

As a general recommendation:

-   If you are running Incus on a single system or in a public cloud, use a [Bridge network](../../reference/network_bridge/#network-bridge).

-   If you are running Incus in your own private cloud, use an [OVN network](../../reference/network_ovn/#network-ovn).

    ::: 
    Note

    OVN requires a shared L2 uplink network for proper operation.
    Therefore, using OVN is usually not possible if you run Incus in a public cloud.
    :::

-   To connect an instance NIC to a managed network, use the [`network`] property rather than the [`parent`] property, if possible.
    This way, the NIC can inherit the settings from the network and you don't need to specify the [`nictype`].
