# Type: <span class="pre">`nic`</span><a href="#type-nic" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

The <span class="pre">`nic`</span> device type is supported for both
containers and VMs.

NICs support hotplugging for both containers and VMs (with the exception
of the <span class="pre">`ipvlan`</span> NIC type).

</div>

Network devices, also referred to as *Network Interface Controllers* or
*NICs*, supply a connection to a network. Incus supports several
different types of network devices (*NIC types*).

<div class="admonition note">

Note

When using a USB network adapter with a VM, mainline QEMU will replace
the leading two bytes of a MAC address with
<span class="pre">`40:`</span>. Those affected by this may want to
manually set the <span class="pre">`hwaddr`</span> property to a MAC
address starting with <span class="pre">`40:`</span> to align the host
and guest reporting of the MAC.

</div>

<div id="nictype-vs-network" class="section">

## <span class="pre">`nictype`</span> vs. <span class="pre">`network`</span><a href="#nictype-vs-network" class="headerlink"
title="Link to this heading">¶</a>

When adding a network device to an instance, there are two methods to
specify the type of device that you want to add: through the
<span class="pre">`nictype`</span> device option or the
<span class="pre">`network`</span> device option.

These two device options are mutually exclusive, and you can specify
only one of them when you create a device. However, note that when you
specify the <span class="pre">`network`</span> option, the
<span class="pre">`nictype`</span> option is derived automatically from
the network type.

<span class="pre">`nictype`</span>  
When using the <span class="pre">`nictype`</span> device option, you can
specify a network interface that is not controlled by Incus. Therefore,
you must specify all information that Incus needs to use the network
interface.

When using this method, the <span class="pre">`nictype`</span> option
must be specified when creating the device, and it cannot be changed
later.

<span class="pre">`network`</span>  
When using the <span class="pre">`network`</span> device option, the NIC
is linked to an existing
<a href="../../explanation/networks/#managed-networks"
class="reference internal"><span class="std std-ref">managed
network</span></a>. In this case, Incus has all required information
about the network, and you need to specify only the network name when
adding the device.

When using this method, Incus derives the
<span class="pre">`nictype`</span> option automatically. The value is
read-only and cannot be changed.

Other device options that are inherited from the network are marked with
a “yes” in the “Managed” column of the NIC-specific tables of device
options. You cannot customize these options directly for the NIC if
you’re using the <span class="pre">`network`</span> method.

See <a href="../../explanation/networks/#networks"
class="reference internal"><span class="std std-ref">About
networking</span></a> for more information.

</div>

<div id="available-nic-types" class="section">

## Available NIC types<a href="#available-nic-types" class="headerlink"
title="Link to this heading">¶</a>

The following NICs can be added using the
<span class="pre">`nictype`</span> or <span class="pre">`network`</span>
options:

- <a href="#nic-bridged" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">bridged</code></span></span></a>:
  Uses an existing bridge on the host and creates a virtual device pair
  to connect the host bridge to the instance.

- <a href="#nic-macvlan" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">macvlan</code></span></span></a>:
  Sets up a new network device based on an existing one, but using a
  different MAC address.

- <a href="#nic-sriov" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">sriov</code></span></span></a>:
  Passes a virtual function of an SR-IOV-enabled physical network device
  into the instance.

- <a href="#nic-physical" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">physical</code></span></span></a>:
  Passes a physical device from the host through to the instance. The
  targeted device will vanish from the host and appear in the instance.

The following NICs can be added using only the
<span class="pre">`network`</span> option:

- <a href="#nic-ovn" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">ovn</code></span></span></a>:
  Uses an existing OVN network and creates a virtual device pair to
  connect the instance to it.

The following NICs can be added using only the
<span class="pre">`nictype`</span> option:

- <a href="#nic-ipvlan" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">ipvlan</code></span></span></a>:
  Sets up a new network device based on an existing one, using the same
  MAC address but a different IP.

- <a href="#nic-p2p" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">p2p</code></span></span></a>:
  Creates a virtual device pair, putting one side in the instance and
  leaving the other side on the host.

- <a href="#nic-routed" class="reference internal"><span
  class="std std-ref"><span class="pre"><code
  class="docutils literal notranslate">routed</code></span></span></a>:
  Creates a virtual device pair to connect the host to the instance and
  sets up static routes and proxy ARP/NDP entries to allow the instance
  to join the network of a designated parent interface.

The available device options depend on the NIC type and are listed in
the tables in the following sections.

<div id="nictype-bridged" class="section">

<span id="nic-bridged"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`bridged`</span><a href="#nictype-bridged" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type through the
<span class="pre">`nictype`</span> option or the
<span class="pre">`network`</span> option (see
<a href="../network_bridge/#network-bridge"
class="reference internal"><span class="std std-ref">Bridge
network</span></a> for information about the managed
<span class="pre">`bridge`</span> network).

</div>

A <span class="pre">`bridged`</span> NIC uses an existing bridge on the
host and creates a virtual device pair to connect the host bridge to the
instance.

<div id="device-options" class="section">

#### Device options<a href="#device-options" class="headerlink"
title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`bridged`</span> have the
following device options:

<div id="devices-nic_bridged:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_bridged:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_bridged:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_bridged:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:connected"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`connected`</span></span><span class="shortdesc"></span>

Whether the NIC is connected to the host network

<span class="anchor"><a href="#devices-nic_bridged:connected"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">connected</code></span></td>
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

<div id="devices-nic_bridged:host_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`host_name`</span></span><span class="shortdesc"></span>

The name of the interface on the host

<span class="anchor"><a href="#devices-nic_bridged:host_name"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">host_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_bridged:hwaddr"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Override the bus for the device (can be
<span class="pre">`virtio`</span> or <span class="pre">`usb`</span>) (VM
only)

<span class="anchor"><a href="#devices-nic_bridged:io.bus"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
class="docutils literal notranslate">virtio</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv4.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.address`</span></span><span class="shortdesc"></span>

An IPv4 address to assign to the instance through DHCP (can be
<span class="pre">`none`</span> to restrict all IPv4 traffic when
<span class="pre">`security.ipv4_filtering`</span> is set, or a CIDR
value to statically configure the address inside an OCI container)

<span class="anchor"><a href="#devices-nic_bridged:ipv4.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.address</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv4.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway`</span></span><span class="shortdesc"></span>

