[]{#instances-backup}

# How to back up instances

There are different ways of backing up your instances:

-   [Use snapshots for instance backup](#instances-snapshots)

-   [Use export files for instance backup](#instances-backup-export)

-   [Copy an instance to a backup server](#instances-backup-copy)

Which method to choose depends both on your use case and on the storage driver you use.

In general, snapshots are quick and space efficient (depending on the storage driver), but they are stored in the same storage pool as the instance and therefore not too reliable.
Export files can be stored on different disks and are therefore more reliable.
They can also be used to restore the instance into a different storage pool.
If you have a separate, network-connected Incus server available, regularly copying instances to this other server gives high reliability as well, and this method can also be used to back up snapshots of the instance.

**Note**

Custom storage volumes might be attached to an instance, but they are not part of the instance.
Therefore, the content of a custom storage volume is not stored when you back up your instance.
You must back up the data of your storage volume separately.
See [How to back up custom storage volumes](../storage_backup_volume/#howto-storage-backup-volume) for instructions.

[]{#instances-snapshots}

## Use snapshots for instance backup

You can save your instance at a point in time by creating an instance snapshot, which makes it easy to restore the instance to a previous state.

Instance snapshots are stored in the same storage pool as the instance volume itself.

Most storage drivers support optimized snapshot creation (see [Feature comparison](../../reference/storage_drivers/#storage-drivers-features)).
For these drivers, creating snapshots is both quick and space-efficient.
For the [`dir`] driver, snapshot functionality is available but not very efficient.
For the [`lvm`] driver, snapshot creation is quick, but restoring snapshots is efficient only when using thin-pool mode.

### Create a snapshot

Use the following command to create a snapshot of an instance:

    incus snapshot create <instance_name> [<snapshot name>]

Add the [`--reuse`] flag in combination with a snapshot name to replace an existing snapshot.

By default, snapshots are kept forever, unless the [`snapshots.expiry`] configuration option is set.
To retain a specific snapshot even if a general expiry time is set, use the [`--no-expiry`] flag.

For virtual machines, you can add the [`--stateful`] flag to capture not only the data included in the instance volume but also the running state of the instance.
Note that this feature is not fully supported for containers because of CRIU limitations.

### View, edit or delete snapshots

Use the following command to display the snapshots for an instance:

    incus info <instance_name>

You can view or modify snapshots in a similar way to instances, by referring to the snapshot with [`<instance_name>/<snapshot_name>`].

To show configuration information about a snapshot, use the following command:

    incus config show <instance_name>/<snapshot_name>

To change the expiry date of a snapshot, use the following command:

    incus config edit <instance_name>/<snapshot_name>

**Note**

In general, snapshots cannot be edited, because they preserve the state of the instance.
The only exception is the expiry date.
Other changes to the configuration are silently ignored.

To delete a snapshot, use the following command:

    incus snapshot delete <instance_name> <snapshot_name>

### Schedule instance snapshots

You can configure an instance to automatically create snapshots at specific times (at most once every minute).
To do so, set the [`snapshots.schedule`](../../reference/instance_options/#instance-snapshots:snapshots.schedule) instance option.

For example, to configure daily snapshots, use the following command:

    incus config set <instance_name> snapshots.schedule=@daily

To configure taking a snapshot every day at 6 am, use the following command:

    incus config set <instance_name> snapshots.schedule="0 6 * * *"

When scheduling regular snapshots, consider setting an automatic expiry ([`snapshots.expiry`](../../reference/instance_options/#instance-snapshots:snapshots.expiry)) and a naming pattern for snapshots ([`snapshots.pattern`](../../reference/instance_options/#instance-snapshots:snapshots.pattern)).
You should also configure whether you want to take snapshots of instances that are not running ([`snapshots.schedule.stopped`](../../reference/instance_options/#instance-snapshots:snapshots.schedule.stopped)).

### Restore an instance snapshot

You can restore an instance to any of its snapshots.

To do so, use the following command:

    incus snapshot restore <instance_name> <snapshot_name>

If the snapshot is stateful (which means that it contains information about the running state of the instance), you can add the [`--stateful`] flag to restore the state.

[]{#instances-backup-export}

## Use export files for instance backup

You can export the full content of your instance to a standalone file that can be stored at any location.
For highest reliability, store the backup file on a different file system to ensure that it does not get lost or corrupted.

### Export an instance

Use the following command to export an instance to a compressed file (for example, [`/path/to/my-instance.tgz`]):

    incus export <instance_name> [<file_path>]

If you do not specify a file path, the export file is saved as [`<instance_name>.<extension>`] in the working directory (for example, [`my-container.tar.gz`]).

You can add any of the following flags to the command:

[`--compression`]

:   By default, the output file uses [`gzip`] compression.
    You can specify a different compression algorithm (for example, [`bzip2`]) or turn off compression with [`--compression=none`].

[`--optimized-storage`]

:   If your storage pool uses the [`btrfs`] or the [`zfs`] driver, add the [`--optimized-storage`] flag to store the data as a driver-specific binary blob instead of an archive of individual files.
    In this case, the export file can only be used with pools that use the same storage driver.

    Exporting a volume in optimized mode is usually quicker than exporting the individual files.
    Snapshots are exported as differences from the main volume, which decreases their size and makes them easily accessible.

```{=html}
<!-- -->
```

[`--instance-only`]

:   By default, the export file contains all snapshots of the instance.
    Add this flag to export the instance without its snapshots.

### Restore an instance from an export file

You can import an export file (for example, [`/path/to/my-backup.tgz`]) as a new instance.
To do so, use the following command:

    incus import <file_path> [<instance_name>]

If you do not specify an instance name, the original name of the exported instance is used for the new instance.
If an instance with that name already (or still) exists in the specified storage pool, the command returns an error.
In that case, either delete the existing instance before importing the backup or specify a different instance name for the import.

[]{#instances-backup-copy}

## Copy an instance to a backup server

You can copy an instance to a secondary backup server to back it up.

See [How to move existing Incus instances between servers](../move_instances/#move-instances) for instructions.
