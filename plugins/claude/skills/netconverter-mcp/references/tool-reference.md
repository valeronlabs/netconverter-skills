# Tool routing and schema discipline

The connected client's current tool schemas govern arguments. This is a task map,
not a promise that every account exposes every tool. Public reference:
https://netconverter.ai/docs/mcp-tools.html.
The receipt, paging and explicit document-file workflows in this source are
private candidates. Verify the live tools **and their fields** before calling;
an old tool name alone does not establish the newer contract.

| Task | Tools | Important constraint |
|---|---|---|
| Access and capabilities | `get_account`; bridge `account` | Read effective view, grants and limits; no privacy escalation |
| Discover source | bridge `list_workspace_files`; hosted `list_source_files` | Local relative paths and uploaded server file IDs are different; bounded metadata only, not format validation |
| Start guided migration | bridge `submit_file(job_type="prescan")`; hosted `start_prescan` | Authorized path or uploaded file ID; explicit source, target and role; no manual UI prescan prerequisite when exposed |
| Submit analysis/direct conversion | bridge `submit_file` | Workspace path only; requested job type and role-specific vendor codes |
| Wait/resume | `wait_for_job`, `job_status`, `list_jobs` | Bounded waits; retain receipt; no blind resubmission |
| Overview/evidence | `get_job_brief`, `get_evidence` | Page sections; preserve truncation and unknowns |
| Reports | `get_report` | Explicit validation or optimization job id |
| Source interfaces | `get_interface_summary` | Source IR only; no `side` argument; never label these rows emitted output |
| Emitted target interfaces | `get_target_config(section="interface_details")` | Verify `source_of_truth=emitted_config`; mapped-model fallback is not emitted proof |
| Zones | `get_zone_summary` | Use only sides and limits actually exposed by its schema |
| Policy and NAT | `get_acl_summary`, `get_nat_summary`, `list_rules_matching` | Ordered evaluation and NAT semantics require evidence |
| Object inventory | `get_object_summary`, `get_unused_objects`, `list_objects_with_attribute` | Unused does not automatically mean safe to delete |
| Object uses and impact | `get_object_dependencies`, `get_dependency_chains`, `get_critical_object_impact` | Direct uses, modeled transitive chains and blast radius are distinct; retain scope and coverage |
| Translation lineage | `get_translation_edge_summary` | `TRANSLATES_TO` evidence describes recorded source/target mappings, not equivalence |
| Routes, VPN, scope | `get_route_summary`, `get_vpn_summary`, `get_management_config` | Disclose unmodelled/manual elements |
| Flow/intent | `simulate_flow`, `compare_against_intent` | Modelled scope only; no live traffic claim |
| Output | `get_target_config`; bridge `download` | Authorized samples versus files; explicit YAML/NCIR exports require advertised `export_kind`; receipts only |
| Guided decisions | `list_operations`, `get_decision_document`, `validate_decision_document`, `propose_operations`, `submit_conversion` | Explicit prescan id; bounded reads; schema-check choices before writes, validate persisted revision afterward |
| Server human review | `request_submission_review`, `get_submission_approval` | Eligible accounts only; exact revision and verified server receipt; no local-confirmation substitute |
| Mapping | `get_mapping_questions`, `validate_mapping`, `apply_mapping_answers` | Page questions and used ports when advertised; at most 200 answers per validate/apply; new conversion |
| Diagram | `get_diagram`; bridge `preview_diagram` | Use returned descriptor; bounded nodes |
| Knowledge | `kb_search`, `kb_explain`, `vendor_capability_lookup` | Knowledge coverage is not product support |
| Other parsed facts | `query_ir` | Bounded NCIR fallback after purpose-built tools; no arbitrary Cypher or raw GraphDB access |

`prescan` is a job type for bridge `submit_file`. The conditional hosted
`start_prescan` entrypoint accepts an uploaded `file_id`, vendor pair, explicit
`device_type` and `idempotency_key`; it does not accept a path or configuration
text. Inspect the actual schema before use. Never infer support from a tool name.

The skill's guided-decisions reference covers revision-bound reads and approval;
its file-transfers reference covers negotiated uploads and receipt-only downloads.
These are conditional instructions, not new tools created by a skill.

## Questions about decisions, facts and relationships

Route by what the customer needs to know, within the connected tool's actual
schema and modeled scope. Use explicit job IDs and source/target selectors where
supported; do not invent a selector for a tool that lacks it.

| Example question | Evidence to use |
|---|---|
| "Why did we map this interface to that port?" | Bounded decision section and revision ledger/diff, operation state and the recorded project choice. Distinguish stored value from recorded rationale; if rationale is absent, say so. |
| "What routes or policy did NetConverter parse from my source?" | Same-job purpose-built NCIR summaries; `query_ir` only for a structured fact no narrower tool exposes. |
| "Did the requested mapping actually reach the target configuration?" | Conversion's effective decision binding, bounded target output and its evidence; the latest prescan revision alone cannot answer this. |
| "Which rules or groups use this object?" | `get_object_dependencies` for direct references; retain returned reference kinds, omissions and totals. |
| "What depends on this object indirectly, and what would a change affect?" | `get_dependency_chains` for modeled paths and `get_critical_object_impact` for reported blast radius. Disclose depth/count bounds and relationships the model does not cover. |
| "Which target objects came from this source object?" | `get_translation_edge_summary` for recorded `TRANSLATES_TO` lineage and returned confidence/provenance; verify any behavior claim against the same job's output/evidence. |

Keep tenant/access context, job, selected manager scope, source/target side and
available source/document/build versions with the answer. Missing identity or
version evidence stays unverified; do not join another job's graph, NCIR or YAML
because object names match. Missing or unmodeled relationships, partial reads,
`digest_truncated`, non-full `data_quality`, and zero counts without adequate
coverage cannot prove absence or safe deletion. Explain the bounded answer and
the missing evidence. Backend graph use for another purpose does not create an
exposed customer tool; disclose that gap rather than inventing database queries.

For full YAML, source NCIR, configurations or reports, use advertised authenticated
API/editor downloads or the terminal's supported `download` exports. Return file
receipts; never dump full documents, credentials or raw database contents into the
conversation. Download authorization does not itself authorize model inspection.

