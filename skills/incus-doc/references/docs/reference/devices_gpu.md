# Type: `gpu`

GPU devices make the specified GPU device or devices appear in the instance.

For containers, a `gpu` device may match multiple GPUs at once. For VMs, each device can match only a single GPU.

The following types of GPUs can be added using the `gputype` device option:

- [`physical`](#gpu-physical) (container and VM): Passes an entire GPU through into the instance. This value is the default if `gputype` is unspecified.
- [`mdev`](#gpu-mdev) (VM only): Creates and passes a virtual GPU through into the instance.
- [`mig`](#gpu-mig) (container only): Creates and passes a MIG (Multi-Instance GPU) through into the instance.
- [`sriov`](#gpu-sriov) (VM only): Passes a virtual function of an SR-IOV-enabled GPU into the instance.

## `gputype`: `physical`

The `physical` GPU type is supported for both containers and VMs. It supports hotplugging only for containers, not for VMs.

A `physical` GPU device passes an entire GPU through into the instance.

### Device options

GPU devices of type `physical` have the following device options:

`gid`

GID of the device owner in the instance (container only)

**Key:** `gid`

**Type:** int

**Default:** 0

**Required:** no

`id`

The DRM card ID of the GPU device

**Key:** `id`

**Type:** string

**Required:** no

`mode`

Mode of the device in the instance (container only)

**Key:** `mode`

**Type:** int

**Default:** 0660

**Required:** no

`pci`

The PCI address of the GPU device

**Key:** `pci`

**Type:** string

**Required:** no

`productid`

The product ID of the GPU device

**Key:** `productid`

**Type:** string

**Required:** no

`uid`

UID of the device owner in the instance (container only)

**Key:** `uid`

**Type:** int

**Default:** 0

**Required:** no

`vendorid`

The vendor ID of the GPU device

**Key:** `vendorid`

**Type:** string

**Required:** no

## `gputype`: `mdev`

The `mdev` GPU type is supported only for VMs. It does not support hotplugging.

An `mdev` GPU device creates and passes a virtual GPU through into the instance.

### Device options

GPU devices of type `mdev` have the following device options:

`id`

The DRM card ID of the GPU device

**Key:** `id`

**Type:** string

**Required:** no

`mdev`

The mediated device profile to use (required - for example, `i915-GVTg_V5_4`)

**Key:** `mdev`

**Type:** string

**Required:** yes

`productid`

The product ID of the GPU device

**Key:** `productid`

**Type:** string

**Required:** no

`vendorid`

The vendor ID of the GPU device

**Key:** `vendorid`

**Type:** string

**Required:** no

## `gputype`: `mig`

The `mig` GPU type is supported only for containers. It does not support hotplugging.

A `mig` GPU device creates and passes a MIG compute instance through into the instance.

### Device options

GPU devices of type `mig` have the following device options:

`id`

The DRM card ID of the GPU device

**Key:** `id`

**Type:** string

**Required:** no

`mig.ci`

Existing MIG compute instance ID

**Key:** `mig.ci`

**Type:** int

**Required:** no

`mig.gi`

Existing MIG GPU instance ID

**Key:** `mig.gi`

**Type:** int

**Required:** no

`mig.uuid`

Existing MIG device UUID (MIG- prefix can be omitted)

**Key:** `mig.uuid`

**Type:** string

**Required:** no

`pci`

The PCI address of the GPU device

**Key:** `pci`

**Type:** string

**Required:** no

`productid`

The product ID of the GPU device

**Key:** `productid`

**Type:** string

**Required:** no

`vendorid`

The vendor ID of the GPU device

**Key:** `vendorid`

**Type:** string

**Required:** no

## `gputype`: `sriov`

The `sriov` GPU type is supported only for VMs. It does not support hotplugging.

An `sriov` GPU device passes a virtual function of an SR-IOV-enabled GPU into the instance.

### Device options

GPU devices of type `sriov` have the following device options:

`id`

The DRM card ID of the parent GPU device

**Key:** `id`

**Type:** string

**Required:** no

`pci`

The PCI address of the parent GPU device

**Key:** `pci`

**Type:** string

**Required:** no

`productid`

The product ID of the parent GPU device

**Key:** `productid`

**Type:** string

**Required:** no

`vendorid`

The vendor ID of the parent GPU device

**Key:** `vendorid`

**Type:** string

**Required:** no