IPv4 default gateway to statically configure inside an OCI container

<span class="anchor"><a href="#devices-nic_bridged:ipv4.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv4.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to add on host to NIC

<span class="anchor"><a href="#devices-nic_bridged:ipv4.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv4.routes.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes.external`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to route to the NIC and
publish on uplink network (BGP)

<span class="anchor"><a href="#devices-nic_bridged:ipv4.routes.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv6.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.address`</span></span><span class="shortdesc"></span>

An IPv6 address to assign to the instance through DHCP (can be
<span class="pre">`none`</span> to restrict all IPv6 traffic when
<span class="pre">`security.ipv6_filtering`</span> is set, or a CIDR
value to statically configure the address inside an OCI container)

<span class="anchor"><a href="#devices-nic_bridged:ipv6.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.address</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv6.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway`</span></span><span class="shortdesc"></span>

IPv6 default gateway to statically configure inside an OCI container

<span class="anchor"><a href="#devices-nic_bridged:ipv6.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv6.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to add on host to NIC

<span class="anchor"><a href="#devices-nic_bridged:ipv6.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:ipv6.routes.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes.external`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to route to the NIC and
publish on uplink network (BGP)

<span class="anchor"><a href="#devices-nic_bridged:ipv6.routes.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:limits.egress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.egress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for outgoing traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_bridged:limits.egress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.egress</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:limits.ingress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.ingress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for incoming traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_bridged:limits.ingress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.ingress</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:limits.max"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.max`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for both incoming and outgoing traffic (same as
setting both limits.ingress and limits.egress)

<span class="anchor"><a href="#devices-nic_bridged:limits.max"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:limits.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.priority`</span></span><span class="shortdesc"></span>

The priority for outgoing traffic, to be used by the kernel queuing
discipline to prioritize network packets

<span class="anchor"><a href="#devices-nic_bridged:limits.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_bridged:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>MTU of the parent device</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_bridged:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:network"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`network`</span></span><span class="shortdesc"></span>

