# TrueNAS - <span class="pre">`truenas`</span><a href="#truenas-truenas" class="headerlink"
title="Link to this heading">¶</a>

<div id="the-truenas-storage-driver-in-incus" class="section">

## The <span class="pre">`truenas`</span> storage driver in Incus<a href="#the-truenas-storage-driver-in-incus" class="headerlink"
title="Link to this heading">¶</a>

The <span class="pre">`truenas`</span> storage driver enables an Incus
node to use a remote TrueNAS storage server to host one or more Incus
storage pools. When the node is part of a cluster, all cluster members
can access the storage pool simultaneously, making it ideal for use
cases such as live migrating virtual machines (VMs) between nodes.

The driver operates in a block-based manner, meaning that all Incus
volumes are created as ZFS Volume block devices on the remote TrueNAS
server. These ZFS Volume block devices are accessed on the local Incus
node via iSCSI.

Modeled after the existing ZFS driver, the
<span class="pre">`truenas`</span> driver supports most standard ZFS
functionality, but operates on remote TrueNAS servers. For instance, a
local VM can be snapshotted and cloned, with the snapshot and clone
operations performed on the remote server after synchronizing the local
file system. The clone is then activated through iSCSI as necessary.

Each storage pool corresponds to a ZFS dataset on a remote TrueNAS host.
The dataset is created automatically if it does not exist. The driver
uses ZFS features available on the remote host to support efficient
image handling, copy operations, and snapshot management without
requiring nested ZFS (ZFS-on-ZFS).

To reference a remote dataset, the <span class="pre">`source`</span>
property can be specified in the form:
<span class="pre">`[<remote`</span>` `<span class="pre">`host>:]<remote`</span>` `<span class="pre">`pool>[[/<remote`</span>` `<span class="pre">`dataset>]...][/]`</span>

If the path ends with a trailing <span class="pre">`/`</span>, the
dataset name will be derived from the Incus storage pool name (e.g.,
<span class="pre">`tank/pool1`</span>).

</div>

<div id="requirements" class="section">

## Requirements<a href="#requirements" class="headerlink"
title="Link to this heading">¶</a>

The driver relies on the
<a href="https://github.com/truenas/truenas_incus_ctl"
class="reference external"><span class="pre"><code
class="docutils literal notranslate">truenas_incus_ctl</code></span></a>
tool to interact with the TrueNAS API and perform actions on the remote
server. This tool also manages the activation and deactivation of remote
ZFS Volumes via <span class="pre">`open-iscsi`</span>. If
<span class="pre">`truenas_incus_ctl`</span> is not installed or
available in the system’s PATH, the driver will be disabled.

To install the required tool, download the latest version (v0.7.2+ is
required) from the
<a href="https://github.com/truenas/truenas_incus_ctl"
class="reference external"><span class="pre"><code
class="docutils literal notranslate">truenas\_incus\_ctl</code></span>
GitHub page</a>. Additionally, ensure that
<span class="pre">`open-iscsi`</span> is installed on the system, which
can be done using:

<div class="highlight-none notranslate">

<div class="highlight">

    sudo apt install open-iscsi

</div>

</div>

</div>

<div id="logging-in-to-the-truenas-host" class="section">

## Logging in to the TrueNAS host<a href="#logging-in-to-the-truenas-host" class="headerlink"
title="Link to this heading">¶</a>

As an alternative to manually creating an API Key and supplying using
the <span class="pre">`truenas.api_key`</span> property, you can instead
<span class="pre">`login`</span> to the remote server using the
<span class="pre">`truenas_incus_ctl`</span> tool.

<div class="highlight-none notranslate">

<div class="highlight">

    sudo truenas_incus_ctl config login

</div>

</div>

This will prompt you to provide connection details for the TrueNAS
server, including authentication details, and will save the
configuration to a local file. After logging in, you can verify the
iSCSI setup with:

<div class="highlight-none notranslate">

<div class="highlight">

    sudo truenas_incus_ctl share iscsi setup --test

</div>

</div>

Once the tool is configured, you can use it to interact with remote
datasets and create storage pools:

<div class="highlight-none notranslate">

<div class="highlight">

    incus storage create <poolname> truenas source=[host:]<pool>[/<dataset>]/[remote-poolname]

</div>

</div>

In this command:

- <span class="pre">`source`</span> refers to the location on the remote
  TrueNAS host where the storage pool will be created.

