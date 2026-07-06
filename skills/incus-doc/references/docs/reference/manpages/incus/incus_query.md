(incus_query.md)=
# `incus query`

Send a raw query to the server

## Synopsis
```{line-block}

Description:
  Send a raw query to the server



```
```
incus query [<remote>:]<API path> [flags]
```

## Examples

```
  incus query -X DELETE --wait /1.0/instances/c1
      Delete local instance "c1".
```

## Options

```
  -d, --data      Input data
      --raw       Print the raw response
  -X, --request   Action (default "GET")
      --wait      Wait for the operation to complete
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

