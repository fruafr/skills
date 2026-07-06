# Btrfs - <span class="pre">`btrfs`</span><a href="#btrfs-btrfs" class="headerlink"
title="Link to this heading">¶</a>

Btrfs is a local file system based on the COW principle. COW means that
data is stored to a different block after it has been modified instead
of overwriting the existing data, reducing the risk of data corruption.
Unlike other file systems, Btrfs is extent-based, which means that it
stores data in contiguous areas of memory.

In addition to basic file system features, Btrfs offers RAID and volume
management, pooling, snapshots, checksums, compression and other
features.

To use Btrfs, make sure you have <span class="pre">`btrfs-progs`</span>
installed on your machine.

<div id="terminology" class="section">

## Terminology<a href="#terminology" class="headerlink"
title="Link to this heading">¶</a>

A Btrfs file system can have *subvolumes*, which are named binary
subtrees of the main tree of the file system with their own independent
file and directory hierarchy. A *Btrfs snapshot* is a special type of
subvolume that captures a specific state of another subvolume. Snapshots
can be read-write or read-only.

</div>

<div id="btrfs-driver-in-incus" class="section">

## <span class="pre">`btrfs`</span> driver in Incus<a href="#btrfs-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

The <span class="pre">`btrfs`</span> driver in Incus uses a subvolume
per instance, image and snapshot. When creating a new entity (for
example, launching a new instance), it creates a Btrfs snapshot.

Btrfs doesn’t natively support storing block devices. Therefore, when
using Btrfs for VMs, Incus creates a big file on disk to store the VM.
This approach is not very efficient and might cause issues when creating
snapshots.

Btrfs can be used as a storage backend inside a container in a nested
Incus environment. In this case, the parent container itself must use
Btrfs. Note, however, that the nested Incus setup does not inherit the
Btrfs quotas from the parent (see
<a href="#storage-btrfs-quotas" class="reference internal"><span
class="std std-ref">Quotas</span></a> below).

<div id="quotas" class="section">

<span id="storage-btrfs-quotas"></span>

### Quotas<a href="#quotas" class="headerlink" title="Link to this heading">¶</a>

Btrfs supports storage quotas via qgroups. Btrfs qgroups are
hierarchical, but new subvolumes will not automatically be added to the
qgroups of their parent subvolumes. This means that users can trivially
escape any quotas that are set. Therefore, if strict quotas are needed,
you should consider using a different storage driver (for example, ZFS
with <span class="pre">`refquota`</span> or LVM with Btrfs on top).

When using quotas, you must take into account that Btrfs extents are
immutable. When blocks are written, they end up in new extents. The old
extents remain until all their data is dereferenced or rewritten. This
means that a quota can be reached even if the total amount of space used
by the current files in the subvolume is smaller than the quota.

<div class="admonition note">

Note

This issue is seen most often when using VMs on Btrfs, due to the random
I/O nature of using raw disk image files on top of a Btrfs subvolume.

Therefore, you should never use VMs with Btrfs storage pools.

If you really need to use VMs with Btrfs storage pools, set the instance
root disk’s
<a href="../devices_disk/#devices-disk" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">size.state</code></span></span></a>
property to twice the size of the root disk’s size. This configuration
allows all blocks in the disk image file to be rewritten without
reaching the qgroup quota. The per-volume
<span class="pre">`btrfs.compression`</span> option can also help with
this quota issue, as enabling compression reduces the maximum extent
size so that block rewrites don’t cause as much storage to be
double-tracked. It does however prevent Incus from disabling
copy-on-write on the volume, since the two are mutually exclusive. Set
<span class="pre">`btrfs.compression=none`</span> to keep copy-on-write
disabled.

</div>

</div>

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`btrfs`</span> driver and for storage volumes
in these pools.

<div id="storage-pool-configuration" class="section">

<span id="storage-btrfs-pool-config"></span>

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_btrfs-common:btrfs.create_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`btrfs.create_options`</span></span><span class="shortdesc"></span>

Additional options to pass to <span class="pre">`mkfs.btrfs`</span> when
creating the pool

<span class="anchor"><a href="#storage_btrfs-common:btrfs.create_options"
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
class="docutils literal notranslate">btrfs.create_options</code></span></td>
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
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_btrfs-common:btrfs.mount_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`btrfs.mount_options`</span></span><span class="shortdesc"></span>

