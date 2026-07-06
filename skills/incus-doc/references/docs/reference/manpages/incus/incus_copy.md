(incus_copy.md)=
# `incus copy`

Copy instances within or in between servers

## Synopsis
```{line-block}

Description:
  Copy instances within or in between servers

  Transfer modes (--mode):
   - pull: Target server pulls the data from the source server (source must listen on network)
   - push: Source server pushes the data to the target server (target must listen on network)
   - relay: The CLI connects to both source and server and proxies the data (both source and target must listen on network)

  The pull transfer mode is the default as it is compatible with all server versions.




```
```
incus copy [<remote>:]<instance>[/<snapshot>] [<remote>:][<new instance name>] [flags]
```

## Options

```
      --allow-inconsistent      Ignore copy errors for volatile files
  -c, --config                  Config key/value to apply to the new instance
  -d, --device                  New key/value to apply to a specific device
  -e, --ephemeral               Ephemeral instance
      --instance-only           Copy the instance without its snapshots
      --mode                    Transfer mode. One of pull, push or relay (default "pull")
      --no-profiles             Create the instance with no profiles applied
  -p, --profile                 Profile to apply to the new instance
      --refresh                 Perform an incremental copy
      --refresh-exclude-older   During incremental copy, exclude source snapshots earlier than latest target snapshot
      --stateless               Copy a stateful instance stateless
  -s, --storage                 Storage pool name
      --target                  Cluster member name
      --target-project          Copy to a project different from the source
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

