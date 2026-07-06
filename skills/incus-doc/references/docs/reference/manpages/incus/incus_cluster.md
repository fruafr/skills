(incus_cluster.md)=
# `incus cluster`

Manage cluster members

## Synopsis
```{line-block}

Description:
  Manage cluster members



```
```
incus cluster [flags]
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
* [incus cluster add](incus_cluster_add.md)	 - Request a join token for adding a cluster member
* [incus cluster edit](incus_cluster_edit.md)	 - Edit cluster member configurations as YAML
* [incus cluster enable](incus_cluster_enable.md)	 - Enable clustering on a single non-clustered server
* [incus cluster evacuate](incus_cluster_evacuate.md)	 - Evacuate cluster member
* [incus cluster get](incus_cluster_get.md)	 - Get values for cluster member configuration keys
* [incus cluster group](incus_cluster_group.md)	 - Manage cluster groups
* [incus cluster info](incus_cluster_info.md)	 - Show useful information about a cluster member
* [incus cluster join](incus_cluster_join.md)	 - Join an existing server to a cluster
* [incus cluster list](incus_cluster_list.md)	 - List all the cluster members
* [incus cluster list-tokens](incus_cluster_list-tokens.md)	 - List all active cluster member join tokens
* [incus cluster remove](incus_cluster_remove.md)	 - Remove a member from the cluster
* [incus cluster rename](incus_cluster_rename.md)	 - Rename a cluster member
* [incus cluster restore](incus_cluster_restore.md)	 - Restore cluster member
* [incus cluster revoke-token](incus_cluster_revoke-token.md)	 - Revoke cluster member join token
* [incus cluster role](incus_cluster_role.md)	 - Manage cluster roles
* [incus cluster set](incus_cluster_set.md)	 - Set a cluster member's configuration keys
* [incus cluster show](incus_cluster_show.md)	 - Show details of a cluster member
* [incus cluster unset](incus_cluster_unset.md)	 - Unset a cluster member's configuration keys
* [incus cluster update-certificate](incus_cluster_update-certificate.md)	 - Update cluster certificate

```{toctree}
:titlesonly:
:glob:
:hidden:

cluster/*
```
