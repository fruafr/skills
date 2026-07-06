[]{#exp-storage}

# About storage pools, volumes and buckets

Incus stores its data in storage pools, divided into storage volumes of different content types (like images or instances).
You could think of a storage pool as the disk that is used to store data, while storage volumes are different partitions on this disk that are used for specific purposes.

In addition to storage volumes, there are storage buckets, which use the [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/API/Welcome.html) protocol.
Like storage volumes, storage buckets are part of a storage pool.

[]{#id1}

## Storage pools

During initialization, Incus prompts you to create a first storage pool.
If required, you can create additional storage pools later (see [Create a storage pool](../../howto/storage_pools/#storage-create-pool)).

Each storage pool uses a storage driver.
The following storage drivers are supported:

-   [[Directory - [`dir`]]](../../reference/storage_dir/#storage-dir)

-   [[Btrfs - [`btrfs`]]](../../reference/storage_btrfs/#storage-btrfs)

-   [[LVM - [`lvm`]]](../../reference/storage_lvm/#storage-lvm)

-   [[LVM Cluster - [`lvmcluster`]]](../../reference/storage_lvm/#storage-lvmcluster)

-   [[ZFS - [`zfs`]]](../../reference/storage_zfs/#storage-zfs)

-   [[Ceph RBD - [`ceph`]]](../../reference/storage_ceph/#storage-ceph)

-   [[CephFS - [`cephfs`]]](../../reference/storage_cephfs/#storage-cephfs)

-   [[Ceph Object - [`cephobject`]]](../../reference/storage_cephobject/#storage-cephobject)

-   [[LINSTOR - [`linstor`]]](../../reference/storage_linstor/#storage-linstor)

-   [[TrueNAS - [`truenas`]]](../../reference/storage_truenas/#storage-truenas)

See the following how-to guides for additional information:

-   [How to manage storage pools](../../howto/storage_pools/#howto-storage-pools)

-   [How to create an instance in a specific storage pool](../../howto/storage_create_instance/#howto-storage-create-instance)

[]{#storage-location}

### Data storage location

Where the Incus data is stored depends on the configuration and the selected storage driver.
Depending on the storage driver that is used, Incus can either share the file system with its host or keep its data separate.

  Storage location           Directory   Btrfs   LVM (all)   ZFS   Ceph (all)   LINSTOR
  -------------------------- ----------- ------- ----------- ----- ------------ ---------
  Shared with the host       ✓           ✓       \-          ✓     \-           \-
  Dedicated disk/partition   \-          ✓       ✓           ✓     \-           ✓
  Loop disk                  \-          ✓       ✓           ✓     \-           ✓
  Remote storage             \-          \-      ✓           \-    ✓            ✓

#### Shared with the host

Sharing the file system with the host is usually the most space-efficient way to run Incus.
In most cases, it is also the easiest to manage.

This option is supported for the [`dir`] driver, the [`btrfs`] driver (if the host is Btrfs and you point Incus to a dedicated sub-volume) and the [`zfs`] driver (if the host is ZFS and you point Incus to a dedicated dataset on your zpool).

#### Dedicated disk or partition

Having Incus use an empty partition on your main disk or a full dedicated disk keeps its storage completely independent from the host.

This option is supported for the [`btrfs`] driver, the [`lvm`] driver, the [`zfs`] driver and the [`linstor`] driver.

#### Loop disk

Incus can create a loop file on your main drive and have the selected storage driver use that.
This method is functionally similar to using a disk or partition, but it uses a large file on your main drive instead.
This means that every write must go through the storage driver and your main drive's file system, which leads to decreased performance.

The loop files reside in [`/var/lib/incus/disks/`].

Loop files usually cannot be shrunk.
They will grow up to the configured limit, but deleting instances or images will not cause the file to shrink.
You can increase their size though; see [Resize a storage pool](../../howto/storage_pools/#storage-resize-pool).

#### Remote storage

The [`ceph`], [`cephfs`] and [`cephobject`] drivers store the data in a completely independent Ceph storage cluster that must be set up separately.
The [`lvmcluster`] driver relies on a shared block device being available to all cluster members and on a pre-existing [`lvmlockd`] setup.
The [`linstor`] driver stores the data in a LINSTOR storage cluster that must be setup separately.
The [`truenas`] driver stores the data on a TrueNAS storage server that must be setup separately.

[]{#storage-default-pool}

### Default storage pool

There is no concept of a default storage pool in Incus.

When you create a storage volume, you must specify the storage pool to use.

When Incus automatically creates a storage volume during instance creation, it uses the storage pool that is configured for the instance.
This configuration can be set in either of the following ways:

-   Directly on an instance: [[[`incus`]` `[`launch`]` `[`<image>`]` `[`<instance_name>`]` `[`--storage`]` `[`<storage_pool>`]]](../../reference/manpages/incus/launch/#incus-launch-md)

-   Through a profile: [[[`incus`]` `[`profile`]` `[`device`]` `[`add`]` `[`<profile_name>`]` `[`root`]` `[`disk`]` `[`path=/`]` `[`pool=<storage_pool>`]]](../../reference/manpages/incus/profile/device/add/#incus-profile-device-add-md) and [[[`incus`]` `[`launch`]` `[`<image>`]` `[`<instance_name>`]` `[`--profile`]` `[`<profile_name>`]]](../../reference/manpages/incus/launch/#incus-launch-md)

-   Through the default profile

In a profile, the storage pool to use is defined by the pool for the root disk device:

      root:
        type: disk
        path: /
        pool: default

In the default profile, this pool is set to the storage pool that was created during initialization.

[]{#id2}

## Storage volumes

When you create an instance, Incus automatically creates the required storage volumes for it.
You can create additional storage volumes.

See the following how-to guides for additional information:

-   [How to manage storage volumes](../../howto/storage_volumes/#howto-storage-volumes)

-   [How to move or copy storage volumes](../../howto/storage_move_volume/#howto-storage-move-volume)

-   [How to back up custom storage volumes](../../howto/storage_backup_volume/#howto-storage-backup-volume)

[]{#id3}

### Storage volume types

Storage volumes can be of the following types:

[`container`]/[`virtual-machine`]

:   Incus automatically creates one of these storage volumes when you launch an instance.
    It is used as the root disk for the instance, and it is destroyed when the instance is deleted.

    This storage volume is created in the storage pool that is specified in the profile used when launching the instance (or the default profile, if no profile is specified).
    The storage pool can be explicitly specified by providing the [`--storage`] flag to the launch command.

[`image`]

:   Incus automatically creates one of these storage volumes when it unpacks an image to launch one or more instances from it.
    You can delete it after the instance has been created.
    If you do not delete it manually, it is deleted automatically ten days after it was last used to launch an instance.

    The image storage volume is created in the same storage pool as the instance storage volume, and only for storage pools that use a [storage driver](../../reference/storage_drivers/#storage-drivers) that supports optimized image storage.

[`custom`]

:   You can add one or more custom storage volumes to hold data that you want to store separately from your instances.
    Custom storage volumes can be shared between instances, and they are retained until you delete them.

    You can also use custom storage volumes to hold your backups or images.

    You must specify the storage pool for the custom volume when you create it.

[]{#storage-content-types}

### Content types

Each storage volume uses one of the following content types:

[`filesystem`]

:   This content type is used for containers and container images.
    It is the default content type for custom storage volumes.

    Custom storage volumes of content type [`filesystem`] can be attached to both containers and virtual machines, and they can be shared between instances.

[`block`]

:   This content type is used for virtual machines and virtual machine images.
    You can create a custom storage volume of type [`block`] by using the [`--type=block`] flag.

    Custom storage volumes of content type [`block`] can only be attached to virtual machines.
    They should not be shared between instances, because simultaneous access can lead to data corruption.

[`iso`]

:   This content type is used for custom ISO volumes.
    A custom storage volume of type [`iso`] can only be created by importing an ISO file using [[[`incus`]` `[`storage`]` `[`volume`]` `[`import`]]](../../reference/manpages/incus/storage/volume/import/#incus-storage-volume-import-md).

    Custom storage volumes of content type [`iso`] can only be attached to virtual machines.
    They can be attached to multiple machines simultaneously as they are always read-only.

[]{#id4}

## Storage buckets

Storage buckets provide object storage functionality via the S3 protocol.

They can be used in a way that is similar to custom storage volumes.
However, unlike storage volumes, storage buckets are not attached to an instance.
Instead, applications can access a storage bucket directly using its URL.

Each storage bucket is assigned one or more access keys, which the applications must use to access it.

Storage buckets can be located on local storage (with [`dir`], [`btrfs`], [`lvm`] or [`zfs`] pools) or on remote storage (with [`cephobject`] pools).

To enable storage buckets for local storage pool drivers and allow applications to access the buckets via the S3 protocol, you must configure the [`core.storage_buckets_address`](../../server_config/#server-core:core.storage_buckets_address) server setting.

See the following how-to guide for additional information:

-   [How to manage storage buckets and keys](../../howto/storage_buckets/#howto-storage-buckets)
