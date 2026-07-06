[]{#server}

# Server configuration

The Incus server can be configured through a set of key/value configuration options.

The key/value configuration is namespaced.
The following options are available:

-   [Core configuration](#server-options-core)

-   [ACME configuration](#server-options-acme)

-   [Cluster configuration](#server-options-cluster)

-   [Images configuration](#server-options-images)

-   [Logging configuration](#server-options-logging)

-   [Miscellaneous options](#server-options-misc)

-   [OpenID Connect configuration](#server-options-oidc)

-   [OpenFGA configuration](#server-options-openfga)

See [How to configure the Incus server](../howto/server_configure/#server-configure) for instructions on how to set the configuration options.

**Note**

Options marked with a [`global`] scope are immediately applied to all cluster members.
Options with a [`local`] scope must be set on a per-member basis.

[]{#server-options-core}

## Core configuration

The following server options control the core daemon configuration:

[[`core.bgp_address`]][]

Address to bind the BGP server to

[[*!*](#server-core:core.bgp_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.bgp_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [How to configure Incus as a BGP server](../howto/network_bgp/#network-bgp).

[[`core.bgp_asn`]][]

BGP Autonomous System Number for the local server

[[*!*](#server-core:core.bgp_asn)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.bgp_asn`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.bgp_routerid`]][]

A unique identifier for the BGP server

[[*!*](#server-core:core.bgp_routerid)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.bgp_routerid`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

The identifier must be formatted as an IPv4 address.

[[`core.debug_address`]][]

Address to bind the [`pprof`] debug server to (HTTP)

[[*!*](#server-core:core.debug_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.debug_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

[[`core.dns_address`]][]

Address to bind the authoritative DNS server to

[[*!*](#server-core:core.dns_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.dns_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [Enable the built-in DNS server](../howto/network_zones/#network-dns-server).

[[`core.https_address`]][]

Address to bind for the remote API (HTTPS)

[[*!*](#server-core:core.https_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.https_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [How to expose Incus to the network](../howto/server_expose/#server-expose).

[[`core.https_allowed_credentials`]][]

Whether to set [`Access-Control-Allow-Credentials`]

[[*!*](#server-core:core.https_allowed_credentials)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.htt                        |
|                                   | ps_allowed_credentials`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

If enabled, the [`Access-Control-Allow-Credentials`] HTTP header value is set to [`true`].

[[`core.https_allowed_headers`]][]

[`Access-Control-Allow-Headers`] HTTP header value

[[*!*](#server-core:core.https_allowed_headers)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core                            |
|                                   | .https_allowed_headers`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.https_allowed_methods`]][]

[`Access-Control-Allow-Methods`] HTTP header value

[[*!*](#server-core:core.https_allowed_methods)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core                            |
|                                   | .https_allowed_methods`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.https_allowed_origin`]][]

[`Access-Control-Allow-Origin`] HTTP header value

[[*!*](#server-core:core.https_allowed_origin)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`cor                             |
|                                   | e.https_allowed_origin`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.https_allowed_websocket_origin`]][]

Allowed [`Origin`] values for WebSocket connections

[[*!*](#server-core:core.https_allowed_websocket_origin)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.https_al                   |
|                                   | lowed_websocket_origin`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.https_trusted_proxy`]][]

Trusted servers to provide the client's address

[[*!*](#server-core:core.https_trusted_proxy)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`co                              |
|                                   | re.https_trusted_proxy`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify a comma-separated list of IP addresses of trusted servers that provide the client's address through the proxy connection header.

[[`core.metrics_address`]][]

Address to bind the metrics server to (HTTPS)

[[*!*](#server-core:core.metrics_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.metrics_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [How to monitor metrics](../metrics/#metrics).

[[`core.metrics_authentication`]][]

Whether to enforce authentication on the metrics endpoint

[[*!*](#server-core:core.metrics_authentication)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.                           |
|                                   | metrics_authentication`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`true`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.proxy_http`]][]

HTTP proxy to use

[[*!*](#server-core:core.proxy_http)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.proxy_http`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

If this option is not specified, the daemon falls back to the [`HTTP_PROXY`] environment variable (if set).

[[`core.proxy_https`]][]

HTTPS proxy to use

[[*!*](#server-core:core.proxy_https)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.proxy_https`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

If this option is not specified, the daemon falls back to the [`HTTPS_PROXY`] environment variable (if set).

[[`core.proxy_ignore_hosts`]][]

Hosts that don't need the proxy

[[*!*](#server-core:core.proxy_ignore_hosts)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`c                               |
|                                   | ore.proxy_ignore_hosts`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify this option in a similar format to [`NO_PROXY`] (for example, [`1.2.3.4,1.2.3.5`])

If this option is not specified, the daemon falls back to the [`NO_PROXY`] environment variable (if set).

[[`core.remote_token_expiry`]][]

Time after which a remote add token expires

[[*!*](#server-core:core.remote_token_expiry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`co                              |
|                                   | re.remote_token_expiry`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | no expiry                         |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`core.shutdown_action`]][]

Action to perform on server shutdown

[[*!*](#server-core:core.shutdown_action)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.shutdown_action`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`shutdown`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the action to take when the daemon is being shut down.
Supported values are [`shutdown`] (stop all instances) and [`evacuate`] (attempt to evacuate the clustered server).

[[`core.shutdown_timeout`]][]

How long to wait before shutdown

[[*!*](#server-core:core.shutdown_timeout)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `core.shutdown_timeout`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`5`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the number of minutes to wait for running operations to complete before the daemon shuts down.

[[`core.storage_buckets_address`]][]

Address to bind the storage object server to (HTTPS)

[[*!*](#server-core:core.storage_buckets_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.s                          |
|                                   | torage_buckets_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [How to manage storage buckets and keys](../howto/storage_buckets/#howto-storage-buckets).

[[`core.syslog_socket`]][]

Whether to enable the syslog unixgram socket listener

[[*!*](#server-core:core.syslog_socket)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core.syslog_socket`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

Set this option to [`true`] to enable the syslog unixgram socket to receive log messages from external processes.

[[`core.trust_ca_certificates`]][]

Whether to automatically trust clients signed by the CA

[[*!*](#server-core:core.trust_ca_certificates)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`core                            |
|                                   | .trust_ca_certificates`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[]{#server-options-acme}

## ACME configuration

The following server options control the [ACME](../authentication/#authentication-server-certificate) configuration:

[[`acme.agree_tos`]][]

Agree to ACME terms of service

[[*!*](#server-acme:acme.agree_tos)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.agree_tos`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.ca_url`]][]

URL to the directory resource of the ACME service

[[*!*](#server-acme:acme.ca_url)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.ca_url`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`https://acme-v02.api.let        |
|                                   | sencrypt.org/directory`]     |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.challenge`]][]

ACME challenge type to use

[[*!*](#server-acme:acme.challenge)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.challenge`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`HTTP-01`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Possible values are [`DNS-01`] and [`HTTP-01`].

[[`acme.domain`]][]

Domain for which the certificate is issued

[[*!*](#server-acme:acme.domain)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.domain`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.email`]][]

Email address used for the account registration

[[*!*](#server-acme:acme.email)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.email`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.http.port`]][]

Port and interface for HTTP server (used by HTTP-01)

[[*!*](#server-acme:acme.http.port)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.http.port`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`:80`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Set the port and interface to use for HTTP-01 based challenges to listen on

[[`acme.provider`]][]

Backend provider for the challenge (used by DNS-01)

[[*!*](#server-acme:acme.provider)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acme.provider`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | \`\`                              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.provider.environment`]][]

Environment variables to set during the challenge (used by DNS-01)

[[*!*](#server-acme:acme.provider.environment)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`acm                             |
|                                   | e.provider.environment`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | \`\`                              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`acme.provider.resolvers`]][]

Comma-separated list of DNS resolvers (used by DNS-01)

[[*!*](#server-acme:acme.provider.resolvers)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`a                               |
|                                   | cme.provider.resolvers`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | \`\`                              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

DNS resolvers to use for performing (recursive) [`CNAME`] resolving and apex domain determination during DNS-01 challenge.

[]{#server-options-oidc}

## OpenID Connect configuration

The following server options configure external user authentication through [OpenID Connect authentication](../authentication/#authentication-openid):

[[`oidc.audience`]][]

Expected audience value for the application

[[*!*](#server-oidc:oidc.audience)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`oidc.audience`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

This value is required by some providers.

[[`oidc.claim`]][]

OpenID Connect claim to use as the username

[[*!*](#server-oidc:oidc.claim)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`oidc.claim`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Note that the claim must be contained in the access token.

[[`oidc.client.id`]][]

OpenID Connect client ID

[[*!*](#server-oidc:oidc.client.id)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`oidc.client.id`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`oidc.issuer`]][]

OpenID Connect Discovery URL for the provider

[[*!*](#server-oidc:oidc.issuer)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`oidc.issuer`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`oidc.scopes`]][]

Comma separated list of OpenID Connect scopes

[[*!*](#server-oidc:oidc.scopes)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`oidc.scopes`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[]{#server-options-openfga}

## OpenFGA configuration

The following server options configure external user authorization through [Open Fine-Grained Authorization (OpenFGA)](../authorization/#authorization-openfga):

[[`openfga.api.token`]][]

API token of the OpenFGA server

[[*!*](#server-openfga:openfga.api.token)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`openfga.api.token`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`openfga.api.url`]][]

URL of the OpenFGA server

[[*!*](#server-openfga:openfga.api.url)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`openfga.api.url`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`openfga.store.id`]][]

ID of the OpenFGA permission store

[[*!*](#server-openfga:openfga.store.id)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`openfga.store.id`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[]{#server-options-cluster}

## Cluster configuration

The following server options control [Clustering](../clustering/#clustering):

[[`cluster.healing_threshold`]][]

Threshold when to evacuate an offline cluster member

[[*!*](#server-cluster:cluster.healing_threshold)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clu                             |
|                                   | ster.healing_threshold`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`0`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the number of seconds after which an offline cluster member is to be evacuated.
To disable evacuating offline members, set this option to [`0`].

[[`cluster.https_address`]][]

Address to use for clustering traffic

[[*!*](#server-cluster:cluster.https_address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `cluster.https_address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

See [Separate REST API and clustering networks](../howto/cluster_config_networks/#cluster-https-address).

[[`cluster.images_minimal_replica`]][]

Number of cluster members that replicate an image

[[*!*](#server-cluster:cluster.images_minimal_replica)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`cluster.                        |
|                                   | images_minimal_replica`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`3`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the minimal number of cluster members that keep a copy of a particular image.
Set this option to [`1`] for no replication, or to [`-1`] to replicate images on all members.

[[`cluster.join_token_expiry`]][]

Time after which a cluster join token expires

[[*!*](#server-cluster:cluster.join_token_expiry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clu                             |
|                                   | ster.join_token_expiry`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`3H`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`cluster.max_standby`]][]

Number of database stand-by members

[[*!*](#server-cluster:cluster.max_standby)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`cluster.max_standby`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`2`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the maximum number of cluster members that are assigned the database stand-by role.
This must be a number between [`0`] and [`5`].

[[`cluster.max_voters`]][]

Number of database voter members

[[*!*](#server-cluster:cluster.max_voters)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`cluster.max_voters`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`3`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the maximum number of cluster members that are assigned the database voter role.
This must be an odd number \>= [`3`].

[[`cluster.offline_threshold`]][]

Threshold when an unresponsive member is considered offline

[[*!*](#server-cluster:cluster.offline_threshold)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clu                             |
|                                   | ster.offline_threshold`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`20`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the number of seconds after which an unresponsive member is considered offline.

[[`cluster.rebalance.batch`]][]

Maximum number of instances to move during one re-balancing run

[[*!*](#server-cluster:cluster.rebalance.batch)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`c                               |
|                                   | luster.rebalance.batch`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`1`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`cluster.rebalance.cooldown`]][]

Amount of time during which an instance will not be moved again

[[*!*](#server-cluster:cluster.rebalance.cooldown)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clus                            |
|                                   | ter.rebalance.cooldown`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`6H`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`cluster.rebalance.interval`]][]

How often (in minutes) to consider re-balancing things. 0 to disable (default)

[[*!*](#server-cluster:cluster.rebalance.interval)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clus                            |
|                                   | ter.rebalance.interval`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`0`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`cluster.rebalance.threshold`]][]

Percentage load difference between most and least busy server needed to trigger a migration

[[*!*](#server-cluster:cluster.rebalance.threshold)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`clust                           |
|                                   | er.rebalance.threshold`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`20`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[]{#server-options-images}

## Images configuration

The following server options configure how to handle [Images](../images/#images):

[[`images.auto_update_cached`]][]

Whether to automatically update cached images

[[*!*](#server-images:images.auto_update_cached)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ima                             |
|                                   | ges.auto_update_cached`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`true`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`images.auto_update_interval`]][]

Interval at which to look for updates to cached images

[[*!*](#server-images:images.auto_update_interval)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`image                           |
|                                   | s.auto_update_interval`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`6`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the interval in hours.
To disable looking for updates to cached images, set this option to [`0`].

[[`images.compression_algorithm`]][]

Compression algorithm to use for new images

[[*!*](#server-images:images.compression_algorithm)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`images                          |
|                                   | .compression_algorithm`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`gzip`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Possible values are [`bzip2`], [`gzip`], [`lz4`], [`lzma`], [`xz`], [`zstd`] or [`none`].

[[`images.default_architecture`]][]

Default architecture to use in a mixed-architecture cluster

[[*!*](#server-images:images.default_architecture)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`image                           |
|                                   | s.default_architecture`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

[[`images.remote_cache_expiry`]][]

When an unused cached remote image is flushed

[[*!*](#server-images:images.remote_cache_expiry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`imag                            |
|                                   | es.remote_cache_expiry`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`10`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the number of days after which the unused cached image expires.

[]{#server-options-logging}

## Logging configuration

The logging system now supports multiple configurable targets, each identified by a unique name (e.g., [`loki01`], [`syslog01`]).
Each target can be independently configured and assigned specific log types.

### Supported Targets

-   [`loki`] - For sending logs to a Grafana Loki server

-   [`syslog`] - For sending logs to remote syslog endpoint

### Example configuration

    logging.loki01.target.type: loki
    logging.loki01.target.address: https://loki01.int.example.net
    logging.loki01.target.username: foo
    logging.loki01.target.password: bar
    logging.loki01.types: lifecycle,network-acl
    logging.loki01.lifecycle.types: instance

    logging.syslog01.target.type: syslog
    logging.syslog01.target.address: syslog01.int.example.net
    logging.syslog01.target.facility: security
    logging.syslog01.types: logging
    logging.syslog01.logging.level: warning

[[`logging.NAME.lifecycle.projects`]][]

Comma separate list of projects, empty means all

[[*!*](#server-logging:logging.NAME.lifecycle.projects)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`logging.N                       |
|                                   | AME.lifecycle.projects`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.lifecycle.types`]][]

E.g., [`instance`], comma separate, empty means all

[[*!*](#server-logging:logging.NAME.lifecycle.types)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggin                          |
|                                   | g.NAME.lifecycle.types`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.logging.level`]][]

Minimum log level to send to the logger

[[*!*](#server-logging:logging.NAME.logging.level)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`logg                            |
|                                   | ing.NAME.logging.level`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`info`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.address`]][]

Address of the logger

[[*!*](#server-logging:logging.NAME.target.address)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggi                           |
|                                   | ng.NAME.target.address`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify the protocol, name or IP and port. For example [`tcp://syslog01.int.example.net:514`].

[[`logging.NAME.target.ca_cert`]][]

CA certificate for the server

[[*!*](#server-logging:logging.NAME.target.ca_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggi                           |
|                                   | ng.NAME.target.ca_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.facility`]][]

The syslog facility defines the category of the log message

[[*!*](#server-logging:logging.NAME.target.facility)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggin                          |
|                                   | g.NAME.target.facility`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.instance`]][]

Name to use as the instance field in Loki events.

[[*!*](#server-logging:logging.NAME.target.instance)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggin                          |
|                                   | g.NAME.target.instance`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | Local server host name or cluster |
|                                   | member name                       |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

This allows replacing the default instance value (server host name) by a more relevant value like a cluster identifier.

[[`logging.NAME.target.labels`]][]

Labels for a Loki log entry

[[*!*](#server-logging:logging.NAME.target.labels)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`logg                            |
|                                   | ing.NAME.target.labels`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify a comma-separated list of values that should be used as labels for a Loki log entry.

[[`logging.NAME.target.password`]][]

Password used for authentication

[[*!*](#server-logging:logging.NAME.target.password)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggin                          |
|                                   | g.NAME.target.password`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.retry`]][]

number of delivery retries, default 3

[[*!*](#server-logging:logging.NAME.target.retry)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`log                             |
|                                   | ging.NAME.target.retry`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | integer                           |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.type`]][]

The type of the logger. One of [`loki`], [`syslog`] or [`webhook`].

[[*!*](#server-logging:logging.NAME.target.type)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`lo                              |
|                                   | gging.NAME.target.type`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.target.username`]][]

User name used for authentication

[[*!*](#server-logging:logging.NAME.target.username)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`loggin                          |
|                                   | g.NAME.target.username`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`logging.NAME.types`]][]

Events to send to the logger

[[*!*](#server-logging:logging.NAME.types)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`logging.NAME.types`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`lifecycle,logging`]     |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Specify a comma-separated list of events to send to the logger.
The events can be any combination of [`lifecycle`], [`logging`], and [`network-acl`].

[]{#server-options-misc}

## Miscellaneous options

The following server options configure server-specific settings for [Instances](../instances/#instances), [OVN](../reference/network_ovn/#network-ovn) integration, [Backups](../backup/#backups) and [Storage](../storage/#storage):

[[`authorization.scriptlet`]][]

Authorization scriptlet

[[*!*](#server-miscellaneous:authorization.scriptlet)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`a                               |
|                                   | uthorization.scriptlet`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

When using scriptlet-based authorization, this option stores the scriptlet.

[[`backups.compression_algorithm`]][]

Compression algorithm to use for backups

[[*!*](#server-miscellaneous:backups.compression_algorithm)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`backups                         |
|                                   | .compression_algorithm`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`gzip`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Possible values are [`bzip2`], [`gzip`], [`lz4`], [`lzma`], [`xz`], [`zstd`] or [`none`].

[[`instances.lxcfs.per_instance`]][]

Whether to run LXCFS on a per-instance basis

[[*!*](#server-miscellaneous:instances.lxcfs.per_instance)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`instan                          |
|                                   | ces.lxcfs.per_instance`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | bool                              |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`false`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

LXCFS is used to provide overlays for common [`/proc`] and [`/sys`]
files which reflect the resource limits applied to the container.

It normally operates through a single file system mount on the host which is then shared by all containers.
This is very efficient but comes with the downside that a crash of LXCFS will break all containers.

With this option, it's now possible to run a LXCFS instance per
container instead, using more system resources but reducing the impact
of a crash.

[[`instances.nic.host_name`]][]

How to set the host name for a NIC

[[*!*](#server-miscellaneous:instances.nic.host_name)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`i                               |
|                                   | nstances.nic.host_name`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`random`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Possible values are [`random`] and [`mac`].

If set to [`random`], use the random host interface name as the host name.
If set to [`mac`], generate a host name in the form [`inc<mac_address>`] (MAC without leading two digits).

[[`instances.placement.scriptlet`]][]

Instance placement scriptlet for automatic instance placement

[[*!*](#server-miscellaneous:instances.placement.scriptlet)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`instanc                         |
|                                   | es.placement.scriptlet`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

When using custom automatic instance placement logic, this option stores the scriptlet.
See [Instance placement scriptlet](../explanation/clustering/#clustering-instance-placement-scriptlet) for more information.

[[`instances.tpm.platform_cert`]][]

Platform CA certificate used to provision TPM Endorsement Keys

[[*!*](#server-miscellaneous:instances.tpm.platform_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`insta                           |
|                                   | nces.tpm.platform_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

PEM encoded platform CA certificate used to sign the Endorsement
Key and platform certificate of newly created TPM devices.
Must be set together with [`instances.tpm.platform_key`].

[[`instances.tpm.platform_key`]][]

Private key for the TPM platform CA certificate

[[*!*](#server-miscellaneous:instances.tpm.platform_key)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`inst                            |
|                                   | ances.tpm.platform_key`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

PEM encoded private key matching [`instances.tpm.platform_cert`].
Used to sign the Endorsement Key and platform certificate of newly
created TPM devices.

[[`network.ovn.ca_cert`]][]

OVN SSL certificate authority

[[*!*](#server-miscellaneous:network.ovn.ca_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`network.ovn.ca_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | Content of                        |
|                                   | [`/e                              |
|                                   | tc/ovn/ovn-central.crt`] if  |
|                                   | present                           |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`network.ovn.client_cert`]][]

OVN SSL client certificate

[[*!*](#server-miscellaneous:network.ovn.client_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`n                               |
|                                   | etwork.ovn.client_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | Content of                        |
|                                   | [`/etc/ovn/cert_host`] if  |
|                                   | present                           |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`network.ovn.client_key`]][]

OVN SSL client key

[[*!*](#server-miscellaneous:network.ovn.client_key)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`                                |
|                                   | network.ovn.client_key`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | Content of                        |
|                                   | [`/etc/ovn/key_host`] if  |
|                                   | present                           |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`network.ovn.integration_bridge`]][]

OVS integration bridge to use for OVN networks

[[*!*](#server-miscellaneous:network.ovn.integration_bridge)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`network.                        |
|                                   | ovn.integration_bridge`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`br-int`]              |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`network.ovn.northbound_connection`]][]

OVN northbound database connection string

[[*!*](#server-miscellaneous:network.ovn.northbound_connection)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`network.ovn                     |
|                                   | .northbound_connection`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`unix:                           |
|                                   | /run/ovn/ovnnb_db.sock`]     |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`network.ovs.connection`]][]

OVS socket path

[[*!*](#server-miscellaneous:network.ovs.connection)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`                                |
|                                   | network.ovs.connection`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`unix:/r                         |
|                                   | un/openvswitch/db.sock`]     |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`storage.backups_volume`]][]

Volume to use to store backup tarballs

[[*!*](#server-miscellaneous:storage.backups_volume)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`                                |
|                                   | storage.backups_volume`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

Specify the volume using the syntax [`POOL/VOLUME`].

[[`storage.images_volume`]][]

Volume to use to store the image tarballs

[[*!*](#server-miscellaneous:storage.images_volume)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [                                 |
|                                   | `storage.images_volume`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

Specify the volume using the syntax [`POOL/VOLUME`].

[[`storage.linstor.ca_cert`]][]

LINSTOR SSL certificate authority

[[*!*](#server-miscellaneous:storage.linstor.ca_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`s                               |
|                                   | torage.linstor.ca_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`storage.linstor.client_cert`]][]

LINSTOR SSL client certificate

[[*!*](#server-miscellaneous:storage.linstor.client_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`stora                           |
|                                   | ge.linstor.client_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`storage.linstor.client_key`]][]

LINSTOR SSL client key

[[*!*](#server-miscellaneous:storage.linstor.client_key)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`stor                            |
|                                   | age.linstor.client_key`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`storage.linstor.controller_connection`]][]

LINSTOR controller connection string

[[*!*](#server-miscellaneous:storage.linstor.controller_connection)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`storage.linstor                 |
|                                   | .controller_connection`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`storage.linstor.satellite.name`]][]

LINSTOR satellite node name override

[[*!*](#server-miscellaneous:storage.linstor.satellite.name)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`storage.                        |
|                                   | linstor.satellite.name`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

Set this option to the name of the local LINSTOR satellite node, should it be different from the Incus server name.

[[`storage.logs_volume`]][]

Volume to use to store instance log directories

[[*!*](#server-miscellaneous:storage.logs_volume)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`storage.logs_volume`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | local                             |
+-----------------------------------+-----------------------------------+

Specify the volume using the syntax [`POOL/VOLUME`].

[]{#server-options-user}

## User options

Additional user defined configuration keys are available within the [`user.`] namespace.
User defined configuration keys are always of type [`string`] and have [`global`] scope.
Note that keys starting with [`user.ui.`] are used for web UI configuration options and are visible even to unauthenticated users.
