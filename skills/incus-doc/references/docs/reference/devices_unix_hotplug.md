# Type: `unix-hotplug`

The `unix-hotplug` device type is supported for containers. It supports hotplugging.

Unix hotplug devices make the requested Unix device appear as a device in the instance (under `/dev`). If the device exists on the host system, you can read from it and write to it.

The implementation depends on `systemd-udev` to be run on the host.

## Device options

`unix-hotplug` devices have the following device options:

`gid`

GID of the device owner in the instance

**Key:** `gid`

**Type:** int

**Default:** 0

`mode`

Mode of the device in the instance

**Key:** `mode`

**Type:** int

**Default:** 0660

`pci`

The PCI address of a USB controller to monitor

**Key:** `pci`

**Type:** string

`productid`

The product ID of the USB device

**Key:** `productid`

**Type:** string

`required`

Whether this device is required to start the instance

**Key:** `required`

**Type:** bool

**Default:** true

`uid`

UID of the device owner in the instance

**Key:** `uid`

**Type:** int

**Default:** 0

`vendorid`

The vendor ID of the USB device

**Key:** `vendorid`

**Type:** string