The managed network to link the device to (instead of specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_bridged:network"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">network</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the parent host device (required if specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_bridged:parent"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:queue.tx.length"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`queue.tx.length`</span></span><span class="shortdesc"></span>

The transmit queue length for the NIC

<span class="anchor"><a href="#devices-nic_bridged:queue.tx.length"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">queue.tx.length</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.acls"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls`</span></span><span class="shortdesc"></span>

Comma-separated list of network ACLs to apply

<span class="anchor"><a href="#devices-nic_bridged:security.acls"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.acls.default.egress.action"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.egress.action`</span></span><span class="shortdesc"></span>

Action to use for egress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_bridged:security.acls.default.egress.action"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.egress.action</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>drop</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.acls.default.egress.logged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.egress.logged`</span></span><span class="shortdesc"></span>

Whether to log egress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_bridged:security.acls.default.egress.logged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.egress.logged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.acls.default.ingress.action"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.ingress.action`</span></span><span class="shortdesc"></span>

Action to use for ingress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_bridged:security.acls.default.ingress.action"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.ingress.action</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>drop</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.acls.default.ingress.logged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.ingress.logged`</span></span><span class="shortdesc"></span>

Whether to log ingress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_bridged:security.acls.default.ingress.logged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.ingress.logged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.ipv4_filtering"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.ipv4_filtering`</span></span><span class="shortdesc"></span>

Prevent the instance from spoofing another instance’s IPv4 address
(enables <span class="pre">`security.mac_filtering`</span>)

<span class="anchor"><a href="#devices-nic_bridged:security.ipv4_filtering"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.ipv4_filtering</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.ipv6_filtering"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.ipv6_filtering`</span></span><span class="shortdesc"></span>

Prevent the instance from spoofing another instance’s IPv6 address
(enables <span class="pre">`security.mac_filtering`</span>)

<span class="anchor"><a href="#devices-nic_bridged:security.ipv6_filtering"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.ipv6_filtering</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.mac_filtering"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.mac_filtering`</span></span><span class="shortdesc"></span>

Prevent the instance from spoofing another instance’s MAC address

<span class="anchor"><a href="#devices-nic_bridged:security.mac_filtering"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.mac_filtering</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:security.port_isolation"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.port_isolation`</span></span><span class="shortdesc"></span>

Prevent the NIC from communicating with other NICs in the network that
have port isolation enabled

<span class="anchor"><a href="#devices-nic_bridged:security.port_isolation"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.port_isolation</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to use for non-tagged traffic (can be none to remove port
from default VLAN)

<span class="anchor"><a href="#devices-nic_bridged:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_bridged:vlan.tagged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan.tagged`</span></span><span class="shortdesc"></span>

Comma-delimited list of VLAN IDs or VLAN ranges to join for tagged
traffic

<span class="anchor"><a href="#devices-nic_bridged:vlan.tagged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan.tagged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
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

<div id="nictype-macvlan" class="section">

<span id="nic-macvlan"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`macvlan`</span><a href="#nictype-macvlan" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type through the
<span class="pre">`nictype`</span> option or the
<span class="pre">`network`</span> option (see
<a href="../network_macvlan/#network-macvlan"
class="reference internal"><span class="std std-ref">Macvlan
network</span></a> for information about the managed
<span class="pre">`macvlan`</span> network).

</div>

A <span class="pre">`macvlan`</span> NIC sets up a new network device
based on an existing one, but using a different MAC address.

If you are using a <span class="pre">`macvlan`</span> NIC, communication
between the Incus host and the instances is not possible. Both the host
and the instances can talk to the gateway, but they cannot communicate
directly.

<div id="id1" class="section">

#### Device options<a href="#id1" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`macvlan`</span> have the
following device options:

<div id="devices-nic_macvlan:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_macvlan:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_macvlan:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_macvlan:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:connected"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`connected`</span></span><span class="shortdesc"></span>

Whether the NIC is connected to the host network (VM only)

<span class="anchor"><a href="#devices-nic_macvlan:connected"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">connected</code></span></td>
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

<div id="devices-nic_macvlan:gvrp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`gvrp`</span></span><span class="shortdesc"></span>

Register VLAN using GARP VLAN Registration Protocol

<span class="anchor"><a href="#devices-nic_macvlan:gvrp" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">gvrp</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_macvlan:hwaddr"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Override the bus for the device (can be
<span class="pre">`virtio`</span> or <span class="pre">`usb`</span>) (VM
only)

<span class="anchor"><a href="#devices-nic_macvlan:io.bus"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
class="docutils literal notranslate">virtio</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mode`</span></span><span class="shortdesc"></span>

Macvlan mode (one of <span class="pre">`bridge`</span>,
<span class="pre">`vepa`</span>, <span class="pre">`passthru`</span> or
<span class="pre">`private`</span>)

<span class="anchor"><a href="#devices-nic_macvlan:mode" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mode</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>bridge</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_macvlan:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>MTU of the parent device</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_macvlan:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:network"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`network`</span></span><span class="shortdesc"></span>

The managed network to link the device to (instead of specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_macvlan:network"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">network</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the parent host device (required if specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_macvlan:parent"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_macvlan:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#devices-nic_macvlan:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
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

<div id="nictype-sriov" class="section">

<span id="nic-sriov"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`sriov`</span><a href="#nictype-sriov" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type through the
<span class="pre">`nictype`</span> option or the
<span class="pre">`network`</span> option (see
<a href="../network_sriov/#network-sriov"
class="reference internal"><span class="std std-ref">SR-IOV
network</span></a> for information about the managed
<span class="pre">`sriov`</span> network).

</div>

An <span class="pre">`sriov`</span> NIC passes a virtual function of an
SR-IOV-enabled physical network device into the instance.

An SR-IOV-enabled network device associates a set of virtual functions
(VFs) with the single physical function (PF) of the network device. PFs
are standard PCIe functions. VFs, on the other hand, are very
lightweight PCIe functions that are optimized for data movement. They
come with a limited set of configuration capabilities to prevent
changing properties of the PF.

Given that VFs appear as regular PCIe devices to the system, they can be
passed to instances just like a regular physical device.

VF allocation  
The <span class="pre">`sriov`</span> interface type expects to be passed
the name of an SR-IOV enabled network device on the system via the
<span class="pre">`parent`</span> property. Incus then checks for any
available VFs on the system.

By default, Incus allocates the first free VF it finds. If it detects
that either none are enabled or all currently enabled VFs are in use, it
bumps the number of supported VFs to the maximum value and uses the
first free VF. If all possible VFs are in use or the kernel or card
doesn’t support incrementing the number of VFs, Incus returns an error.

<div class="admonition note">

Note

If you need Incus to use a specific VF, use a
<span class="pre">`physical`</span> NIC instead of a
<span class="pre">`sriov`</span> NIC and set its
<span class="pre">`parent`</span> option to the VF name.

</div>

<div id="id2" class="section">

#### Device options<a href="#id2" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`sriov`</span> have the following
device options:

<div id="devices-nic_sriov:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_sriov:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_sriov:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_sriov:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_sriov:hwaddr" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:mtu" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_sriov:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_sriov:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:network"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`network`</span></span><span class="shortdesc"></span>

The managed network to link the device to (instead of specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_sriov:network" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">network</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the parent host device (required if specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_sriov:parent" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:pci" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`pci`</span></span><span class="shortdesc"></span>

The PCI address of the parent host device

<span class="anchor"><a href="#devices-nic_sriov:pci" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">pci</code></span></td>
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

<div id="devices-nic_sriov:productid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`productid`</span></span><span class="shortdesc"></span>

The product ID of the parent host device

<span class="anchor"><a href="#devices-nic_sriov:productid"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">productid</code></span></td>
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

<div id="devices-nic_sriov:security.mac_filtering"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.mac_filtering`</span></span><span class="shortdesc"></span>

Prevent the instance from spoofing another instance’s MAC address

<span class="anchor"><a href="#devices-nic_sriov:security.mac_filtering"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.mac_filtering</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:security.trusted"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.trusted`</span></span><span class="shortdesc"></span>

Allows the instance to configure the NIC in ways that may negatively
impact security.

<span class="anchor"><a href="#devices-nic_sriov:security.trusted"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.trusted</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false, if supported by parent device</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_sriov:vendorid"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vendorid`</span></span><span class="shortdesc"></span>

The vendor ID of the parent host device

<span class="anchor"><a href="#devices-nic_sriov:vendorid"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vendorid</code></span></td>
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

<div id="devices-nic_sriov:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#devices-nic_sriov:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
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

<div id="nictype-ovn" class="section">

<span id="nic-ovn"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`ovn`</span><a href="#nictype-ovn" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type only through the
<span class="pre">`network`</span> option (see
<a href="../network_ovn/#network-ovn" class="reference internal"><span
class="std std-ref">OVN network</span></a> for information about the
managed <span class="pre">`ovn`</span> network).

</div>

An <span class="pre">`ovn`</span> NIC uses an existing OVN network and
creates a virtual device pair to connect the instance to it.

SR-IOV hardware acceleration  
To use <span class="pre">`acceleration=sriov`</span>, you must have a
compatible SR-IOV physical NIC that supports the Ethernet switch device
driver model (<span class="pre">`switchdev`</span>) in your Incus host.
Incus assumes that the physical NIC (PF) is configured in
<span class="pre">`switchdev`</span> mode and connected to the OVN
integration OVS bridge, and that it has one or more virtual functions
(VFs) active.

To achieve this, follow these basic prerequisite setup steps:

1.  Set up PF and VF:

    1.  Activate some VFs on PF (called
        <span class="pre">`enp9s0f0np0`</span> in the following example,
        with a PCI address of <span class="pre">`0000:09:00.0`</span>)
        and unbind them.

    2.  Enable <span class="pre">`switchdev`</span> mode and
        <span class="pre">`hw-tc-offload`</span> on the PF.

    3.  Rebind the VFs.

    <div class="highlight-default notranslate">

    <div class="highlight">

        echo 4 > /sys/bus/pci/devices/0000:09:00.0/sriov_numvfs
        for i in $(lspci -nnn | grep "Virtual Function" | cut -d' ' -f1); do echo 0000:$i > /sys/bus/pci/drivers/mlx5_core/unbind; done
        devlink dev eswitch set pci/0000:09:00.0 mode switchdev
        ethtool -K enp9s0f0np0 hw-tc-offload on
        for i in $(lspci -nnn | grep "Virtual Function" | cut -d' ' -f1); do echo 0000:$i > /sys/bus/pci/drivers/mlx5_core/bind; done

    </div>

    </div>

2.  Set up OVS by enabling hardware offload and adding the PF NIC to the
    integration bridge (normally called
    <span class="pre">`br-int`</span>):

    <div class="highlight-default notranslate">

    <div class="highlight">

        ovs-vsctl set open_vswitch . other_config:hw-offload=true
        systemctl restart openvswitch-switch
        ovs-vsctl add-port br-int enp9s0f0np0
        ip link set enp9s0f0np0 up

    </div>

    </div>

VDPA hardware acceleration  
To use <span class="pre">`acceleration=vdpa`</span>, you must have a
compatible VDPA physical NIC. The setup is the same as for SR-IOV
hardware acceleration, except that you must also enable the
<span class="pre">`vhost_vdpa`</span> module and check that you have
some available VDPA management devices :

<div class="highlight-default notranslate">

<div class="highlight">

    modprobe vhost_vdpa && vdpa mgmtdev show

</div>

</div>

<div id="id3" class="section">

#### Device options<a href="#id3" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`ovn`</span> have the following
device options:

<div id="devices-nic_ovn:acceleration"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`acceleration`</span></span><span class="shortdesc"></span>

Enable hardware offloading (either <span class="pre">`none`</span>,
<span class="pre">`sriov`</span> or <span class="pre">`vdpa`</span>)

<span class="anchor"><a href="#devices-nic_ovn:acceleration"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">acceleration</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>none</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_ovn:attached" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_ovn:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_ovn:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:connected"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`connected`</span></span><span class="shortdesc"></span>

Whether the NIC is connected to the host network (requires
<span class="pre">`acceleration`</span> set to
<span class="pre">`none`</span>)

<span class="anchor"><a href="#devices-nic_ovn:connected" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">connected</code></span></td>
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

<div id="devices-nic_ovn:host_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`host_name`</span></span><span class="shortdesc"></span>

The name of the interface inside the host

<span class="anchor"><a href="#devices-nic_ovn:host_name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">host_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_ovn:hwaddr" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Override the bus for the device (can be
<span class="pre">`virtio`</span> or <span class="pre">`usb`</span>,
requires <span class="pre">`acceleration`</span> set to
<span class="pre">`none`</span>) (VM only)

<span class="anchor"><a href="#devices-nic_ovn:io.bus" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
class="docutils literal notranslate">virtio</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv4.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.address`</span></span><span class="shortdesc"></span>

An IPv4 address to assign to the instance through DHCP,
<span class="pre">`none`</span> can be used to disable IP allocation (or
a CIDR value to statically configure the address inside an OCI
container)

<span class="anchor"><a href="#devices-nic_ovn:ipv4.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.address</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv4.address.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.address.external`</span></span><span class="shortdesc"></span>

Select a specific external address (typically from a network forward)

<span class="anchor"><a href="#devices-nic_ovn:ipv4.address.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.address.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv4.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway`</span></span><span class="shortdesc"></span>

IPv4 default gateway to statically configure inside an OCI container

<span class="anchor"><a href="#devices-nic_ovn:ipv4.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv4.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to route to the NIC

<span class="anchor"><a href="#devices-nic_ovn:ipv4.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv4.routes.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes.external`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to route to the NIC and
publish on uplink network

<span class="anchor"><a href="#devices-nic_ovn:ipv4.routes.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv6.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.address`</span></span><span class="shortdesc"></span>

An IPv6 address to assign to the instance through DHCP,
<span class="pre">`none`</span> can be used to disable IP allocation (or
a CIDR value to statically configure the address inside an OCI
container)

<span class="anchor"><a href="#devices-nic_ovn:ipv6.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.address</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv6.address.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.address.external`</span></span><span class="shortdesc"></span>

Select a specific external address (typically from a network forward)

<span class="anchor"><a href="#devices-nic_ovn:ipv6.address.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.address.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv6.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway`</span></span><span class="shortdesc"></span>

IPv6 default gateway to statically configure inside an OCI container

<span class="anchor"><a href="#devices-nic_ovn:ipv6.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv6.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to route to the NIC

<span class="anchor"><a href="#devices-nic_ovn:ipv6.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:ipv6.routes.external"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes.external`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to route to the NIC and
publish on uplink network

<span class="anchor"><a href="#devices-nic_ovn:ipv6.routes.external"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes.external</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:limits.egress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.egress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for outgoing traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_ovn:limits.egress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.egress</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:limits.ingress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.ingress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for incoming traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_ovn:limits.ingress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.ingress</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:limits.max"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.max`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for both incoming and outgoing traffic. (same as
setting both limits.ingress and limits.egress / mutually exclusive with
limits.ingress and limits.egress)

<span class="anchor"><a href="#devices-nic_ovn:limits.max"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:limits.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.priority`</span></span><span class="shortdesc"></span>

The priority for outgoing traffic, to be used by the kernel queuing
discipline to prioritize network packets

<span class="anchor"><a href="#devices-nic_ovn:limits.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.priority</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>100</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:mtu" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_ovn:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>MTU of the parent network</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:name" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_ovn:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:nested"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`nested`</span></span><span class="shortdesc"></span>

The parent NIC name to nest this NIC under (see also
<span class="pre">`vlan`</span>)

<span class="anchor"><a href="#devices-nic_ovn:nested" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">nested</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:network"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`network`</span></span><span class="shortdesc"></span>

The managed network to link the device to (required)

<span class="anchor"><a href="#devices-nic_ovn:network" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">network</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.acls"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls`</span></span><span class="shortdesc"></span>

Comma-separated list of network ACLs to apply

<span class="anchor"><a href="#devices-nic_ovn:security.acls"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.acls.default.egress.action"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.egress.action`</span></span><span class="shortdesc"></span>

Action to use for egress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_ovn:security.acls.default.egress.action"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.egress.action</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>reject</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.acls.default.egress.logged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.egress.logged`</span></span><span class="shortdesc"></span>

Whether to log egress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_ovn:security.acls.default.egress.logged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.egress.logged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.acls.default.ingress.action"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.ingress.action`</span></span><span class="shortdesc"></span>

Action to use for ingress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_ovn:security.acls.default.ingress.action"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.ingress.action</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>reject</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.acls.default.ingress.logged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.acls.default.ingress.logged`</span></span><span class="shortdesc"></span>

Whether to log ingress traffic that doesn’t match any ACL rule

<span class="anchor"><a href="#devices-nic_ovn:security.acls.default.ingress.logged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.acls.default.ingress.logged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:security.promiscuous"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`security.promiscuous`</span></span><span class="shortdesc"></span>

Have OVN send unknown network traffic to this network interface
(required for some nesting cases)

<span class="anchor"><a href="#devices-nic_ovn:security.promiscuous"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">security.promiscuous</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ovn:vlan" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to use when nesting (see also
<span class="pre">`nested`</span>)

<span class="anchor"><a href="#devices-nic_ovn:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div class="admonition note">

Note

Note that using <span class="pre">`none`</span> with either
<span class="pre">`ipv4.address`</span> or
<span class="pre">`ipv6.address`</span> needs the other protocol to also
be disabled. There is currently no way for OVN to disable IP allocation
just on IPv4 or IPv6.

</div>

</div>

</div>

<div id="nictype-physical" class="section">

<span id="nic-physical"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`physical`</span><a href="#nictype-physical" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

- You can select this NIC type through the
  <span class="pre">`nictype`</span> option or the
  <span class="pre">`network`</span> option (see
  <a href="../network_physical/#network-physical"
  class="reference internal"><span class="std std-ref">Physical
  network</span></a> for information about the managed
  <span class="pre">`physical`</span> network).

- You can have only one <span class="pre">`physical`</span> NIC for each
  parent device.

</div>

A <span class="pre">`physical`</span> NIC provides straight physical
device pass-through from the host. The targeted device will vanish from
the host and appear in the instance (which means that you can have only
one <span class="pre">`physical`</span> NIC for each targeted device).

<div id="id4" class="section">

#### Device options<a href="#id4" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`physical`</span> have the
following device options:

<div id="devices-nic_physical:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_physical:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_physical:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_physical:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:gvrp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`gvrp`</span></span><span class="shortdesc"></span>

Register VLAN using GARP VLAN Registration Protocol

<span class="anchor"><a href="#devices-nic_physical:gvrp" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">gvrp</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_physical:hwaddr"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_physical:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>MTU of the parent device</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_physical:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:network"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`network`</span></span><span class="shortdesc"></span>

The managed network to link the device to (instead of specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_physical:network"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">network</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the parent host device (required if specifying the
<span class="pre">`nictype`</span> directly)

<span class="anchor"><a href="#devices-nic_physical:parent"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>yes</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#devices-nic_physical:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
<td><span class="ignoreP"></span>
<p>no</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_physical:vlan.tagged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan.tagged`</span></span><span class="shortdesc"></span>

Comma-delimited list of VLAN IDs or VLAN ranges to join for tagged
traffic

<span class="anchor"><a href="#devices-nic_physical:vlan.tagged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan.tagged</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>container</p></td>
</tr>
<tr class="even row-even">
<td><strong>Managed:</strong></td>
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

<div id="nictype-ipvlan" class="section">

<span id="nic-ipvlan"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`ipvlan`</span><a href="#nictype-ipvlan" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

- This NIC type is available only for containers, not for virtual
  machines.

- You can select this NIC type only through the
  <span class="pre">`nictype`</span> option.

- This NIC type does not support hotplugging.

</div>

An <span class="pre">`ipvlan`</span> NIC sets up a new network device
based on an existing one, using the same MAC address but a different IP.

If you are using an <span class="pre">`ipvlan`</span> NIC, communication
between the Incus host and the instances is not possible. Both the host
and the instances can talk to the gateway, but they cannot communicate
directly.

Incus currently supports IPVLAN in L2 and L3S mode. In this mode, the
gateway is automatically set by Incus, but the IP addresses must be
manually specified using the <span class="pre">`ipv4.address`</span>
and/or <span class="pre">`ipv6.address`</span> options before the
container is started.

DNS  
The name servers must be configured inside the container, because they
are not set automatically. To do this, set the following
<span class="pre">`sysctls`</span>:

- When using IPv4 addresses:

  <div class="highlight-default notranslate">

  <div class="highlight">

      net.ipv4.conf.<parent>.forwarding=1

  </div>

  </div>

- When using IPv6 addresses:

  <div class="highlight-default notranslate">

  <div class="highlight">

      net.ipv6.conf.<parent>.forwarding=1
      net.ipv6.conf.<parent>.proxy_ndp=1

  </div>

  </div>

<div id="id5" class="section">

#### Device options<a href="#id5" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`ipvlan`</span> have the following
device options:

<div id="devices-nic_ipvlan:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_ipvlan:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_ipvlan:gvrp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`gvrp`</span></span><span class="shortdesc"></span>

Register VLAN using GARP VLAN Registration Protocol

<span class="anchor"><a href="#devices-nic_ipvlan:gvrp" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">gvrp</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_ipvlan:hwaddr" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:ipv4.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.address`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static addresses to add to the instance (in
l2 mode, these can be specified as CIDR values or singular addresses
using a subnet of /24)

<span class="anchor"><a href="#devices-nic_ipvlan:ipv4.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.address</code></span></td>
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

<div id="devices-nic_ipvlan:ipv4.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway`</span></span><span class="shortdesc"></span>

In <span class="pre">`l3s`</span> mode, whether to add an automatic
default IPv4 gateway (can be <span class="pre">`auto`</span> or
<span class="pre">`none`</span>). In <span class="pre">`l2`</span> mode,
the IPv4 address of the gateway

<span class="anchor"><a href="#devices-nic_ipvlan:ipv4.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.gateway</code></span></td>
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
class="docutils literal notranslate">auto</code></span> (in <span
class="pre"><code class="docutils literal notranslate">l3s</code></span>
mode), <span class="pre"><code
class="docutils literal notranslate">-</code></span> (in <span
class="pre"><code class="docutils literal notranslate">l2</code></span>
mode)</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:ipv4.host_table"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.host_table`</span></span><span class="shortdesc"></span>

The custom policy routing table ID to add IPv4 static routes to (in
addition to the main routing table)

<span class="anchor"><a href="#devices-nic_ipvlan:ipv4.host_table"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.host_table</code></span></td>
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

<div id="devices-nic_ipvlan:ipv6.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.address`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static addresses to add to the instance (in
<span class="pre">`l2`</span> mode, these can be specified as CIDR
values or singular addresses using a subnet of /64)

<span class="anchor"><a href="#devices-nic_ipvlan:ipv6.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.address</code></span></td>
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

<div id="devices-nic_ipvlan:ipv6.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway`</span></span><span class="shortdesc"></span>

In <span class="pre">`l3s`</span> mode, whether to add an automatic
default IPv6 gateway (can be <span class="pre">`auto`</span> or
<span class="pre">`none`</span>). In <span class="pre">`l2`</span> mode,
the IPv6 address of the gateway

<span class="anchor"><a href="#devices-nic_ipvlan:ipv6.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.gateway</code></span></td>
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
class="docutils literal notranslate">auto</code></span> (in <span
class="pre"><code class="docutils literal notranslate">l3s</code></span>
mode), <span class="pre"><code
class="docutils literal notranslate">-</code></span> (in <span
class="pre"><code class="docutils literal notranslate">l2</code></span>
mode)</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:ipv6.host_table"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.host_table`</span></span><span class="shortdesc"></span>

The custom policy routing table ID to add IPv6 static routes to (in
addition to the main routing table)

<span class="anchor"><a href="#devices-nic_ipvlan:ipv6.host_table"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.host_table</code></span></td>
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

<div id="devices-nic_ipvlan:mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mode`</span></span><span class="shortdesc"></span>

The IPVLAN mode (either <span class="pre">`l2`</span> or
<span class="pre">`l3s`</span>)

<span class="anchor"><a href="#devices-nic_ipvlan:mode" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mode</code></span></td>
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
class="docutils literal notranslate">l3s</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_ipvlan:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>MTU of the parent device</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_ipvlan:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_ipvlan:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the host device (required)

<span class="anchor"><a href="#devices-nic_ipvlan:parent" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
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

<div id="devices-nic_ipvlan:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#devices-nic_ipvlan:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
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

</div>

</div>

<div id="nictype-p2p" class="section">

<span id="nic-p2p"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`p2p`</span><a href="#nictype-p2p" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type only through the
<span class="pre">`nictype`</span> option.

</div>

A <span class="pre">`p2p`</span> NIC creates a virtual device pair,
putting one side in the instance and leaving the other side on the host.

<div id="id6" class="section">

#### Device options<a href="#id6" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`p2p`</span> have the following
device options:

<div id="devices-nic_p2p:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_p2p:attached" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_p2p:boot.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`boot.priority`</span></span><span class="shortdesc"></span>

Boot priority for VMs (higher value boots first)

<span class="anchor"><a href="#devices-nic_p2p:boot.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:connected"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`connected`</span></span><span class="shortdesc"></span>

Whether the NIC is connected to the host network

<span class="anchor"><a href="#devices-nic_p2p:connected" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">connected</code></span></td>
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

<div id="devices-nic_p2p:host_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`host_name`</span></span><span class="shortdesc"></span>

The name of the interface on the host

<span class="anchor"><a href="#devices-nic_p2p:host_name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">host_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_p2p:hwaddr" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Override the bus for the device (can be
<span class="pre">`virtio`</span> or <span class="pre">`usb`</span>) (VM
only)

