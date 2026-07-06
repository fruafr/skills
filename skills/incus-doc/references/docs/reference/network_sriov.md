# SR-IOV network<a href="#sr-iov-network" class="headerlink"
title="Link to this heading">¶</a>

SR-IOV is a hardware standard that allows a single network card port to
appear as several virtual network interfaces in a virtualized
environment.

The <span class="pre">`sriov`</span> network type allows to specify
presets to use when connecting instances to a parent interface. In this
case, the instance NICs can simply set the
<span class="pre">`network`</span> option to the network they connect to
without knowing any of the underlying configuration details.

<div id="configuration-options" class="section">

<span id="network-sriov-options"></span>

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration key namespaces are currently supported for
the <span class="pre">`sriov`</span> network type:

- <span class="pre">`user`</span> (free-form key/value for user
  metadata)

<div class="admonition note">

Note

Incus uses the
<a href="https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing"
class="reference external">CIDR notation</a> where network subnet
information is required, for example,
<span class="pre">`192.0.2.0/24`</span> or
<span class="pre">`2001:db8::/32`</span>. This does not apply to cases
where a single address is required, for example, local/remote addresses
of tunnels, NAT addresses or specific addresses to apply to an instance.

</div>

The following configuration options are available for the
<span class="pre">`sriov`</span> network type:

<div id="network_sriov-common:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The MTU of the new interface

<span class="anchor"><a href="#network_sriov-common:mtu" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="network_sriov-common:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

Parent interface to create <span class="pre">`sriov`</span> NICs on

<span class="anchor"><a href="#network_sriov-common:parent"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Condition:</strong></td>
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

<div id="network_sriov-common:user.*"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`user.*`</span></span><span class="shortdesc"></span>

User-provided free-form key/value pairs

<span class="anchor"><a href="#network_sriov-common:user.*"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="network_sriov-common:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#network_sriov-common:vlan" class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div class="related-pages">

<a href="../network_physical/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

Physical network

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../network_macvlan/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

Macvlan network

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">