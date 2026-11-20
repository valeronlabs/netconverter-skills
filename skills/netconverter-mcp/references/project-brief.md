# Project intake and low-level design draft

Use this workflow for an architecture or migration-planning request. It uses
existing discovery, summary, decision and evidence tools when available; there
is no assumed LLD-generation endpoint. Reuse earlier answers and keep intake
proportional to the task. Do not turn a single status or export request into a
full project questionnaire.

## Establish intent before selecting operations

Capture a compact brief in the user's terms. Ask about the next consequential
unknown, rather than demanding every field before useful planning can begin.

| Brief item | Record or clarify |
|---|---|
| Outcome | Migration, like-for-like replacement, redesign, validation or optimization; what must improve and what must remain equivalent |
| Device identity | Source and target platform, firewall/router/switch role, known model and firmware, input/output format; label unknowns |
| Scope | Devices and selected manager scope, VDOM/VSYS/device groups, inherited policy, routing instances, included/excluded services and dependencies |
| Topology | Current and intended connectivity, zones/VLANs, physical/logical interfaces, routing domains, upstream/downstream peers, management and out-of-band path |
| Desired changes | Routing protocols and policy, interface/port/zone mapping, addressing, cleanup or security changes; distinguish goals from already authorized choices |
| Constraints | Required features/licenses, coexistence, operational ownership, maintenance window, tolerated outage and rollback deadline |
| Acceptance | Critical traffic and services, measurable success/failure criteria, test owners, evidence required and who can approve the change |

Treat a desired protocol or topology change as a requirement to assess, not proof
that NetConverter implements it. With a suitable job and a verified bounded
catalog response, use `list_operations` to check current implementation,
applicability, execution mode and parameter schema. Apply the per-tool checks in the
guided-decisions reference first; an older catalog can include unbounded stored
decision values.
Offer automated choices only where the live catalog supports this source, target
and role. Keep absent, planned, excluded or guidance-only work explicit. Do not
translate an unsupported requirement into a different operation without the user.

Record missing critical evidence with its consequence and owner: for example,
uncertain manager inheritance, unknown target firmware/license, unmapped physical
ports, missing routing/peer state, untested VPN dependencies or no rollback access.
Block the dependent decision when the gap matters; continue independent planning.
Do not invent source facts, interface mappings, traffic equivalence, HA behavior,
VPN interoperability or routing support to complete the brief.

General preparation does not need a NetConverter account. The standalone
safe-config-collection skill can plan scoped collection, and the
firewall-pre-migration-checklist skill can assess its covered firewall pairs.
Honor their separate scope and no-upload/offline rules. Without backend access,
produce a preliminary plan with product support and job evidence marked unverified;
do not turn a missing MCP capability into a claim that all planning is blocked.

## Build the draft from bounded evidence

For backend work, discover live schemas and account capabilities first. Verify
the response bounds of `get_job_brief`, relevant interface/zone/route summaries,
`get_evidence` sections and the report for the actual job kind before use. Follow
decision and mapping pages only when their contracts are supported. Do not call
`get_decision_document` merely to obtain a revision/hash: older versions return
the complete structured document even with `include_yaml=False`. If bounded
decision reads are unavailable, keep the LLD preliminary with the decision binding
and stored choices unverified; continue from independently safe evidence and user
requirements. Do not infer stored choices from desired changes or a jobless catalog.
For a targeted design question, use `kb_search`, `kb_explain`
or `vendor_capability_lookup` with explicit device role. Cite the returned vendor
references and preserve their strong/weak/none coverage. Knowledge coverage is
not proof of conversion support or emitted behavior.

Separate observed job evidence, user-stated requirements, documented vendor
behavior and engineering assumptions. Give each consequential recommendation its
evidence reference and confidence/limitation; do not manufacture a confidence
score or citation. A topology diagram describes the model, not live reachability.
If evidence is missing or truncated, say which conclusion remains unverified.

