# Instance options<a href="#instance-options" class="headerlink"
title="Link to this heading">¶</a>

Instance options are configuration options that are directly related to
the instance.

See
<a href="../../howto/instances_configure/#instances-configure-options"
class="reference internal"><span class="std std-ref">Configure instance
options</span></a> for instructions on how to set the instance options.

The key/value configuration is namespaced. The following options are
available:

- <a href="#instance-options-misc" class="reference internal"><span
  class="std std-ref">Miscellaneous options</span></a>

- <a href="#instance-options-boot" class="reference internal"><span
  class="std std-ref">Boot-related options</span></a>

- <a href="#instance-options-cloud-init" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">cloud-init</code></span>
  configuration</span></a>

- <a href="#instance-options-limits" class="reference internal"><span
  class="std std-ref">Resource limits</span></a>

- <a href="#instance-options-migration" class="reference internal"><span
  class="std std-ref">Migration options</span></a>

- <a href="#instance-options-nvidia" class="reference internal"><span
  class="std std-ref">NVIDIA and CUDA configuration</span></a>

- <a href="#instance-options-oci" class="reference internal"><span
  class="std std-ref">OCI configuration</span></a>

- <a href="#instance-options-raw" class="reference internal"><span
  class="std std-ref">Raw instance configuration overrides</span></a>

- <a href="#instance-options-security" class="reference internal"><span
  class="std std-ref">Security policies</span></a>

- <a href="#instance-options-snapshots" class="reference internal"><span
  class="std std-ref">Snapshot scheduling and configuration</span></a>

- <a href="#instance-options-volatile" class="reference internal"><span
  class="std std-ref">Volatile internal data</span></a>

Note that while a type is defined for each option, all values are stored
as strings and should be exported over the REST API as strings (which
makes it possible to support any extra values without breaking backward
compatibility).

<div id="miscellaneous-options" class="section">

<span id="instance-options-misc"></span>

## Miscellaneous options<a href="#miscellaneous-options" class="headerlink"
title="Link to this heading">¶</a>

In addition to the configuration options listed in the following
sections, these instance options are supported:

<div id="instance-miscellaneous:agent.nic_config"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`agent.nic_config`</span></span><span class="shortdesc"></span>

Whether to use the name and MTU of the default network interfaces

<span class="anchor"><a href="#instance-miscellaneous:agent.nic_config"
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
class="docutils literal notranslate">agent.nic_config</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

For containers, the name and MTU of the default network interfaces is
used for the instance devices. For virtual machines, set this option to
<span class="pre">`true`</span> to set the name and MTU of the default
network interfaces to be the same as the instance devices.

</div>

</div>

<div id="instance-miscellaneous:cluster.evacuate"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cluster.evacuate`</span></span><span class="shortdesc"></span>

What to do when evacuating the instance

<span class="anchor"><a href="#instance-miscellaneous:cluster.evacuate"
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
class="docutils literal notranslate">cluster.evacuate</code></span></td>
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
class="docutils literal notranslate">auto</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

The <span class="pre">`cluster.evacuate`</span> provides control over
how instances are handled when a cluster member is being evacuated.

Available Modes:

- <span class="pre">`auto`</span> *(default)*: The system will
  automatically decide the best evacuation method based on the
  instance’s type and configured devices:

  - If any device is not suitable for migration, the instance will not
    be migrated (only stopped).

  - Live migration will be used only for virtual machines with the
    <span class="pre">`migration.stateful`</span> setting enabled and
    for which all its devices can be migrated as well.

- <span class="pre">`live-migrate`</span>: Instances are live-migrated
  to another server. This means the instance remains running and
  operational during the migration process, ensuring minimal disruption.

- <span class="pre">`migrate`</span>: In this mode, instances are
  migrated to another server in the cluster. The migration process will
  not be live, meaning there will be a brief downtime for the instance
  during the migration.

- <span class="pre">`stop`</span>: Instances are not migrated. Instead,
  they are stopped on the current server.

- <span class="pre">`stateful-stop`</span>: Instances are not migrated.
  Instead, they are stopped on the current server but with their runtime
  state (memory) stored on disk for resuming on restore.

- <span class="pre">`force-stop`</span>: Instances are not migrated.
  Instead, they are forcefully stopped.

See <a href="../../howto/cluster_manage/#cluster-evacuate"
class="reference internal"><span class="std std-ref">Evacuate and
restore cluster members</span></a> for more information.

</div>

</div>

<div id="instance-miscellaneous:environment.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`environment.*`</span></span><span class="shortdesc"></span>

Free-form environment key/value

<span class="anchor"><a href="#instance-miscellaneous:environment.*"
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
class="docutils literal notranslate">environment.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Extra environment variables to set on boot and during exec.

</div>

</div>

<div id="instance-miscellaneous:linux.kernel_modules"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`linux.kernel_modules`</span></span><span class="shortdesc"></span>

Kernel modules to load before starting the instance

<span class="anchor"><a href="#instance-miscellaneous:linux.kernel_modules"
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
class="docutils literal notranslate">linux.kernel_modules</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Specify the kernel modules as a comma-separated list.

</div>

</div>

<div id="instance-miscellaneous:linux.sysctl.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`linux.sysctl.*`</span></span><span class="shortdesc"></span>

Override for the corresponding <span class="pre">`sysctl`</span> setting
in the container

<span class="anchor"><a href="#instance-miscellaneous:linux.sysctl.*"
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
class="docutils literal notranslate">linux.sysctl.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-miscellaneous:smbios11.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`smbios11.*`</span></span><span class="shortdesc"></span>

Free-form
<span class="pre">`SMBIOS`</span>` `<span class="pre">`Type`</span>` `<span class="pre">`11`</span>
key/value

<span class="anchor"><a href="#instance-miscellaneous:smbios11.*"
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
class="docutils literal notranslate">smbios11.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

<span class="pre">`SMBIOS`</span>` `<span class="pre">`Type`</span>` `<span class="pre">`11`</span>
configuration keys.

</div>

</div>

<div id="instance-miscellaneous:systemd.credential-binary.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`systemd.credential-binary.*`</span></span><span class="shortdesc"></span>

Systemd credential key/value, where value is Base64 encoded

<span class="anchor"><a href="#instance-miscellaneous:systemd.credential-binary.*"
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
class="docutils literal notranslate">systemd.credential-binary.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Systemd credential key/value pair passed as a read-only bind mount in
containers and as
<span class="pre">`SMBIOS`</span>` `<span class="pre">`Type`</span>` `<span class="pre">`11`</span>
data in virtual machines. The value is Base64 encoded.

</div>

</div>

<div id="instance-miscellaneous:systemd.credential.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`systemd.credential.*`</span></span><span class="shortdesc"></span>

Systemd credential key/value

<span class="anchor"><a href="#instance-miscellaneous:systemd.credential.*"
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
class="docutils literal notranslate">systemd.credential.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Systemd credential key/value pair passed as a read-only bind mount in
containers and as
<span class="pre">`SMBIOS`</span>` `<span class="pre">`Type`</span>` `<span class="pre">`11`</span>
data in virtual machines.

</div>

</div>

<div id="instance-miscellaneous:user.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`user.*`</span></span><span class="shortdesc"></span>

Free-form user key/value storage

<span class="anchor"><a href="#instance-miscellaneous:user.*"
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
class="docutils literal notranslate">user.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

User keys can be used in search.

</div>

</div>

<div id="instance-miscellaneous:environment.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`environment.*`</span></span><span class="shortdesc"></span>

Environment variables for the instance

<span class="anchor"><a href="#instance-miscellaneous:environment.*"
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
class="docutils literal notranslate">environment.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes (exec)</p></td>
</tr>
</tbody>
</table>

</div>

You can export key/value environment variables to the instance. These
are then set for <a href="../manpages/incus/exec/#incus-exec-md"
class="reference internal"><span class="std std-ref"><span
class="pre"><code
class="docutils literal notranslate">incus</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">exec</code></span></span></a>.

</div>

</div>

</div>

<div id="boot-related-options" class="section">

<span id="instance-options-boot"></span>

## Boot-related options<a href="#boot-related-options" class="headerlink"
title="Link to this heading">¶</a>

The following instance options control the boot-related behavior of the
instance:

<div id="instance-boot:boot.autorestart"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.autorestart`</span></span><span class="shortdesc"></span>

Whether to automatically restart an instance on unexpected exit

<span class="anchor"><a href="#instance-boot:boot.autorestart"
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
class="docutils literal notranslate">boot.autorestart</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

If set to <span class="pre">`true`</span> will attempt up to 10 restarts
over a 1 minute period upon unexpected instance exit.

</div>

</div>

<div id="instance-boot:boot.autostart"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.autostart`</span></span><span class="shortdesc"></span>

Whether to always start the instance when the daemon starts

<span class="anchor"><a href="#instance-boot:boot.autostart"
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
class="docutils literal notranslate">boot.autostart</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

If unset or set to <span class="pre">`last-state`</span>, restores the
last state.

</div>

</div>

<div id="instance-boot:boot.autostart.delay"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.autostart.delay`</span></span><span class="shortdesc"></span>

Delay after starting the instance

<span class="anchor"><a href="#instance-boot:boot.autostart.delay"
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
class="docutils literal notranslate">boot.autostart.delay</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>0</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

The number of seconds to wait after the instance started before starting
the next one.

</div>

</div>

<div id="instance-boot:boot.autostart.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.autostart.priority`</span></span><span class="shortdesc"></span>

What order to start the instances in

<span class="anchor"><a href="#instance-boot:boot.autostart.priority"
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
class="docutils literal notranslate">boot.autostart.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

The instance with the highest value is started first. Instances without
a priority set will be started (with some parallelism) ahead of
instances with a priority set.

</div>

</div>

<div id="instance-boot:boot.host_shutdown_action"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.host_shutdown_action`</span></span><span class="shortdesc"></span>

What action to take on the instance when the host is shut down

<span class="anchor"><a href="#instance-boot:boot.host_shutdown_action"
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
class="docutils literal notranslate">boot.host_shutdown_action</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>stop</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Action to take on host shut down

Valid values are: <span class="pre">`stop`</span>,
<span class="pre">`force-stop`</span> or
<span class="pre">`stateful-stop`</span>

</div>

</div>

<div id="instance-boot:boot.host_shutdown_timeout"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.host_shutdown_timeout`</span></span><span class="shortdesc"></span>

How long to wait for the instance to shut down

<span class="anchor"><a href="#instance-boot:boot.host_shutdown_timeout"
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
class="docutils literal notranslate">boot.host_shutdown_timeout</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>30</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Number of seconds to wait for the instance to shut down before it is
force-stopped.

</div>

</div>

<div id="instance-boot:boot.stop.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.stop.priority`</span></span><span class="shortdesc"></span>

What order to shut down the instances in

<span class="anchor"><a href="#instance-boot:boot.stop.priority"
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
class="docutils literal notranslate">boot.stop.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>0</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

The instance with the highest value is shut down first.

</div>

</div>

</div>

<div id="cloud-init-configuration" class="section">

<span id="instance-options-cloud-init"></span>

## <span class="pre">`cloud-init`</span> configuration<a href="#cloud-init-configuration" class="headerlink"
title="Link to this heading">¶</a>