<span class="anchor"><a href="#devices-nic_p2p:io.bus" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
class="docutils literal notranslate">virtio</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:ipv4.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to add on host to NIC

<span class="anchor"><a href="#devices-nic_p2p:ipv4.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes</code></span></td>
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

<div id="devices-nic_p2p:ipv6.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to add on host to NIC

<span class="anchor"><a href="#devices-nic_p2p:ipv6.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes</code></span></td>
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

<div id="devices-nic_p2p:limits.egress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.egress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for outgoing traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_p2p:limits.egress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.egress</code></span></td>
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

<div id="devices-nic_p2p:limits.ingress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.ingress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for incoming traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_p2p:limits.ingress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.ingress</code></span></td>
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

<div id="devices-nic_p2p:limits.max"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.max`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for both incoming and outgoing traffic (same as
setting both limits.ingress and limits.egress)

<span class="anchor"><a href="#devices-nic_p2p:limits.max"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:limits.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.priority`</span></span><span class="shortdesc"></span>

The priority for outgoing traffic, to be used by the kernel queuing
discipline to prioritize network packets

<span class="anchor"><a href="#devices-nic_p2p:limits.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.priority</code></span></td>
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

<div id="devices-nic_p2p:mtu" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_p2p:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:name" class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_p2p:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_p2p:queue.tx.length"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`queue.tx.length`</span></span><span class="shortdesc"></span>

