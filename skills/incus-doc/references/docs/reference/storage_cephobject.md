# Ceph Object - <span class="pre">`cephobject`</span><a href="#ceph-object-cephobject" class="headerlink"
title="Link to this heading">¶</a>

<a href="https://ceph.io/en/" class="reference external">Ceph</a> is an
open-source storage platform that stores its data in a storage cluster
based on RADOS. It is highly scalable and, as a distributed system
without a single point of failure, very reliable.

Ceph provides different components for block storage and for file
systems.

<a href="https://docs.ceph.com/en/latest/radosgw/"
class="reference external">Ceph Object Gateway</a> is an object storage
interface built on top of
<a href="https://docs.ceph.com/en/latest/rados/api/librados-intro/"
class="reference external"><span class="pre"><code
class="docutils literal notranslate">librados</code></span></a> to
provide applications with a RESTful gateway to
<a href="https://docs.ceph.com/en/latest/rados/"
class="reference external">Ceph Storage Clusters</a>. It provides object
storage functionality with an interface that is compatible with a large
subset of the Amazon S3 RESTful API.

<div id="terminology" class="section">

## Terminology<a href="#terminology" class="headerlink"
title="Link to this heading">¶</a>

Ceph uses the term *object* for the data that it stores. The daemon that
is responsible for storing and managing data is the *Ceph OSD*. Ceph’s
storage is divided into *pools*, which are logical partitions for
storing objects. They are also referred to as *data pools*, *storage
pools* or *OSD pools*.

A *Ceph Object Gateway* consists of several OSD pools and one or more
*Ceph Object Gateway daemon* (<span class="pre">`radosgw`</span>)
processes that provide object gateway functionality.

</div>

<div id="cephobject-driver-in-incus" class="section">

## <span class="pre">`cephobject`</span> driver in Incus<a href="#cephobject-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

The <span class="pre">`cephobject`</span> driver can only be used for
buckets.

For storage volumes, use the
<a href="../storage_ceph/#storage-ceph" class="reference internal"><span
class="std std-ref">Ceph</span></a> or
<a href="../storage_cephfs/#storage-cephfs"
class="reference internal"><span class="std std-ref">CephFS</span></a>
drivers.

</div>

Unlike other storage drivers, this driver does not set up the storage
system but assumes that you already have a Ceph cluster installed.

You must set up a <span class="pre">`radosgw`</span> environment
beforehand and ensure that its HTTP/HTTPS endpoint URL is reachable from
the Incus server or servers. See
<a href="https://docs.ceph.com/en/latest/install/manual-deployment/"
class="reference external">Manual Deployment</a> for information on how
to set up a Ceph cluster and
<a href="https://docs.ceph.com/en/latest/radosgw/"
class="reference external">Ceph Object Gateway</a> on how to set up a
<span class="pre">`radosgw`</span> environment.

The <span class="pre">`radosgw`</span> URL can be specified at pool
creation time using the <a href="#storage-cephobject-pool-config"
class="reference internal"><span class="std std-ref"><span
class="pre"><code
class="docutils literal notranslate">cephobject.radosgw.endpoint</code></span></span></a>
option.

Incus uses the <span class="pre">`radosgw-admin`</span> command to
manage buckets. So this command must be available and operational on the
Incus servers.

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

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`cephobject`</span> driver and for storage
buckets in these pools.

<div id="storage-pool-configuration" class="section">

<span id="storage-cephobject-pool-config"></span>

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_cephobject-common:cephobject.bucket_name_prefix"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephobject.bucket_name_prefix`</span></span><span class="shortdesc"></span>

Prefix to add to bucket names in Ceph

<span class="anchor"><a href="#storage_cephobject-common:cephobject.bucket_name_prefix"
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
class="docutils literal notranslate">cephobject.bucket_name_prefix</code></span></td>
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

<div id="storage_cephobject-common:cephobject.cluster_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephobject.cluster_name`</span></span><span class="shortdesc"></span>

The Ceph cluster to use

<span class="anchor"><a href="#storage_cephobject-common:cephobject.cluster_name"
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
class="docutils literal notranslate">cephobject.cluster_name</code></span></td>
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

<div id="storage_cephobject-common:cephobject.radosgw.endpoint"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephobject.radosgw.endpoint`</span></span><span class="shortdesc"></span>

URL of the <span class="pre">`radosgw`</span> gateway process

<span class="anchor"><a href="#storage_cephobject-common:cephobject.radosgw.endpoint"
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
class="docutils literal notranslate">cephobject.radosgw.endpoint</code></span></td>
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

<div id="storage_cephobject-common:cephobject.radosgw.endpoint_cert_file"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephobject.radosgw.endpoint_cert_file`</span></span><span class="shortdesc"></span>

Path to the file containing the TLS client certificate to use for
endpoint communication

<span class="anchor"><a
href="#storage_cephobject-common:cephobject.radosgw.endpoint_cert_file"
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
class="docutils literal notranslate">cephobject.radosgw.endpoint_cert_file</code></span></td>
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

<div id="storage_cephobject-common:cephobject.user.name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cephobject.user.name`</span></span><span class="shortdesc"></span>

The Ceph user to use

<span class="anchor"><a href="#storage_cephobject-common:cephobject.user.name"
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
class="docutils literal notranslate">cephobject.user.name</code></span></td>
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

<div id="storage_cephobject-common:volatile.pool.pristine"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.pool.pristine`</span></span><span class="shortdesc"></span>

Whether the <span class="pre">`radosgw`</span>
<span class="pre">`incus-admin`</span> user existed at creation time

<span class="anchor"><a href="#storage_cephobject-common:volatile.pool.pristine"
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

</div>

<div id="storage-bucket-configuration" class="section">

### Storage bucket configuration<a href="#storage-bucket-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_bucket_cephobject-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Quota of the storage bucket

<span class="anchor"><a href="#storage_bucket_cephobject-common:size"
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
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="related-pages">

<a href="../storage_linstor/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

LINSTOR - <span class="pre">`linstor`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_cephfs/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

CephFS - <span class="pre">`cephfs`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">