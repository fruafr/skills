(incus_rebuild.md)=
# `incus rebuild`

Rebuild instances

## Synopsis
```{line-block}

Description:
  Wipe the instance root disk and re-initialize with a new image (or empty volume).



```
```
incus rebuild (--empty|[<remote>:]<image>) [<remote>:]<instance> [flags]
```

## Options

```
      --empty   Rebuild as an empty instance
  -f, --force   If an instance is running, stop it and then rebuild it
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

