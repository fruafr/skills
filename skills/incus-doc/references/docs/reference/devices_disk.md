# Type: <span class="pre">`disk`</span><a href="#type-disk" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

The <span class="pre">`disk`</span> device type is supported for both
containers and VMs. It supports hotplugging for both containers and VMs.

</div>

Disk devices supply additional storage to instances.

For containers, they are essentially mount points inside the instance
(either as a bind-mount of an existing file or directory on the host,
or, if the source is a block device, a regular mount). Virtual machines
share host-side mounts or directories through
<span class="pre">`9p`</span> or <span class="pre">`virtiofs`</span> (if
available), or as VirtIO disks for block-based disks.

<div class="admonition warning">

Warning

The device name affects the serial generated for the device. If the
device name exceeds 14 characters for <span class="pre">`nvme`</span>
and <span class="pre">`virtio-blk`</span>, or 30 characters for
<span class="pre">`virtio-scsi`</span>, Incus will hash the device value
to ensure the generated serial remains within supported length
constraints. The device name itself is left unchanged.

</div>

<div id="types-of-disk-devices" class="section">

<span id="devices-disk-types"></span>

## Types of disk devices<a href="#types-of-disk-devices" class="headerlink"
title="Link to this heading">¶</a>

You can create disk devices from different sources. The value that you
specify for the <span class="pre">`source`</span> option specifies the
type of disk device that is added:

Storage volume  
The most common type of disk device is a storage volume. To add a
storage volume, specify its name as the
<span class="pre">`source`</span> of the device:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk pool=<pool_name> source=<volume_name> [path=<path_in_instance>]

</div>

</div>

The path is required for file system volumes, but not for block volumes.

Alternatively, you can use the <a
href="../manpages/incus/storage/volume/attach/#incus-storage-volume-attach-md"
class="reference internal"><span class="std std-ref"><span
class="pre"><code
class="docutils literal notranslate">incus</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">storage</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">volume</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">attach</code></span></span></a>
command to <a href="../../howto/storage_volumes/#storage-attach-volume"
class="reference internal"><span class="std std-ref">Attach the volume
to an instance</span></a>. Both commands use the same mechanism to add a
storage volume as a disk device.

It’s possible to attach a sub-path of a custom volume to an instance
using the <span class="pre">`source=<volume_name>/<sub_path>`</span>
syntax. If the sub-path doesn’t exist inside the custom volume, it will
be created automatically when the device is started using the
<span class="pre">`initial.XYZ`</span> configuration keys for ownership
and permissions.

Path on the host  
You can share a path on your host (either a file system or a block
device) to your instance by adding it as a disk device with the host
path as the <span class="pre">`source`</span>:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=<path_on_host> [path=<path_in_instance>]

</div>

</div>

The path is required for file systems, but not for block devices.

Ceph RBD  
Incus can use Ceph to manage an internal file system for the instance,
but if you have an existing, externally managed Ceph RBD that you would
like to use for an instance, you can add it with the following command:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=ceph:<pool_name>/<volume_name> ceph.user_name=<user_name> ceph.cluster_name=<cluster_name> [path=<path_in_instance>]

</div>

</div>

The path is required for file systems, but not for block devices.

CephFS  
Incus can use Ceph to manage an internal file system for the instance,
but if you have an existing, externally managed Ceph file system that
you would like to use for an instance, you can add it with the following
command:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=cephfs:<fs_name>/<path> ceph.user_name=<user_name> ceph.cluster_name=<cluster_name> path=<path_in_instance>

</div>

</div>

ISO file  
You can add an ISO file as a disk device for a virtual machine. It is
added as a ROM device inside the VM.

This source type is applicable only to VMs.

To add an ISO file, specify its file path as the
<span class="pre">`source`</span>:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=<file_path_on_host>

</div>

</div>

VM <span class="pre">`cloud-init`</span>  
You can generate a <span class="pre">`cloud-init`</span> configuration
ISO from the <a
href="../instance_options/#instance-cloud-init:cloud-init.vendor-data"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">cloud-init.vendor-data</code></span></a>
and
<a href="../instance_options/#instance-cloud-init:cloud-init.user-data"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">cloud-init.user-data</code></span></a>
configuration keys and attach it to a virtual machine. The
<span class="pre">`cloud-init`</span> that is running inside the VM then
detects the drive on boot and applies the configuration.

This source type is applicable only to VMs.

To add such a device, use the following command:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=cloud-init:config

</div>

</div>

VM <span class="pre">`agent`</span>  
You can generate an <span class="pre">`agent`</span> configuration ISO
which will contain the agent binary, configuration files and
installation scripts. This is required for environments where
<span class="pre">`9p`</span> isn’t supported and where an alternative
way to load the agent is required.

This source type is applicable only to VMs.

