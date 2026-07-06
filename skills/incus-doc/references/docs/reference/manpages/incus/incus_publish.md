(incus_publish.md)=
# `incus publish`

Publish instances as images

## Synopsis
```{line-block}

Description:
  Publish instances as images



```
```
incus publish [<remote>:]<instance>[/<snapshot>] [<remote>:] [<key>=<value>...] [flags]
```

## Options

```
      --alias         New alias to define at target
      --compression   Compression algorithm to use (`none` for uncompressed)
      --expire        Image expiration date (format: rfc3339)
  -f, --force         Stop the instance if currently running
      --format        Image format (default "unified")
      --public        Make the image public
      --reuse         If the image alias already exists, delete and create a new one
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

