(incus_info.md)=
# `incus info`

Show instance or server information

## Synopsis
```{line-block}

Description:
  Show instance or server information



```
```
incus info [<remote>:][<instance>] [flags]
```

## Examples

```
  incus info [<remote>:]<instance> [--show-log]
      For instance information.

  incus info [<remote>:] [--resources]
      For server information.
```

## Options

```
      --resources              Show the resources available to the server
      --show-access            Show the instance's access list
      --show-log[="default"]   Show the instance's recent log entries
      --show-sensitive         Show the server's sensitive information (full certificates, private keys and the API extension list)
      --target                 Cluster member name
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

