(incus_create.md)=
# `incus create`

Create instances from images

## Synopsis
```{line-block}

Description:
  Create instances from images



```
```
incus create (--empty|[<remote>:]<image>) [<remote>:][<new instance name>] [flags]
```

## Examples

```
  incus create images:debian/12 u1
      Create the instance u1

  incus create images:debian/12 u1 < config.yaml
      Create the instance with configuration from config.yaml

  incus launch images:debian/12 v2 --vm -d root,size=50GiB -d root,io.bus=nvme
      Create and start a virtual machine, overriding the disk size and bus
```

## Options

```
  -c, --config             Config key/value to apply to the new instance
      --description        Instance description
  -d, --device             New key/value to apply to a specific device
      --empty              Create an empty instance
      --environment-file   Include environment variables from file
  -e, --ephemeral          Ephemeral instance
  -n, --network            Network name
      --no-profiles        Create the instance with no profiles applied
  -p, --profile            Profile to apply to the new instance
  -s, --storage            Storage pool name
      --target             Cluster member name
  -t, --type               Instance type
      --vm                 Create a virtual machine
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