The following instance options control the
<a href="../../cloud-init/#cloud-init" class="reference internal"><span
class="std std-ref"><span class="pre"><code
class="docutils literal notranslate">cloud-init</code></span></span></a>
configuration of the instance:

<div id="instance-cloud-init:cloud-init.network-config"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cloud-init.network-config`</span></span><span class="shortdesc"></span>

Network configuration for <span class="pre">`cloud-init`</span>

<span class="anchor"><a href="#instance-cloud-init:cloud-init.network-config"
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
class="docutils literal notranslate">cloud-init.network-config</code></span></td>
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
class="docutils literal notranslate">DHCP</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">on</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">eth0</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

The content is used as seed value for
<span class="pre">`cloud-init`</span>.

</div>

</div>

<div id="instance-cloud-init:cloud-init.user-data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cloud-init.user-data`</span></span><span class="shortdesc"></span>

User data for <span class="pre">`cloud-init`</span>

<span class="anchor"><a href="#instance-cloud-init:cloud-init.user-data"
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
class="docutils literal notranslate">cloud-init.user-data</code></span></td>
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
class="docutils literal notranslate">#cloud-config</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

The content is used as seed value for
<span class="pre">`cloud-init`</span>.

</div>

</div>

<div id="instance-cloud-init:cloud-init.vendor-data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`cloud-init.vendor-data`</span></span><span class="shortdesc"></span>

Vendor data for <span class="pre">`cloud-init`</span>

<span class="anchor"><a href="#instance-cloud-init:cloud-init.vendor-data"
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
class="docutils literal notranslate">cloud-init.vendor-data</code></span></td>
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
class="docutils literal notranslate">#cloud-config</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

The content is used as seed value for
<span class="pre">`cloud-init`</span>.

</div>

</div>

<div id="instance-cloud-init:user.network-config"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`user.network-config`</span></span><span class="shortdesc"></span>

Legacy version of <span class="pre">`cloud-init.network-config`</span>

<span class="anchor"><a href="#instance-cloud-init:user.network-config"
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
class="docutils literal notranslate">user.network-config</code></span></td>
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
class="docutils literal notranslate">DHCP</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">on</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">eth0</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-cloud-init:user.user-data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`user.user-data`</span></span><span class="shortdesc"></span>

Legacy version of <span class="pre">`cloud-init.user-data`</span>

<span class="anchor"><a href="#instance-cloud-init:user.user-data"
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
class="docutils literal notranslate">user.user-data</code></span></td>
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
class="docutils literal notranslate">#cloud-config</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-cloud-init:user.vendor-data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`user.vendor-data`</span></span><span class="shortdesc"></span>

Legacy version of <span class="pre">`cloud-init.vendor-data`</span>

<span class="anchor"><a href="#instance-cloud-init:user.vendor-data"
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
class="docutils literal notranslate">user.vendor-data</code></span></td>
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
class="docutils literal notranslate">#cloud-config</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>If supported by image</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

Support for these options depends on the image that is used and is not
guaranteed.

If you specify both <span class="pre">`cloud-init.user-data`</span> and
<span class="pre">`cloud-init.vendor-data`</span>, the content of both
options is merged. Therefore, make sure that the
<span class="pre">`cloud-init`</span> configuration you specify in those
options does not contain the same keys.

</div>

<div id="resource-limits" class="section">

<span id="instance-options-limits"></span>

## Resource limits<a href="#resource-limits" class="headerlink"
title="Link to this heading">¶</a>

The following instance options specify resource limits for the instance:

<div id="instance-resource-limits:limits.cpu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.cpu`</span></span><span class="shortdesc"></span>

Which CPUs to expose to the instance

<span class="anchor"><a href="#instance-resource-limits:limits.cpu"
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
class="docutils literal notranslate">limits.cpu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>1 (VMs)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

A number or a specific range of CPUs to expose to the instance. For
virtual machines, a CPU topology of the form
<span class="pre">`sockets=2,cores=4,threads=2`</span> may also be
provided.

See
<a href="#instance-options-limits-cpu" class="reference internal"><span
class="std std-ref">CPU pinning</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.cpu.allowance"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.cpu.allowance`</span></span><span class="shortdesc"></span>

How much of the CPU can be used

<span class="anchor"><a href="#instance-resource-limits:limits.cpu.allowance"
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
class="docutils literal notranslate">limits.cpu.allowance</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>100%</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

To control how much of the CPU can be used, specify either a percentage
(<span class="pre">`50%`</span>) for a soft limit or a chunk of time
(<span class="pre">`25ms/100ms`</span>) for a hard limit.

See <a href="#instance-options-limits-cpu-container"
class="reference internal"><span class="std std-ref">Allowance and
priority (container only)</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.cpu.nodes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.cpu.nodes`</span></span><span class="shortdesc"></span>

Which NUMA nodes to place the instance CPUs on

<span class="anchor"><a href="#instance-resource-limits:limits.cpu.nodes"
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
class="docutils literal notranslate">limits.cpu.nodes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

A comma-separated list of NUMA node IDs or ranges to place the instance
CPUs on. Alternatively, the value <span class="pre">`balanced`</span>
may be used to have Incus pick the least busy NUMA node on startup.

See <a href="#instance-options-limits-cpu-container"
class="reference internal"><span class="std std-ref">Allowance and
priority (container only)</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.cpu.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.cpu.priority`</span></span><span class="shortdesc"></span>

CPU scheduling priority compared to other instances

<span class="anchor"><a href="#instance-resource-limits:limits.cpu.priority"
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
class="docutils literal notranslate">limits.cpu.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">10</code></span> (maximum)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

When overcommitting resources, specify the CPU scheduling priority
compared to other instances that share the same CPUs. Specify an integer
between 0 and 10.

See <a href="#instance-options-limits-cpu-container"
class="reference internal"><span class="std std-ref">Allowance and
priority (container only)</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.disk.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.disk.priority`</span></span><span class="shortdesc"></span>

Priority of the instance’s I/O requests

<span class="anchor"><a href="#instance-resource-limits:limits.disk.priority"
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
class="docutils literal notranslate">limits.disk.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">5</code></span> (medium)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Controls how much priority to give to the instance’s I/O requests when
under load.

Specify an integer between 0 and 10.

</div>

</div>

<div id="instance-resource-limits:limits.hugepages.1GB"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.hugepages.1GB`</span></span><span class="shortdesc"></span>

Limit for the number of 1 GB huge pages

<span class="anchor"><a href="#instance-resource-limits:limits.hugepages.1GB"
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
class="docutils literal notranslate">limits.hugepages.1GB</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Fixed value (in bytes) to limit the number of 1 GB huge pages. Various
suffixes are supported (see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>).

See <a href="#instance-options-limits-hugepages"
class="reference internal"><span class="std std-ref">Huge page
limits</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.hugepages.1MB"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.hugepages.1MB`</span></span><span class="shortdesc"></span>

Limit for the number of 1 MB huge pages

<span class="anchor"><a href="#instance-resource-limits:limits.hugepages.1MB"
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
class="docutils literal notranslate">limits.hugepages.1MB</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Fixed value (in bytes) to limit the number of 1 MB huge pages. Various
suffixes are supported (see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>).

See <a href="#instance-options-limits-hugepages"
class="reference internal"><span class="std std-ref">Huge page
limits</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.hugepages.2MB"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.hugepages.2MB`</span></span><span class="shortdesc"></span>

Limit for the number of 2 MB huge pages

<span class="anchor"><a href="#instance-resource-limits:limits.hugepages.2MB"
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
class="docutils literal notranslate">limits.hugepages.2MB</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Fixed value (in bytes) to limit the number of 2 MB huge pages. Various
suffixes are supported (see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>).

See <a href="#instance-options-limits-hugepages"
class="reference internal"><span class="std std-ref">Huge page
limits</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.hugepages.64KB"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.hugepages.64KB`</span></span><span class="shortdesc"></span>

Limit for the number of 64 KB huge pages

<span class="anchor"><a href="#instance-resource-limits:limits.hugepages.64KB"
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
class="docutils literal notranslate">limits.hugepages.64KB</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Fixed value (in bytes) to limit the number of 64 KB huge pages. Various
suffixes are supported (see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>).

See <a href="#instance-options-limits-hugepages"
class="reference internal"><span class="std std-ref">Huge page
limits</span></a> for more information.

</div>

</div>

<div id="instance-resource-limits:limits.memory"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory`</span></span><span class="shortdesc"></span>

Usage limit for the host’s memory

<span class="anchor"><a href="#instance-resource-limits:limits.memory"
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
class="docutils literal notranslate">limits.memory</code></span></td>
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
class="docutils literal notranslate">1GiB</code></span> (VMs)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Percentage of the host’s memory or a fixed value in bytes. Various
suffixes are supported.

See <a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a> for details.

</div>

</div>

<div id="instance-resource-limits:limits.memory.enforce"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.enforce`</span></span><span class="shortdesc"></span>

Whether the memory limit is <span class="pre">`hard`</span> or
<span class="pre">`soft`</span>

<span class="anchor"><a href="#instance-resource-limits:limits.memory.enforce"
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
class="docutils literal notranslate">limits.memory.enforce</code></span></td>
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
class="docutils literal notranslate">hard</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

If the instance’s memory limit is <span class="pre">`hard`</span>, the
instance cannot exceed its limit. If it is
<span class="pre">`soft`</span>, the instance can exceed its memory
limit when extra host memory is available.

</div>

</div>

<div id="instance-resource-limits:limits.memory.hotplug"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.hotplug`</span></span><span class="shortdesc"></span>

Control upper limit for hotplugged memory or disable memory hotplug.

<span class="anchor"><a href="#instance-resource-limits:limits.memory.hotplug"
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
class="docutils literal notranslate">limits.memory.hotplug</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

If this option is set to <span class="pre">`false`</span>, disable
memory hotplug entirely. Alternatively, it can be set to a bytes value
which will define an upper limit for hotplugged memory. The value must
be greater than or equal to limits.memory.

</div>

</div>

<div id="instance-resource-limits:limits.memory.hugepages"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.hugepages`</span></span><span class="shortdesc"></span>

Whether to back the instance using huge pages

<span class="anchor"><a href="#instance-resource-limits:limits.memory.hugepages"
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
class="docutils literal notranslate">limits.memory.hugepages</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

If this option is set to <span class="pre">`false`</span>, regular
system memory is used.

</div>

</div>

<div id="instance-resource-limits:limits.memory.oom_priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.oom_priority`</span></span><span class="shortdesc"></span>

Out Of Memory killer priority adjustment for the instance

<span class="anchor"><a href="#instance-resource-limits:limits.memory.oom_priority"
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
class="docutils literal notranslate">limits.memory.oom_priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">0</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

Specify an integer between -1000 and 1000. A negative value makes the
instance less likely to be killed by the Out Of Memory killer, while a
positive value makes it more likely to be killed. The default value of 0
means no adjustment to the Out Of Memory score.

</div>

</div>

