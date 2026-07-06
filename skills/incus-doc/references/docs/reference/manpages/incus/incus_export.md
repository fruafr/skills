(incus_export.md)=
# `incus export`

Export instance backups

## Synopsis
```{line-block}

Description:
  Export instances as backup tarballs.



```
```
incus export [<remote>:]<instance> [<target file>] [flags]
```

## Examples

```
  incus export u1 backup0.tar.gz
  	Download a backup tarball of the u1 instance.

  incus export u1 -
  	Download a backup tarball with it written to the standard output.
```

## Options

```
      --compression         Compression algorithm to use (none for uncompressed)
  -f, --force               Force overwriting existing backup file
      --instance-only       Whether or not to only backup the instance (without snapshots)
      --optimized-storage   Use storage driver optimized format (can only be restored on a similar pool)
      --root-only           Whether or not to only backup the instance (without dependent volumes)
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