To add such a device, use the following command:

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=agent:config

</div>

</div>

Tmpfs  
You can back a disk device with an in-memory file system by using the
<span class="pre">`tmpfs:`</span> source.

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=tmpfs: path=<path_in_instance> [size=<size>] [initial.uid=<uid>] [initial.gid=<gid>] [initial.mode=<mode>]

</div>

</div>

Both <span class="pre">`source`</span> and
<span class="pre">`path`</span> are required. This creates a
<span class="pre">`tmpfs`</span> mount inside the instance and supports
optional properties for size, ownership, and permissions.

Tmpfs with overlayfs behavior  
If you want the same tmpfs behavior but combined with overlayfs
semantics, use <span class="pre">`tmpfs-overlay:`</span> as the source.

<div class="highlight-none notranslate">

<div class="highlight">

    incus config device add <instance_name> <device_name> disk source=tmpfs-overlay: path=<path_in_instance> [size=<size>] [initial.uid=<uid>] [initial.gid=<gid>] [initial.mode=<mode>]

</div>

</div>

Both <span class="pre">`source`</span> and
<span class="pre">`path`</span> are required. Additionally, the target
<span class="pre">`path`</span> must already exist inside the container.
This provides an ephemeral in-memory file system with overlayfs
handling.

</div>

<div id="initial-volume-configuration-for-instance-root-disk-devices"
class="section">

<span id="devices-disk-initial-config"></span>

## Initial volume configuration for instance root disk devices<a href="#initial-volume-configuration-for-instance-root-disk-devices"
class="headerlink" title="Link to this heading">¶</a>

Initial volume configuration allows setting specific configurations for
the root disk devices of new instances. These settings are prefixed with
<span class="pre">`initial.`</span> and are only applied when the
instance is created. This method allows creating instances that have
unique configurations, independent of the default storage pool settings.

For example, you can add an initial volume configuration for
<span class="pre">`zfs.block_mode`</span> to an existing profile, and
this will then take effect for each new instance you create using this
profile:

<div class="highlight-none notranslate">

<div class="highlight">

    incus profile device set <profile_name> <device_name> initial.zfs.block_mode=true

</div>

</div>

You can also set an initial configuration directly when creating an
instance. For example:

<div class="highlight-none notranslate">

<div class="highlight">

    incus init <image> <instance_name> --device <device_name>,initial.zfs.block_mode=true

</div>

</div>

Note that you cannot use initial volume configurations with custom
volume options or to set the volume’s size.

</div>

<div id="initial-uid-initial-gid-and-initial-mode" class="section">

<span id="devices-disk-initial-uid-gid-mode"></span>

## <span class="pre">`initial.uid`</span>, <span class="pre">`initial.gid`</span> and <span class="pre">`initial.mode`</span><a href="#initial-uid-initial-gid-and-initial-mode" class="headerlink"
title="Link to this heading">¶</a>

<span class="pre">`initial.uid`</span>,
<span class="pre">`initial.gid`</span> and
<span class="pre">`initial.mode`</span> apply in three different
scenarios:

- On root disk devices, they are passed to the storage driver to set the
  ownership and mode of the instance’s root volume at creation time
  (when supported by the driver).

- On <span class="pre">`tmpfs:`</span> and
  <span class="pre">`tmpfs-overlay:`</span> disk devices, they are
  translated into <span class="pre">`uid=`</span>,
  <span class="pre">`gid=`</span> and <span class="pre">`mode=`</span>
  mount options for the underlying <span class="pre">`tmpfs`</span>
  mount.

- On custom volume disks where the <span class="pre">`source`</span>
  includes a sub-path (for example
  <span class="pre">`source=myvol/sub/path`</span>), they are used as
  the ownership and mode of any sub-directory that has to be created
  automatically when the device is started.

In all cases, <span class="pre">`initial.uid`</span> and
<span class="pre">`initial.gid`</span> default to
<span class="pre">`0`</span> and <span class="pre">`initial.mode`</span>
defaults to <span class="pre">`0711`</span> (octal).

</div>

<div id="device-options" class="section">

## Device options<a href="#device-options" class="headerlink"
title="Link to this heading">¶</a>

<span class="pre">`disk`</span> devices have the following device
options:

<div id="devices-disk:attached" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Only for VMs: Whether the disk is attached or ejected

<span class="anchor"><a href="#devices-disk:attached" class="reference external"><em><img
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
class="docutils literal notranslate">attached</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-disk:boot.priority"
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
class="docutils literal notranslate">boot.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:ceph.cluster_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ceph.cluster_name`</span></span><span class="shortdesc"></span>

The cluster name of the Ceph cluster (required for Ceph or CephFS
sources)

<span class="anchor"><a href="#devices-disk:ceph.cluster_name"
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
class="docutils literal notranslate">ceph.cluster_name</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:ceph.user_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ceph.user_name`</span></span><span class="shortdesc"></span>