The transmit queue length for the NIC

<span class="anchor"><a href="#devices-nic_p2p:queue.tx.length"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">queue.tx.length</code></span></td>
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

</div>

</div>

<div id="nictype-routed" class="section">

<span id="nic-routed"></span>

### <span class="pre">`nictype`</span>: <span class="pre">`routed`</span><a href="#nictype-routed" class="headerlink"
title="Link to this heading">¶</a>

<div class="admonition note">

Note

You can select this NIC type only through the
<span class="pre">`nictype`</span> option.

</div>

A <span class="pre">`routed`</span> NIC creates a virtual device pair to
connect the host to the instance and sets up static routes and proxy
ARP/NDP entries to allow the instance to join the network of a
designated parent interface. For containers it uses a virtual Ethernet
device pair, and for VMs it uses a TAP device.

This NIC type is similar in operation to
<span class="pre">`ipvlan`</span>, in that it allows an instance to join
an external network without needing to configure a bridge and shares the
host’s MAC address. However, it differs from
<span class="pre">`ipvlan`</span> because it does not need IPVLAN
support in the kernel, and the host and the instance can communicate
with each other.

This NIC type respects <span class="pre">`netfilter`</span> rules on the
host and uses the host’s routing table to route packets, which can be
useful if the host is connected to multiple networks.

