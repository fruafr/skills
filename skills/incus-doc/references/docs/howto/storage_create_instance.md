[]{#howto-storage-create-instance}

# How to create an instance in a specific storage pool

Instance storage volumes are created in the storage pool that is specified by the instance's root disk device.
This configuration is normally provided by the profile or profiles applied to the instance.
See [Default storage pool](../../explanation/storage/#storage-default-pool) for detailed information.

To use a different storage pool when creating or launching an instance, add the [`--storage`] flag.
This flag overrides the root disk device from the profile.
For example:

    incus launch <image> <instance_name> --storage <storage_pool>

## Move instance storage volumes to another pool

To move an instance storage volume to another storage pool, make sure the instance is stopped.
Then use the following command to move the instance to a different pool:

    incus move <instance_name> --storage <target_pool_name>
