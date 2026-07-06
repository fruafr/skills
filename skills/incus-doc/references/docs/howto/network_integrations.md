[]{#network-integrations}

# How to configure network integrations

**Note**

Network integrations are currently only available for the [OVN network](../../reference/network_ovn/#network-ovn).

Network integrations can be used to connect networks on the local Incus
deployment to remote networks hosted on Incus or other platforms.

## OVN interconnection

At this time the only type of network integrations supported is OVN
which makes use of OVN interconnection gateways to peer OVN networks
together across multiple deployments.

For this to work one needs a working OVN interconnection setup with:

-   OVN interconnection [`NorthBound`] and [`SouthBound`] databases

-   Two or more OVN clusters with their availability-zone names set properly ([`name`] property)

-   All OVN clusters need to have the [`ovn-ic`] daemon running

-   OVN clusters configured to advertise and learn routes from interconnection

-   At least one server marked as an OVN interconnection gateway

More details can be found in the [upstream documentation](https://docs.ovn.org/en/latest/tutorials/ovn-interconnection.html).

## Creating a network integration

A network integration can be created with [`incus`]` `[`network`]` `[`integration`]` `[`create`].
Integrations are global to the Incus deployment, they are not tied to a network or project.

An example for an OVN integration would be:

    incus network integration create ovn-region ovn
    incus network integration set ovn-region ovn.northbound_connection tcp:[192.0.2.12]:6645,tcp:[192.0.3.13]:6645,tcp:[192.0.3.14]:6645
    incus network integration set ovn-region ovn.southbound_connection tcp:[192.0.2.12]:6646,tcp:[192.0.3.13]:6646,tcp:[192.0.3.14]:6646

## Using a network integration

To make use of a network integration, one needs to peer with it.

This is done through [`incus`]` `[`network`]` `[`peer`]` `[`create`], for example:

    incus network peer create default region ovn-region --type=remote

## Integration properties

Address sets have the following properties:

  Property                                                 Type     Required   Description
  -------------------------------------------------------- -------- ---------- ---------------------------------------------------------------------------------------------
  [`name`]          string   yes        Name of the network integration
  [`description`]   string   no         Description of the network integration
  [`type`]          string   yes        Type of network integration (currently only [`ovn`])

## Integration configuration options

The following configuration options are available for all network integrations:

[[`user.*`]][]

Free form user key/value storage

[[*!*](#network_integration-common:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

User keys can be used in search.

### OVN configuration options

Those options are specific to the OVN network integrations:

[[`ovn.ca_cert`]][]

OVN SSL certificate authority for the inter-connection database

[[*!*](#network_integration-ovn:ovn.ca_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn.ca_cert`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`ovn.client_cert`]][]

OVN SSL client certificate

[[*!*](#network_integration-ovn:ovn.client_cert)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn.client_cert`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`ovn.client_key`]][]

OVN SSL client key

[[*!*](#network_integration-ovn:ovn.client_key)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn.client_key`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`ovn.northbound_connection`]][]

OVN northbound inter-connection connection string

[[*!*](#network_integration-ovn:ovn.northbound_connection)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn                             |
|                                   | .northbound_connection`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`ovn.southbound_connection`]][]

OVN southbound inter-connection connection string

[[*!*](#network_integration-ovn:ovn.southbound_connection)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn                             |
|                                   | .southbound_connection`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Scope:**                        | []                      |
|                                   |                                   |
|                                   | global                            |
+-----------------------------------+-----------------------------------+

[[`ovn.transit.pattern`]][]

Template for the transit switch name

[[*!*](#network_integration-ovn:ovn.transit.pattern)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`ovn.transit.pattern`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`ts-incus-{{`]` `[`integrationName`]` `[`}}-{{`]` `[`projectName`]` `[`}}-{{`]` `[`networkName`]` `[`}}`]     |
+-----------------------------------+-----------------------------------+

Specify a Pongo2 template string that represents the transit switch name.
This template gets access to the project name ([`projectName`]),
integration name ([`integrationName`]), network name ([`networkName`])
and peer name ([`peerName`]).
