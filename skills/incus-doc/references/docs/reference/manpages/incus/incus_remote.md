(incus_remote.md)=
# `incus remote`

Manage the list of remote servers

## Synopsis
```{line-block}

Description:
  Manage the list of remote servers



```
```
incus remote [flags]
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
* [incus remote add](incus_remote_add.md)	 - Add new remote servers
* [incus remote generate-certificate](incus_remote_generate-certificate.md)	 - Generate the client certificate
* [incus remote get-client-certificate](incus_remote_get-client-certificate.md)	 - Print or retrieve the client certificate used by this Incus client
* [incus remote get-client-token](incus_remote_get-client-token.md)	 - Generate a client token derived from the client certificate
* [incus remote get-default](incus_remote_get-default.md)	 - Show the default remote
* [incus remote list](incus_remote_list.md)	 - List the available remotes
* [incus remote proxy](incus_remote_proxy.md)	 - Run a local API proxy
* [incus remote remove](incus_remote_remove.md)	 - Remove remotes
* [incus remote rename](incus_remote_rename.md)	 - Rename remotes
* [incus remote set-keepalive](incus_remote_set-keepalive.md)	 - Set a keepalive timeout for a remote
* [incus remote set-urls](incus_remote_set-urls.md)	 - Set the URL(s) for the remote
* [incus remote switch](incus_remote_switch.md)	 - Switch the default remote

```{toctree}
:titlesonly:
:glob:
:hidden:

remote/*
```
