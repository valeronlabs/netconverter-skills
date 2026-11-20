# File transfer without bulk model content

These unreleased candidate behaviors require the connected tool's live schema
and supported client/server capabilities. A schema fixture or local test is not
proof of a deployed service or accepted customer package. The standard full-API
bridge, Quick pilot and restricted guided pilot have different contracts.

## Choose the source without opening its contents

With a supported local MCP connection, `list_workspace_files(folder=".")` lists
one bounded, nonrecursive page within Terminal's configured workspace. Enter an
explicit returned relative directory to inspect that directory; keep its opaque
cursor with the same folder. Directory changes or cursor expiry require a fresh
listing. The private candidate supports POSIX listing only and requires the
human-selected `full` exposure level because names can contain sensitive data.
The assistant must not enable that level. Hidden client state, unsafe links and
names that cannot be returned safely are excluded; disclose exclusions rather
than claiming an exhaustive inventory. On an unsupported client, a known
workspace path remains usable through the existing submission contract.

The hosted `list_source_files(limit, cursor)` returns uploaded records visible to
the account. Names may be clipped or masked and duplicates can exist; select a
stable `file_id`, not a guessed server path. `start_prescan` rechecks ownership,
input support and limits. Neither listing proves that a file is a supported
configuration. Keep local discovery and server discovery distinct; connecting the
hosted service alone does not give it access to a customer's Mac folder.

## Upload an authorized workspace file

Use `submit_file` with the authorized path and migration settings. The standard
full-API bridge reads authenticated upload policy: `max_upload_bytes` governs
file allowance and `file_id_submission=true` enables file upload followed by
file-ID job submission. Its local ceiling is 100 MiB; the effective allowance
may be smaller. `max_inline_config_bytes` governs the older JSON transport.
Let the client enforce negotiated limits; never split or paste a configuration
into tool calls to bypass them. Only a policy endpoint's older-server 404 permits
the client's legacy 5 MiB inline fallback. Other policy failures stop submission.
Quick uses its own smaller allowance. Do not select it to evade a refusal.

Native input must be nonempty UTF-8 text without NUL bytes. Opaque ZIP transport
requires file-ID support. Use `job_type="convert"` or `job_type="prescan"` only
where the connected client/server supports that job type for the selected-device
handoff: Check Point gateway/cluster or FMC device snapshot, with its required
manifest, capture evidence and manager scope. An older bridge or server may
support conversion only; respect that refusal and explain the upgrade/workflow
gap. A random ZIP, raw manager export, or transport test does not establish a
supported migration. ZIPs remain refused for validate, optimize, Quick and legacy
inline upload. After a supported ZIP prescan completes, discover that path's
operations and review contract; unsupported or missing guided steps remain gaps.
A direct conversion cannot stand in for a guided pass. Keep collection and
selected-device/manager scope intact.

The model receives job identity, status, source SHA-256 and byte count, not file
contents. On an ambiguous standard-bridge submission, retry only the same path,
settings, account and server through its saved receipt. It reuses the original
upload and request key; changed work must not replace an unresolved request.
Pending retries have a bounded window (23 hours in this candidate). An expired
or invalid receipt requires reconciliation, not an automatically new key. An
identical completed request may return its original job with `replayed: true`.
Use an advertised rerun workflow when the user actually wants a new run.

Older clients without that receipt behavior need their supported status/resume
path. A filename or vendor pair is insufficient duplicate evidence. Preserve an
unknown outcome until reconciled; neither repeated confirmation nor re-upload
proves the earlier job did not queue.

## Explicit decision YAML or source NCIR export

Check that `download` advertises `export_kind` with the requested value. These
exports need the standard full-API bridge and an authenticated server export
route; they do not change ordinary artifact downloads or the pilot contracts.
Only on explicit request:

| Requested file | Arguments beyond explicit `job_id` and workspace `dest_dir` |
|---|---|
| Exact prescan decisions | `export_kind="decision-yaml"`, `revision` from the overview, and `expected_document_sha256` equal to that overview's `sha256` |
| Conversion's effective decisions | `export_kind="decision-yaml"`; omit prescan `revision`; use an expected hash only if it identifies this conversion's own document |
| Verified source NCIR | `export_kind="source-ncir"`; omit both YAML-only revision/hash fields |

Prescan revision 0 is valid. Omitting a prescan revision exports the current
stored revision, so use an explicit revision/hash for an exact reviewed file.
Conversion export uses that conversion's frozen effective YAML. The latest
prescan or a baseline document cannot substitute for unavailable historical
conversion decisions. A model decision overview behind a conversion is not
necessarily that conversion's frozen effective artifact.

Source NCIR is a customer projection of the verified original Stage 2.5 source,
not a mapped preview, target configuration, newly parsed file or approval receipt.
Its exported byte hash can differ from the stored artifact hash. Similarly, the
canonical decision-document hash is not the YAML file's byte hash. Do not compare
those different identities as though they were interchangeable.

The client verifies the transfer's byte hash, provenance and applicable revision
before saving. Return only the resulting filename, size and SHA-256 receipt;
do not read the saved file back into chat or run commands that print it. A failed
publication may leave a file without a success receipt; treat that as unresolved,
preserve existing customer files and do not claim verified download success.

Missing, incomplete, refused or unverifiable artifacts are real limitations.
Preserve 403/404/409 refusals; do not reconstruct YAML/NCIR from MCP summaries,
request an older artifact as if it were current, or widen access. On an older
client/server, use its authenticated editor/download path if available, otherwise
explain the upgrade or artifact-availability gap. Never synthesize an export URL
or use a bulk JSON response as a substitute for file transport.

## Resume a large evidence workbook

For full-API conversion jobs, newer `download` contracts save verified primary
files and prepare a durable workbook. Inspect `files`, `complete`, `pending_export`,
`export_reference` and `failure` separately. `complete=false` is an incomplete
bundle even when some files are verified and saved. Return those valid receipts.

After the returned retry interval, use the same `job_id` and authorized `dest_dir`
with `artifact_kind="evidence-workbook"` and the exact returned `export_id`.
This resumes that export only; it does not resubmit conversion or fetch the other
files again. If a status response is lost, retain the existing `export_reference`.
If preparation returned no ID, the outcome is unknown: reconcile before explicitly
preparing again. Never invent an export ID, repeatedly prepare replacements, or
use the old synchronous endpoint as an automatic timeout workaround.

A failed, stale, inaccessible or unsupported export remains incomplete. Review a
source revision conflict or failure before an explicitly requested replacement.
The client verifies content type, length and SHA-256 before non-overwriting local
publication. Its 50 MiB workbook ceiling is separate from the source-upload policy;
it does not silently trim the report. YAML/NCIR, diagram, validation/optimization
and Quick downloads keep their separate contracts. File receipts do not authorize
opening report bodies in the model or establish complete evidence coverage.
