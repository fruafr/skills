# LVM - <span class="pre">`lvm`</span><a href="#lvm-lvm" class="headerlink" title="Link to this heading">¶</a>

LVM is a storage management framework rather than a file system. It is
used to manage physical storage devices, allowing you to create a number
of logical storage volumes that use and virtualize the underlying
physical storage devices.

Note that it is possible to over-commit the physical storage in the
process, to allow flexibility for scenarios where not all available
storage is in use at the same time.

To use LVM, make sure you have <span class="pre">`lvm2`</span> installed
on your machine.

<div id="terminology" class="section">

## Terminology<a href="#terminology" class="headerlink"
title="Link to this heading">¶</a>

LVM can combine several physical storage devices into a *volume group*.
You can then allocate *logical volumes* of different types from this
volume group.

One supported volume type is a *thin pool*, which allows over-committing
the resources by creating thinly provisioned volumes whose total allowed
maximum size is larger than the available physical storage. Another type
is a *volume snapshot*, which captures a specific state of a logical
volume.

</div>

<div id="lvm-driver-in-incus" class="section">

## <span class="pre">`lvm`</span> driver in Incus<a href="#lvm-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

The <span class="pre">`lvm`</span> driver in Incus uses logical volumes
for images, and volume snapshots for instances and snapshots.

Incus assumes that it has full control over the volume group. Therefore,
you should not maintain any file system entities that are not owned by
Incus in an LVM volume group, because Incus might delete them. However,
if you need to reuse an existing volume group (for example, because your
setup has only one volume group), you can do so by setting the
<a href="#storage-lvm-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">lvm.vg.force_reuse</code></span></span></a>
configuration.

By default, LVM storage pools use an LVM thin pool and create logical
volumes for all Incus storage entities (images, instances and custom
volumes) in there. This behavior can be changed by setting
<a href="#storage-lvm-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">lvm.use_thinpool</code></span></span></a>
to <span class="pre">`false`</span> when you create the pool. In this
case, Incus uses “normal” logical volumes for all storage entities that
are not snapshots. Note that this entails serious performance and space
reductions for the <span class="pre">`lvm`</span> driver (close to the
<span class="pre">`dir`</span> driver both in speed and storage usage).
The reason for this is that most storage operations must fall back to
using <span class="pre">`rsync`</span>, because logical volumes that are
not thin pools do not support snapshots of snapshots. In addition,
non-thin snapshots take up much more storage space than thin snapshots,
because they must reserve space for their maximum size at creation time.
Therefore, this option should only be chosen if the use case requires
it.

For environments with a high instance turnover (for example, continuous
integration) you should tweak the backup
<span class="pre">`retain_min`</span> and
<span class="pre">`retain_days`</span> settings in
<span class="pre">`/etc/lvm/lvm.conf`</span> to avoid slowdowns when
interacting with Incus.

</div>

<div id="lvmcluster-driver-in-incus" class="section">

<span id="storage-lvmcluster"></span>

## <span class="pre">`lvmcluster`</span> driver in Incus<a href="#lvmcluster-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

A second <span class="pre">`lvmcluster`</span> driver is available for
use within clusters.

This relies on the <span class="pre">`lvmlockd`</span> and
<span class="pre">`sanlock`</span> daemons to provide distributed
locking over a shared disk or set of disks.

It allows using a remote shared block device like a
<span class="pre">`FiberChannel`</span>` `<span class="pre">`LUN`</span>,
<span class="pre">`NVMEoF/NVMEoTCP`</span> disk or
<span class="pre">`iSCSI`</span> drive as the backing for a LVM storage
pool.

<div class="admonition note">

Note

Thin provisioning is incompatible with clustered LVM, so expect higher
disk usage.

</div>

To use this with Incus, you must:

- Have a shared block device available on all your cluster members

- Install the relevant packages for <span class="pre">`lvm`</span>,
  <span class="pre">`lvmlockd`</span> and
  <span class="pre">`sanlock`</span>

- Enable <span class="pre">`lvmlockd`</span> by setting
  <span class="pre">`use_lvmlockd`</span>` `<span class="pre">`=`</span>` `<span class="pre">`1`</span>
  in your <span class="pre">`/etc/lvm/lvm.conf`</span>

- Set a unique (within your cluster) <span class="pre">`host_id`</span>
  value in <span class="pre">`/etc/lvm/lvmlocal.conf`</span>

