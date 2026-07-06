# <span class="pre">`linstor`</span> driver internals<a href="#linstor-driver-internals" class="headerlink"
title="Link to this heading">¶</a>

This section describes some of the internal details of the
<span class="pre">`linstor`</span> driver implementation. Although
knowledge of these details is not needed to use the driver, they can be
relevant for operators when troubleshooting or even to get a better
understanding of the interactions between Incus and LINSTOR.

<div id="naming-objects" class="section">

## Naming objects<a href="#naming-objects" class="headerlink"
title="Link to this heading">¶</a>

At the time of writing, LINSTOR does not support renaming any of its
objects. This includes resources definitions, resource groups, storage
pools and snapshots. Since Incus needs the ability to rename the
resources it manages, this limitation requires an alternative way of
naming objects in LINSTOR while still being able to relate them to the
Incus’ database view of the objects.

At a first glance, using Incus’ database ID to name LINSTOR objects
seems like a viable option. Unfortunately, that does not work in
<a href="../../howto/disaster_recovery/#disaster-recovery"
class="reference internal"><span class="std std-ref">disaster
recovery</span></a> and
<a href="../../howto/instances_backup/#instances-backup-export"
class="reference internal"><span class="std std-ref">backups</span></a>
scenarios. To work around those limitations while accounting for the
mentioned scenarios, Incus uses auxiliary properties on LINSTOR resource
definitions. The auxiliary properties store metadata about the volume
from Incus’ perspective. Incus can then query LINSTOR using those
auxiliary properties to find the resource definition for a given volume.
This makes the resource definition name irrelevant for Incus, which
enables the use of a randomly generated value that is combined with the
configured <span class="pre">`linstor.volume.prefix`</span> value.

Visualizing the auxiliary properties in LINSTOR can be done by either
adding the <span class="pre">`--show-props`</span> flag on the
<span class="pre">`linstor`</span> command or by using the
<span class="pre">`resource-definition`</span>` `<span class="pre">`list-properties`</span>
command on a specific resource definition. As shown in the following
example:

<div class="highlight-default notranslate">

<div class="highlight">

    # incus storage volume list linstor
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    |            TYPE            |                               NAME                               | DESCRIPTION | CONTENT-TYPE | USED BY | LOCATION |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | container                  | c1                                                               |             | filesystem   | 1       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | custom                     | vol                                                              |             | block        | 0       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | image                      | d21a26af7d5a95c3aa6e923257a1cb5cd765b102796b92ab111fb29ebfb86137 |             | block        | 1       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | image                      | e3b67bf05e20c6c977f161a425733b00efe88834914e7fc8dd910c4b51cd5804 |             | filesystem   | 1       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | virtual-machine            | v1                                                               |             | block        | 1       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    | virtual-machine (snapshot) | v1/snap0                                                         |             | block        | 0       |          |
    +----------------------------+------------------------------------------------------------------+-------------+--------------+---------+----------+
    # linstor resource-definition list --show-props Aux/Incus/name Aux/Incus/content-type Aux/Incus/type
    ╭─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
    ┊ ResourceName                                  ┊ Port ┊ ResourceGroup ┊ Layers       ┊ State ┊ Aux/Incus/name                                                                ┊ Aux/Incus/content-type ┊ Aux/Incus/type   ┊
    ╞═════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════╡
    ┊ incus-volume-4df9f0598bd14d73953151614428d298 ┊ 7004 ┊ linstor       ┊ DRBD,STORAGE ┊ ok    ┊ incus-volume-v1                                                               ┊ block                  ┊ virtual-machines ┊
    ┊ incus-volume-61be43e12fed4845ab3f2443cccbe50c ┊ 7001 ┊ linstor       ┊ DRBD,STORAGE ┊ ok    ┊ incus-volume-c1                                                               ┊ filesystem             ┊ containers       ┊
    ┊ incus-volume-d0531e21ce6b4218b9b8996582b9bf31 ┊ 7002 ┊ linstor       ┊ DRBD,STORAGE ┊ ok    ┊ incus-volume-d21a26af7d5a95c3aa6e923257a1cb5cd765b102796b92ab111fb29ebfb86137 ┊ block                  ┊ images           ┊
    ┊ incus-volume-e3e682324ff54fa3a3cf3203d5029366 ┊ 7000 ┊ linstor       ┊ DRBD,STORAGE ┊ ok    ┊ incus-volume-default_vol                                                      ┊ block                  ┊ custom           ┊
    ┊ incus-volume-ef2f8cc7bfc148c58a7259f5a201658f ┊ 7003 ┊ linstor       ┊ DRBD,STORAGE ┊ ok    ┊ incus-volume-e3b67bf05e20c6c977f161a425733b00efe88834914e7fc8dd910c4b51cd5804 ┊ filesystem             ┊ images           ┊
    ╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
    # linstor snapshot list
    ╭───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
    ┊ ResourceName                                  ┊ SnapshotName                                  ┊ NodeNames          ┊ Volumes               ┊ CreatedOn           ┊ State      ┊
    ╞═══════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════════╡
    ┊ incus-volume-4df9f0598bd14d73953151614428d298 ┊ incus-volume-19dd0b015d22445f8097ba5e740948de ┊ server01, server02 ┊ 0: 10 GiB, 1: 500 MiB ┊ 2025-03-13 16:01:31 ┊ Successful ┊
    ╰───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
    # linstor resource-definition list-properties incus-volume-4df9f0598bd14d73953151614428d298
    ╭─────────────────────────────────────────────────────────────────────────────────────╮
    ┊ Key                                 ┊ Value                                         ┊
    ╞═════════════════════════════════════════════════════════════════════════════════════╡
    ┊ Aux/Incus/SnapshotName/snap0        ┊ incus-volume-19dd0b015d22445f8097ba5e740948de ┊
    ┊ Aux/Incus/content-type              ┊ block                                         ┊
    ┊ Aux/Incus/name                      ┊ incus-volume-v1                               ┊
    ┊ Aux/Incus/type                      ┊ virtual-machines                              ┊
    ┊ DrbdOptions/Net/allow-two-primaries ┊ yes                                           ┊
    ┊ DrbdOptions/Resource/on-no-quorum   ┊ io-error                                      ┊
    ┊ DrbdOptions/Resource/quorum         ┊ majority                                      ┊
    ┊ DrbdOptions/auto-verify-alg         ┊ crct10dif                                     ┊
    ┊ DrbdPrimarySetOn                    ┊ SERVER01                                      ┊
    ┊ cloned-from                         ┊ incus-volume-d0531e21ce6b4218b9b8996582b9bf31 ┊
    ╰─────────────────────────────────────────────────────────────────────────────────────╯

</div>

</div>

</div>

</div>

</div>

<div class="related-pages">

<a href="../storage_truenas/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

TrueNAS - <span class="pre">`truenas`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../../howto/storage_linstor_setup/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

How to set up LINSTOR with Incus

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">