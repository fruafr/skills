# Directory - <span class="pre">`dir`</span><a href="#directory-dir" class="headerlink"
title="Link to this heading">¶</a>

The directory storage driver is a basic backend that stores its data in
a standard file and directory structure. This driver is quick to set up
and allows inspecting the files directly on the disk, which can be
convenient for testing. However, Incus operations are
<a href="../storage_drivers/#storage-drivers-features"
class="reference internal"><span class="std std-ref">not
optimized</span></a> for this driver.

<div id="dir-driver-in-incus" class="section">

## <span class="pre">`dir`</span> driver in Incus<a href="#dir-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

The <span class="pre">`dir`</span> driver in Incus is fully functional
and provides the same set of features as other drivers. However, it is
much slower than all the other drivers because it must unpack images and
do instant copies of instances, snapshots and images.

Unless specified differently during creation (with the
<span class="pre">`source`</span> configuration option), the data is
stored in the <span class="pre">`/var/lib/incus/storage-pools/`</span>
directory.

<div id="quotas" class="section">

<span id="storage-dir-quotas"></span>

### Quotas<a href="#quotas" class="headerlink" title="Link to this heading">¶</a>

The <span class="pre">`dir`</span> driver supports storage quotas when
running on either ext4 or XFS with project quotas enabled at the file
system level.

</div>

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`dir`</span> driver and for storage volumes in
these pools.

<div id="storage-pool-configuration" class="section">

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_dir-common:rsync.bwlimit"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`rsync.bwlimit`</span></span><span class="shortdesc"></span>

The upper limit to be placed on the socket I/O when
<span class="pre">`rsync`</span> must be used to transfer storage
entities

<span class="anchor"><a href="#storage_dir-common:rsync.bwlimit"
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
class="docutils literal notranslate">rsync.bwlimit</code></span></td>
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
class="docutils literal notranslate">0</code></span> (no limit)</p></td>
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

<div id="storage_dir-common:rsync.compression"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`rsync.compression`</span></span><span class="shortdesc"></span>

Whether to use compression while migrating storage pools

<span class="anchor"><a href="#storage_dir-common:rsync.compression"
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
class="docutils literal notranslate">rsync.compression</code></span></td>
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

<div id="storage_dir-common:source"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

Path to an existing directory

<span class="anchor"><a href="#storage_dir-common:source" class="reference external"><em><img
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

<div id="storage_volume_dir-common:initial.gid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.gid`</span></span><span class="shortdesc"></span>

GID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_dir-common:initial.gid"
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

<div id="storage_volume_dir-common:initial.mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.mode`</span></span><span class="shortdesc"></span>

Mode of the volume in the instance

<span class="anchor"><a href="#storage_volume_dir-common:initial.mode"
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

<div id="storage_volume_dir-common:initial.uid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.uid`</span></span><span class="shortdesc"></span>

UID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_dir-common:initial.uid"
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

<div id="storage_volume_dir-common:security.shared"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shared`</span></span><span class="shortdesc"></span>

Enable sharing the volume across multiple instances

<span class="anchor"><a href="#storage_volume_dir-common:security.shared"
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

<div id="storage_volume_dir-common:security.shifted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shifted`</span></span><span class="shortdesc"></span>

Enable ID shifting overlay (allows attach by multiple isolated
instances)

<span class="anchor"><a href="#storage_volume_dir-common:security.shifted"
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

<div id="storage_volume_dir-common:security.size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.size`</span></span><span class="shortdesc"></span>

Size/quota of the storage volume

<span class="anchor"><a href="#storage_volume_dir-common:security.size"
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
class="docutils literal notranslate">security.size</code></span></td>
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

<div id="storage_volume_dir-common:security.unmapped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.unmapped`</span></span><span class="shortdesc"></span>

Disable ID mapping for the volume

<span class="anchor"><a href="#storage_volume_dir-common:security.unmapped"
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

<div id="storage_volume_dir-common:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_dir-common:snapshots.expiry"
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

<div id="storage_volume_dir-common:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_dir-common:snapshots.expiry.manual"
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

<div id="storage_volume_dir-common:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Pongo2 template string that represents the snapshot name (used for
scheduled snapshots and unnamed snapshots)
<a href="#id2" id="id1" class="footnote-reference brackets"
role="doc-noteref"><span class="fn-bracket">[</span>1<span
class="fn-bracket">]</span></a>

<span class="anchor"><a href="#storage_volume_dir-common:snapshots.pattern"
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

<div id="storage_volume_dir-common:snapshots.schedule"
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

<span class="anchor"><a href="#storage_volume_dir-common:snapshots.schedule"
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

Storage buckets do not have any configuration for
<span class="pre">`dir`</span> pools. Unlike the other storage pool
drivers, the <span class="pre">`dir`</span> driver does not support
bucket quotas via the <span class="pre">`size`</span> setting.

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

<a href="../storage_btrfs/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Btrfs - <span class="pre">`btrfs`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_drivers/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Storage drivers

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">