Mount options for block devices

<span class="anchor"><a href="#storage_btrfs-common:btrfs.mount_options"
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
class="docutils literal notranslate">btrfs.mount_options</code></span></td>
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
class="docutils literal notranslate">user_subvol_rm_allowed</code></span></p></td>
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

<div id="storage_btrfs-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size of the storage pool when creating loop-based pools (in bytes,
suffixes supported, can be increased to grow storage pool)

<span class="anchor"><a href="#storage_btrfs-common:size" class="reference external"><em><img
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
<p>auto (20% of free disk space, &gt;= 5 GiB and &lt;= 30 GiB)</p></td>
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

<div id="storage_btrfs-common:source"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

Path to an existing block device, loop file or Btrfs subvolume

<span class="anchor"><a href="#storage_btrfs-common:source"
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

<div id="storage_btrfs-common:source.wipe"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source.wipe`</span></span><span class="shortdesc"></span>

Wipe the block device specified in <span class="pre">`source`</span>
prior to creating the storage pool

<span class="anchor"><a href="#storage_btrfs-common:source.wipe"
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

### Storage volume configuration<a href="#storage-volume-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_volume_btrfs-common:btrfs.compression"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`btrfs.compression`</span></span><span class="shortdesc"></span>

Compression algorithm to set on the volume, mapping to the Btrfs
<span class="pre">`compression`</span> property (for example
<span class="pre">`zstd`</span>, <span class="pre">`lzo`</span>,
<span class="pre">`zlib`</span> or <span class="pre">`none`</span>)

<span class="anchor"><a href="#storage_volume_btrfs-common:btrfs.compression"
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
class="docutils literal notranslate">btrfs.compression</code></span></td>
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
class="docutils literal notranslate">volume.btrfs.compression</code></span></p></td>
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

<div id="storage_volume_btrfs-common:initial.gid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.gid`</span></span><span class="shortdesc"></span>

GID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_btrfs-common:initial.gid"
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

<div id="storage_volume_btrfs-common:initial.mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.mode`</span></span><span class="shortdesc"></span>

Mode of the volume in the instance

<span class="anchor"><a href="#storage_volume_btrfs-common:initial.mode"
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

<div id="storage_volume_btrfs-common:initial.uid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.uid`</span></span><span class="shortdesc"></span>

UID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_btrfs-common:initial.uid"
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

<div id="storage_volume_btrfs-common:security.shared"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shared`</span></span><span class="shortdesc"></span>

Enable sharing the volume across multiple instances

<span class="anchor"><a href="#storage_volume_btrfs-common:security.shared"
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

<div id="storage_volume_btrfs-common:security.shifted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shifted`</span></span><span class="shortdesc"></span>

Enable ID shifting overlay (allows attach by multiple isolated
instances)

<span class="anchor"><a href="#storage_volume_btrfs-common:security.shifted"
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

<div id="storage_volume_btrfs-common:security.unmapped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.unmapped`</span></span><span class="shortdesc"></span>

Disable ID mapping for the volume

<span class="anchor"><a href="#storage_volume_btrfs-common:security.unmapped"
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

<div id="storage_volume_btrfs-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage volume

<span class="anchor"><a href="#storage_volume_btrfs-common:size"
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

<div id="storage_volume_btrfs-common:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_btrfs-common:snapshots.expiry"
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

<div id="storage_volume_btrfs-common:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_btrfs-common:snapshots.expiry.manual"
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

<div id="storage_volume_btrfs-common:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Pongo2 template string that represents the snapshot name (used for
scheduled snapshots and unnamed snapshots)
<a href="#id2" id="id1" class="footnote-reference brackets"
role="doc-noteref"><span class="fn-bracket">[</span>1<span
class="fn-bracket">]</span></a>

<span class="anchor"><a href="#storage_volume_btrfs-common:snapshots.pattern"
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

<div id="storage_volume_btrfs-common:snapshots.schedule"
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

<span class="anchor"><a href="#storage_volume_btrfs-common:snapshots.schedule"
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

<div id="storage_bucket_btrfs-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage bucket

<span class="anchor"><a href="#storage_bucket_btrfs-common:size"
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

<a href="../storage_lvm/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

LVM - <span class="pre">`lvm`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_dir/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Directory - <span class="pre">`dir`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">