# Physical network<a href="#physical-network" class="headerlink"
title="Link to this heading">¶</a>

The <span class="pre">`physical`</span> network type connects to an
existing physical network, which can be a network interface or a bridge,
and serves as an uplink network for OVN.

This network type allows to specify presets to use when connecting OVN
networks to a parent interface or to allow an instance to use a physical
interface as a NIC. In this case, the instance NICs can simply set the
<span class="pre">`network`</span>option to the network they connect to
without knowing any of the underlying configuration details.

<div id="configuration-options" class="section">

<span id="network-physical-options"></span>

## Configuration options<a href="#configuration-options" class="headerlink"
title="Link to this heading">¶</a>

The following configuration key namespaces are currently supported for
the <span class="pre">`physical`</span> network type:

- <span class="pre">`bgp`</span> (BGP peer configuration)

- <span class="pre">`dns`</span> (DNS server and resolution
  configuration)

- <span class="pre">`ipv4`</span> (L3 IPv4 configuration)

- <span class="pre">`ipv6`</span> (L3 IPv6 configuration)

- <span class="pre">`ovn`</span> (OVN configuration)

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
<span class="pre">`physical`</span> network type:

</div>

<div id="bgp-options" class="section">

## BGP options<a href="#bgp-options" class="headerlink"
title="Link to this heading">¶</a>

These options configure BGP peering for OVN downstream networks:

<div id="network_physical-bgp:bgp.peers.NAME.address"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`bgp.peers.NAME.address`</span></span><span class="shortdesc"></span>

Peer address (IPv4 or IPv6) for use by <span class="pre">`ovn`</span>
downstream networks

<span class="anchor"><a href="#network_physical-bgp:bgp.peers.NAME.address"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">bgp.peers.NAME.address</code></span></td>
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
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>BGP server</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-bgp:bgp.peers.NAME.asn"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`bgp.peers.NAME.asn`</span></span><span class="shortdesc"></span>

Peer AS number for use by <span class="pre">`ovn`</span> downstream
networks

<span class="anchor"><a href="#network_physical-bgp:bgp.peers.NAME.asn"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">bgp.peers.NAME.asn</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>integer</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<ul>
<li></li>
</ul></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>BGP server</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-bgp:bgp.peers.NAME.holdtime"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`bgp.peers.NAME.holdtime`</span></span><span class="shortdesc"></span>

Peer session hold time (in seconds; optional)

<span class="anchor"><a href="#network_physical-bgp:bgp.peers.NAME.holdtime"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">bgp.peers.NAME.holdtime</code></span></td>
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
class="docutils literal notranslate">180</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>BGP server</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-bgp:bgp.peers.NAME.password"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`bgp.peers.NAME.password`</span></span><span class="shortdesc"></span>

Peer session password (optional) for use by
<span class="pre">`ovn`</span> downstream networks

<span class="anchor"><a href="#network_physical-bgp:bgp.peers.NAME.password"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">bgp.peers.NAME.password</code></span></td>
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
<li><p>(no password)</p></li>
</ul></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>BGP server</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="dns-options" class="section">

## DNS options<a href="#dns-options" class="headerlink"
title="Link to this heading">¶</a>

These keys control the DNS servers and search domains used by the
physical network:

<div id="network_physical-dns:dns.nameservers"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`dns.nameservers`</span></span><span class="shortdesc"></span>

List of DNS server IPs on <span class="pre">`physical`</span> network

<span class="anchor"><a href="#network_physical-dns:dns.nameservers"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">dns.nameservers</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>string</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>standard mode</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="ipv4-options" class="section">

## IPV4 options<a href="#ipv4-options" class="headerlink"
title="Link to this heading">¶</a>

These options define the IPv4 configuration for the physical network:

<div id="network_physical-ipv4:ipv4.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway`</span></span><span class="shortdesc"></span>

IPv4 address for the gateway and network (CIDR)

<span class="anchor"><a href="#network_physical-ipv4:ipv4.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>standard mode</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-ipv4:ipv4.gateway.hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.gateway.hwaddr`</span></span><span class="shortdesc"></span>

MAC address of the gateway (to avoid discovery)

<span class="anchor"><a href="#network_physical-ipv4:ipv4.gateway.hwaddr"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.gateway.hwaddr</code></span></td>
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

<div id="network_physical-ipv4:ipv4.ovn.ranges"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.ovn.ranges`</span></span><span class="shortdesc"></span>

Comma-separated list of IPv4 ranges to use for child OVN network routers
(FIRST-LAST format)

<span class="anchor"><a href="#network_physical-ipv4:ipv4.ovn.ranges"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.ovn.ranges</code></span></td>
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

<div id="network_physical-ipv4:ipv4.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes`</span></span><span class="shortdesc"></span>

Comma-separated list of additional IPv4 CIDR subnets that can be used
with child OVN networks <span class="pre">`ipv4.routes.external`</span>
setting

<span class="anchor"><a href="#network_physical-ipv4:ipv4.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>IPv4 address</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-ipv4:ipv4.routes.anycast"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv4.routes.anycast`</span></span><span class="shortdesc"></span>

Allow the overlapping routes to be used on multiple networks/NIC at the
same time

<span class="anchor"><a href="#network_physical-ipv4:ipv4.routes.anycast"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv4.routes.anycast</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>‘false’</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>IPv4 address</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="ipv6-options" class="section">

## IPV6 options<a href="#ipv6-options" class="headerlink"
title="Link to this heading">¶</a>

These options define the IPv6 configuration for the physical network:

<div id="network_physical-ipv6:ipv6.gateway"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway`</span></span><span class="shortdesc"></span>

IPv6 address for the gateway and network (CIDR)