<div id="instance-resource-limits:limits.memory.swap"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.swap`</span></span><span class="shortdesc"></span>

Control swap usage by the instance

<span class="anchor"><a href="#instance-resource-limits:limits.memory.swap"
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
class="docutils literal notranslate">limits.memory.swap</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

When set to <span class="pre">`true`</span> or
<span class="pre">`false`</span>, it controls whether the container is
likely to get some of its memory swapped by the kernel. Alternatively,
it can be set to a bytes value which will then allow the container to
make use of additional memory through swap.

</div>

</div>

<div id="instance-resource-limits:limits.memory.swap.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.memory.swap.priority`</span></span><span class="shortdesc"></span>

Prevents the instance from being swapped to disk

<span class="anchor"><a href="#instance-resource-limits:limits.memory.swap.priority"
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
class="docutils literal notranslate">limits.memory.swap.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">10</code></span> (maximum)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Specify an integer between 0 and 10. The higher the value, the less
likely the instance is to be swapped to disk.

</div>

</div>

<div id="instance-resource-limits:limits.processes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.processes`</span></span><span class="shortdesc"></span>

Maximum number of processes that can run in the instance

<span class="anchor"><a href="#instance-resource-limits:limits.processes"
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
class="docutils literal notranslate">limits.processes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>empty</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

If left empty, no limit is set.

</div>

</div>

<div id="instance-resource-limits:limits.kernel.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.*`</span></span><span class="shortdesc"></span>

Kernel resources per instance

<span class="anchor"><a href="#instance-resource-limits:limits.kernel.*"
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
class="docutils literal notranslate">limits.kernel.*</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

You can set kernel limits on an instance, for example, you can limit the
number of open files. See <a href="#instance-options-limits-kernel"
class="reference internal"><span class="std std-ref">Kernel resource
limits</span></a> for more information.

</div>

</div>

<div id="memory-limits-in-virtual-machines" class="section">

### Memory limits in virtual machines<a href="#memory-limits-in-virtual-machines" class="headerlink"
title="Link to this heading">¶</a>

Incus supports both increasing and decreasing the memory allocation of
virtual machines.

Increasing the memory is done through memory hot plug, effectively
adding virtual memory sticks to the VM. There is a limit of 16 virtual
slots for this, limiting the number of memory increases that can be done
without rebooting the VM.

<div class="admonition note">

Note

To avoid high resource usage and compatibility issues with guests, Incus
limits memory hotplug to a maximum of 1TiB in clustered environments. On
standalone hosts, the default limit matches the total memory amount of
the host system. Some CPUs further reduce that amount based on how much
memory they are able to address (physical / virtual bits).

Exceeding that limit require the instance be stopped, its
<span class="pre">`memory.limit`</span> updated and then started back
up.

</div>

Decreasing memory is not done through hot remove as that has a high risk
of causing guest issues. Instead the memory balloon device is used,
causing memory pressure inside the guest and causing memory to be
released.

This is a pretty slow process, so it is common for a memory reduction to
fail to meet the requested value. When that happens, re-applying the
lower value will trigger another attempt.

As each attempt will cause the effective memory available to the guest
to be reduced, it should eventually succeed and lead to the guest having
the desired memory limit applied.

</div>

<div id="cpu-limits" class="section">

### CPU limits<a href="#cpu-limits" class="headerlink"
title="Link to this heading">¶</a>

You have different options to limit CPU usage:

- Set <span class="pre">`limits.cpu`</span> to restrict which CPUs the
  instance can see and use. See
  <a href="#instance-options-limits-cpu" class="reference internal"><span
  class="std std-ref">CPU pinning</span></a> for how to set this option.

- Set <span class="pre">`limits.cpu.allowance`</span> to restrict the
  load an instance can put on the available CPUs. This option is
  available only for containers. See
  <a href="#instance-options-limits-cpu-container"
  class="reference internal"><span class="std std-ref">Allowance and
  priority (container only)</span></a> for how to set this option.

It is possible to set both options at the same time to restrict both
which CPUs are visible to the instance and the allowed usage of those
instances. However, if you use
<span class="pre">`limits.cpu.allowance`</span> with a time limit, you
should avoid using <span class="pre">`limits.cpu`</span> in addition,
because that puts a lot of constraints on the scheduler and might lead
to less efficient allocations.

The CPU limits are implemented through a mix of the
<span class="pre">`cpuset`</span> and <span class="pre">`cpu`</span>
cgroup controllers.

<div id="cpu-pinning" class="section">

<span id="instance-options-limits-cpu"></span>

#### CPU pinning<a href="#cpu-pinning" class="headerlink"
title="Link to this heading">¶</a>

<span class="pre">`limits.cpu`</span> results in CPU pinning through the
<span class="pre">`cpuset`</span> controller. You can specify either
which CPUs or how many CPUs are visible and available to the instance:

- To specify which CPUs to use, set
  <span class="pre">`limits.cpu`</span> to either a set of CPUs (for
  example, <span class="pre">`1,2,3`</span>) or a CPU range (for
  example, <span class="pre">`0-3`</span>).

  To pin to a single CPU, use the range syntax (for example,
  <span class="pre">`1-1`</span>) to differentiate it from a number of
  CPUs.

- If you specify a number (for example, <span class="pre">`4`</span>) of
  CPUs, Incus will do dynamic load-balancing of all instances that
  aren’t pinned to specific CPUs, trying to spread the load on the
  machine. Instances are re-balanced every time an instance starts or
  stops, as well as whenever a CPU is added to the system.

<div id="cpu-limits-for-virtual-machines" class="section">

##### CPU limits for virtual machines<a href="#cpu-limits-for-virtual-machines" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

Incus supports live-updating the <span class="pre">`limits.cpu`</span>
option. However, for virtual machines, this only means that the
respective CPUs are hotplugged. Depending on the guest operating system,
you might need to either restart the instance or complete some manual
actions to bring the new CPUs online.

</div>

Incus virtual machines default to having just one vCPU allocated, which
shows up as matching the host CPU vendor and type, but has a single core
and no threads.

When <span class="pre">`limits.cpu`</span> is set to a single integer,
Incus allocates multiple vCPUs and exposes them to the guest as full
cores. Those vCPUs are not pinned to specific physical cores on the
host. The number of vCPUs can be updated while the VM is running.

<div class="admonition note">

Note

To avoid high resource usage and compatibility issues with guests, Incus
limits CPU hotplug to a maximum of 64 cores. VMs needing more than 64
CPU cores will need to be shut down to adjust their
<span class="pre">`limits.cpu`</span> property.

</div>

You can also request a specific CPU topology by setting
<span class="pre">`limits.cpu`</span> to a value of the form
<span class="pre">`sockets=2,cores=4,threads=2`</span>. Any of the three
fields may be omitted, in which case it defaults to
<span class="pre">`1`</span>. For example,
<span class="pre">`sockets=2`</span> results in two vCPUs (two sockets,
each with a single core and a single thread), while
<span class="pre">`cores=4`</span> results in four vCPUs (a single
socket with four cores). The resulting topology is exposed verbatim to
the guest and the vCPUs are not pinned to specific physical cores on the
host.

<div class="admonition note">

Note

Using the CPU topology syntax is incompatible with CPU hotplugging. The
virtual machine must be stopped to switch to or away from such a value.

</div>

When <span class="pre">`limits.cpu`</span> is set to a range or
comma-separated list of CPU IDs (as provided by
<a href="../manpages/incus/info/#incus-info-md"
class="reference internal"><span class="std std-ref"><span
class="pre"><code
class="docutils literal notranslate">incus</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">info</code></span><code
class="docutils literal notranslate"> </code><span class="pre"><code
class="docutils literal notranslate">--resources</code></span></span></a>),
the vCPUs are pinned to those physical cores. In this scenario, Incus
checks whether the CPU configuration lines up with a realistic hardware
topology and if it does, it replicates that topology in the guest. When
doing CPU pinning, it is not possible to change the configuration while
the VM is running.

For example, if the pinning configuration includes eight threads, with
each pair of thread coming from the same core and an even number of
cores spread across two CPUs, the guest will show two CPUs, each with
two cores and each core with two threads. The NUMA layout is similarly
replicated and in this scenario, the guest would most likely end up with
two NUMA nodes, one for each CPU socket.

In such an environment with multiple NUMA nodes, the memory is similarly
divided across NUMA nodes and be pinned accordingly on the host and then
exposed to the guest.

All this allows for very high performance operations in the guest as the
guest scheduler can properly reason about sockets, cores and threads as
well as consider NUMA topology when sharing memory or moving processes
across NUMA nodes.

</div>

</div>

<div id="allowance-and-priority-container-only" class="section">

<span id="instance-options-limits-cpu-container"></span>

#### Allowance and priority (container only)<a href="#allowance-and-priority-container-only" class="headerlink"
title="Link to this heading">¶</a>

<span class="pre">`limits.cpu.allowance`</span> drives either the CFS
scheduler quotas when passed a time constraint, or the generic CPU
shares mechanism when passed a percentage value:

- The time constraint (for example,
  <span class="pre">`20ms/50ms`</span>) is a hard limit. For example, if
  you want to allow the container to use a maximum of one CPU, set
  <span class="pre">`limits.cpu.allowance`</span> to a value like
  <span class="pre">`100ms/100ms`</span>. The value is relative to one
  CPU worth of time, so to restrict to two CPUs worth of time, use
  something like <span class="pre">`100ms/50ms`</span> or
  <span class="pre">`200ms/100ms`</span>.

- When using a percentage value, the limit is a soft limit that is
  applied only when under load. It is used to calculate the scheduler
  priority for the instance, relative to any other instance that is
  using the same CPU or CPUs. For example, to limit the CPU usage of the
  container to one CPU when under load, set
  <span class="pre">`limits.cpu.allowance`</span> to
  <span class="pre">`100%`</span>.

<span class="pre">`limits.cpu.priority`</span> is another factor that is
used to compute the scheduler priority score when a number of instances
sharing a set of CPUs have the same percentage of CPU assigned to them.

</div>

</div>

<div id="huge-page-limits" class="section">

<span id="instance-options-limits-hugepages"></span>

### Huge page limits<a href="#huge-page-limits" class="headerlink"
title="Link to this heading">¶</a>

Incus allows to limit the number of huge pages available to a container
through the <span class="pre">`limits.hugepage.[size]`</span> key.

Architectures often expose multiple huge-page sizes. The available
huge-page sizes depend on the architecture.

Setting limits for huge pages is especially useful when Incus is
configured to intercept the <span class="pre">`mount`</span> syscall for
the <span class="pre">`hugetlbfs`</span> file system in unprivileged
containers. When Incus intercepts a <span class="pre">`hugetlbfs`</span>
<span class="pre">`mount`</span> syscall, it mounts the
<span class="pre">`hugetlbfs`</span> file system for a container with
correct <span class="pre">`uid`</span> and
<span class="pre">`gid`</span> values as mount options. This makes it
possible to use huge pages from unprivileged containers. However, it is
recommended to limit the number of huge pages available to the container
through <span class="pre">`limits.hugepages.[size]`</span> to stop the
container from being able to exhaust the huge pages available to the
host.

Limiting huge pages is done through the
<span class="pre">`hugetlb`</span> cgroup controller, which means that
the host system must expose the <span class="pre">`hugetlb`</span>
controller for these limits to apply.

</div>

<div id="kernel-resource-limits" class="section">

<span id="instance-options-limits-kernel"></span>

