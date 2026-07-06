(incus_monitor.md)=
# `incus monitor`

Monitor a local or remote server

## Synopsis
```{line-block}

Description:
  Monitor a local or remote server

  By default the monitor will listen to all message types.



```
```
incus monitor [<remote>:] [flags]
```

## Examples

```
  incus monitor --type=logging
      Only show log messages.

  incus monitor --pretty --type=logging --loglevel=info
      Show a pretty log of messages with info level or higher.

  incus monitor --type=lifecycle
      Only show lifecycle events.
```

## Options

```
      --all-projects   Show events from all projects
  -f, --format         Format (json|pretty|yaml) (default "yaml")
      --loglevel       Minimum level for log messages (only available when using pretty format)
      --pretty         Pretty rendering (short for --format=pretty)
  -t, --type           Event type to listen for
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