- <span class="pre">`host`</span> is optional, and can be specified
  using the <span class="pre">`truenas.host`</span> property, or by
  specifying a configuration with
  <span class="pre">`truenas.config`</span>

- If <span class="pre">`remote-poolname`</span> is not supplied, it will
  default to the name of the local pool.

</div>

<div id="configuration-options" class="section">

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration options are available for storage pools that
use the <span class="pre">`truenas`</span> driver and for storage
volumes in these pools.

<div id="storage-pool-configuration" class="section">

<span id="storage-truenas-pool-config"></span>

### Storage pool configuration<a href="#storage-pool-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_truenas-common:source"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

ZFS dataset to use on the remote TrueNAS host. Format:
<span class="pre">`[<host>:]<pool>[/<dataset>][/]`</span>. If
<span class="pre">`host`</span> is omitted here, it must be set via
<span class="pre">`truenas.host`</span>.

<span class="anchor"><a href="#storage_truenas-common:source"
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

<div id="storage_truenas-common:truenas.allow_insecure"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.allow_insecure`</span></span><span class="shortdesc"></span>

If set to <span class="pre">`true`</span>, allows insecure (non-TLS)
connections to the TrueNAS API.

<span class="anchor"><a href="#storage_truenas-common:truenas.allow_insecure"
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
class="docutils literal notranslate">truenas.allow_insecure</code></span></td>
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

<div id="storage_truenas-common:truenas.api_key"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.api_key`</span></span><span class="shortdesc"></span>

API key used to authenticate with the TrueNAS host.

<span class="anchor"><a href="#storage_truenas-common:truenas.api_key"
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
class="docutils literal notranslate">truenas.api_key</code></span></td>
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

<div id="storage_truenas-common:truenas.clone_copy"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.clone_copy`</span></span><span class="shortdesc"></span>

Whether to use lightweight clones rather than full dataset copies.

<span class="anchor"><a href="#storage_truenas-common:truenas.clone_copy"
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
class="docutils literal notranslate">truenas.clone_copy</code></span></td>
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

<div id="storage_truenas-common:truenas.config"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.config`</span></span><span class="shortdesc"></span>

Path to a configuration file for the TrueNAS client tool.

<span class="anchor"><a href="#storage_truenas-common:truenas.config"
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
class="docutils literal notranslate">truenas.config</code></span></td>
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

<div id="storage_truenas-common:truenas.dataset"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.dataset`</span></span><span class="shortdesc"></span>

Remote dataset name. Typically inferred from
<span class="pre">`source`</span>, but can be overridden.

<span class="anchor"><a href="#storage_truenas-common:truenas.dataset"
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
class="docutils literal notranslate">truenas.dataset</code></span></td>
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

<div id="storage_truenas-common:truenas.force_reuse"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.force_reuse`</span></span><span class="shortdesc"></span>

Allow to use an existing non-empty pool.

<span class="anchor"><a href="#storage_truenas-common:truenas.force_reuse"
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
class="docutils literal notranslate">truenas.force_reuse</code></span></td>
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

<div id="storage_truenas-common:truenas.host"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.host`</span></span><span class="shortdesc"></span>

Hostname or IP address of the remote TrueNAS system. Optional if
included in the <span class="pre">`source`</span>, or a configuration is
used.

<span class="anchor"><a href="#storage_truenas-common:truenas.host"
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
class="docutils literal notranslate">truenas.host</code></span></td>
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

<div id="storage_truenas-common:truenas.initiator"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.initiator`</span></span><span class="shortdesc"></span>

iSCSI initiator name used during block volume attachment.

<span class="anchor"><a href="#storage_truenas-common:truenas.initiator"
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
class="docutils literal notranslate">truenas.initiator</code></span></td>
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

<div id="storage_truenas-common:truenas.portal"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.portal`</span></span><span class="shortdesc"></span>

iSCSI portal address to use for block volume connections.

<span class="anchor"><a href="#storage_truenas-common:truenas.portal"
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
class="docutils literal notranslate">truenas.portal</code></span></td>
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

<span id="storage-truenas-vol-config"></span>

### Storage volume configuration<a href="#storage-volume-configuration" class="headerlink"
title="Link to this heading">¶</a>

<div id="storage_volume_truenas-common:block.create_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.create_options`</span></span><span class="shortdesc"></span>

Additional options to pass to the file system creation tool when
formatting the volume

<span class="anchor"><a href="#storage_volume_truenas-common:block.create_options"
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
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_truenas-common:block.filesystem"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.filesystem`</span></span><span class="shortdesc"></span>

