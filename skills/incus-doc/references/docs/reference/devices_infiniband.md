# Type: `infiniband`

The `infiniband` device type is supported for both containers and VMs. It supports hotplugging only for containers, not for VMs.

Incus supports two different kinds of network types for InfiniBand devices:

- `physical`: Passes a physical device from the host through to the instance. The targeted device will vanish from the host and appear in the instance.
- `sriov`: Passes a virtual function of an SR-IOV-enabled physical network device into the instance.

To create a `physical` `infiniband` device, use the following command:

    incus config device add <instance_name> <device_name> infiniband nictype=physical parent=<device>

To create an `sriov` `infiniband` device, use the following command:

    incus config device add <instance_name> <device_name> infiniband nictype=sriov parent=<sriov_enabled_device>

## Device options

`infiniband` devices have the following device options:

`hwaddr`

The MAC address of the new interface (can be either the full 20-byte variant or the short 8-byte variant, which will only modify the last 8 bytes of the parent device)

**Key:** `hwaddr`

**Type:** string

**Default:** randomly assigned

**Required:** no

`mtu`

The MTU of the new interface

**Key:** `mtu`

**Type:** integer

**Default:** parent MTU

**Required:** no

`name`

The name of the interface inside the instance

**Key:** `name`

**Type:** string

**Default:** kernel assigned

**Required:** no

`nictype`

The device type (one of `physical` or `sriov`)

**Key:** `nictype`

**Type:** string

**Required:** yes

`node_guid`

The node GUID to assign to the virtual function (8 bytes of hex separated by colons)

**Key:** `node_guid`

**Type:** string

**Default:** kernel assigned

**Required:** no

`parent`

The name of the interface inside the instance

**Key:** `parent`

**Type:** string

**Default:** kernel assigned

**Required:** no

`port_guid`

The port GUID to assign to the virtual function (8 bytes of hex separated by colons)

**Key:** `port_guid`

**Type:** string

**Default:** kernel assigned

**Required:** no