### Kernel resource limits<a href="#kernel-resource-limits" class="headerlink"
title="Link to this heading">¶</a>

For container instances, Incus exposes a generic namespaced key
<span class="pre">`limits.kernel.*`</span> that can be used to set
resource limits.

It is generic in the sense that Incus does not perform any validation on
the resource that is specified following the
<span class="pre">`limits.kernel.*`</span> prefix. Incus cannot know
about all the possible resources that a given kernel supports. Instead,
Incus simply passes down the corresponding resource key after the
<span class="pre">`limits.kernel.*`</span> prefix and its value to the
kernel. The kernel does the appropriate validation. This allows users to
specify any supported limit on their system.

Some common limits are:

<div id="kernel-limits:limits.kernel.as"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.as`</span></span><span class="shortdesc"></span>

Maximum size of the process’s virtual memory

<span class="anchor"><a href="#kernel-limits:limits.kernel.as"
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
class="docutils literal notranslate">limits.kernel.as</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_AS</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.core"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.core`</span></span><span class="shortdesc"></span>

Maximum size of the process’s core dump file

<span class="anchor"><a href="#kernel-limits:limits.kernel.core"
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
class="docutils literal notranslate">limits.kernel.core</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_CORE</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.cpu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.cpu`</span></span><span class="shortdesc"></span>

Limit in seconds on the amount of CPU time the process can consume

<span class="anchor"><a href="#kernel-limits:limits.kernel.cpu"
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
class="docutils literal notranslate">limits.kernel.cpu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_CPU</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.data`</span></span><span class="shortdesc"></span>

Maximum size of the process’s data segment

<span class="anchor"><a href="#kernel-limits:limits.kernel.data"
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
class="docutils literal notranslate">limits.kernel.data</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_DATA</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.fsize"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.fsize`</span></span><span class="shortdesc"></span>

Maximum size of files the process may create

<span class="anchor"><a href="#kernel-limits:limits.kernel.fsize"
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
class="docutils literal notranslate">limits.kernel.fsize</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_FSIZE</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.locks"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.locks`</span></span><span class="shortdesc"></span>

Limit on the number of file locks that this process may establish

<span class="anchor"><a href="#kernel-limits:limits.kernel.locks"
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
class="docutils literal notranslate">limits.kernel.locks</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_LOCKS</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.memlock"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.memlock`</span></span><span class="shortdesc"></span>

Limit on the number of bytes of memory that the process may lock in RAM

<span class="anchor"><a href="#kernel-limits:limits.kernel.memlock"
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
class="docutils literal notranslate">limits.kernel.memlock</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_MEMLOCK</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.nice"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.nice`</span></span><span class="shortdesc"></span>

Maximum value to which the process’s nice value can be raised

<span class="anchor"><a href="#kernel-limits:limits.kernel.nice"
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
class="docutils literal notranslate">limits.kernel.nice</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_NICE</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.nofile"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.nofile`</span></span><span class="shortdesc"></span>

Maximum number of open files for the process

<span class="anchor"><a href="#kernel-limits:limits.kernel.nofile"
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
class="docutils literal notranslate">limits.kernel.nofile</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_NOFILE</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.nproc"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.nproc`</span></span><span class="shortdesc"></span>

Maximum number of processes that can be created for the user of the
calling process

<span class="anchor"><a href="#kernel-limits:limits.kernel.nproc"
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
class="docutils literal notranslate">limits.kernel.nproc</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_NPROC</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.rtprio"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.rtprio`</span></span><span class="shortdesc"></span>

Maximum value on the real-time-priority that may be set for this process

<span class="anchor"><a href="#kernel-limits:limits.kernel.rtprio"
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
class="docutils literal notranslate">limits.kernel.rtprio</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_RTPRIO</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="kernel-limits:limits.kernel.sigpending"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.kernel.sigpending`</span></span><span class="shortdesc"></span>

Limit on the number of bytes of memory that the process may lock in RAM

<span class="anchor"><a href="#kernel-limits:limits.kernel.sigpending"
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
class="docutils literal notranslate">limits.kernel.sigpending</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Resource:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">RLIMIT_SIGPENDING</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

A full list of all available limits can be found in the manpages for the
<span class="pre">`getrlimit(2)`</span>/<span class="pre">`setrlimit(2)`</span>
system calls.

To specify a limit within the <span class="pre">`limits.kernel.*`</span>
namespace, use the resource name in lowercase without the
<span class="pre">`RLIMIT_`</span> prefix. For example,
<span class="pre">`RLIMIT_NOFILE`</span> should be specified as
<span class="pre">`nofile`</span>.

A limit is specified as two colon-separated values that are either
numeric or the word <span class="pre">`unlimited`</span> (for example,
<span class="pre">`limits.kernel.nofile=1000:2000`</span>). A single
value can be used as a shortcut to set both soft and hard limit to the
same value (for example,
<span class="pre">`limits.kernel.nofile=3000`</span>).

A resource with no explicitly configured limit will inherit its limit
from the process that starts up the container. Note that this
inheritance is not enforced by Incus but by the kernel.

</div>

</div>

<div id="migration-options" class="section">

<span id="instance-options-migration"></span>

## Migration options<a href="#migration-options" class="headerlink"
title="Link to this heading">¶</a>

The following instance options control the behavior if the instance is
<a href="../../howto/move_instances/#move-instances"
class="reference internal"><span class="std std-ref">moved from one
Incus server to another</span></a>:

<div id="instance-migration:migration.incremental.memory"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`migration.incremental.memory`</span></span><span class="shortdesc"></span>

Whether to use incremental memory transfer

<span class="anchor"><a href="#instance-migration:migration.incremental.memory"
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
class="docutils literal notranslate">migration.incremental.memory</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Using incremental memory transfer of the instance’s memory can reduce
downtime.

</div>

</div>

<div id="instance-migration:migration.incremental.memory.goal"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`migration.incremental.memory.goal`</span></span><span class="shortdesc"></span>

Percentage of memory to have in sync before stopping the instance

<span class="anchor"><a href="#instance-migration:migration.incremental.memory.goal"
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
class="docutils literal notranslate">migration.incremental.memory.goal</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">70</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-migration:migration.incremental.memory.iterations"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`migration.incremental.memory.iterations`</span></span><span class="shortdesc"></span>

Maximum number of transfer operations to go through before stopping the
instance

<span class="anchor"><a href="#instance-migration:migration.incremental.memory.iterations"
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
class="docutils literal notranslate">migration.incremental.memory.iterations</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p><span class="pre"><code
class="docutils literal notranslate">10</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-migration:migration.stateful"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`migration.stateful`</span></span><span class="shortdesc"></span>

Whether to allow for stateful stop/start and snapshots

<span class="anchor"><a href="#instance-migration:migration.stateful"
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
class="docutils literal notranslate">migration.stateful</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

Enabling this option prevents the use of some features that are
incompatible with it.

</div>

</div>

</div>

<div id="nvidia-and-cuda-configuration" class="section">

<span id="instance-options-nvidia"></span>

## NVIDIA and CUDA configuration<a href="#nvidia-and-cuda-configuration" class="headerlink"
title="Link to this heading">¶</a>

The following instance options specify the NVIDIA and CUDA configuration
of the instance:

<div id="instance-nvidia:nvidia.driver.capabilities"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`nvidia.driver.capabilities`</span></span><span class="shortdesc"></span>

What driver capabilities the instance needs

<span class="anchor"><a href="#instance-nvidia:nvidia.driver.capabilities"
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
class="docutils literal notranslate">nvidia.driver.capabilities</code></span></td>
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
class="docutils literal notranslate">compute,utility</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

The specified driver capabilities are used to set
<span class="pre">`libnvidia-container`</span>` `<span class="pre">`NVIDIA_DRIVER_CAPABILITIES`</span>.

</div>

</div>

<div id="instance-nvidia:nvidia.require.cuda"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`nvidia.require.cuda`</span></span><span class="shortdesc"></span>

Required CUDA version

<span class="anchor"><a href="#instance-nvidia:nvidia.require.cuda"
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
class="docutils literal notranslate">nvidia.require.cuda</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

The specified version expression is used to set
<span class="pre">`libnvidia-container`</span>` `<span class="pre">`NVIDIA_REQUIRE_CUDA`</span>.

</div>

</div>

<div id="instance-nvidia:nvidia.require.driver"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`nvidia.require.driver`</span></span><span class="shortdesc"></span>

Required driver version

<span class="anchor"><a href="#instance-nvidia:nvidia.require.driver"
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
class="docutils literal notranslate">nvidia.require.driver</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

The specified version expression is used to set
<span class="pre">`libnvidia-container`</span>` `<span class="pre">`NVIDIA_REQUIRE_DRIVER`</span>.

</div>

</div>

<div id="instance-nvidia:nvidia.runtime"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`nvidia.runtime`</span></span><span class="shortdesc"></span>

Whether to pass the host NVIDIA and CUDA runtime libraries into the
instance

<span class="anchor"><a href="#instance-nvidia:nvidia.runtime"
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
class="docutils literal notranslate">nvidia.runtime</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="oci-configuration" class="section">

<span id="instance-options-oci"></span>

## OCI configuration<a href="#oci-configuration" class="headerlink"
title="Link to this heading">¶</a>

The following instance options specify the OCI configuration of the
instance:

<div id="instance-oci:oci.cwd" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.cwd`</span></span><span class="shortdesc"></span>

Working directory

<span class="anchor"><a href="#instance-oci:oci.cwd" class="reference external"><em><img
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
class="docutils literal notranslate">oci.cwd</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Override the working directory of an OCI container.

</div>

</div>

<div id="instance-oci:oci.dns.domain"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.dns.domain`</span></span><span class="shortdesc"></span>

DNS domain

<span class="anchor"><a href="#instance-oci:oci.dns.domain"
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
class="docutils literal notranslate">oci.dns.domain</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Domain name for the initial <span class="pre">`resolv.conf`</span>.

</div>

</div>

<div id="instance-oci:oci.dns.nameservers"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.dns.nameservers`</span></span><span class="shortdesc"></span>

DNS name servers

<span class="anchor"><a href="#instance-oci:oci.dns.nameservers"
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
class="docutils literal notranslate">oci.dns.nameservers</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Comma-separated list of name server addresses for the initial
<span class="pre">`resolv.conf`</span>.

</div>

</div>

<div id="instance-oci:oci.dns.search"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.dns.search`</span></span><span class="shortdesc"></span>

DNS search domains

<span class="anchor"><a href="#instance-oci:oci.dns.search"
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
class="docutils literal notranslate">oci.dns.search</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Comma-separated list of search domains for the initial
<span class="pre">`resolv.conf`</span>.

</div>

</div>

<div id="instance-oci:oci.entrypoint"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.entrypoint`</span></span><span class="shortdesc"></span>

Entry point

<span class="anchor"><a href="#instance-oci:oci.entrypoint"
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
class="docutils literal notranslate">oci.entrypoint</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Override the entry point of an OCI container.

</div>

</div>

