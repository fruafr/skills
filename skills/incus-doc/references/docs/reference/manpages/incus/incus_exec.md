(incus_exec.md)=
# `incus exec`

Execute commands in instances

## Synopsis
```{line-block}

Description:
  Execute commands in instances

  The command is executed directly using exec, so there is no shell and
  shell patterns (variables, file redirects, ...) won't be understood.
  If you need a shell environment you need to execute the shell
  executable, passing the shell commands as arguments, for example:

    incus exec <instance> -- sh -c "cd /tmp && pwd"

  Mode defaults to non-interactive, interactive mode is selected if both stdin AND stdout are terminals (stderr is ignored).



```
```
incus exec [<remote>:]<instance> [flags] [--] <command-line argument>...
```

## Examples

```
  incus exec c1 bash
  	Run the "bash" command in instance "c1"

  incus exec c1 -- ls -lh /
  	Run the "ls -lh /" command in instance "c1"
```

## Options

```
      --cwd                    Directory to run the command in (default /root)
  -n, --disable-stdin          Disable stdin (reads from /dev/null)
      --env                    Environment variable to set (e.g. HOME=/home/foo)
  -t, --force-interactive      Force pseudo-terminal allocation
  -T, --force-noninteractive   Disable pseudo-terminal allocation
      --group                  Group ID to run the command as (default 0)
      --mode                   Override the terminal mode (auto, interactive or non-interactive) (default "auto")
      --user                   User ID to run the command as (default 0)
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