IP addresses, gateways and routes  
You must manually specify the IP addresses (using
<span class="pre">`ipv4.address`</span> and/or
<span class="pre">`ipv6.address`</span>) before the instance is started.

For containers, the NIC configures the following link-local gateway IPs
on the host end and sets them as the default gateways in the container’s
NIC interface:

<div class="highlight-none notranslate">

<div class="highlight">

    169.254.0.1
    fe80::1

</div>

</div>

For VMs, the gateways must be configured manually or via a mechanism
like <span class="pre">`cloud-init`</span> (see the
<a href="../../howto/instances_routed_nic_vm/#instances-routed-nic-vm"
class="reference internal"><span class="std std-ref">how to
guide</span></a>).

<div class="admonition note">

Note

If your container image is configured to perform DHCP on the interface,
it will likely remove the automatically added configuration. In this
case, you must configure the IP addresses and gateways manually or via a
mechanism like <span class="pre">`cloud-init`</span>.

</div>

The NIC type configures static routes on the host pointing to the
instance’s <span class="pre">`veth`</span> interface for all of the
instance’s IPs.

Multiple IP addresses  
Each NIC device can have multiple IP addresses added to it.

However, it might be preferable to use multiple
<span class="pre">`routed`</span> NIC interfaces instead. In this case,
set the <span class="pre">`ipv4.gateway`</span> and
<span class="pre">`ipv6.gateway`</span> values to
<span class="pre">`none`</span> on any subsequent interfaces to avoid
default gateway conflicts. Also consider specifying a different
host-side address for these subsequent interfaces using
<span class="pre">`ipv4.host_address`</span> and/or
<span class="pre">`ipv6.host_address`</span>.

