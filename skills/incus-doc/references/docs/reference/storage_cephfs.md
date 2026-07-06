# CephFS - <span class="pre">`cephfs`</span><a href="#cephfs-cephfs" class="headerlink"
title="Link to this heading">¶</a>

<a href="https://ceph.io/en/" class="reference external">Ceph</a> is an
open-source storage platform that stores its data in a storage cluster
based on RADOS. It is highly scalable and, as a distributed system
without a single point of failure, very reliable.

Ceph provides different components for block storage and for file
systems.

CephFS is Ceph’s file system component that provides a robust,
fully-featured POSIX-compliant distributed file system. Internally, it
maps files to Ceph objects and stores file metadata (for example, file
ownership, directory paths, access permissions) in a separate data pool.

<div id="terminology" class="section">

## Terminology<a href="#terminology" class="headerlink"
title="Link to this heading">¶</a>

Ceph uses the term *object* for the data that it stores. The daemon that
is responsible for storing and managing data is the *Ceph OSD*. Ceph’s
storage is divided into *pools*, which are logical partitions for
storing objects. They are also referred to as *data pools*, *storage
pools* or *OSD pools*.

A *CephFS file system* consists of two OSD storage pools, one for the
actual data and one for the file metadata.

</div>

<div id="cephfs-driver-in-incus" class="section">

## <span class="pre">`cephfs`</span> driver in Incus<a href="#cephfs-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

The <span class="pre">`cephfs`</span> driver can only be used for custom
storage volumes with content type <span class="pre">`filesystem`</span>.

For other storage volumes, use the
<a href="../storage_ceph/#storage-ceph" class="reference internal"><span
class="std std-ref">Ceph</span></a> driver. That driver can also be used
for custom storage volumes with content type
<span class="pre">`filesystem`</span>, but it implements them through
Ceph RBD images.

</div>

Unlike other storage drivers, this driver does not set up the storage
system but assumes that you already have a Ceph cluster installed.

