# Vendor-pair concept notes

These notes are planning aids based on generally documented platform differences. They are not a conversion guide and they do not describe any particular product's translator. Always confirm behavior against current vendor documentation for the OS versions in scope.

Use only the section that matches the user's pair, then add any extra gaps the inventory turned up.

## Shared themes (every pair)

- Zone vs interface vs security-level enforcement is the first mapping exercise. Do not assume a one-to-one name match.
- NAT models differ (interface vs object/manual vs central NAT). Inventory every NAT rule, including identity NAT and exemptions.
- VPN may be policy-based (proxy IDs) on one side and route-based (tunnel interface plus routes) on the other. List every proxy ID and every tunnel route.
- Application-aware policy (App-ID, FortiGate applications) does not map 1:1 to port-based ACLs. Call out rules that depend on application or user identity.
- HA is vendor-specific. Collect failover links, monitored interfaces, and whether sessions are expected to survive a failover. Do not paste failover keys into tickets or checklists.
- Management-plane access can be lost at cutover if HTTPS/SSH allow-lists, source zones, or dedicated management interfaces are wrong. Require an out-of-band path.

## Cisco ASA to PAN-OS

- ASA security-levels and same-security permit/deny behavior have no direct PAN-OS equivalent. Map each interface to a zone and decide intra-zone policy explicitly.
- `access-group` is interface-and-direction based; PAN-OS security policy is zone-based. Inventory every interface assignment.
- Network and service objects generally map, but nested groups, `object-group protocol`, and name/FQDN objects need a line-item review.
- ASA twice-NAT / manual NAT / Auto-NAT versus PAN-OS ordered NAT policy. List NAT order dependencies.
- Identity firewall, CSA/SGT, or user-group rules need a PAN-OS User-ID source; that is a project of its own.
- For FMC-managed devices use the separate FMC section below; ASA ACL and security-level semantics must not be assumed for FMC policy.
- Any VPN that relies on IKEv1, remote-access AnyConnect profiles, or unused crypto maps should be called out as a manual design review.

## Cisco ASA to FortiGate

- Security-level plus implicit inter-interface rules vs FortiGate VDOM, interface, and zone policy. Decide whether the target uses zones, and how implicit deny is expressed.
- FortiGate policy is commonly a single table with source/destination interface or zone. Rebuild the ASA direction model explicitly.
- NAT on FortiGate often uses VIP (destination) and IP pool / central NAT (source). An ASA twice-NAT rule may become more than one FortiGate object.
- VIP, IP pool, and `ippool` naming should be decided before conversion day so operators can recognize production objects.
- FortiGate features such as UTM profiles, SSL inspection, and SD-WAN are not implied by an ASA ACL and should be listed as optional target work, not assumed to appear automatically.
- VDOM scope and inter-VDOM links need an explicit decision if the target is not a single VDOM.


## PAN-OS to FortiGate

- App-ID and service ports do not line up 1:1 with FortiGate application and service objects. Rules that rely on application default ports need a human choice: app, port, or both.
- Security profiles (AV, vulnerability, URL, wildfire, and so on) vs FortiGate UTM profiles are a design mapping, not a rename.
- User-ID vs FortiGate FSSO/LDAP/SAML identity sources should be planned separately from the dataplane policy.
- NAT rule order and destination NAT (VIP) need an explicit walkthrough; do not assume identical precedence.
- Virtual routers vs FortiGate VDOMs or VRFs: confirm routing-instance scope.
- IKE gateways, tunnel interfaces, and proxy IDs vs FortiGate phase1/phase2 must be inventoried together with static or BGP routes over the tunnel.

## FortiGate to PAN-OS

- Interface- or zone-based FortiGate policy into PAN-OS zones, including any `any` interface rules that hide the real path.
- VIP and central NAT into PAN-OS ordered NAT rules; separately preserve Panorama pre/post inheritance when present.
- FortiGate applications, internet-service databases, and FQDN addresses may need PAN-OS application, EDL, or FQDN objects, each with a different operational model.
- UTM profile bundles vs PAN-OS security profile groups.
- SD-WAN or session-based routing on FortiGate will not appear as a simple PAN-OS security rule; list it as a manual design item.
- VDOM to vsys mapping, if both sides are multi-tenant.


## Check Point R80+ to PAN-OS

- Check Point policy is organized in layers and a rulebase; PAN-OS uses zone-based security policy. Inventory every layer in scope, including inline and ordered layers, and decide how they map to PAN-OS rulebases or device groups.
- Objects, groups, and services often map, but dynamic objects and domain objects need a line-item review.
- NAT and VPN (community, star, or route-based) should be inventoried separately from the access rulebase.
- Identity Awareness versus PAN-OS User-ID is a directory-integration project of its own.
- Confirm the authoritative export is a SmartConsole or management-database export, not only a gateway file.

## When the manager is in the path

- FMC, Panorama, FortiManager, and Check Point SmartConsole can be the source of truth rather than the device. The checklist should say which export was used.
- Device-group, template, or policy-package structure on the target is a design decision. Do not assume a flat device-local config is the end state.
- Shared objects and inherited policy need an ownership rule (what stays shared, what becomes local).

## What to leave as a manual follow-up

Always call these out rather than burying them in a "converted" assumption:

- TLS decryption / SSL inspection policy.
- SD-WAN, PBF, or policy-based routing exceptions.
- Identity, SGT, or dynamic-user rules without a confirmed directory integration.
- IPv6 policy if only IPv4 was inventoried, and the reverse.
- QoS, traffic shaping, and multicast.
- Dynamic DNS, external block lists, and threat-feed objects.
- Hidden or implicit rules (ASA default, FortiGate implicit, default-deny at the end).

## FMC-managed FTD to PAN-OS or FortiGate

Use the selected-device NetConverter collector snapshot, not FTD CLI or a raw manager export. Inventory domain/device/policy UUIDs, ordered access rules, default action, NAT and inherited policy. Monitor actions log without ending rule evaluation; do not translate them into permits. Security intelligence, intrusion/file inspection, identity and encrypted-traffic policies need explicit feature-gap review. Do not import ASA security-level assumptions.

## Authoritative references

- [Cisco access control policy](https://secure.cisco.com/secure-firewall/docs/access-control-policy): rule handling, Monitor actions and defaults.
- [PAN-OS NAT policy](https://docs.paloaltonetworks.com/ngfw/networking/nat/nat-policy-rules): first matching NAT rule and packet matching criteria.
- [Check Point ordered and inline layers](https://sc1.checkpoint.com/documents/R81.20/WebAdminGuides/EN/CP_R81.20_SecurityManagement_AdminGuide/Content/Topics-SECMG/Ordered-Layers-and-Inline-Layers.htm): preserve layers and cleanup behavior.
- [Check Point policy management](https://sc1.checkpoint.com/documents/R81/WebAdminGuides/EN/CP_R81_SecurityManagement_AdminGuide/Topics-SECMG/Managing-Policies.htm): preserve selected package and installation scope.
- [FortiGate administration guide](https://docs.fortinet.com/document/fortigate/7.6.4/administration-guide): verify the NAT, VDOM, policy and HA behavior for the installed release.

These references support planning questions, not a claim that NetConverter implements every feature. Confirm software versions before applying a vendor fact.
