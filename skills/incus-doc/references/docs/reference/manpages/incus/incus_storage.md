(incus_storage.md)=
# `incus storage`

Manage storage pools and volumes

## Synopsis
```{line-block}

Description:
  Manage storage pools and volumes



```
```
incus storage [flags]
```

## Options inherited from parent commands

```
      --debug          Show all debug messages
      --explain        If the command is valid, explain its parsed arguments instead of running it
      --force-local    Force using the local unix socket
  -h, --help           Print help
      --project        Override the source project
  -q, --quiet          Don't show progress information
      --sub-commands   Use with help or --help to view sub-commands
  -v, --verbose        Show all information messages
      --version        Print version number
```

## SEE ALSO

* [incus](incus.md)	 - Command line client for Incus
* [incus storage bucket](incus_storage_bucket.md)	 - Manage storage buckets
* [incus storage create](incus_storage_create.md)	 - Create storage pools
* [incus storage delete](incus_storage_delete.md)	 - Delete storage pools
* [incus storage edit](incus_storage_edit.md)	 - Edit storage pool configurations as YAML
* [incus storage get](incus_storage_get.md)	 - Get values for storage pool configuration keys
* [incus storage info](incus_storage_info.md)	 - Show useful information about storage pools
* [incus storage list](incus_storage_list.md)	 - List available storage pools
* [incus storage set](incus_storage_set.md)	 - Set storage pool configuration keys
* [incus storage show](incus_storage_show.md)	 - Show storage pool configurations and resources
* [incus storage unset](incus_storage_unset.md)	 - Unset storage pool configuration keys
* [incus storage volume](incus_storage_volume.md)	 - Manage storage volumes

```{toctree}
:titlesonly:
:glob:
:hidden:

storage/*
```
