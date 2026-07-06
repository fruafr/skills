# Type: `unix-block`

The `unix-block` device type is supported for containers. It supports hotplugging.

Unix block devices make the specified block device appear as a device in the instance (under `/dev`). You can read from the device and write to it.

## Device options

`unix-block` devices have the following device options:

`gid`

GID of the device owner in the instance

**Key:** `gid`

**Type:** int

**Default:** 0

`major`

Device major number

**Key:** `major`

**Type:** int

**Default:** device on host

`minor`

Device minor number

**Key:** `minor`

**Type:** int

**Default:** device on host

`mode`

Mode of the device in the instance

**Key:** `mode`

**Type:** int

**Default:** 0660

`path`

Path inside the instance (one of `source` and `path` must be set)

**Key:** `path`

**Type:** string

`required`

Whether this device is required to start the instance

**Key:** `required`

**Type:** bool

**Default:** true

`source`

Path on the host (one of `source` and `path` must be set)

**Key:** `source`

**Type:** string

`uid`

UID of the device owner in the instance

**Key:** `uid`

**Type:** int

**Default:** 0

## Hotplugging

Hotplugging is enabled if you set `required=false` and specify the `source` option for the device.

In this case, the device is automatically passed into the container when it appears on the host, even after the container starts. If the device disappears from the host system, it is removed from the container as well.