<span class="anchor"><a href="#network_physical-ipv6:ipv6.gateway"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>standard mode</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-ipv6:ipv6.gateway.hwaddr"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.gateway.hwaddr`</span></span><span class="shortdesc"></span>

MAC address of the gateway (to avoid discovery)

<span class="anchor"><a href="#network_physical-ipv6:ipv6.gateway.hwaddr"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.gateway.hwaddr</code></span></td>
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

<div id="network_physical-ipv6:ipv6.ovn.ranges"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.ovn.ranges`</span></span><span class="shortdesc"></span>

Comma-separated list of IPv6 ranges to use for child OVN network routers
(FIRST-LAST format)

<span class="anchor"><a href="#network_physical-ipv6:ipv6.ovn.ranges"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.ovn.ranges</code></span></td>
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

<div id="network_physical-ipv6:ipv6.routes"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes`</span></span><span class="shortdesc"></span>

Comma-separated list of additional IPv6 CIDR subnets that can be used
with child OVN networks <span class="pre">`ipv6.routes.external`</span>
setting

<span class="anchor"><a href="#network_physical-ipv6:ipv6.routes"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>IPv6 address</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

<div id="network_physical-ipv6:ipv6.routes.anycast"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ipv6.routes.anycast`</span></span><span class="shortdesc"></span>

Allow the overlapping routes to be used on multiple networks/NIC at the
same time

<span class="anchor"><a href="#network_physical-ipv6:ipv6.routes.anycast"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ipv6.routes.anycast</code></span></td>
</tr>
<tr class="even row-even">
<td><strong>Type:</strong></td>
<td><span class="ignoreP"></span>
<p>bool</p></td>
</tr>
<tr class="odd row-odd">
<td><strong>Default:</strong></td>
<td><span class="ignoreP"></span>
<p>‘false’</p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>IPv6 address</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="ovn-options" class="section">

## OVN options<a href="#ovn-options" class="headerlink"
title="Link to this heading">¶</a>

These options apply when using a physical network as an OVN uplink:

<div id="network_physical-ovn:ovn.ingress_mode"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`ovn.ingress_mode`</span></span><span class="shortdesc"></span>

Sets the method how OVN NIC external IPs will be advertised on uplink
network: <span class="pre">`l2proxy`</span> (proxy ARP/NDP) or
<span class="pre">`routed`</span>

<span class="anchor"><a href="#network_physical-ovn:ovn.ingress_mode"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

<table class="fields docutils align-default">
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd row-odd">
<td><strong>Key:</strong></td>
<td><span class="pre"><code
class="docutils literal notranslate">ovn.ingress_mode</code></span></td>
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
class="docutils literal notranslate">l2proxy</code></span></p></td>
</tr>
<tr class="even row-even">
<td><strong>Condition:</strong></td>
<td><span class="ignoreP"></span>
<p>standard mode</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="common-options" class="section">

## Common options<a href="#common-options" class="headerlink"
title="Link to this heading">¶</a>

These apply to all physical networks regardless of other features:

<div id="network_physical-common:gvrp"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`gvrp`</span></span><span class="shortdesc"></span>

Register VLAN using GARP VLAN Registration Protocol

<span class="anchor"><a href="#network_physical-common:gvrp"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<p>‘false’</p></td>
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

<div id="network_physical-common:mtu"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`mtu`</span></span><span class="shortdesc"></span>

The MTU of the new interface

<span class="anchor"><a href="#network_physical-common:mtu"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="network_physical-common:parent"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`parent`</span></span><span class="shortdesc"></span>

Existing interface to use for network

<span class="anchor"><a href="#network_physical-common:parent"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="network_physical-common:vlan"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan`</span></span><span class="shortdesc"></span>

The VLAN ID to attach to

<span class="anchor"><a href="#network_physical-common:vlan"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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

<div id="network_physical-common:vlan.tagged"
class="configoption docutils container">

<div class="basicinfo docutils container">

<span class="key"><span class="pre">`vlan.tagged`</span></span><span class="shortdesc"></span>

Comma-delimited list of VLAN IDs or VLAN ranges to join for tagged
traffic

<span class="anchor"><a href="#network_physical-common:vlan.tagged"
class="reference external"><em><img
src="data:image/svg+xml;base64,PHN2Zz48dXNlIGhyZWY9IiNzdmctYXJyb3ctcmlnaHQiIC8+PC9zdmc+" /></em></a></span>

</div>

<div class="details docutils container">

<div class="table-wrapper fields docutils container">

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
<p>Parent must be an existing bridge</p></td>
</tr>
</tbody>
</table>

</div>

</div>

</div>

</div>

<div id="supported-features" class="section">

<span id="network-physical-features"></span>

## Supported features<a href="#supported-features" class="headerlink"
title="Link to this heading">¶</a>

The following features are supported for the
<span class="pre">`physical`</span> network type:

- <a href="../../howto/network_bgp/#network-bgp"
  class="reference internal"><span class="std std-ref">How to configure
  Incus as a BGP server</span></a>

</div>

</div>

</div>

<div class="related-pages">

<a href="../../howto/network_increase_bandwidth/" class="next-page"></a>

<div class="page-info">

<div class="context">

Next

</div>

<div class="title">

How to increase the network bandwidth

</div>

</div>

<img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" />
<a href="../network_sriov/" class="prev-page"><img
src="data:image/svg+xml;base64,PHN2ZyBjbGFzcz0iZnVyby1yZWxhdGVkLWljb24iPjx1c2UgaHJlZj0iI3N2Zy1hcnJvdy1yaWdodCIgLz48L3N2Zz4="
class="furo-related-icon" /></a>

<div class="page-info">

<div class="context">

Previous

</div>

<div class="title">

SR-IOV network

</div>

</div>

</div>

<div class="bottom-of-page">

<div class="left-details">

<div class="copyright">