You can either create the CephFS file system that you want to use
beforehand and specify it through the
<a href="#storage-cephfs-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">source</code></span></span></a>
option, or specify the
<a href="#storage-cephfs-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">cephfs.create_missing</code></span></span></a>
option to automatically create the file system and the data and metadata
OSD pools (with the names given in
<a href="#storage-cephfs-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">cephfs.data_pool</code></span></span></a>
and
<a href="#storage-cephfs-pool-config" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">cephfs.meta_pool</code></span></span></a>).

This driver also behaves differently than other drivers in that it
provides remote storage. As a result and depending on the internal
network, storage access might be a bit slower than for local storage. On
the other hand, using remote storage has big advantages in a cluster
setup, because all cluster members have access to the same storage pools
with the exact same contents, without the need to synchronize storage
pools.

Incus assumes that it has full control over the OSD storage pool.
Therefore, you should never maintain any file system entities that are
not owned by Incus in an Incus OSD storage pool, because Incus might
delete them.

The <span class="pre">`cephfs`</span> driver in Incus supports snapshots
if snapshots are enabled on the server side.

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`cephfs`</span> driver and for storage volumes
in these pools.

<div id="storage-pool-configuration" class="section">

<span id="storage-cephfs-pool-config"></span>

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_cephfs-common:cephfs.cluster_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.cluster_name`</span></span><span class="shortdesc"></span>

Name of the Ceph cluster that contains the CephFS file system

<span class="anchor"><a href="#storage_cephfs-common:cephfs.cluster_name"
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
class="docutils literal notranslate">cephfs.cluster_name</code></span></td>
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
class="docutils literal notranslate">ceph</code></span></p></td>
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

<div id="storage_cephfs-common:cephfs.create_missing"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.create_missing`</span></span><span class="shortdesc"></span>

Create the file system and the missing data and metadata OSD pools

<span class="anchor"><a href="#storage_cephfs-common:cephfs.create_missing"
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
class="docutils literal notranslate">cephfs.create_missing</code></span></td>
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
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_cephfs-common:cephfs.data_pool"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.data_pool`</span></span><span class="shortdesc"></span>

Data OSD pool name to create for the file system

<span class="anchor"><a href="#storage_cephfs-common:cephfs.data_pool"
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
class="docutils literal notranslate">cephfs.data_pool</code></span></td>
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

<div id="storage_cephfs-common:cephfs.fscache"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.fscache`</span></span><span class="shortdesc"></span>

Enable use of kernel <span class="pre">`fscache`</span> and
<span class="pre">`cachefilesd`</span>

<span class="anchor"><a href="#storage_cephfs-common:cephfs.fscache"
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
class="docutils literal notranslate">cephfs.fscache</code></span></td>
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
<p>global</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_cephfs-common:cephfs.meta_pool"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.meta_pool`</span></span><span class="shortdesc"></span>

Metadata OSD pool name to create for the file system

<span class="anchor"><a href="#storage_cephfs-common:cephfs.meta_pool"
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
class="docutils literal notranslate">cephfs.meta_pool</code></span></td>
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

<div id="storage_cephfs-common:cephfs.osd_pg_num"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.osd_pg_num`</span></span><span class="shortdesc"></span>

OSD pool <span class="pre">`pg_num`</span> to use when creating missing
OSD pools

<span class="anchor"><a href="#storage_cephfs-common:cephfs.osd_pg_num"
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
class="docutils literal notranslate">cephfs.osd_pg_num</code></span></td>
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

<div id="storage_cephfs-common:cephfs.path"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.path`</span></span><span class="shortdesc"></span>

The base path for the CephFS mount

<span class="anchor"><a href="#storage_cephfs-common:cephfs.path"
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
class="docutils literal notranslate">cephfs.path</code></span></td>
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
class="docutils literal notranslate">/</code></span></p></td>
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

<div id="storage_cephfs-common:cephfs.user.name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephfs.user.name`</span></span><span class="shortdesc"></span>

The Ceph user to use

<span class="anchor"><a href="#storage_cephfs-common:cephfs.user.name"
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
class="docutils literal notranslate">cephfs.user.name</code></span></td>
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
class="docutils literal notranslate">admin</code></span></p></td>
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

<div id="storage_cephfs-common:source"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

Existing CephFS file system or file system path to use

<span class="anchor"><a href="#storage_cephfs-common:source"
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

<div id="storage_cephfs-common:volatile.pool.pristine"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.pool.pristine`</span></span><span class="shortdesc"></span>

Whether the CephFS file system was empty on creation time

<span class="anchor"><a href="#storage_cephfs-common:volatile.pool.pristine"
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
class="docutils literal notranslate">volatile.pool.pristine</code></span></td>
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

<div id="storage_volume_cephfs-common:initial.gid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.gid`</span></span><span class="shortdesc"></span>

GID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_cephfs-common:initial.gid"
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

<div id="storage_volume_cephfs-common:initial.mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.mode`</span></span><span class="shortdesc"></span>

Mode of the volume in the instance

<span class="anchor"><a href="#storage_volume_cephfs-common:initial.mode"
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

<div id="storage_volume_cephfs-common:initial.uid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.uid`</span></span><span class="shortdesc"></span>

UID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_cephfs-common:initial.uid"
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

<div id="storage_volume_cephfs-common:security.shared"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shared`</span></span><span class="shortdesc"></span>

Enable sharing the volume across multiple instances

<span class="anchor"><a href="#storage_volume_cephfs-common:security.shared"
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

<div id="storage_volume_cephfs-common:security.shifted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shifted`</span></span><span class="shortdesc"></span>

Enable ID shifting overlay (allows attach by multiple isolated
instances)

<span class="anchor"><a href="#storage_volume_cephfs-common:security.shifted"
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

<div id="storage_volume_cephfs-common:security.unmapped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.unmapped`</span></span><span class="shortdesc"></span>

Disable ID mapping for the volume

<span class="anchor"><a href="#storage_volume_cephfs-common:security.unmapped"
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

<div id="storage_volume_cephfs-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage volume

<span class="anchor"><a href="#storage_volume_cephfs-common:size"
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

<div id="storage_volume_cephfs-common:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_cephfs-common:snapshots.expiry"
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

<div id="storage_volume_cephfs-common:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_cephfs-common:snapshots.expiry.manual"
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

<div id="storage_volume_cephfs-common:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Pongo2 template string that represents the snapshot name (used for
scheduled snapshots and unnamed snapshots)
<a href="#id2" id="id1" class="footnote-reference brackets"
role="doc-noteref"><span class="fn-bracket">[</span>1<span
class="fn-bracket">]</span></a>

<span class="anchor"><a href="#storage_volume_cephfs-common:snapshots.pattern"
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

<div id="storage_volume_cephfs-common:snapshots.schedule"
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

<span class="anchor"><a href="#storage_volume_cephfs-common:snapshots.schedule"
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

<a href="../storage_cephobject/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Ceph Object - <span class="pre">`cephobject`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_ceph/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Ceph RBD - <span class="pre">`ceph`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">