- Ensure that both <span class="pre">`lvmlockd`</span> and
  <span class="pre">`sanlock`</span> daemons are running

<div class="admonition warning">

Warning

<span class="pre">`lvmcluster`</span> has the following limitations:

- Snapshots for shared custom storage volumes are not supported.

- Snapshots of raw block volumes are not supported.

- Only <span class="pre">`raw`</span> custom volumes support the
  <span class="pre">`security.shared`</span> option.

</div>

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`lvm`</span> driver and for storage volumes in
these pools.

<div id="storage-pool-configuration" class="section">

<span id="storage-lvm-pool-config"></span>

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_lvm-common:block.type"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.type`</span></span><span class="shortdesc"></span>

Type of the block volume

<span class="anchor"><a href="#storage_lvm-common:block.type"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">block.type</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.block.type</code></span></p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>block-based volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.metadata_size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.metadata_size`</span></span><span class="shortdesc"></span>

The size of the metadata space for the physical volume.

<span class="anchor"><a href="#storage_lvm-common:lvm.metadata_size"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.metadata_size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">0</code></span> (auto)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.thinpool_metadata_size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.thinpool_metadata_size`</span></span><span class="shortdesc"></span>

The size of the thin pool metadata volume (the default is to let LVM
calculate an appropriate size). Not usable with
<span class="pre">`lvmcluster`</span>.

<span class="anchor"><a href="#storage_lvm-common:lvm.thinpool_metadata_size"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.thinpool_metadata_size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">0</code></span> (auto)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.thinpool_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.thinpool_name`</span></span><span class="shortdesc"></span>

Thin pool where volumes are created. Not usable with
<span class="pre">`lvmcluster`</span>.

<span class="anchor"><a href="#storage_lvm-common:lvm.thinpool_name"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.thinpool_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">IncusThinPool</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.use_thinpool"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.use_thinpool`</span></span><span class="shortdesc"></span>

Whether the storage pool uses a thin pool for logical volumes. Not
usable with <span class="pre">`lvmcluster`</span>.

<span class="anchor"><a href="#storage_lvm-common:lvm.use_thinpool"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.use_thinpool</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">true</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.vg.force_reuse"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.vg.force_reuse`</span></span><span class="shortdesc"></span>

Force using an existing non-empty volume group. Not usable with
<span class="pre">`lvmcluster`</span>.

<span class="anchor"><a href="#storage_lvm-common:lvm.vg.force_reuse"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.vg.force_reuse</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:lvm.vg_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.vg_name`</span></span><span class="shortdesc"></span>

Name of the volume group to create.

<span class="anchor"><a href="#storage_lvm-common:lvm.vg_name"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.vg_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>name of the pool</p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Storage pool size (in bytes, suffixes supported, can be increased to
grow storage pool). <span class="pre">`lvmcluster`</span> pools: cannot
set at creation, but can be updated.

<span class="anchor"><a href="#storage_lvm-common:size" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto (20% of free disk space, &gt;= 5 GiB and &lt;= 30 GiB) for <span
class="pre"><code
class="docutils literal notranslate">lvm</code></span>.</p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:source"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

Path to an existing block device, loop file or LVM volume group.

<span class="anchor"><a href="#storage_lvm-common:source" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">source</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<ul>
<li></li>
</ul></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_lvm-common:source.wipe"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source.wipe`</span></span><span class="shortdesc"></span>

Wipe the block device specified in <span class="pre">`source`</span>
prior to creating the storage pool.

<span class="anchor"><a href="#storage_lvm-common:source.wipe"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">source.wipe</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Scope:</strong></td>
<td><span class="ignoreP"></span>
<p>local</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="admonition tip">

Tip

In addition to these configurations, you can also set default values for
the storage volume configurations. See
<a href="../../howto/storage_volumes/#storage-configure-vol-default"
class="reference internal"><span class="std std-ref">Configure default
values for storage volumes</span></a>.

</div>

</div>

<div id="storage-volume-configuration" class="section">

<span id="storage-lvm-vol-config"></span>

### Storage volume configuration<a href="#storage-volume-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_volume_lvm-common:block.create_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.create_options`</span></span><span class="shortdesc"></span>

Additional options to pass to the file system creation tool when
formatting the volume