<div id="instance-oci:oci.gid" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.gid`</span></span><span class="shortdesc"></span>

Process GID

<span class="anchor"><a href="#instance-oci:oci.gid" class="reference external"><em><img
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
class="docutils literal notranslate">oci.gid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Override the GID of the process run in an OCI container.

</div>

</div>

<div id="instance-oci:oci.uid" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`oci.uid`</span></span><span class="shortdesc"></span>

Process UID

<span class="anchor"><a href="#instance-oci:oci.uid" class="reference external"><em><img
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
class="docutils literal notranslate">oci.uid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>OCI container</p></td>
</tr>
</tbody>
</table>

</div>

Override the UID of the process run in an OCI container.

</div>

</div>

</div>

<div id="raw-instance-configuration-overrides" class="section">

<span id="instance-options-raw"></span>

## Raw instance configuration overrides<a href="#raw-instance-configuration-overrides" class="headerlink"
title="Link to this heading">¶</a>

The following instance options allow direct interaction with the backend
features that Incus itself uses:

<div id="instance-raw:raw.apparmor"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.apparmor`</span></span><span class="shortdesc"></span>

AppArmor profile entries

<span class="anchor"><a href="#instance-raw:raw.apparmor" class="reference external"><em><img
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
class="docutils literal notranslate">raw.apparmor</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

The specified entries are appended to the generated profile.

</div>

</div>

<div id="instance-raw:raw.idmap"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.idmap`</span></span><span class="shortdesc"></span>

Raw idmap configuration

<span class="anchor"><a href="#instance-raw:raw.idmap" class="reference external"><em><img
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
class="docutils literal notranslate">raw.idmap</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

For example:
<span class="pre">`both`</span>` `<span class="pre">`1000`</span>` `<span class="pre">`1000`</span>

</div>

</div>

<div id="instance-raw:raw.lxc" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.lxc`</span></span><span class="shortdesc"></span>

Raw LXC configuration to be appended to the generated one

<span class="anchor"><a href="#instance-raw:raw.lxc" class="reference external"><em><img
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
class="docutils literal notranslate">raw.lxc</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.qemu" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu`</span></span><span class="shortdesc"></span>

Raw QEMU configuration to be appended to the generated command line

<span class="anchor"><a href="#instance-raw:raw.qemu" class="reference external"><em><img
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
class="docutils literal notranslate">raw.qemu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.qemu.conf"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu.conf`</span></span><span class="shortdesc"></span>

Addition/override to the generated <span class="pre">`qemu.conf`</span>
file

<span class="anchor"><a href="#instance-raw:raw.qemu.conf"
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
class="docutils literal notranslate">raw.qemu.conf</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

See <a href="#instance-options-qemu" class="reference internal"><span
class="std std-ref">Override QEMU configuration</span></a> for more
information.

</div>

</div>

<div id="instance-raw:raw.qemu.qmp.early"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu.qmp.early`</span></span><span class="shortdesc"></span>

QMP commands to run before Incus QEMU initialization

<span class="anchor"><a href="#instance-raw:raw.qemu.qmp.early"
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
class="docutils literal notranslate">raw.qemu.qmp.early</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.qemu.qmp.post-start"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu.qmp.post-start`</span></span><span class="shortdesc"></span>

QMP commands to run after the VM has started

<span class="anchor"><a href="#instance-raw:raw.qemu.qmp.post-start"
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
class="docutils literal notranslate">raw.qemu.qmp.post-start</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.qemu.qmp.pre-start"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu.qmp.pre-start`</span></span><span class="shortdesc"></span>

QMP commands to run after Incus QEMU initialization and before the VM
has started

<span class="anchor"><a href="#instance-raw:raw.qemu.qmp.pre-start"
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
class="docutils literal notranslate">raw.qemu.qmp.pre-start</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.qemu.scriptlet"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.qemu.scriptlet`</span></span><span class="shortdesc"></span>

QEMU scriptlet to run at early, pre-start and post-start stages

<span class="anchor"><a href="#instance-raw:raw.qemu.scriptlet"
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
class="docutils literal notranslate">raw.qemu.scriptlet</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-raw:raw.seccomp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`raw.seccomp`</span></span><span class="shortdesc"></span>

Raw Seccomp configuration

<span class="anchor"><a href="#instance-raw:raw.seccomp" class="reference external"><em><img
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
class="docutils literal notranslate">raw.seccomp</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>blob</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="admonition important">

Important

Setting these <span class="pre">`raw.*`</span> keys might break Incus in
non-obvious ways. Therefore, you should avoid setting any of these keys.

</div>

<div id="override-qemu-configuration" class="section">

<span id="instance-options-qemu"></span>

### Override QEMU configuration<a href="#override-qemu-configuration" class="headerlink"
title="Link to this heading">¶</a>

For VM instances, Incus configures QEMU through a configuration file
that is passed to QEMU with the <span class="pre">`-readconfig`</span>
command-line option. This configuration file is generated for each
instance before boot. It can be found at
<span class="pre">`/run/incus/<instance_name>/qemu.conf`</span>.

The default configuration works fine for Incus’ most common use case:
modern UEFI guests with VirtIO devices. In some situations, however, you
might need to override the generated configuration. For example:

- To run an old guest OS that doesn’t support UEFI.

- To specify custom virtual devices when VirtIO is not supported by the
  guest OS.

- To add devices that are not supported by Incus before the machines
  boots.

- To remove devices that conflict with the guest OS.

To override the configuration, set the
<span class="pre">`raw.qemu.conf`</span> option. It supports a format
similar to <span class="pre">`qemu.conf`</span>, with some additions.
Since it is a multi-line configuration option, you can use it to modify
multiple sections or keys.

- To replace a section or key in the generated configuration file, add a
  section with a different value.

  For example, use the following section to override the default
  <span class="pre">`virtio-gpu-pci`</span> GPU driver:

  <div class="highlight-default notranslate">

  <div class="highlight">

      raw.qemu.conf: |-
          [device "qemu_gpu"]
          driver = "qxl-vga"

  </div>

  </div>

- To remove a section, specify a section without any keys. For example:

  <div class="highlight-default notranslate">

  <div class="highlight">

      raw.qemu.conf: |-
          [device "qemu_gpu"]

  </div>

  </div>

- To remove a key, specify an empty string as the value. For example:

  <div class="highlight-default notranslate">

  <div class="highlight">

      raw.qemu.conf: |-
          [device "qemu_gpu"]
          driver = ""

  </div>

  </div>

- To add a new section, specify a section name that is not present in
  the configuration file.

The configuration file format used by QEMU allows multiple sections with
the same name. Here’s a piece of the configuration generated by Incus:

<div class="highlight-default notranslate">

<div class="highlight">

    [global]
    driver = "ICH9-LPC"
    property = "disable_s3"
    value = "1"

    [global]
    driver = "ICH9-LPC"
    property = "disable_s4"
    value = "0"

</div>

</div>

The first <span class="pre">`global`</span> section disabled S3(Suspend
to RAM), the second <span class="pre">`global`</span> section enabled
S4(suspend to disk). In order to disable S4, the second
<span class="pre">`global`</span> section index needs to be specified:

<div class="highlight-default notranslate">

<div class="highlight">

    raw.qemu.conf: |-
        [global][1]
        value = "1"

</div>

</div>

Section indexes start at 0 (which is the default value when not
specified), so the above example would generate the following
configuration:

<div class="highlight-default notranslate">

<div class="highlight">

    [global]
    driver = "ICH9-LPC"
    property = "disable_s3"
    value = "1"

    [global]
    driver = "ICH9-LPC"
    property = "disable_s4"
    value = "1"

</div>

</div>

</div>

<div id="override-qemu-runtime-objects" class="section">

### Override QEMU runtime objects<a href="#override-qemu-runtime-objects" class="headerlink"
title="Link to this heading">¶</a>

While <span class="pre">`raw.qemu`</span> and
<span class="pre">`raw.qemu.conf`</span> can be used to alter the
arguments and configuration file that’s passed to QEMU, a lot of devices
are now added through QMP instead.

This is used by Incus for any device which may need to be re-configured
at runtime, effectively anything that can be hot-plugged.

Those devices cannot be overridden through the configuration or the
command line, but instead additional configuration keys are available to
run QMP commands directly.

Fixed commands can be provided through the
<span class="pre">`raw.qemu.early`</span>,
<span class="pre">`raw.qemu.pre-start`</span> and
<span class="pre">`raw.qemu.post-start`</span> configuration keys. Those
take a JSON encoded list of QMP commands to run.

The hooks correspond to:

- <span class="pre">`early`</span>, run prior to any device having been
  added by Incus through QMP, after QEMU has started

- <span class="pre">`pre-start`</span>, run following Incus having added
  all its devices but prior to the VM being started

- <span class="pre">`post-start`</span>, run immediately following the
  VM starting up

</div>

<div id="advanced-use" class="section">

### Advanced use<a href="#advanced-use" class="headerlink"
title="Link to this heading">¶</a>

For anyone needing dynamic QMP interactions, for example to retrieve the
current value of some objects before modifying or generating new
objects, it’s also possible to attach to those same hooks using a
scriptlet.

This is done through <span class="pre">`raw.qemu.scriptlet`</span>. The
scriptlet must define the
<span class="pre">`qemu_hook(instance,`</span>` `<span class="pre">`stage)`</span>
function. The <span class="pre">`instance`</span> arguments is an object
representing the VM, whose attributes are those of the
<span class="pre">`api.Instance`</span> struct. The
<span class="pre">`stage`</span> argument is the name of the hook
(<span class="pre">`config`</span>, <span class="pre">`early`</span>,
<span class="pre">`pre-start`</span> or
<span class="pre">`post-start`</span>), with
<span class="pre">`config`</span> being run before starting QEMU, and
the other hooks defined above.

The following commands are exposed to that scriptlet:

- <span class="pre">`log_info`</span> will log an
  <span class="pre">`INFO`</span> message

- <span class="pre">`log_warn`</span> will log a
  <span class="pre">`WARNING`</span> message

- <span class="pre">`log_error`</span> will log an
  <span class="pre">`ERROR`</span> message

- <span class="pre">`run_qmp`</span> will run an arbitrary QMP command
  (JSON) and return its output

- <span class="pre">`run_command`</span> will run the specified command
  with an optional list of arguments and return its output

- <span class="pre">`get_qemu_cmdline`</span> will return the list of
  command-line arguments passed to QEMU

- <span class="pre">`set_qemu_cmdline`</span> will set them

- <span class="pre">`get_qemu_conf`</span> will return the QEMU
  configuration file as a dictionary

- <span class="pre">`set_qemu_conf`</span> will set it from a dictionary

Additionally the following alias commands (internally use
<span class="pre">`run_command`</span>) are also available to simplify
scripts:

- <span class="pre">`blockdev_add`</span>

- <span class="pre">`blockdev_del`</span>

- <span class="pre">`chardev_add`</span>

- <span class="pre">`chardev_change`</span>

- <span class="pre">`chardev_remove`</span>

- <span class="pre">`device_add`</span>

- <span class="pre">`device_del`</span>

- <span class="pre">`netdev_add`</span>

- <span class="pre">`netdev_del`</span>

- <span class="pre">`object_add`</span>

- <span class="pre">`object_del`</span>

- <span class="pre">`qom_get`</span>

- <span class="pre">`qom_list`</span>

- <span class="pre">`qom_set`</span>