File system of the storage volume: <span class="pre">`btrfs`</span>,
<span class="pre">`ext4`</span> or <span class="pre">`xfs`</span>
(<span class="pre">`ext4`</span> if not set)

<span class="anchor"><a href="#storage_volume_truenas-common:block.filesystem"
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
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_truenas-common:block.mount_options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`block.mount_options`</span></span><span class="shortdesc"></span>

Mount options for block-backed file system volumes

<span class="anchor"><a href="#storage_volume_truenas-common:block.mount_options"
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
<ul>
<li></li>
</ul></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="storage_volume_truenas-common:initial.gid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.gid`</span></span><span class="shortdesc"></span>

GID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_truenas-common:initial.gid"
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

<div id="storage_volume_truenas-common:initial.mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.mode`</span></span><span class="shortdesc"></span>

Mode of the volume in the instance

<span class="anchor"><a href="#storage_volume_truenas-common:initial.mode"
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

<div id="storage_volume_truenas-common:initial.uid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.uid`</span></span><span class="shortdesc"></span>

UID of the volume owner in the instance

<span class="anchor"><a href="#storage_volume_truenas-common:initial.uid"
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

<div id="storage_volume_truenas-common:security.shared"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shared`</span></span><span class="shortdesc"></span>

Enable sharing the volume across multiple instances

<span class="anchor"><a href="#storage_volume_truenas-common:security.shared"
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

<div id="storage_volume_truenas-common:security.shifted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.shifted`</span></span><span class="shortdesc"></span>

Enable ID shifting overlay (allows attach by multiple isolated
instances)

<span class="anchor"><a href="#storage_volume_truenas-common:security.shifted"
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

<div id="storage_volume_truenas-common:security.unmapped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.unmapped`</span></span><span class="shortdesc"></span>

Disable ID mapping for the volume

<span class="anchor"><a href="#storage_volume_truenas-common:security.unmapped"
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

<div id="storage_volume_truenas-common:size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Size/quota of the storage volume

<span class="anchor"><a href="#storage_volume_truenas-common:size"
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

<div id="storage_volume_truenas-common:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_truenas-common:snapshots.expiry"
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

<div id="storage_volume_truenas-common:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

Controls when newly created snapshots are to be deleted (expects an
expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>)

<span class="anchor"><a href="#storage_volume_truenas-common:snapshots.expiry.manual"
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

<div id="storage_volume_truenas-common:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Pongo2 template string that represents the snapshot name (used for
scheduled snapshots and unnamed snapshots)

<span class="anchor"><a href="#storage_volume_truenas-common:snapshots.pattern"
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

<div id="storage_volume_truenas-common:snapshots.schedule"
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

<span class="anchor"><a href="#storage_volume_truenas-common:snapshots.schedule"
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

<div id="storage_volume_truenas-common:truenas.blocksize"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.blocksize`</span></span><span class="shortdesc"></span>

Size of the ZFS block in range from 512 bytes to 16 MiB (must be power
of 2) - for block volume, a maximum value of 128 KiB will be used even
if a higher value is set

<span class="anchor"><a href="#storage_volume_truenas-common:truenas.blocksize"
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
class="docutils literal notranslate">truenas.blocksize</code></span></td>
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
class="docutils literal notranslate">volume.truenas.blocksize</code></span></p></td>
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

<div id="storage_volume_truenas-common:truenas.remove_snapshots"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.remove_snapshots`</span></span><span class="shortdesc"></span>

Remove snapshots as needed

<span class="anchor"><a href="#storage_volume_truenas-common:truenas.remove_snapshots"
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
class="docutils literal notranslate">truenas.remove_snapshots</code></span></td>
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
class="docutils literal notranslate">volume.truenas.remove_snapshots</code></span>
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

<div id="storage_volume_truenas-common:truenas.use_refquota"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`truenas.use_refquota`</span></span><span class="shortdesc"></span>

Use <span class="pre">`refquota`</span> instead of
<span class="pre">`quota`</span> for space

<span class="anchor"><a href="#storage_volume_truenas-common:truenas.use_refquota"
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
class="docutils literal notranslate">truenas.use_refquota</code></span></td>
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
class="docutils literal notranslate">volume.truenas.use_refquota</code></span>
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

</div>

</div>

</div>

</div>

<div class="related-pages">

<a href="../../networks/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Networking

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../storage_linstor_internals/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

<span class="pre">`linstor`</span> driver internals

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">