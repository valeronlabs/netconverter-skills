# Troubleshooting NetConverter MCP

Phrase errors to the user plainly, keep credentials out of chat, and do not
retry a refusal in a loop. Current details:
https://netconverter.ai/docs/troubleshooting.html.

## Connection and access

- **No NetConverter tools in the client.** Missing tools alone do not establish
  a login failure. Inspect the selected connection, client tool filters and
  connection status first. Claude Code may suppress a bundled MCP server when
  another configured server or claude.ai connector has the same URL. A successful
  login does not prove that this particular plugin namespace is exposed. Do not
  ask the user to sign in again without an authentication error, or silently
  substitute a different connection/account. For private qualification, isolate
  unrelated connectors in a session-scoped test, preserving saved configuration;
  record the exact connection used. Follow the client's official MCP guidance:
  https://code.claude.com/docs/en/mcp. For a genuinely unconfigured connection use
  https://netconverter.ai/mcp.html#connect, then verify its tool catalog.
- **"MCP not enabled" / access not granted / 403.** The account has no MCP
  access, or the credential does not allow that operation. Only the user's
  NetConverter administrator can change this. Stop.
- **The client's sign-in is refused.** The client is not configured the way
  https://netconverter.ai/mcp.html describes. Follow that page; do not invent
  a workaround.
- **Organization ambiguity.** If the user belongs to several organizations and
  access is refused, they should switch organization in the NetConverter
  portal and retry.

## Tools present but refusing

- **ChatGPT still exposes an older tool schema after reconnecting.** OAuth
  reconnection and tool-catalog refresh are separate. For a developer-owned
  custom plugin, open its ChatGPT plugin settings and use **Refresh tools**
  ("Load the latest tools from the app's MCP server"), then start a fresh
  conversation. Verify the schema in that conversation: bounded decision reads
  expose `view`, `section`, `cursor` and `expected_sha256`. Do not assume refresh
  succeeded from sign-in alone. If that control is unavailable, ask the plugin
  owner to refresh its registered tools; retain the compatibility gap until the
  new schema and a bounded read are observed. Never substitute full YAML or
  change Clerk, MFA, disclosure or tenant permissions to solve stale metadata.
- **A filename is known but no prescan exists.** Check the source location first.
  Use local workspace discovery plus `submit_file(job_type="prescan")`, or hosted
  uploaded-file discovery plus `start_prescan`, when exposed. Do not require a
  manual UI prescan solely because the user supplied a filename. If neither entry
  is available, explain the connection/upgrade gap and supported alternative.
- **Only some artifacts downloaded.** Preserve their verified receipts. Follow
  a returned workbook `export_reference` after its retry interval when supported;
  do not rerun the migration. Missing export identity or an ambiguous preparation
  outcome requires reconciliation before another prepare.

- **Write or guided tools missing, or refused as disabled.** They are off for
  the tenant, or the session is in a stricter view. Call `get_account` and
  relay what it says. Offer the web app's Quick Convert or Conversion Studio.
- **Upload needs confirmation, or was declined.** Obtain any missing user
  authorization covering that exact upload before retrying. Reuse an explicit
  authorization already given; never manufacture a token or answer a human
  approval ceremony yourself. A decline is final. A server-approved conversion
  receipt does not require a repeated bridge queue prompt for the same request.
- **Invalid arguments.** Re-read the tool's schema from the server. Remove
  fields you guessed. Do not invent vendor codes.
- **Local dispatch error such as `TypeError: ... is not a function`.** This
  does not establish a NetConverter outage or permission refusal. Discover the
  exact callable identifier and schema exposed in this conversation; do not
  construct a function name from the plugin's display name or reuse another
  client's namespace. Distinguish the main connection from similarly named
  test connections. If no server request was dispatched, invoke the authorized
  operation through the discovered tool once. If dispatch is uncertain, first
  reconcile the existing revision/job receipt; never repeat a proposal or queue
  new work merely to recover a client error. Report a missing callable as a
  client compatibility gap, without changing permissions or switching identity.
- **Too many requests / daily budget reached.** Stop, tell the user, and resume
  later. Don't retry in a loop.
- **Job not found.** Unknown, foreign and unselected jobs all look the same.
  Re-check with `list_jobs`; pass `job_id` explicitly for non-conversion jobs.
- **Document changed since you read it** (guided migration). Re-read with
  `get_decision_document`, redo the change with the user, and pass the new
  `sha256`.
- **Stale decision/mapping cursor.** Refresh the decision overview or restart
  mapping questions without a cursor. Keep job, view, limits and revision/hash
  consistent; never splice old pages into the refreshed result.
- **`approval_required`, pending, expired, revoked or stale review.** Follow the
  advertised server review workflow. A chat yes or native confirmation is not
  a verified receipt. If the client lacks the receipt fields, explain the
  upgrade/human-workflow gap; do not retry without them.
- **Unsupported file policy, oversize upload or ZIP prescan refused.** Respect
  the negotiated limit, vendor, selected scope and job-type contract. Selected
  Check Point/FMC ZIP prescans need compatible file-ID client/server support;
  other bundles or older transports may not support them. Do not paste, split,
  rename or automatically switch job types to bypass the refusal.
- **Explicit export absent or refused.** Use supported authenticated file/editor
  access, or report unavailability. Do not assemble a document from summaries.
  A revision/hash conflict requires reconciliation, not a latest-revision retry.

## Results that look wrong

- **Names look like tokens / addresses look like `<ip>`.** Expected in masked
  or stricter views. Long mapping names can also be tokenized in default view.
  Pass identifiers and option values back unchanged.
- **"Not available" for a target, report or section.** That artifact was not
  produced for this job (blocked, validate-only, unfinished). Say so.
- **`digest_truncated`, `truncated` or `data_quality` other than `full`.** Read
  the advertised sections/pages needed for the question, or disclose incomplete
  evidence. Missing warnings in a partial digest do not establish their absence.
- **Evidence identifies another job.** Stop that result's use and retain the
  mismatch. Reconcile the explicit request, returned binding and artifact
  provenance; do not substitute a similar earlier job or count its warnings.
- **`NEEDS REVIEW` or `BLOCKED`.** A result, not an error. Use `get_evidence`,
  then the report.
- **Unsupported vendor or path.** Say so and link
  https://netconverter.ai/docs/supported-vendors.html. Never rename a file or
  vendor to force detection.

## Asking for help

Share the client version, OS, the operation, the safe error text and the job
id through the user's normal NetConverter support channel. Remove credentials
and customer data first.
