# Type: `usb`

The `usb` device type is supported for both containers and VMs. It supports hotplugging for both containers and VMs.

USB devices make the specified USB device appear in the instance. For performance issues, avoid using devices that require high throughput or low latency.

For containers, only `libusb` devices (at `/dev/bus/usb`) are passed to the instance. This method works for devices that have user-space drivers. For devices that require dedicated kernel drivers, use a [`unix-char` device](../devices_unix_char/#devices-unix-char) or a [`unix-hotplug` device](../devices_unix_hotplug/#devices-unix-hotplug) instead.

For virtual machines, the entire USB device is passed through, so any USB device is supported. When a device is passed to the instance, it vanishes from the host.

## Device options

`usb` devices have the following device options:

`attached`

Whether the USB device is plugged in or not

**Key:** `attached`

**Type:** bool

**Default:** `true`

**Required:** no

`busnum`

The bus number of which the USB device is attached

**Key:** `busnum`

**Type:** int

`devnum`

The device number of the USB device

**Key:** `devnum`

**Type:** int

`gid`

Only for containers: GID of the device owner in the instance

**Key:** `gid`

**Type:** int

**Default:** `0`

`mode`

Only for containers: Mode of the device in the instance

**Key:** `mode`

**Type:** int

**Default:** `0660`

`productid`

The product ID of the USB device

**Key:** `productid`

**Type:** string

`required`

Whether this device is required to start the instance (the default is `false`, and all devices can be hotplugged)

**Key:** `required`

**Type:** bool

**Default:** `false`

`serial`

The serial number of the USB device

**Key:** `serial`

**Type:** string

`uid`

Only for containers: UID of the device owner in the instance

**Key:** `uid`

**Type:** int

**Default:** `0`

`vendorid`

The vendor ID of the USB device

**Key:** `vendorid`

**Type:** string
