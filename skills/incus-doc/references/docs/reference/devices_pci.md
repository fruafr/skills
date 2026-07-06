# Type: `pci`

The `pci` device type is supported for VMs. It does not support hotplugging.

PCI devices are used to pass raw PCI devices from the host into a virtual machine.

They are mainly intended to be used for specialized single-function PCI cards like sound cards or video capture cards. In theory, you can also use them for more advanced PCI devices like GPUs or network cards, but it's usually more convenient to use the specific device types that Incus provides for these devices ([`gpu` device](../devices_gpu/#devices-gpu) or [`nic` device](../devices_nic/#devices-nic)).

## Device options

`pci` devices have the following device options:

`address`

PCI address of the device

**Key:** `address`

**Type:** string

**Required:** yes

`firmware`

Whether to expose the device's option ROM to the VM

**Key:** `firmware`

**Type:** bool

**Default:** true

**Required:** no