The functions allowing to change QEMU configuration can only be run
during the <span class="pre">`config`</span> hook. In parallel, the
functions running QMP commands cannot be run during the
<span class="pre">`config`</span> hook.

</div>

</div>

<div id="security-policies" class="section">

<span id="instance-options-security"></span>

## Security policies<a href="#security-policies" class="headerlink"
title="Link to this heading">¶</a>

The following instance options control the
<a href="../../security/#security" class="reference internal"><span
class="std std-ref">Security</span></a> policies of the instance:

<div id="instance-security:security.agent.metrics"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.agent.metrics`</span></span><span class="shortdesc"></span>

Whether the <span class="pre">`incus-agent`</span> is queried for state
information and metrics

<span class="anchor"><a href="#instance-security:security.agent.metrics"
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
class="docutils literal notranslate">security.agent.metrics</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.bpffs.delegate_attachs"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.bpffs.delegate_attachs`</span></span><span class="shortdesc"></span>

What BPF attach types to delegate

<span class="anchor"><a href="#instance-security:security.bpffs.delegate_attachs"
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
class="docutils literal notranslate">security.bpffs.delegate_attachs</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

See <a href="../../explanation/bpf-tokens/#bpf-tokens"
class="reference internal"><span class="std std-ref">BPF token
delegation</span></a> for more information.

</div>

</div>

<div id="instance-security:security.bpffs.delegate_cmds"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.bpffs.delegate_cmds`</span></span><span class="shortdesc"></span>

What BPF command types to delegate

<span class="anchor"><a href="#instance-security:security.bpffs.delegate_cmds"
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
class="docutils literal notranslate">security.bpffs.delegate_cmds</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

See <a href="../../explanation/bpf-tokens/#bpf-tokens"
class="reference internal"><span class="std std-ref">BPF token
delegation</span></a> for more information.

</div>

</div>

<div id="instance-security:security.bpffs.delegate_maps"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.bpffs.delegate_maps`</span></span><span class="shortdesc"></span>

What BPF map types to delegate

<span class="anchor"><a href="#instance-security:security.bpffs.delegate_maps"
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
class="docutils literal notranslate">security.bpffs.delegate_maps</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

See <a href="../../explanation/bpf-tokens/#bpf-tokens"
class="reference internal"><span class="std std-ref">BPF token
delegation</span></a> for more information.

</div>

</div>

<div id="instance-security:security.bpffs.delegate_progs"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.bpffs.delegate_progs`</span></span><span class="shortdesc"></span>

What BPF program types to delegate

<span class="anchor"><a href="#instance-security:security.bpffs.delegate_progs"
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
class="docutils literal notranslate">security.bpffs.delegate_progs</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

See <a href="../../explanation/bpf-tokens/#bpf-tokens"
class="reference internal"><span class="std std-ref">BPF token
delegation</span></a> for more information.

</div>

</div>

<div id="instance-security:security.bpffs.path"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.bpffs.path`</span></span><span class="shortdesc"></span>

The path to mount the BPF file system at

<span class="anchor"><a href="#instance-security:security.bpffs.path"
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
class="docutils literal notranslate">security.bpffs.path</code></span></td>
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
class="docutils literal notranslate">/sys/fs/bpf</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

The specified path must exist in the container. The BPF file system is
only mounted if any of the
<span class="pre">`security.bpffs.delegate_*`</span> options are set.
See <a href="../../explanation/bpf-tokens/#bpf-tokens"
class="reference internal"><span class="std std-ref">BPF token
delegation</span></a> for more information.

</div>

</div>

<div id="instance-security:security.csm"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.csm`</span></span><span class="shortdesc"></span>

Whether to use a firmware that supports UEFI-incompatible operating
systems

<span class="anchor"><a href="#instance-security:security.csm"
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
class="docutils literal notranslate">security.csm</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

When enabling this option, set
<a href="#instance-security:security.secureboot"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">security.secureboot</code></span></a>
to <span class="pre">`false`</span>.

</div>

</div>

<div id="instance-security:security.guestapi"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.guestapi`</span></span><span class="shortdesc"></span>

Whether <span class="pre">`/dev/incus`</span> is present in the instance

<span class="anchor"><a href="#instance-security:security.guestapi"
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
class="docutils literal notranslate">security.guestapi</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

See
<a href="../../dev-incus/#dev-incus" class="reference internal"><span
class="std std-ref">Communication between instance and host</span></a>
for more information.

</div>

</div>

<div id="instance-security:security.guestapi.images"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.guestapi.images`</span></span><span class="shortdesc"></span>

Controls the availability of the <span class="pre">`/1.0/images`</span>
API over <span class="pre">`guestapi`</span>

<span class="anchor"><a href="#instance-security:security.guestapi.images"
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
class="docutils literal notranslate">security.guestapi.images</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.idmap.base"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.idmap.base`</span></span><span class="shortdesc"></span>

The base host ID to use for the allocation

<span class="anchor"><a href="#instance-security:security.idmap.base"
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
class="docutils literal notranslate">security.idmap.base</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

Setting this option overrides auto-detection.

</div>

</div>

<div id="instance-security:security.idmap.isolated"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.idmap.isolated`</span></span><span class="shortdesc"></span>

Whether to use a unique idmap for this instance

<span class="anchor"><a href="#instance-security:security.idmap.isolated"
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
class="docutils literal notranslate">security.idmap.isolated</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

If specified, the idmap used for this instance is unique among instances
that have this option set.

</div>

</div>

<div id="instance-security:security.idmap.size"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.idmap.size`</span></span><span class="shortdesc"></span>

The size of the idmap to use

<span class="anchor"><a href="#instance-security:security.idmap.size"
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
class="docutils literal notranslate">security.idmap.size</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>unprivileged container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.iommu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.iommu`</span></span><span class="shortdesc"></span>

Whether to enable virtual IOMMU, useful for device passthrough and
nesting

<span class="anchor"><a href="#instance-security:security.iommu"
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
class="docutils literal notranslate">security.iommu</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.nesting"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.nesting`</span></span><span class="shortdesc"></span>

Whether to support running Incus (nested) inside the instance

<span class="anchor"><a href="#instance-security:security.nesting"
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
class="docutils literal notranslate">security.nesting</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.privileged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.privileged`</span></span><span class="shortdesc"></span>

Whether to run the instance in privileged mode

<span class="anchor"><a href="#instance-security:security.privileged"
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
class="docutils literal notranslate">security.privileged</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.protection.delete"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.protection.delete`</span></span><span class="shortdesc"></span>

Prevents the instance from being deleted

<span class="anchor"><a href="#instance-security:security.protection.delete"
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
class="docutils literal notranslate">security.protection.delete</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.protection.shift"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.protection.shift`</span></span><span class="shortdesc"></span>

Whether to protect the file system from being UID/GID shifted

<span class="anchor"><a href="#instance-security:security.protection.shift"
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
class="docutils literal notranslate">security.protection.shift</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Set this option to <span class="pre">`true`</span> to prevent the
instance’s file system from being UID/GID shifted on startup.

</div>

</div>

<div id="instance-security:security.secureboot"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.secureboot`</span></span><span class="shortdesc"></span>

Whether UEFI secure boot is enforced with the default Microsoft keys

<span class="anchor"><a href="#instance-security:security.secureboot"
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
class="docutils literal notranslate">security.secureboot</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

When disabling this option, consider enabling
<a href="#instance-security:security.csm"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">security.csm</code></span></a>.

</div>

</div>

<div id="instance-security:security.selinux.domain"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.selinux.domain`</span></span><span class="shortdesc"></span>

SELinux process domain override

<span class="anchor"><a href="#instance-security:security.selinux.domain"
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
class="docutils literal notranslate">security.selinux.domain</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto-detected (<span class="pre"><code
class="docutils literal notranslate">container_init_t</code></span> for
containers, <span class="pre"><code
class="docutils literal notranslate">qemu_t</code></span> for
VMs)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container or virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

Override the SELinux process domain for the instance.

</div>

</div>

<div id="instance-security:security.selinux.label_rootfs"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.selinux.label_rootfs`</span></span><span class="shortdesc"></span>

SELinux rootfs labeling mode (auto, always, never)

<span class="anchor"><a href="#instance-security:security.selinux.label_rootfs"
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
class="docutils literal notranslate">security.selinux.label_rootfs</code></span></td>
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
class="docutils literal notranslate">auto</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Control SELinux rootfs labeling behavior. The default
(<span class="pre">`auto`</span>) will label rootfs files if it is
required. Labeling will be skipped only if
<span class="pre">`security.selinux.level`</span> is explicitly set for
the instance, it is not started for the first time and the persisted
SELinux context is still valid. Setting to
<span class="pre">`always`</span> will label rootfs files on every start
and <span class="pre">`never`</span> will never touch any file labels in
the rootfs.

</div>

</div>

<div id="instance-security:security.selinux.level"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.selinux.level`</span></span><span class="shortdesc"></span>

SELinux MCS level override

<span class="anchor"><a href="#instance-security:security.selinux.level"
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
class="docutils literal notranslate">security.selinux.level</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto-generated</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container or virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

Override the SELinux MCS level for the instance. This key must only be
set on individual instances, never on profiles, since using the same MCS
level across instances breaks isolation. Values set via profiles are
ignored.

</div>

</div>

<div id="instance-security:security.selinux.type"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.selinux.type`</span></span><span class="shortdesc"></span>

SELinux file type override

<span class="anchor"><a href="#instance-security:security.selinux.type"
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
class="docutils literal notranslate">security.selinux.type</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto-detected (<span class="pre"><code
class="docutils literal notranslate">container_file_t</code></span> for
containers, <span class="pre"><code
class="docutils literal notranslate">qemu_image_t</code></span> for
VMs)</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container or virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

Override the SELinux file type used for labeling instance storage.

</div>

</div>

<div id="instance-security:security.sev"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.sev`</span></span><span class="shortdesc"></span>

Whether AMD SEV (Secure Encrypted Virtualization) is enabled for this VM

<span class="anchor"><a href="#instance-security:security.sev"
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
class="docutils literal notranslate">security.sev</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.sev.policy.es"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.sev.policy.es`</span></span><span class="shortdesc"></span>

Whether AMD SEV-ES (SEV Encrypted State) is enabled for this VM

<span class="anchor"><a href="#instance-security:security.sev.policy.es"
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
class="docutils literal notranslate">security.sev.policy.es</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.sev.session.data"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.sev.session.data`</span></span><span class="shortdesc"></span>

The guest owner’s <span class="pre">`base64`</span>-encoded session blob

<span class="anchor"><a href="#instance-security:security.sev.session.data"
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
class="docutils literal notranslate">security.sev.session.data</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.sev.session.dh"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.sev.session.dh`</span></span><span class="shortdesc"></span>

The guest owner’s <span class="pre">`base64`</span>-encoded
Diffie-Hellman key

<span class="anchor"><a href="#instance-security:security.sev.session.dh"
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
class="docutils literal notranslate">security.sev.session.dh</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>virtual machine</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.syscalls.allow"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.allow`</span></span><span class="shortdesc"></span>

List of syscalls to allow

<span class="anchor"><a href="#instance-security:security.syscalls.allow"
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
class="docutils literal notranslate">security.syscalls.allow</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