<span class="anchor"><a href="#storage_volume_lvm-common:block.create_options"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">block.create_options</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.block.create_options</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>block-based volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:block.filesystem"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.filesystem`</span></span><span class="shortdesc"></span>

File system of the storage volume: <span class="pre">`btrfs`</span>,
<span class="pre">`ext4`</span> or <span class="pre">`xfs`</span>
(<span class="pre">`ext4`</span> if not set)

<span class="anchor"><a href="#storage_volume_lvm-common:block.filesystem"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">block.filesystem</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.block.filesystem</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>block-based volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:block.mount_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.mount_options`</span></span><span class="shortdesc"></span>

Mount options for block-backed file system volumes

<span class="anchor"><a href="#storage_volume_lvm-common:block.mount_options"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">block.mount_options</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.block.mount_options</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>block-based volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:initial.gid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.gid`</span></span><span class="shortdesc"></span>

GID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_lvm-common:initial.gid"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">initial.gid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>int</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.initial.gid</code></span> or
<span class="pre"><code
class="docutils literal notranslate">0</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:initial.mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.mode`</span></span><span class="shortdesc"></span>

Mode of the volume in the instance

<span class="anchor"><a href="#storage_volume_lvm-common:initial.mode"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">initial.mode</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>int</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.initial.mode</code></span>
or <span class="pre"><code
class="docutils literal notranslate">711</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:initial.uid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.uid`</span></span><span class="shortdesc"></span>

UID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_lvm-common:initial.uid"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">initial.uid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>int</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.initial.uid</code></span> or
<span class="pre"><code
class="docutils literal notranslate">0</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume with content type <span class="pre"><code
class="docutils literal notranslate">filesystem</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:lvm.stripes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.stripes`</span></span><span class="shortdesc"></span>

Number of stripes to use for new volumes (or thin pool volume)

<span class="anchor"><a href="#storage_volume_lvm-common:lvm.stripes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.stripes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.lvm.stripes</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:lvm.stripes.size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvm.stripes.size`</span></span><span class="shortdesc"></span>

Size of stripes to use (at least 4096 bytes and multiple of 512 bytes)

<span class="anchor"><a href="#storage_volume_lvm-common:lvm.stripes.size"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvm.stripes.size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.lvm.stripes.size</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:lvmcluster.remove_snapshots"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`lvmcluster.remove_snapshots`</span></span><span class="shortdesc"></span>

Remove snapshots as needed

<span class="anchor"><a href="#storage_volume_lvm-common:lvmcluster.remove_snapshots"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">lvmcluster.remove_snapshots</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.lvmcluster.remove_snapshots</code></span>
or <span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:security.shared"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shared`</span></span><span class="shortdesc"></span>

Enable sharing the volume across multiple instances

<span class="anchor"><a href="#storage_volume_lvm-common:security.shared"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.shared</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.security.shared</code></span>
or <span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom block volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:security.shifted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shifted`</span></span><span class="shortdesc"></span>

Enable ID shifting overlay (allows attach by multiple isolated
instances)

<span class="anchor"><a href="#storage_volume_lvm-common:security.shifted"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.shifted</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.security.shifted</code></span>
or <span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:security.unmapped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.unmapped`</span></span><span class="shortdesc"></span>

Disable ID mapping for the volume

<span class="anchor"><a href="#storage_volume_lvm-common:security.unmapped"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.unmapped</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.security.unmapped</code></span>
or <span class="pre"><code
class="docutils literal notranslate">false</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage volume

<span class="anchor"><a href="#storage_volume_lvm-common:size"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>default: same as <span class="pre"><code
class="docutils literal notranslate">volume.size</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_lvm-common:snapshots.expiry"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">snapshots.expiry</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.snapshot.expiry</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

This value is used to compute the expiry date of newly created
snapshots. It is added to the current time when a snapshot is taken, and
the resulting timestamp is stored as that snapshot’s individual expiry
date. Changing this value only affects snapshots created after the
change; the expiry date of existing snapshots is not modified.

The supported units are <span class="pre">`S`</span> (seconds),
<span class="pre">`M`</span> (minutes), <span class="pre">`H`</span>
(hours), <span class="pre">`d`</span> (days),
<span class="pre">`w`</span> (weeks), <span class="pre">`m`</span>
(months) and <span class="pre">`y`</span> (years). Note that
<span class="pre">`M`</span> stands for minutes and
<span class="pre">`m`</span> for months. Each unit may only be specified
once, and months and years are computed as calendar months and years
rather than fixed numbers of days.

