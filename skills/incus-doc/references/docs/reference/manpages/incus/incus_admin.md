(incus_admin.md)=
# `incus admin`

Manage incus daemon

## Synopsis
```{line-block}

Description:
  Manage incus daemon



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
* [incus admin cluster](incus_admin_cluster.md)	 - Low-level cluster administration commands
* [incus admin init](incus_admin_init.md)	 - Configure the daemon
* [incus admin os](incus_admin_os.md)	 - Manage IncusOS systems
* [incus admin recover](incus_admin_recover.md)	 - Recover missing instances and volumes from existing and unknown storage pools
* [incus admin shutdown](incus_admin_shutdown.md)	 - Tell the daemon to shutdown all instances and exit
* [incus admin sql](incus_admin_sql.md)	 - Execute a SQL query against the local or global database
* [incus admin update-certificate](incus_admin_update-certificate.md)	 - Update the server certificate
* [incus admin waitready](incus_admin_waitready.md)	 - Wait for the daemon to be ready to process requests

```{toctree}
:titlesonly:
:glob:
:hidden:

admin/*
```
