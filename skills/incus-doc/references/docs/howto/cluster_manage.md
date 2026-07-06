[]{#cluster-manage}

# How to manage a cluster

After your cluster is formed, use [[[`incus`]` `[`cluster`]` `[`list`]]](../../reference/manpages/incus/cluster/list/#incus-cluster-list-md) to see a list of its members and their status:

[`~$`]` `[`incus`]` `[`cluster`]` `[`list`]

[`+---------+----------------------------+------------------+--------------+----------------+-------------+--------+-------------------+`][`|`]` `[`NAME`]`    `[`|`]`            `[`URL`]`             `[`|`]`      `[`ROLES`]`       `[`|`]` `[`ARCHITECTURE`]` `[`|`]` `[`FAILURE`]` `[`DOMAIN`]` `[`|`]` `[`DESCRIPTION`]` `[`|`]` `[`STATE`]`  `[`|`]`      `[`MESSAGE`]`      `[`|`][`+---------+----------------------------+------------------+--------------+----------------+-------------+--------+-------------------+`][`|`]` `[`server1`]` `[`|`]` `[`https://192.0.2.101:8443`]`   `[`|`]` `[`database-leader`]`  `[`|`]` `[`x86_64`]`       `[`|`]` `[`default`]`        `[`|`]`             `[`|`]` `[`ONLINE`]` `[`|`]` `[`Fully`]` `[`operational`]` `[`|`][`|`]`         `[`|`]`                            `[`|`]` `[`database`]`         `[`|`]`              `[`|`]`                `[`|`]`             `[`|`]`        `[`|`]`                   `[`|`][`+---------+----------------------------+------------------+--------------+----------------+-------------+--------+-------------------+`][`|`]` `[`server2`]` `[`|`]` `[`https://192.0.2.102:8443`]`   `[`|`]` `[`database-standby`]` `[`|`]` `[`aarch64`]`      `[`|`]` `[`default`]`        `[`|`]`             `[`|`]` `[`ONLINE`]` `[`|`]` `[`Fully`]` `[`operational`]` `[`|`][`+---------+----------------------------+------------------+--------------+----------------+-------------+--------+-------------------+`][`|`]` `[`server3`]` `[`|`]` `[`https://192.0.2.103:8443`]`   `[`|`]` `[`database-standby`]` `[`|`]` `[`aarch64`]`      `[`|`]` `[`default`]`        `[`|`]`             `[`|`]` `[`ONLINE`]` `[`|`]` `[`Fully`]` `[`operational`]` `[`|`][`+---------+----------------------------+------------------+--------------+----------------+-------------+--------+-------------------+`]

To see more detailed information about an individual cluster member, run the following command:

    incus cluster show <member_name>

To see state and usage information for a cluster member, run the following command:

    incus cluster info <member_name>

## Configure your cluster

To configure your cluster, use [[[`incus`]` `[`config`]]](../../reference/manpages/incus/config/#incus-config-md).
For example:

    incus config set cluster.max_voters=5

Keep in mind that some [server configuration options](../../server_config/#server) are global and others are local.
You can configure the global options on any cluster member, and the changes are propagated to the other cluster members through the distributed database.
The local options are set only on the server where you configure them (or alternatively on the server that you target with [`--target`]).

In addition to the server configuration, there are a few cluster configurations that are specific to each cluster member.
See [Cluster member configuration](../../reference/cluster_member_config/#cluster-member-config) for all available configurations.

To set these configuration options, use [[[`incus`]` `[`cluster`]` `[`set`]]](../../reference/manpages/incus/cluster/set/#incus-cluster-set-md) or [[[`incus`]` `[`cluster`]` `[`edit`]]](../../reference/manpages/incus/cluster/edit/#incus-cluster-edit-md).
For example:

    incus cluster set server1 scheduler.instance manual

### Assign member roles

To add or remove a [member role](../../explanation/clustering/#clustering-member-roles) for a cluster member, use the [[[`incus`]` `[`cluster`]` `[`role`]]](../../reference/manpages/incus/cluster/role/#incus-cluster-role-md) command.
For example:

    incus cluster role add server1 event-hub

**Note**

You can add or remove only those roles that are not assigned automatically by Incus.

### Edit the cluster member configuration

To edit all properties of a cluster member, including the member-specific configuration, the member roles, the failure domain and the cluster groups, use the [[[`incus`]` `[`cluster`]` `[`edit`]]](../../reference/manpages/incus/cluster/edit/#incus-cluster-edit-md) command.

[]{#cluster-evacuate}

## Evacuate and restore cluster members

There are scenarios where you might need to empty a given cluster member of all its instances (for example, for routine maintenance like applying system updates that require a reboot, or to perform hardware changes).

To do so, use the [[[`incus`]` `[`cluster`]` `[`evacuate`]]](../../reference/manpages/incus/cluster/evacuate/#incus-cluster-evacuate-md) command.
This command migrates all instances on the given server, moving them to other cluster members.
The evacuated cluster member is then transitioned to an "evacuated" state, which prevents the creation of any instances on it.

You can control how each instance is moved through the [`cluster.evacuate`](../../reference/instance_options/#instance-miscellaneous:cluster.evacuate) instance configuration key.
Instances are shut down cleanly, respecting the [`boot.host_shutdown_timeout`] configuration key.

When the evacuated server is available again, use the [[[`incus`]` `[`cluster`]` `[`restore`]]](../../reference/manpages/incus/cluster/restore/#incus-cluster-restore-md) command to move the server back into a normal running state.
This command also moves the evacuated instances back from the servers that were temporarily holding them.

[]{#cluster-automatic-evacuation}

### Cluster healing

Incus can automatically detect and recover from a broken server. This is done by setting the [`cluster.healing_threshold`](../../server_config/#server-cluster:cluster.healing_threshold) configuration to a non-zero value.
Instances are automatically evacuated to other servers after the leader has marked a cluster member has offline.

When the broken server is available again, you must manually restore it as if it had been manually evacuated.

**Note**

This automatic cluster healing only applies to instances on shared storage and which don't use any local devices.

**Warning**

Enabling this feature can come at the risk of data corruption should a server be deemed offline as a result of partial connectivity issues.
Incus considers a server to be offline when it fails to respond to heartbeat packets and when it also fails to respond to ICMP packets.

It's critical to ensure that a server which is considered offline is in fact offline and isn't still running its instances.
One way to automatically achieve this is to have a piece of software monitor Incus for a [`cluster-member-healed`] event and promptly cut the
power to the server in question by interacting with its BMC or PDU.

[]{#cluster-automatic-balancing}

### Cluster re-balancing

Incus can automatically balance the load across cluster members.

This is done through a few configuration options:

-   [`cluster.rebalance.batch`](../../server_config/#server-cluster:cluster.rebalance.batch)

-   [`cluster.rebalance.cooldown`](../../server_config/#server-cluster:cluster.rebalance.cooldown)

-   [`cluster.rebalance.interval`](../../server_config/#server-cluster:cluster.rebalance.interval)

-   [`cluster.rebalance.threshold`](../../server_config/#server-cluster:cluster.rebalance.threshold)

Incus will compare the load across all servers and if the difference in
percent exceeds the threshold, it will start identifying
virtual-machines that can be safely live-migrated to the least loaded
server.

[]{#cluster-manage-delete-members}

## Delete cluster members

To cleanly delete a member from the cluster, use the following command:

    incus cluster remove <member_name>

You can only cleanly delete members that are online and that don't have any instances located on them.

### Deal with offline cluster members

If a cluster member goes permanently offline, you can force-remove it from the cluster.
Make sure to do so as soon as you discover that you cannot recover the member.
If you keep an offline member in your cluster, you might encounter issues when upgrading your cluster to a newer version.

To force-remove a cluster member, enter the following command on one of the cluster members that is still online:

    incus cluster remove --force <member_name>

**Caution**

Force-removing a cluster member will leave the member's database in an inconsistent state (for example, the storage pool on the member will not be removed).
As a result, it will not be possible to re-initialize Incus later, and the server must be fully reinstalled.

## Upgrade cluster members

To upgrade a cluster, you must upgrade all of its members.
All members must be upgraded to the same version of Incus.

**Caution**

Do not attempt to upgrade your cluster if any of its members are offline.
Offline members cannot be upgraded, and your cluster will end up in a blocked state.

To upgrade a single member, simply upgrade the Incus package on the host and restart the Incus daemon.
If the new version of the daemon has database schema or API changes, the upgraded member might transition into a "blocked" state.
In this case, the member does not serve any Incus API requests (which means that [`incus`] commands don't work on that member anymore), but any running instances will continue to run.

This happens if there are other cluster members that have not been upgraded and are therefore running an older version.
Run [[[`incus`]` `[`cluster`]` `[`list`]]](../../reference/manpages/incus/cluster/list/#incus-cluster-list-md) on a cluster member that is not blocked to see if any members are blocked.

As you proceed upgrading the rest of the cluster members, they will all transition to the "blocked" state.
When you upgrade the last member, the blocked members will notice that all servers are now up-to-date, and the blocked members become operational again.

## Update the cluster certificate

In an Incus cluster, the API on all servers responds with the same shared certificate, which is usually a standard self-signed certificate with an expiry set to ten years.

The certificate is stored at [`/var/lib/incus/cluster.crt`] and is the same on all cluster members.

You can replace the standard certificate with another one, for example, a valid certificate obtained through ACME services (see [TLS server certificate](../../authentication/#authentication-server-certificate) for more information).
To do so, use the [[[`incus`]` `[`cluster`]` `[`update-certificate`]]](../../reference/manpages/incus/cluster/update-certificate/#incus-cluster-update-certificate-md) command.
This command replaces the certificate on all servers in your cluster.