Parent interface  
This NIC can operate with and without a
<span class="pre">`parent`</span> network interface set.

With the <span class="pre">`parent`</span> network interface set, proxy
ARP/NDP entries of the instance’s IPs are added to the parent interface,
which allows the instance to join the parent interface’s network at
layer 2.

To enable this, the following network configuration must be applied on
the host via <span class="pre">`sysctl`</span>:

- When using IPv4 addresses:

  <div class="highlight-default notranslate">

  <div class="highlight">

      net.ipv4.conf.<parent>.forwarding=1

  </div>

  </div>

- When using IPv6 addresses:

  <div class="highlight-default notranslate">

  <div class="highlight">

      net.ipv6.conf.all.forwarding=1
      net.ipv6.conf.<parent>.forwarding=1
      net.ipv6.conf.all.proxy_ndp=1
      net.ipv6.conf.<parent>.proxy_ndp=1

  </div>

  </div>

<div id="id7" class="section">

#### Device options<a href="#id7" class="headerlink" title="Link to this heading">¶</a>

NIC devices of type <span class="pre">`routed`</span> have the following
device options:

<div id="devices-nic_routed:attached"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`attached`</span></span><span class="shortdesc"></span>

Whether the NIC is plugged in or not

<span class="anchor"><a href="#devices-nic_routed:attached"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="devices-nic_routed:connected"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`connected`</span></span><span class="shortdesc"></span>

Whether the NIC is connected to the host network

<span class="anchor"><a href="#devices-nic_routed:connected"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">connected</code></span></td>
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

<div id="devices-nic_routed:gvrp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`gvrp`</span></span><span class="shortdesc"></span>

Register VLAN using GARP VLAN Registration Protocol

<span class="anchor"><a href="#devices-nic_routed:gvrp" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">gvrp</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>false</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:host_name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`host_name`</span></span><span class="shortdesc"></span>

The name of the interface on the host

<span class="anchor"><a href="#devices-nic_routed:host_name"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">host_name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`hwaddr`</span></span><span class="shortdesc"></span>

The MAC address of the new interface

<span class="anchor"><a href="#devices-nic_routed:hwaddr" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">hwaddr</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>randomly assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:io.bus"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`io.bus`</span></span><span class="shortdesc"></span>

Override the bus for the device (can be
<span class="pre">`virtio`</span> or <span class="pre">`usb`</span>) (VM
only)

<span class="anchor"><a href="#devices-nic_routed:io.bus" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
class="docutils literal notranslate">virtio</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv4.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.address`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static addresses to add to the instance

<span class="anchor"><a href="#devices-nic_routed:ipv4.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.address</code></span></td>
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

<div id="devices-nic_routed:ipv4.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway`</span></span><span class="shortdesc"></span>

Whether to add an automatic default IPv4 gateway (can be
<span class="pre">`auto`</span> or <span class="pre">`none`</span>)

<span class="anchor"><a href="#devices-nic_routed:ipv4.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv4.host_address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.host_address`</span></span><span class="shortdesc"></span>

The IPv4 address to add to the host-side <span class="pre">`veth`</span>
interface

<span class="anchor"><a href="#devices-nic_routed:ipv4.host_address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.host_address</code></span></td>
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
class="docutils literal notranslate">169.254.0.1</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv4.host_table"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.host_table`</span></span><span class="shortdesc"></span>

Deprecated: Use <span class="pre">`ipv4.host_tables`</span> instead

<span class="anchor"><a href="#devices-nic_routed:ipv4.host_table"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.host_table</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
</tbody>
</table>

</div>

The custom policy routing table ID to add IPv4 static routes to (in
addition to the main routing table)

</div>

</div>

<div id="devices-nic_routed:ipv4.host_tables"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.host_tables`</span></span><span class="shortdesc"></span>

Comma-delimited list of routing tables IDs to add IPv4 static routes to

<span class="anchor"><a href="#devices-nic_routed:ipv4.host_tables"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.host_tables</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>254</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv4.neighbor_probe"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.neighbor_probe`</span></span><span class="shortdesc"></span>

Whether to probe the parent network for IP address availability