The user name of the Ceph cluster (required for Ceph or CephFS sources)

<span class="anchor"><a href="#devices-disk:ceph.user_name"
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
class="docutils literal notranslate">ceph.user_name</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:dependent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`dependent`</span></span><span class="shortdesc"></span>

Specifies if the disk is instance dependent

<span class="anchor"><a href="#devices-disk:dependent" class="reference external"><em><img
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
class="docutils literal notranslate">dependent</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:initial.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`initial.*`</span></span><span class="shortdesc"></span>

Initial volume configuration for instance root disk devices

<span class="anchor"><a href="#devices-disk:initial.*" class="reference external"><em><img
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
class="docutils literal notranslate">initial.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

For root disk devices, this is used to override the storage pool’s
default volume configuration when creating the instance’s root volume.

For custom volumes, only <span class="pre">`initial.uid`</span>,
<span class="pre">`initial.gid`</span> and
<span class="pre">`initial.mode`</span> are accepted and they are used
when auto-creating sub-directories inside the custom volume (when the
<span class="pre">`source`</span> includes a sub-path that doesn’t
exist).

<span class="pre">`initial.uid`</span>,
<span class="pre">`initial.gid`</span> and
<span class="pre">`initial.mode`</span> are also used to set the
ownership and mode of the file system when the
<span class="pre">`source`</span> is <span class="pre">`tmpfs:`</span>
or <span class="pre">`tmpfs-overlay:`</span>.

</div>

</div>

<div id="devices-disk:io.bus" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Only for VMs: Override the bus for the device

<span class="anchor"><a href="#devices-disk:io.bus" class="reference external"><em><img
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
class="docutils literal notranslate">io.bus</code></span></td>
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
class="docutils literal notranslate">virtio-scsi</code></span> for
block, <span class="pre"><code
class="docutils literal notranslate">auto</code></span> for file
system</p></td>
</tr>
<tr class="even row-even">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

This controls what bus a disk device should be attached to.

For block devices (disks), this is one of:

- <span class="pre">`nvme`</span>

- <span class="pre">`virtio-blk`</span>

- <span class="pre">`virtio-scsi`</span> (default)

- <span class="pre">`usb`</span>

For file systems (shared directories or custom volumes), this is one of:

- <span class="pre">`9p`</span>

- <span class="pre">`auto`</span> (default)
  (<span class="pre">`virtiofs`</span> if possible, else
  <span class="pre">`9p`</span>)

- <span class="pre">`virtiofs`</span>

<span class="pre">`9p`</span> doesn’t support hotplugging and
<span class="pre">`virtiofs`</span> doesn’t support live migration.
<span class="pre">`auto`</span> tries to use
<span class="pre">`virtiofs`</span> if possible
(<span class="pre">`migration.stateful`</span> not set to
<span class="pre">`true`</span> and host support for
<span class="pre">`virtiofsd`</span>) and falls back to
<span class="pre">`9p`</span> otherwise.

</div>

</div>

<div id="devices-disk:io.cache" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.cache`</span></span><span class="shortdesc"></span>

Only for VMs: Override the caching mode for the device

<span class="anchor"><a href="#devices-disk:io.cache" class="reference external"><em><img
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
class="docutils literal notranslate">io.cache</code></span></td>
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
class="docutils literal notranslate">none</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

This controls what bus a disk device should be attached to.

For block devices (disks), this is one of:

- <span class="pre">`none`</span> (default)

- <span class="pre">`writeback`</span>

- <span class="pre">`unsafe`</span>

For file systems (shared directories or custom volumes), this is one of:

- <span class="pre">`none`</span> (default)

- <span class="pre">`metadata`</span>

- <span class="pre">`unsafe`</span>

</div>

</div>

<div id="devices-disk:limits.max"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.max`</span></span><span class="shortdesc"></span>

I/O limit in byte/s or IOPS for both read and write (same as setting
both <span class="pre">`limits.read`</span> and
<span class="pre">`limits.write`</span>)

<span class="anchor"><a href="#devices-disk:limits.max" class="reference external"><em><img
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
class="docutils literal notranslate">limits.max</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:limits.read"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.read`</span></span><span class="shortdesc"></span>

I/O limit in byte/s (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>) or in IOPS (must be suffixed with
<span class="pre">`iops`</span>) - see also
<a href="../../howto/storage_volumes/#storage-configure-io"
class="reference internal"><span class="std std-ref">Configure I/O
limits</span></a>

<span class="anchor"><a href="#devices-disk:limits.read" class="reference external"><em><img
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
class="docutils literal notranslate">limits.read</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:limits.write"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.write`</span></span><span class="shortdesc"></span>