A <span class="pre">`\n`</span>-separated list of syscalls to allow.
This list must be mutually exclusive with
<span class="pre">`security.syscalls.deny*`</span>.

</div>

</div>

<div id="instance-security:security.syscalls.deny"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.deny`</span></span><span class="shortdesc"></span>

List of syscalls to deny

<span class="anchor"><a href="#instance-security:security.syscalls.deny"
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
class="docutils literal notranslate">security.syscalls.deny</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

A <span class="pre">`\n`</span>-separated list of syscalls to deny. This
list must be mutually exclusive with
<span class="pre">`security.syscalls.allow`</span>.

</div>

</div>

<div id="instance-security:security.syscalls.deny_compat"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.deny_compat`</span></span><span class="shortdesc"></span>

Whether to block <span class="pre">`compat_*`</span> syscalls
(<span class="pre">`x86_64`</span> only)

<span class="anchor"><a href="#instance-security:security.syscalls.deny_compat"
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
class="docutils literal notranslate">security.syscalls.deny_compat</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

On <span class="pre">`x86_64`</span>, this option controls whether to
block <span class="pre">`compat_*`</span> syscalls. On other
architectures, the option is ignored.

</div>

</div>

<div id="instance-security:security.syscalls.deny_default"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.deny_default`</span></span><span class="shortdesc"></span>

Whether to enable the default syscall deny

<span class="anchor"><a href="#instance-security:security.syscalls.deny_default"
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
class="docutils literal notranslate">security.syscalls.deny_default</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.syscalls.intercept.bpf"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.bpf`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`bpf()`</span> system call

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.bpf"
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
class="docutils literal notranslate">security.syscalls.intercept.bpf</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.syscalls.intercept.bpf.devices"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.bpf.devices`</span></span><span class="shortdesc"></span>

Whether to allow BPF programs

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.bpf.devices"
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
class="docutils literal notranslate">security.syscalls.intercept.bpf.devices</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

This option controls whether to allow BPF programs for the devices
cgroup in the unified hierarchy to be loaded.

</div>

</div>

<div id="instance-security:security.syscalls.intercept.mknod"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.mknod`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`mknod`</span> and
<span class="pre">`mknodat`</span> system calls

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.mknod"
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
class="docutils literal notranslate">security.syscalls.intercept.mknod</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

These system calls allow creation of a limited subset of char/block
devices.

</div>

</div>

<div id="instance-security:security.syscalls.intercept.mount"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.mount`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`mount`</span> system call

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.mount"
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
class="docutils literal notranslate">security.syscalls.intercept.mount</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.syscalls.intercept.mount.allowed"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.mount.allowed`</span></span><span class="shortdesc"></span>

File systems that can be mounted

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.mount.allowed"
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
class="docutils literal notranslate">security.syscalls.intercept.mount.allowed</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Specify a comma-separated list of file systems that are safe to mount
for processes inside the instance.

</div>

</div>

<div id="instance-security:security.syscalls.intercept.mount.fuse"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.mount.fuse`</span></span><span class="shortdesc"></span>

File system that should be redirected to FUSE implementation

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.mount.fuse"
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
class="docutils literal notranslate">security.syscalls.intercept.mount.fuse</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

Specify the mounts of a given file system that should be redirected to
their FUSE implementation (for example,
<span class="pre">`ext4=fuse2fs`</span>).

</div>

</div>

<div id="instance-security:security.syscalls.intercept.mount.shift"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.mount.shift`</span></span><span class="shortdesc"></span>

Whether to use idmapped mounts for syscall interception

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.mount.shift"
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
class="docutils literal notranslate">security.syscalls.intercept.mount.shift</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-security:security.syscalls.intercept.sched_setscheduler"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.sched_setscheduler`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`sched_setscheduler`</span>
system call

<span class="anchor"><a
href="#instance-security:security.syscalls.intercept.sched_setscheduler"
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
class="docutils literal notranslate">security.syscalls.intercept.sched_setscheduler</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

This system call allows increasing process priority.

</div>

</div>

<div id="instance-security:security.syscalls.intercept.setxattr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.setxattr`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`setxattr`</span> system call

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.setxattr"
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
class="docutils literal notranslate">security.syscalls.intercept.setxattr</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

This system call allows setting a limited subset of restricted extended
attributes.

</div>

</div>

<div id="instance-security:security.syscalls.intercept.sysinfo"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.syscalls.intercept.sysinfo`</span></span><span class="shortdesc"></span>

Whether to handle the <span class="pre">`sysinfo`</span> system call

<span class="anchor"><a href="#instance-security:security.syscalls.intercept.sysinfo"
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
class="docutils literal notranslate">security.syscalls.intercept.sysinfo</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
</tbody>
</table>

</div>

This system call can be used to get cgroup-based resource usage
information.

</div>

</div>

</div>

<div id="snapshot-scheduling-and-configuration" class="section">

<span id="instance-options-snapshots"></span>

## Snapshot scheduling and configuration<a href="#snapshot-scheduling-and-configuration" class="headerlink"
title="Link to this heading">¶</a>

The following instance options control the creation and expiry of
<a href="../../howto/instances_backup/#instances-snapshots"
class="reference internal"><span class="std std-ref">instance
snapshots</span></a>:

<div id="instance-snapshots:snapshots.expiry"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry`</span></span><span class="shortdesc"></span>

When newly created snapshots are to be deleted

<span class="anchor"><a href="#instance-snapshots:snapshots.expiry"
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

Specify an expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>.

This value is used to compute the expiry date of newly created
snapshots. It is added to the current time when a snapshot is taken, and
the resulting timestamp is stored as that snapshot’s individual expiry
date. Changing this value only affects snapshots created after the
change; the expiry date of existing snapshots is not modified.

The supported units are <span class="pre">`S`</span> (seconds),
<span class="pre">`M`</span> (minutes), <span class="pre">`H`</span>
(hours), <span class="pre">`d`</span> (days),
<span class="pre">`w`</span> (weeks), <span class="pre">`m`</span>
(months) and <span class="pre">`y`</span> (years).

</div>

</div>

<div id="instance-snapshots:snapshots.expiry.manual"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.expiry.manual`</span></span><span class="shortdesc"></span>

When newly created snapshots are to be deleted (for those not created
through scheduling)

<span class="anchor"><a href="#instance-snapshots:snapshots.expiry.manual"
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

Specify an expression like
<span class="pre">`1M`</span>` `<span class="pre">`2H`</span>` `<span class="pre">`3d`</span>` `<span class="pre">`4w`</span>` `<span class="pre">`5m`</span>` `<span class="pre">`6y`</span>.

This value is used to compute the expiry date of newly created snapshots
that are not created through scheduling. It is added to the current time
when a snapshot is taken, and the resulting timestamp is stored as that
snapshot’s individual expiry date. Changing this value only affects
snapshots created after the change; the expiry date of existing
snapshots is not modified.

See <a href="#instance-snapshots:snapshots.expiry"
class="configref reference internal"><span class="pre"><code
class="docutils literal notranslate">snapshots.expiry</code></span></a>
for the supported units.

</div>

</div>

<div id="instance-snapshots:snapshots.pattern"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.pattern`</span></span><span class="shortdesc"></span>

Template for the snapshot name

<span class="anchor"><a href="#instance-snapshots:snapshots.pattern"
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
<p><span class="pre"><code
class="docutils literal notranslate">snap%d</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

Specify a Pongo2 template string that represents the snapshot name. This
template is used for scheduled snapshots and for unnamed snapshots.

See <a href="#instance-options-snapshots-names"
class="reference internal"><span class="std std-ref">Automatic snapshot
names</span></a> for more information.

</div>

</div>

<div id="instance-snapshots:snapshots.schedule"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.schedule`</span></span><span class="shortdesc"></span>

Schedule for automatic instance snapshots

<span class="anchor"><a href="#instance-snapshots:snapshots.schedule"
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
<p>empty</p></td>
</tr>
<tr class="even row-even">
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

Specify either a cron expression
(<span class="pre">`<minute>`</span>` `<span class="pre">`<hour>`</span>` `<span class="pre">`<dom>`</span>` `<span class="pre">`<month>`</span>` `<span class="pre">`<dow>`</span>),
a comma-and-space-separated list of schedule aliases
(<span class="pre">`@startup`</span>,
<span class="pre">`@hourly`</span>, <span class="pre">`@daily`</span>,
<span class="pre">`@midnight`</span>,
<span class="pre">`@weekly`</span>, <span class="pre">`@monthly`</span>,
<span class="pre">`@annually`</span>,
<span class="pre">`@yearly`</span>), or leave empty to disable automatic
snapshots.

Note that unlike most other configuration keys, this one must be
comma-and-space-separated and not just comma-separated as cron
expression can themselves contain commas.

</div>

</div>

<div id="instance-snapshots:snapshots.schedule.stopped"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`snapshots.schedule.stopped`</span></span><span class="shortdesc"></span>

Whether to automatically snapshot stopped instances

<span class="anchor"><a href="#instance-snapshots:snapshots.schedule.stopped"
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
class="docutils literal notranslate">snapshots.schedule.stopped</code></span></td>
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
<td><strong>Live update:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="automatic-snapshot-names" class="section">

<span id="instance-options-snapshots-names"></span>

### Automatic snapshot names<a href="#automatic-snapshot-names" class="headerlink"
title="Link to this heading">¶</a>

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

</div>

<div id="volatile-internal-data" class="section">

<span id="instance-options-volatile"></span>

## Volatile internal data<a href="#volatile-internal-data" class="headerlink"
title="Link to this heading">¶</a>

The following volatile keys are currently used internally by Incus to
store internal data specific to an instance:

<div id="instance-volatile:volatile.<name>.apply_quota"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.apply_quota`</span></span><span class="shortdesc"></span>

Disk quota

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.apply_quota"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.apply_quota</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The disk quota is applied the next time the instance starts.

</div>

</div>

<div id="instance-volatile:volatile.<name>.ceph_rbd"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.ceph_rbd`</span></span><span class="shortdesc"></span>

RBD device path for Ceph disk devices

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.ceph_rbd"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.ceph_rbd</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.<name>.host_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.host_name`</span></span><span class="shortdesc"></span>

Network device name on the host

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.host_name"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.host_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.<name>.hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.hwaddr`</span></span><span class="shortdesc"></span>

Network device MAC address

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.hwaddr"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The network device MAC address is used when no
<span class="pre">`hwaddr`</span> property is set on the device itself.

</div>

</div>

<div id="instance-volatile:volatile.<name>.io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.io.bus`</span></span><span class="shortdesc"></span>

IO bus in use

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.io.bus"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.io.bus</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The IO bus stores the actual IO bus being used, checked in case
<span class="pre">`io.bus=auto`</span>.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.created"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.created`</span></span><span class="shortdesc"></span>

Whether the network device physical device was created

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.created"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.created</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

Possible values are <span class="pre">`true`</span> or
<span class="pre">`false`</span>.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.hwaddr`</span></span><span class="shortdesc"></span>

Network device original MAC

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.hwaddr"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original MAC that was used when moving a physical device into an
instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.ip_addresses"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.ip_addresses`</span></span><span class="shortdesc"></span>