</div>

</div>

<div id="storage_volume_lvm-common:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_lvm-common:snapshots.expiry.manual"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">snapshots.expiry.manual</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.snapshot.expiry.manual</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

This value is used to compute the expiry date of newly created
snapshots. It is added to the current time when a snapshot is taken, and
the resulting timestamp is stored as that snapshot’s individual expiry
date. Changing this value only affects snapshots created after the
change; the expiry date of existing snapshots is not modified.

The supported units are <span class="pre">`S`</span> (seconds),
<span class="pre">`M`</span> (minutes), <span class="pre">`H`</span>
(hours), <span class="pre">`d`</span> (days),
<span class="pre">`w`</span> (weeks), <span class="pre">`m`</span>
(months) and <span class="pre">`y`</span> (years). Note that
<span class="pre">`M`</span> stands for minutes and
<span class="pre">`m`</span> for months. Each unit may only be specified
once, and months and years are computed as calendar months and years
rather than fixed numbers of days.

</div>

</div>

<div id="storage_volume_lvm-common:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Pongo2 template string that represents the snapshot name (used for
scheduled snapshots and unnamed snapshots)
<a href="#id2" id="id1" class="footnote-reference brackets"
role="doc-noteref"><span class="fn-bracket">[</span>1<span
class="fn-bracket">]</span></a>

<span class="anchor"><a href="#storage_volume_lvm-common:snapshots.pattern"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">snapshots.pattern</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.snapshot.pattern</code></span>
or <span class="pre"><code
class="docutils literal notranslate">snap%d</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_lvm-common:snapshots.schedule"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.schedule`</span></span><span class="shortdesc"></span>

Cron expression
(<span class="pre">`<minute>`</span>` `<span class="pre">`<hour>`</span>` `<span class="pre">`<dom>`</span>` `<span class="pre">`<month>`</span>` `<span class="pre">`<dow>`</span>),
a comma-separated list of schedule aliases
(<span class="pre">`@hourly`</span>, <span class="pre">`@daily`</span>,
<span class="pre">`@midnight`</span>,
<span class="pre">`@weekly`</span>, <span class="pre">`@monthly`</span>,
<span class="pre">`@annually`</span>,
<span class="pre">`@yearly`</span>), or empty to disable automatic
snapshots (the default)

<span class="anchor"><a href="#storage_volume_lvm-common:snapshots.schedule"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">snapshots.schedule</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.snapshot.schedule</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>custom volume</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="storage-bucket-configuration" class="section">

### Storage bucket configuration<a href="#storage-bucket-configuration" class="headerlink"
title="Link to this heading">¶</a>

To enable storage buckets for local storage pool drivers and allow
applications to access the buckets via the S3 protocol, you must
configure the
<a href="../../server_config/#server-core:core.storage_buckets_address"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">core.storage_buckets_address</code></span></a>
server setting.

<div id="storage_bucket_lvm-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage bucket

<span class="anchor"><a href="#storage_bucket_lvm-common:size"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>same as <span class="pre"><code
class="docutils literal notranslate">volume.size</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>appropriate driver</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

</div>

</div>

------------------------------------------------------------------------

<span class="label"><span class="fn-bracket">\[</span><a href="#id1" role="doc-backlink">1</a><span class="fn-bracket">\]</span></span>

The <span class="pre">`snapshots.pattern`</span> option takes a Pongo2
template string to format the snapshot name.

To add a time stamp to the snapshot name, use the Pongo2 context
variable <span class="pre">`creation_date`</span>. Make sure to format
the date in your template string to avoid forbidden characters in the
snapshot name. For example, set
<span class="pre">`snapshots.pattern`</span> to
<span class="pre">`{{`</span>` `<span class="pre">`creation_date|date:'2006-01-02_15-04-05'`</span>` `<span class="pre">`}}`</span>
to name the snapshots after their time of creation, down to the
precision of a second.

Another way to avoid name collisions is to use the placeholder
<span class="pre">`%d`</span> in the pattern. For the first snapshot,
the placeholder is replaced with <span class="pre">`0`</span>. For
subsequent snapshots, the existing snapshot names are taken into account
to find the highest number at the placeholder’s position. This number is
then incremented by one for the new name.

</div>

<div class="related-pages">

<a href="../storage_zfs/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

ZFS - <span class="pre">`zfs`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_btrfs/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Btrfs - <span class="pre">`btrfs`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">