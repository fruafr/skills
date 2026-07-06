# How to add remote servers

Remote servers are a concept in the Incus command-line client.
By default, the command-line client interacts with the local Incus daemon, but you can add other servers or clusters to interact with.

One use case for remote servers is to distribute images that can be used to create instances on local servers.
See [Default image server](../reference/image_servers/#image-servers) for more information.

You can also add a full Incus server as a remote server to your client.
In this case, you can interact with the remote server in the same way as with your local daemon.
For example, you can manage instances or update the server configuration on the remote server.

## Authentication

To be able to add an Incus server as a remote server, the server's API must be exposed, which means that its [`core.https_address`](../server_config/#server-core:core.https_address) server configuration option must be set.

When adding the server, you must then authenticate with it using the chosen method for [Remote API authentication](../authentication/#authentication).

See [How to expose Incus to the network](../howto/server_expose/#server-expose) for more information.

## List configured remotes

To see all configured remote servers, enter the following command:

    incus remote list

Remote servers that use the [simple streams format](https://git.launchpad.net/simplestreams/tree/) are pure image servers.
Servers that use the [`incus`] format are Incus servers, which either serve solely as image servers or might provide some images in addition to serving as regular Incus servers.
See [Image server types](../reference/image_servers/#image-server-types) for more information.

## Add a remote Incus server

To add an Incus server as a remote, enter the following command:

    incus remote add <remote_name> <IP|FQDN|URL> [flags]

Some authentication methods require specific flags (for example, use [[[`incus`]` `[`remote`]` `[`add`]` `[`<remote_name>`]` `[`<IP|FQDN|URL>`]` `[`--auth-type=oidc`]]](../reference/manpages/incus/remote/add/#incus-remote-add-md) for OIDC authentication).
See [Authenticate with the Incus server](../howto/server_expose/#server-authenticate) and [Remote API authentication](../authentication/#authentication) for more information.

For example, enter the following command to add a remote through an IP address:

    incus remote add my-remote 192.0.2.10

You are prompted to confirm the remote server fingerprint and then asked for the token.

## Select a default remote

The Incus command-line client is pre-configured with the [`local`] remote, which is the local Incus daemon.

To select a different remote as the default remote, enter the following command:

    incus remote switch <remote_name>

To see which server is configured as the default remote, enter the following command:

    incus remote get-default

## Configure a global remote

You can configure remotes on a global, per-system basis.
These remotes are available for every user of the Incus server for which you add the configuration.

Users can override these system remotes (for example, by running [[[`incus`]` `[`remote`]` `[`rename`]]](../reference/manpages/incus/remote/rename/#incus-remote-rename-md) or [[[`incus`]` `[`remote`]` `[`set-urls`]]](../reference/manpages/incus/remote/set-urls/#incus-remote-set-urls-md)), which results in the remote and its associated certificates being copied to the user configuration.

To configure a global remote, create or edit a [`config.yml`] file that is located in [`/etc/incus/`].

Certificates for the remotes must be stored in the [`servercerts`] directory in the same location (for example, [`/etc/incus/servercerts/`]).
They must match the remote name (for example, [`foo.crt`]).

It's also possible to provide per-remote client certificates by placing them in the [`clientcerts`] directory.
The similarly must match the remote name (for example, [`foo.crt`] and [`foo.key`]).

See the following example configuration:

    remotes:
      foo:
        addr: https://192.0.2.4:8443
        auth_type: tls
        project: default
        protocol: incus
        public: false
      bar:
        addr: https://192.0.2.5:8443
        auth_type: tls
        project: default
        protocol: incus
        public: false

[]{#remote-keepalive}

## Enabling [`keepalive`]

For those frequently interacting with a particular remote, it's possible to enable a new [`keepalive`] mode.

When enabled, Incus will maintain a connection with the target server for up to the configured timeout.
This can significantly reduce the latency when running many [`incus`] commands.

To enable, use the [`incus`]` `[`remote`]` `[`set-keepalive`] command providing the timeout for the remote.

    incus remote set-keepalive my-remote 30

In this example, a timeout of 30 seconds was used.

Alternatively, you can add the remote with the feature enabled with the flag [`--keepalive`].

    incus remote add my-remote 192.0.2.10 --keepalive 30

To disable the [`keepalive`], run [`incus`]` `[`remote`]` `[`set-keepalive`] with timeout value 0.

    incus remote set-keepalive my-remote 0