Last used IP addresses

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.ip_addresses"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.ip_addresses</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

Comma-separated list of the last used IP addresses of the network
device.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.mtu`</span></span><span class="shortdesc"></span>

Network device original MTU

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.mtu"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original MTU that was used when moving a physical device into an
instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.pci.driver"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.pci.driver`</span></span><span class="shortdesc"></span>

PCI original host driver

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.pci.driver"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.pci.driver</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original host driver for the PCI device.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.pci.parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.pci.parent`</span></span><span class="shortdesc"></span>

PCI parent host device

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.pci.parent"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.pci.parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The parent host device used when allocating a PCI device to an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.pci.slot.name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.pci.slot.name`</span></span><span class="shortdesc"></span>

PCI parent slot name

<span class="anchor"><a
href="#instance-volatile:volatile.%3Cname%3E.last_state.pci.slot.name"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.pci.slot.name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The parent host device PCI slot name.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.usb.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.usb.bus`</span></span><span class="shortdesc"></span>

USB bus address

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.usb.bus"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.usb.bus</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original USB bus address.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.usb.device"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.usb.device`</span></span><span class="shortdesc"></span>

USB device identifier

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.usb.device"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.usb.device</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original USB device identifier.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vdpa.name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vdpa.name`</span></span><span class="shortdesc"></span>

VDPA device name

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vdpa.name"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vdpa.name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The VDPA device name used when moving a VDPA device file descriptor into
an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.hwaddr`</span></span><span class="shortdesc"></span>

SR-IOV virtual function original MAC

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.hwaddr"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original MAC used when moving a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.id"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.id`</span></span><span class="shortdesc"></span>

SR-IOV virtual function ID

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.id"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.id</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The ID used when moving a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.parent`</span></span><span class="shortdesc"></span>

SR-IOV parent host device

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.parent"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The parent host device used when allocating a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.spoofcheck"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.spoofcheck`</span></span><span class="shortdesc"></span>

SR-IOV virtual function original spoof check setting

<span class="anchor"><a
href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.spoofcheck"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.spoofcheck</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original spoof check setting used when moving a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.trusted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.trusted`</span></span><span class="shortdesc"></span>

SR-IOV virtual function original trusted setting

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.trusted"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.trusted</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original trusted setting used when moving a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.last_state.vf.vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.last_state.vf.vlan`</span></span><span class="shortdesc"></span>

SR-IOV virtual function original VLAN

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.last_state.vf.vlan"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.last_state.vf.vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The original VLAN used when moving a VF into an instance.

</div>

</div>

<div id="instance-volatile:volatile.<name>.mig.uuid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.mig.uuid`</span></span><span class="shortdesc"></span>

MIG instance UUID

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.mig.uuid"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.mig.uuid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The NVIDIA MIG instance UUID.

</div>

</div>

<div id="instance-volatile:volatile.<name>.name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.name`</span></span><span class="shortdesc"></span>

Network interface name inside of the instance

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.name"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The network interface name inside of the instance when no
<span class="pre">`name`</span> property is set on the device itself.

</div>

</div>

<div id="instance-volatile:volatile.<name>.vgpu.uuid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.<name>.vgpu.uuid`</span></span><span class="shortdesc"></span>

virtual GPU instance UUID

<span class="anchor"><a href="#instance-volatile:volatile.%3Cname%3E.vgpu.uuid"
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
class="docutils literal notranslate">volatile.&lt;name&gt;.vgpu.uuid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The NVIDIA virtual GPU instance UUID.

</div>

</div>

<div id="instance-volatile:volatile.apply_nvram"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.apply_nvram`</span></span><span class="shortdesc"></span>

Whether to regenerate VM NVRAM the next time the instance starts

<span class="anchor"><a href="#instance-volatile:volatile.apply_nvram"
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
class="docutils literal notranslate">volatile.apply_nvram</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.apply_template"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.apply_template`</span></span><span class="shortdesc"></span>

Template hook

<span class="anchor"><a href="#instance-volatile:volatile.apply_template"
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
class="docutils literal notranslate">volatile.apply_template</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The template with the given name is triggered upon next startup.

</div>

</div>

<div id="instance-volatile:volatile.base_image"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.base_image`</span></span><span class="shortdesc"></span>

Hash of the base image

<span class="anchor"><a href="#instance-volatile:volatile.base_image"
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
class="docutils literal notranslate">volatile.base_image</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The hash of the image that the instance was created from (empty if the
instance was not created from an image).

</div>

</div>

<div id="instance-volatile:volatile.cloud_init.instance-id"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.cloud_init.instance-id`</span></span><span class="shortdesc"></span>

<span class="pre">`instance-id`</span> (UUID) exposed to
<span class="pre">`cloud-init`</span>

<span class="anchor"><a href="#instance-volatile:volatile.cloud_init.instance-id"
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
class="docutils literal notranslate">volatile.cloud_init.instance-id</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.cluster.group"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.cluster.group`</span></span><span class="shortdesc"></span>

The original cluster group for the instance

<span class="anchor"><a href="#instance-volatile:volatile.cluster.group"
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
class="docutils literal notranslate">volatile.cluster.group</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The cluster group(s) that the instance was restricted to at creation
time. This is used during re-scheduling events like an evacuation to
keep the instance within the requested set.

</div>

</div>

<div id="instance-volatile:volatile.container.oci"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.container.oci`</span></span><span class="shortdesc"></span>

Whether the container is an OCI application container

<span class="anchor"><a href="#instance-volatile:volatile.container.oci"
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
class="docutils literal notranslate">volatile.container.oci</code></span></td>
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
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.cpu.nodes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.cpu.nodes`</span></span><span class="shortdesc"></span>

Instance NUMA node

<span class="anchor"><a href="#instance-volatile:volatile.cpu.nodes"
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
class="docutils literal notranslate">volatile.cpu.nodes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The NUMA node that was selected for the instance.

</div>

</div>

<div id="instance-volatile:volatile.evacuate.origin"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.evacuate.origin`</span></span><span class="shortdesc"></span>

The origin of the evacuated instance

<span class="anchor"><a href="#instance-volatile:volatile.evacuate.origin"
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
class="docutils literal notranslate">volatile.evacuate.origin</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The cluster member that the instance lived on before evacuation.

</div>

</div>

<div id="instance-volatile:volatile.idmap.base"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.idmap.base`</span></span><span class="shortdesc"></span>

The first ID in the instance’s primary idmap range

<span class="anchor"><a href="#instance-volatile:volatile.idmap.base"
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
class="docutils literal notranslate">volatile.idmap.base</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.idmap.current"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.idmap.current`</span></span><span class="shortdesc"></span>

The idmap currently in use by the instance

<span class="anchor"><a href="#instance-volatile:volatile.idmap.current"
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
class="docutils literal notranslate">volatile.idmap.current</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.idmap.next"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.idmap.next`</span></span><span class="shortdesc"></span>

The idmap to use the next time the instance starts

<span class="anchor"><a href="#instance-volatile:volatile.idmap.next"
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
class="docutils literal notranslate">volatile.idmap.next</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.last_state.agent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.last_state.agent`</span></span><span class="shortdesc"></span>

Instance agent state as of last host shutdown

<span class="anchor"><a href="#instance-volatile:volatile.last_state.agent"
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
class="docutils literal notranslate">volatile.last_state.agent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.last_state.idmap"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.last_state.idmap`</span></span><span class="shortdesc"></span>

Serialized instance UID/GID map

<span class="anchor"><a href="#instance-volatile:volatile.last_state.idmap"
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
class="docutils literal notranslate">volatile.last_state.idmap</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.last_state.power"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.last_state.power`</span></span><span class="shortdesc"></span>

Instance state as of last host shutdown

<span class="anchor"><a href="#instance-volatile:volatile.last_state.power"
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
class="docutils literal notranslate">volatile.last_state.power</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.last_state.ready"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.last_state.ready`</span></span><span class="shortdesc"></span>

Instance marked itself as ready

<span class="anchor"><a href="#instance-volatile:volatile.last_state.ready"
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
class="docutils literal notranslate">volatile.last_state.ready</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.rebalance.last_move"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.rebalance.last_move`</span></span><span class="shortdesc"></span>

Timestamp of last move by automatic live-migration

<span class="anchor"><a href="#instance-volatile:volatile.rebalance.last_move"
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
class="docutils literal notranslate">volatile.rebalance.last_move</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.uuid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.uuid`</span></span><span class="shortdesc"></span>

Instance UUID

<span class="anchor"><a href="#instance-volatile:volatile.uuid"
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
class="docutils literal notranslate">volatile.uuid</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The instance UUID is globally unique across all servers and projects.

</div>

</div>

<div id="instance-volatile:volatile.uuid.generation"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.uuid.generation`</span></span><span class="shortdesc"></span>

Instance generation UUID

<span class="anchor"><a href="#instance-volatile:volatile.uuid.generation"
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
class="docutils literal notranslate">volatile.uuid.generation</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

The instance generation UUID changes whenever the instance’s place in
time moves backwards. It is globally unique across all servers and
projects.

</div>

</div>

<div id="instance-volatile:volatile.vm.boot_state"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.vm.boot_state`</span></span><span class="shortdesc"></span>

JSON encoded VM properties used during live migration and other state
restoration.

<span class="anchor"><a href="#instance-volatile:volatile.vm.boot_state"
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
class="docutils literal notranslate">volatile.vm.boot_state</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.vm.needs_reset"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.vm.needs_reset`</span></span><span class="shortdesc"></span>

Indicates that the VM needs a full reset on next reboot

<span class="anchor"><a href="#instance-volatile:volatile.vm.needs_reset"
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
class="docutils literal notranslate">volatile.vm.needs_reset</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="instance-volatile:volatile.vm.rtc_adjustment"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.vm.rtc_adjustment`</span></span><span class="shortdesc"></span>

Real Time Clock change adjustment

<span class="anchor"><a href="#instance-volatile:volatile.vm.rtc_adjustment"
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
class="docutils literal notranslate">volatile.vm.rtc_adjustment</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>int64</p></td>
</tr>
</tbody>
</table>

</div>

Real Time Clock adjustment time to allow virtual machines to run on a
different base than the host.

</div>

</div>

<div id="instance-volatile:volatile.vm.rtc_offset"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.vm.rtc_offset`</span></span><span class="shortdesc"></span>

Real Time Clock change offset

<span class="anchor"><a href="#instance-volatile:volatile.vm.rtc_offset"
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
class="docutils literal notranslate">volatile.vm.rtc_offset</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>int64</p></td>
</tr>
</tbody>
</table>

</div>

Real Time Clock offset to allow virtual machines to run on a different
base than the host.

</div>

</div>

<div id="instance-volatile:volatile.vsock_id"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`volatile.vsock_id`</span></span><span class="shortdesc"></span>

Instance
<span class="pre">`vsock`</span>` `<span class="pre">`ID`</span> used as
of last start

<span class="anchor"><a href="#instance-volatile:volatile.vsock_id"
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
class="docutils literal notranslate">volatile.vsock_id</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="admonition note">

Note

Volatile keys cannot be set by the user.

</div>

</div>

</div>

</div>

<div class="related-pages">

<a href="../devices/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Devices

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../instance_properties/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Instance properties

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">