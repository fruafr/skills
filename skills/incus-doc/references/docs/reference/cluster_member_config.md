[]{#cluster-member-config}

# Cluster member configuration

Each cluster member has its own key/value configuration with the following supported namespaces:

-   [`user`] (free form key/value for user metadata)

-   [`scheduler`] (options related to how the member is automatically targeted by the cluster)

The following keys are currently supported:

[[`scheduler.instance`]][]

Controls how instances are scheduled to run on this member

[[*!*](#cluster-cluster:scheduler.instance)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`scheduler.instance`]     |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+
| **Default:**                      | []                      |
|                                   |                                   |
|                                   | [`all`]              |
+-----------------------------------+-----------------------------------+

Possible values are [`all`], [`manual`], and [`group`]. See
[Automatic placement of instances](../../explanation/clustering/#clustering-instance-placement) for more information.

[[`user.*`]][]

Free form user key/value storage

[[*!*](#cluster-cluster:user.*)]

+-----------------------------------+-----------------------------------+
| **Key:**                          | [`user.*`]              |
+-----------------------------------+-----------------------------------+
| **Type:**                         | []                      |
|                                   |                                   |
|                                   | string                            |
+-----------------------------------+-----------------------------------+

User keys can be used in search.
