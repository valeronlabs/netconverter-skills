# Collection scope and primary references

Use vendor documentation for the installed software version. References checked
2026-11-18; a vendor export command alone does not establish NetConverter intake
compatibility. The [NetConverter collector repository](https://github.com/valeronlabs/netconverter-collectors)
owns collector commands, release versions and supported bundle formats.

| Platform | Preserve and disclose |
|---|---|
| ASA | Context identity, native configuration, interfaces/routes, ACL/NAT order, VPN and failover evidence; standalone FTD CLI is excluded |
| FMC | Read-only collector's selected device/domain/policy UUID snapshot ZIP; rule ordering, object references, NAT and capture gaps; not arbitrary REST JSON |
| PAN-OS | SET versus XML identity; VSYS and configuration version; operational evidence separately |
| Panorama | Selected serial/VSYS, device-group ancestry and Shared objects/pre/post policy, templates/stack, local overrides and missing device captures |
| FortiGate/FortiManager | Serial/VDOM and applicable manager ADOM/policy-package scope; native FortiGate backup plus supporting capture gaps |
| Check Point | Gateway/cluster, policy package, layers and install targets, management objects plus Gaia/member captures; a management export alone lacks device networking evidence |
| IOS-XE | Explicit router/switch role; VRFs, interfaces/subinterfaces, routes/protocols/ACLs; switches additionally VLANs/trunks/native VLAN, LAG/LACP, STP, SVI/routed ports |
| Junos | Explicit router/switch role and SET/hierarchical format; inheritance/groups, logical systems/routing instances, filters and protocol context |
| Aruba CX / ArubaOS | Distinguish operating systems; VLAN membership, tagging/native behavior, LAG/STP, SVI/VRFs/routes; do not infer full BGP coverage |

Primary references:

- [Cisco ASA command reference](https://www.cisco.com/c/en/us/td/docs/security/asa/asa-cli-reference/A-H/asa-command-ref-A-H.html)
  for ASA command semantics; use the relevant release's show-command reference for capture.
- [Palo Alto configuration exports](https://docs.paloaltonetworks.com/panorama/administration/administer-panorama/manage-panorama-and-firewall-configuration-backups/save-and-export-panorama-and-firewall-configurations).
  Distinguish candidate snapshots from committed versions and manager/device exports.
- [Panorama selective-export limitation](https://knowledgebase.paloaltonetworks.com/KCSArticleDetail?id=kA14u000000kF4BCAU&lang=en_US).
  A selected device-group/template export can omit hierarchy; review inheritance explicitly.
- [Fortinet configuration backups](https://docs.fortinet.com/document/fortigate/7.6.4/administration-guide/702257/configuration-backups-and-reset).
  Record whether a backup masks credentials and its scope; masking is not a complete privacy guarantee.
- [Junos show configuration](https://networkingdocs.hpe.com/documentation/us/en/software/junos/cli-reference/topics/ref/command/show-configuration.html).
  Permissions can hide portions as ACCESS-DENIED; operational-mode output reflects committed configuration.
- [Aruba CX show running-config](https://arubanetworking.hpe.com/techdocs/AOS-CX/10.13/HTML/fundamentals_6200/Content/SysHW_cmds/sho-run-con-10.htm).
  Collect the whole relevant scope, not only the current configuration context.

Missing vendor-version evidence stays Missing; do not substitute commands from a
different platform. Collector documentation and the support matrix govern which
handoff NetConverter accepts.


- [Cisco FMC access control policy](https://secure.cisco.com/secure-firewall/docs/access-control-policy): preserve policy order, Monitor actions and default action.
- [Check Point policy management](https://sc1.checkpoint.com/documents/R81/WebAdminGuides/EN/CP_R81_SecurityManagement_AdminGuide/Topics-SECMG/Managing-Policies.htm): identify selected packages and gateways.
