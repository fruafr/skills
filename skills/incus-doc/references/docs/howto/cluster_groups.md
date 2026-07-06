[]{#howto-cluster-groups}

# How to set up cluster groups

Cluster members can be assigned to [Cluster groups](../../explanation/clustering/#cluster-groups).
By default, all cluster members belong to the [`default`] group.

To create a cluster group, use the [[[`incus`]` `[`cluster`]` `[`group`]` `[`create`]]](../../reference/manpages/incus/cluster/group/create/#incus-cluster-group-create-md) command.
For example:

    incus cluster group create gpu

To assign a cluster member to one or more groups, use the [[[`incus`]` `[`cluster`]` `[`group`]` `[`assign`]]](../../reference/manpages/incus/cluster/group/assign/#incus-cluster-group-assign-md) command.
This command removes the specified cluster member from all the cluster groups it currently is a member of and then adds it to the specified group or groups.

For example, to assign [`server1`] to only the [`gpu`] group, use the following command:

    incus cluster group assign server1 gpu

To assign [`server1`] to the [`gpu`] group and also keep it in the [`default`] group, use the following command:

    incus cluster group assign server1 default,gpu

To add a cluster member to a specific group without removing it from other groups, use the [[[`incus`]` `[`cluster`]` `[`group`]` `[`add`]]](../../reference/manpages/incus/cluster/group/add/#incus-cluster-group-add-md) command.

For example, to add [`server1`] to the [`gpu`] group and also keep it in the [`default`] group, use the following command:

    incus cluster group add server1 gpu

## Configuration options

The following configuration options are available for cluster groups:

[[`instances.vm.cpu.ARCHITECTURE.baseline`]][]

CPU base architecture name

[[*!*](#cluster_group-common:instances.vm.cpu.ARCHITECTURE.baseline)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`instances.vm.cpu                |
|                                   | .ARCHITECTURE.baseline`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

The CPU base architecture name as can be found through [`qemu`]` `[`-cpu`]` `[`?`].

This can be a generic definition like [`qemu64`] or [`kvm64`], or it can be a specific hardware architecture like [`EPYC-v2`].
It's important to ensure that all servers in the group match that baseline.

[[`instances.vm.cpu.ARCHITECTURE.flags`]][]

CPU flags to add/remove to/from the baseline

[[*!*](#cluster_group-common:instances.vm.cpu.ARCHITECTURE.flags)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`instances.vm.                   |
|                                   | cpu.ARCHITECTURE.flags`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

A comma separated list of CPU flags to add on top of CPU baseline or a list of flags to remove from it.

To remove a flag, use [`-flag`].

[[`user.*`]][]

Free form user key/value storage

[[*!*](#cluster_group-common:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

User keys can be used in search.

## Launch an instance on a cluster group member

With cluster groups, you can target an instance to run on one of the members of the cluster group, instead of targeting it to run on a specific member.

Note

[`scheduler.instance`](../../reference/cluster_member_config/#cluster-cluster:scheduler.instance) must be set to either [`all`] (the default) or [`group`] to allow instances to be targeted to a cluster group.

See [Automatic placement of instances](../../explanation/clustering/#clustering-instance-placement) for more information.

To launch an instance on a member of a cluster group, follow the instructions in [Launch an instance on a specific cluster member](../cluster_manage_instance/#cluster-target-instance), but use the group name prefixed with [`@`] for the [`--target`] flag.
For example:

    incus launch images:debian/12 c1 --target=@gpu

## Use with restricted projects

A project can be configured to only have access to servers that are part of specific cluster groups.

This is done by setting both [`restricted=true`] and [`restricted.cluster.groups`] to a comma separated list of group names.

**Note**

If the cluster group is renamed, the project restrictions will need to be updated for the new group name.