I/O limit in byte/s (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>) or in IOPS (must be suffixed with
<span class="pre">`iops`</span>) - see also
<a href="../../howto/storage_volumes/#storage-configure-io"
class="reference internal"><span class="std std-ref">Configure I/O
limits</span></a>

<span class="anchor"><a href="#devices-disk:limits.write" class="reference external"><em><img
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
class="docutils literal notranslate">limits.write</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:path" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`path`</span></span><span class="shortdesc"></span>

Path inside the instance where the disk will be mounted (only for file
system disk devices)

<span class="anchor"><a href="#devices-disk:path" class="reference external"><em><img
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
class="docutils literal notranslate">path</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

This controls which path inside the instance the disk should be mounted
on.

With containers, this option supports mounting file system disk devices,
and paths and single files within them.

With VMs, this option supports mounting file system disk devices and
paths within them. Mounting single files is not supported.

</div>

</div>

<div id="devices-disk:pool" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`pool`</span></span><span class="shortdesc"></span>

The storage pool to which the disk device belongs (only applicable for
storage volumes managed by Incus)

<span class="anchor"><a href="#devices-disk:pool" class="reference external"><em><img
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
class="docutils literal notranslate">pool</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:propagation"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`propagation`</span></span><span class="shortdesc"></span>

Controls how a bind-mount is shared between the instance and the host
(can be one of <span class="pre">`private`</span>, the default, or
<span class="pre">`shared`</span>, <span class="pre">`slave`</span>,
<span class="pre">`unbindable`</span>,
<span class="pre">`rshared`</span>, <span class="pre">`rslave`</span>,
<span class="pre">`runbindable`</span>,
<span class="pre">`rprivate`</span>; see the Linux Kernel <a
href="https://www.kernel.org/doc/Documentation/filesystems/sharedsubtree.txt"
class="reference external">shared subtree</a> documentation for a full
explanation)

<span class="anchor"><a href="#devices-disk:propagation" class="reference external"><em><img
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
class="docutils literal notranslate">propagation</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:raw.mount.options"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.mount.options`</span></span><span class="shortdesc"></span>

File system specific mount options

<span class="anchor"><a href="#devices-disk:raw.mount.options"
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
class="docutils literal notranslate">raw.mount.options</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:readonly" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`readonly`</span></span><span class="shortdesc"></span>

Controls whether to make the mount read-only

<span class="anchor"><a href="#devices-disk:readonly" class="reference external"><em><img
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
class="docutils literal notranslate">readonly</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:recursive"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`recursive`</span></span><span class="shortdesc"></span>

Controls whether to recursively mount the source path

<span class="anchor"><a href="#devices-disk:recursive" class="reference external"><em><img
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
class="docutils literal notranslate">recursive</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:required" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`required`</span></span><span class="shortdesc"></span>

Controls whether to fail if the source doesn’t exist

<span class="anchor"><a href="#devices-disk:required" class="reference external"><em><img
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
class="docutils literal notranslate">required</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:shift" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`shift`</span></span><span class="shortdesc"></span>

Sets up a shifting overlay to translate the source UID/GID to match the
instance (only for containers)

<span class="anchor"><a href="#devices-disk:shift" class="reference external"><em><img
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
class="docutils literal notranslate">shift</code></span></td>
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:size" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size`</span></span><span class="shortdesc"></span>

Disk size in bytes (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>) - only supported for the
<span class="pre">`rootfs`</span> (<span class="pre">`/`</span>)

<span class="anchor"><a href="#devices-disk:size" class="reference external"><em><img
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:size.state"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`size.state`</span></span><span class="shortdesc"></span>

Same as <span class="pre">`size`</span>, but applies to the file-system
volume used for saving runtime state in VMs

<span class="anchor"><a href="#devices-disk:size.state" class="reference external"><em><img
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
class="docutils literal notranslate">size.state</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:source" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`source`</span></span><span class="shortdesc"></span>

Source of a file system or block device (see
<a href="#devices-disk-types" class="reference internal"><span
class="std std-ref">Types of disk devices</span></a> for details)

<span class="anchor"><a href="#devices-disk:source" class="reference external"><em><img
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
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-disk:wwn" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`wwn`</span></span><span class="shortdesc"></span>

Only for VMs: Set the disk World Wide Name (only supported on
<span class="pre">`virtio-scsi`</span> bus)

<span class="anchor"><a href="#devices-disk:wwn" class="reference external"><em><img
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
class="docutils literal notranslate">wwn</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>``</p></td>
</tr>
<tr class="even row-even">
<td><strong>Required:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

</div>

</div>

<div class="related-pages">

<a href="../devices_unix_char/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Type: <span class="pre">`unix-char`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../devices_nic/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Type: <span class="pre">`nic`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">