<span class="anchor"><a href="#devices-nic_routed:ipv4.neighbor_probe"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.neighbor_probe</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>true</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv4.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv4 static routes to add on host to NIC
(without L2 ARP/NDP proxy)

<span class="anchor"><a href="#devices-nic_routed:ipv4.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes</code></span></td>
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

<div id="devices-nic_routed:ipv6.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.address`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static addresses to add to the instance

<span class="anchor"><a href="#devices-nic_routed:ipv6.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.address</code></span></td>
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

<div id="devices-nic_routed:ipv6.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway`</span></span><span class="shortdesc"></span>

Whether to add an automatic default IPv6 gateway (can be
<span class="pre">`auto`</span> or <span class="pre">`none`</span>)

<span class="anchor"><a href="#devices-nic_routed:ipv6.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.gateway</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>auto</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv6.host_address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.host_address`</span></span><span class="shortdesc"></span>

The IPv6 address to add to the host-side <span class="pre">`veth`</span>
interface

<span class="anchor"><a href="#devices-nic_routed:ipv6.host_address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.host_address</code></span></td>
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
class="docutils literal notranslate">fe80::1</code></span></p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv6.host_table"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.host_table`</span></span><span class="shortdesc"></span>

Deprecated: Use <span class="pre">`ipv6.host_tables`</span> instead

<span class="anchor"><a href="#devices-nic_routed:ipv6.host_table"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.host_table</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
</tbody>
</table>

</div>

The custom policy routing table ID to add IPv6 static routes to (in
addition to the main routing table)

</div>

</div>

<div id="devices-nic_routed:ipv6.host_tables"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.host_tables`</span></span><span class="shortdesc"></span>

Comma-delimited list of routing tables IDs to add IPv6 static routes to

<span class="anchor"><a href="#devices-nic_routed:ipv6.host_tables"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.host_tables</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>254</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv6.neighbor_probe"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.neighbor_probe`</span></span><span class="shortdesc"></span>

Whether to probe the parent network for IP address availability

<span class="anchor"><a href="#devices-nic_routed:ipv6.neighbor_probe"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.neighbor_probe</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>true</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:ipv6.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes`</span></span><span class="shortdesc"></span>

Comma-delimited list of IPv6 static routes to add on host to NIC
(without L2 ARP/NDP proxy)

<span class="anchor"><a href="#devices-nic_routed:ipv6.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes</code></span></td>
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

<div id="devices-nic_routed:limits.egress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.egress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for outgoing traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_routed:limits.egress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.egress</code></span></td>
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

<div id="devices-nic_routed:limits.ingress"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.ingress`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for incoming traffic (various suffixes supported, see
<a href="../instance_units/#instances-limit-units"
class="reference internal"><span class="std std-ref">Units for storage,
memory and network limits</span></a>)

<span class="anchor"><a href="#devices-nic_routed:limits.ingress"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.ingress</code></span></td>
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

<div id="devices-nic_routed:limits.max"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.max`</span></span><span class="shortdesc"></span>

I/O limit in bit/s for both incoming and outgoing traffic (same as
setting both limits.ingress and limits.egress)

<span class="anchor"><a href="#devices-nic_routed:limits.max"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:limits.priority"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`limits.priority`</span></span><span class="shortdesc"></span>

The priority for outgoing traffic, to be used by the kernel queuing
discipline to prioritize network packets

<span class="anchor"><a href="#devices-nic_routed:limits.priority"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">limits.priority</code></span></td>
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

<div id="devices-nic_routed:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The Maximum Transmit Unit (MTU) of the new interface

<span class="anchor"><a href="#devices-nic_routed:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">mtu</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>parent MTU</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:name"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`name`</span></span><span class="shortdesc"></span>

The name of the interface inside the instance

<span class="anchor"><a href="#devices-nic_routed:name" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">name</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>kernel assigned</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="devices-nic_routed:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

The name of the parent host device to join the instance to

<span class="anchor"><a href="#devices-nic_routed:parent" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">parent</code></span></td>
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

<div id="devices-nic_routed:queue.tx.length"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`queue.tx.length`</span></span><span class="shortdesc"></span>

The transmit queue length for the NIC

<span class="anchor"><a href="#devices-nic_routed:queue.tx.length"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">queue.tx.length</code></span></td>
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

<div id="devices-nic_routed:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#devices-nic_routed:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vlan</code></span></td>
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

<div id="devices-nic_routed:vrf"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vrf`</span></span><span class="shortdesc"></span>

The VRF on the host in which the host-side interface and routes are
created

<span class="anchor"><a href="#devices-nic_routed:vrf" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">vrf</code></span></td>
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

</div>

</div>

</div>

<div id="bridged-macvlan-or-ipvlan-for-connection-to-physical-network"
class="section">

## <span class="pre">`bridged`</span>, <span class="pre">`macvlan`</span> or <span class="pre">`ipvlan`</span> for connection to physical network<a href="#bridged-macvlan-or-ipvlan-for-connection-to-physical-network"
class="headerlink" title="Link to this heading">¶</a>

The <span class="pre">`bridged`</span>,
<span class="pre">`macvlan`</span> and <span class="pre">`ipvlan`</span>
interface types can be used to connect to an existing physical network.

<span class="pre">`macvlan`</span> effectively lets you fork your
physical NIC, getting a second interface that is then used by the
instance. This method saves you from creating a bridge device and
virtual Ethernet device pairs and usually offers better performance than
a bridge.

The downside to this method is that <span class="pre">`macvlan`</span>
devices, while able to communicate between themselves and to the
outside, cannot talk to their parent device. This means that you can’t
use <span class="pre">`macvlan`</span> if you ever need your instances
to talk to the host itself.

In such case, a <span class="pre">`bridge`</span> device is preferable.
A bridge also lets you use MAC filtering and I/O limits, which cannot be
applied to a <span class="pre">`macvlan`</span> device.

<span class="pre">`ipvlan`</span> is similar to
<span class="pre">`macvlan`</span>, with the difference being that the
forked device has IPs statically assigned to it and inherits the
parent’s MAC address on the network.

</div>

</div>

</div>

<div class="related-pages">

<a href="../devices_disk/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Type: <span class="pre">`disk`</span>

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../devices_none/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Type: <span class="pre">`none`</span>

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">