Use the decision, NCIR and relationship examples in the bundled tool reference
when the customer asks how the design was derived or what a change affects.
For example, "Why that port?" needs the bound YAML choice and recorded rationale;
"What uses this object?" needs direct references or modeled dependency chains;
"Where did it land on the target?" needs translation lineage and emitted evidence.
Carry each answer's job, selected scope, side and available source/document/build
versions into the LLD evidence binding. Keep unknown rationale, graph coverage and
unmodeled relationships explicit. A zero dependency count without coverage cannot
justify cleanup, and a source-to-target edge is not a traffic-equivalence result.

Keep full configurations, YAML/JSON decision documents, NCIR and complete report
bodies out of the model. A structured document is not a summary because no YAML
string is present. Use verified summaries, bounded decision views and file receipts.
Downloading a source or output file does not authorize opening it in chat. Never
reconstruct a bulk configuration or decision document inside the LLD.

## Compact LLD draft outline

Label the document **DRAFT — engineering review required**. Scale detail to the
project; the outline is not a request to generate configuration commands.

1. **Purpose, scope and constraints.** State the agreed goals, device identities,
   included/excluded scope, topology assumptions and acceptance criteria. Identify
   unresolved critical facts and their owners.
2. **Evidence binding.** Record the source job and source SHA-256 from its receipt,
   relevant target identity/format, and the exact prescan decision revision and
   document SHA used for the draft. If a conversion has already run, identify its
   job and its own effective-document hash when available; do not assign it the
   latest prescan revision or confuse document hashes with file byte hashes.
   Reference downloaded artifacts by receipt. If a source receipt or applicable
   decision binding is unavailable, label the draft preliminary/unbound and name
   the missing evidence. For analysis-only work, mark target/decision fields not
   applicable unless the job actually provides them; never invent those bindings.
3. **Proposed design and choices.** Summarize intended connectivity, routing and
   protocol behavior, interface/zone mapping, policy/NAT and management changes
   relevant to the scope. For each offered operation, record its ID, exact chosen
   parameters, selected/declined/undecided state, rationale and intended effect.
   Distinguish proposed choices from decisions already stored in the bound revision;
   refresh the draft's binding after an authorized proposal creates a new revision.
   Keep large parameter sets as revision-bound references, not copied document bodies.
   Keep declined choices declined. Identify manual work separately from automatic
   operations and attach job/section or KB/vendor citations and confidence limits.
4. **Gaps and risks.** List unsupported or partially modeled features, dependencies,
   inherited policy questions, invalidated evidence and remaining manual tasks,
   with owners and disposition. Never replace a server NEEDS REVIEW/BLOCKED verdict
   with a favorable prose verdict.
5. **Validation and acceptance test plan (ATP).** For each critical requirement,
   specify the test, expected result/threshold, environment, owner, evidence to
   retain and status (planned, passed, failed or not tested). Separate document
   validation, conversion/report checks, lab device checks and live traffic tests.
   Include relevant policy/NAT ordering, routes/adjacencies, management access,
   VPN and HA tests only where applicable; an unperformed test stays not tested.
6. **Cutover and rollback plan.** Outline prerequisites, sequence, coexistence,
   operator responsibilities, decision points, stop conditions and communications.
   Name the rollback trigger/deadline, preserved source/backup, restoration owner,
   out-of-band access and post-rollback checks. Missing customer thresholds or
   device-specific steps remain review actions, not invented runnable instructions.
7. **Review and execution record.** List decisions still needed, reviewers and
   approval state. Link the exact backend review/job receipts when present, and
   record verification of selected effects and declined changes after conversion.
   A revised source, target or decision binding makes the prior draft/approval
   unsuitable for the changed request until reconciled.

## Carry approved choices into supported execution

The LLD is an engineering draft, not a deployable artifact, server approval receipt
or permission to change devices. Use the existing guided-decision workflow to
persist authorized operations, validate that stored revision and obtain the exact
server human review where required. Its receipt authorizes the bound backend
submission, not a change window or proof of traffic acceptance. Do not add another
bridge approval prompt for the unchanged, already approved request.

For validation or optimization, use the current job's accepted submission/options
schema and report contract. Do not promise editable analysis-job YAML or apply a
conversion decision API to an unsupported job kind. Findings may become manual
recommendations or authorized choices in a separately supported conversion. Keep
intended improvements distinct from applied and verified effects throughout the
LLD and its ATP. No step here deploys, logs in to devices or certifies equivalence.
