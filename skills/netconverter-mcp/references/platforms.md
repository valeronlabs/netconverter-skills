# Platform, role and format routing

Read the current [published support contract](https://netconverter.ai/docs/supported-vendors.html)
and the credential's live capabilities. Preserve disagreements as findings.
Record the device role, known model/software version, source format and selected
scope separately. A format token or recognized vendor does not prove a hardware
model, firmware release, supported feature set or effective inherited policy.

| Family | Source identity | Target identity / caveat |
|---|---|---|
| Cisco ASA firewall | `cisco_asa`, native ASA configuration | Source only; FTD CLI is not ASA |
| Cisco FMC firewall | `cisco_fmc`, selected-device collector snapshot ZIP | FMC JSON target is a different format; raw FMC export is not the collector snapshot |
| FortiGate firewall | `fortinet`, scoped FortiOS backup | Preserve VDOM; FortiManager is a collection/manager context |
| Palo Alto firewall | `palo_alto_set` or `palo_alto_xml` | Test SET and XML independently |
| Panorama firewall | `palo_alto_panorama`, selected scope | Preserve serial/VSYS, device group, inherited Shared policy/objects and templates |
| Check Point firewall | `check_point`, selected gateway/cluster handoff ZIP | Source only; policy packages/layers/install targets and Gaia captures matter |
| IOS-XE router | `cisco_ios_xe_router` | Router only |
| Junos router | `juniper_junos_router` (hierarchical) or `juniper_set_router` | Router only; preserve routing instances |
| IOS-XE switch | `cisco_ios_xe_switch` | Switch only |
| Junos switch | `juniper_junos_switch` (hierarchical) or `juniper_set_switch` | Switch only; SRX security configuration is excluded |
| Aruba CX switch | `aruba_cx_switch` | Switch only; BGP support is partial |
| ArubaOS switch | `aruba_os_switch` | Source only; do not confuse with CX |

The published matrix distinguishes direct Cisco–Junos router support from switch
paths through Aruba CX. Live capabilities can expose additional direct switch
paths and format-to-format paths. Do not hide these disagreements or claim that
an exposed path has been qualified.

ASA, Check Point and ArubaOS are not targets. Cross-device-type conversions,
standalone FTD, Juniper SRX, Arista EOS and Cisco NX-OS are excluded. SCM is not
a self-service target; refer the request to NetConverter support. Offer an
applicable supported path only when it preserves the user's intended device role,
or explain the manual alternative.

EIGRP has no automatic equivalent; an exposed EIGRP-to-OSPF operation requires
explicit choices and output review. Redundancy mechanisms, VPNs, security profiles,
manager inheritance and routing policy do not become equivalent merely because
the syntax converts.

Collection format support and workflow transport support are separate. The
full-API file-ID bridge can offer selected-device Check Point handoff and FMC
snapshot ZIP conversion or prescan when the connected client/server supports
that exact vendor, manifest and job type. Preserve the selected gateway/cluster
or FMC domain/device/policy scope and required capture evidence. Do not extend
this to arbitrary manager ZIPs, ZIP validation/optimization, or every guided
operation. Inspect the completed prescan's catalog, applicability and approval
contract before proceeding; transport support is not client qualification.
A direct ZIP conversion is not a guided pass. For native manager input, verify
that the selected scope can actually be expressed by this job type's schema;
never silently omit a requested device-group selector or widen to all.

