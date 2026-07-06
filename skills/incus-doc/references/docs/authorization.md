[]{#id1}

# Authorization

When interacting with Incus over the Unix socket, members of the [`incus-admin`] group will have full access to the Incus API.
Those who are only members of the [`incus`] group will instead be restricted to a single project tied to their user.

When interacting with Incus over the network (see [How to expose Incus to the network](../howto/server_expose/#server-expose) for instructions), it is possible to further authenticate and restrict user access.
There are three supported authorization methods:

-   [TLS authorization](#authorization-tls)

-   [Open Fine-Grained Authorization (OpenFGA)](#authorization-openfga)

-   [Scriptlet authorization](#authorization-scriptlet)

[]{#authorization-tls}

## TLS authorization

Incus natively supports restricting [Trusted TLS clients](../authentication/#authentication-trusted-clients) to one or more projects.
When a client certificate is restricted, the client will also be prevented from performing global configuration changes or altering the configuration (limits, restrictions) of the projects it's allowed access to.

To restrict access, use [[[`incus`]` `[`config`]` `[`trust`]` `[`edit`]` `[`<fingerprint>`]]](../reference/manpages/incus/config/trust/edit/#incus-config-trust-edit-md).
Set the [`restricted`] key to [`true`] and specify a list of projects to restrict the client to.
If the list of projects is empty, the client will not be allowed access to any of them.

This authorization method is used if a client authenticates with TLS even if [OpenFGA authorization](#authorization-openfga) is configured.

[]{#authorization-openfga}

## Open Fine-Grained Authorization (OpenFGA)

Incus supports integrating with [OpenFGA](https://openfga.dev).
This authorization method is highly granular.
For example, it can be used to restrict user access to a single instance.

To use OpenFGA for authorization, you must configure and run an OpenFGA server yourself.
Incus will connect to the OpenFGA server, write the [OpenFGA model](#openfga-model), and query this server for authorization for all subsequent requests.

To enable this authorization method in Incus, set the [[[`openfga.*`]]](../server_config/#server-options-openfga) server configuration options.
All options must be set in order to enable OpenFGA. Though, you do not have to create the authorization-model yourself, Incus will generate it including the initial tuple to allow only authenticated users: [`server:incus#authenticated@user:*`].

[]{#id2}

### OpenFGA model

With OpenFGA, access to a particular API resource is determined by the user's relationship to it.
These relationships are determined by an [OpenFGA authorization model](https://openfga.dev/docs/concepts#what-is-an-authorization-model).
The Incus OpenFGA authorization model describes API resources in terms of their relationship to other resources, and a relationship a user or group might have with that resource.

The full Incus OpenFGA authorization model is defined in [`internal/server/auth/driver_openfga_model.openfga`]:

    model
      schema 1.1
    type user

    type group
      relations
        define member: [user]

    type certificate
      relations
        define server: [server]
        define can_edit: [user, group#member] or admin from server
        define can_view: viewer from server

    type image
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type image_alias
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type instance
      relations
        define project: [project]
        define admin: [user, group#member] or admin from project
        define operator: [user, group#member] or admin or operator from project
        define user: [user, group#member] or operator or user from project
        define viewer: [user, group#member] or user or viewer from project
        define can_access_console: [user, group#member] or user
        define can_access_files: [user, group#member] or user
        define can_connect_nbd: [user, group#member] or user
        define can_connect_sftp: [user, group#member] or user
        define can_edit: operator
        define can_exec: [user, group#member] or user
        define can_manage_backups: [user, group#member] or operator
        define can_manage_snapshots: [user, group#member] or operator
        define can_update_state: [user, group#member] or operator
        define can_view: viewer

    type network
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type network_acl
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type network_address_set
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type network_integration
      relations
        define server: [server]
        define can_edit: [user, group#member] or admin from server
        define can_view: viewer from server

    type network_zone
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type profile
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type project
      relations
        define server: [server]
        define admin: [user, group#member] or admin from server
        define operator: [user, group#member] or admin or operator from server
        define user: [user, group#member] or operator or user from server
        define viewer: [user, group#member] or user or viewer from server
        define can_create_image_aliases: [user, group#member] or operator
        define can_create_images: [user, group#member] or operator
        define can_create_instances: [user, group#member] or operator
        define can_create_network_acls: [user, group#member] or operator
        define can_create_network_address_sets: [user, group#member] or operator
        define can_create_networks: [user, group#member] or operator
        define can_create_network_zones: [user, group#member] or operator
        define can_create_profiles: [user, group#member] or operator
        define can_create_storage_buckets: [user, group#member] or operator
        define can_create_storage_volumes: [user, group#member] or operator
        define can_edit: admin
        define can_view_events: [user, group#member] or user
        define can_view_operations: [user, group#member] or user
        define can_view: viewer

    type server
      relations
        define admin: [user, group#member]
        define operator: [user, group#member] or admin
        define user: [user, group#member] or operator
        define viewer: [user, group#member] or user
        define authenticated: [user:*]
        define can_create_certificates: [user, group#member] or admin
        define can_create_network_integrations: [user, group#member] or admin
        define can_create_projects: [user, group#member] or admin
        define can_create_storage_pools: [user, group#member] or admin
        define can_edit: admin
        define can_override_cluster_target_restriction: [user, group#member] or admin
        define can_view_privileged_events: [user, group#member] or admin
        define can_view_metrics: authenticated
        define can_view_resources: authenticated
        define can_view_sensitive: [user, group#member] or viewer
        define can_view: authenticated

    type storage_bucket
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_view: [user, group#member] or can_edit or viewer from project

    type storage_pool
      relations
        define server: [server]
        define can_edit: [user, group#member] or admin from server
        define can_view: authenticated from server

    type storage_volume
      relations
        define project: [project]
        define can_edit: [user, group#member] or operator from project
        define can_manage_backups: [user, group#member] or can_edit
        define can_manage_snapshots: [user, group#member] or can_edit
        define can_view: [user, group#member] or can_edit or viewer from project
        define can_access_files: [user, group#member] or can_edit
        define can_connect_nbd: [user, group#member] or can_edit
        define can_connect_sftp: [user, group#member] or can_edit

**Important**

Users that you do not trust with root access to the host should not be granted the following relations:

-   [`server`]` `[`->`]` `[`admin`]

-   [`server`]` `[`->`]` `[`operator`]

-   [`server`]` `[`->`]` `[`can_edit`]

-   [`server`]` `[`->`]` `[`can_create_storage_pools`]

-   [`server`]` `[`->`]` `[`can_create_projects`]

-   [`server`]` `[`->`]` `[`can_create_certificates`]

-   [`certificate`]` `[`->`]` `[`can_edit`]

-   [`storage_pool`]` `[`->`]` `[`can_edit`]

-   [`project`]` `[`->`]` `[`admin`]

The remaining relations may be granted.
However, you must apply appropriate [Project restrictions](../reference/projects/#project-restrictions).

[]{#authorization-scriptlet}

## Scriptlet authorization

Incus supports defining a scriptlet to manage fine-grained authorization, allowing to write precise authorization rules with no dependency on external tools.

To use scriptlet authorization, you can write a scriptlet in the [`authorization.scriptlet`] server configuration option implementing a function [`authorize`], which takes three arguments:

-   [`details`], an object with the following attributes:

    -   [`Username`]: the user name or certificate fingerprint

    -   [`Protocol`]: the authentication protocol

    -   [`IsAllProjectsRequest`]: whether the request is made on all projects

    -   [`ProjectName`]: the project name

    -   [`Chain`]: the certificate chain as a list of dissected x509 certificates

    -   [`Certificate`]: the certificate data stored in the database

-   [`object`], the object on which the user requests authorization

-   [`entitlement`], the authorization level asked by the user

This function must return a Boolean indicating whether the user has access or not to the given object with the given entitlement.

Additionally, two optional functions can be defined so that users can be listed through the access API:

-   [`get_instance_access`], with two arguments ([`project_name`] and [`instance_name`]), returning a list of users able to access a given instance

-   [`get_project_access`], with one argument ([`project_name`]), returning a list of users able to access a given project
