# NetConverter customer toolkit

NetConverter — by ValeronLabs LLC. **Public prerelease 1.0.0-rc.8.**

Skills for collecting network evidence, planning migrations and guiding NetConverter jobs. The collection and checklist skills work without a NetConverter account. MCP operations require an authorized account and enabled server capabilities. No device deployment is included.

## Included skills

| Skill | Purpose |
|---|---|
| [Firewall pre-migration checklist](skills/firewall-pre-migration-checklist/SKILL.md) | Scope a migration, identify missing evidence, assess risks and plan validation/cutover. |
| [Safe configuration collection](skills/safe-config-collection/SKILL.md) | Select appropriate collection methods, capture scope and supporting evidence, and disclose missing information. |
| [NetConverter MCP](skills/netconverter-mcp/SKILL.md) | Guide server-side migration decisions and review migration, validation and optimization evidence using hosted MCP or Terminal transport. |

The Claude wrapper includes **migration-architect** and **evidence-reviewer** agents for planning and evidence review. These prompts do not grant permissions or approve deployment. The experimental sanitizer is excluded.

## Download and install

Use the [1.0.0-rc.8 release](https://github.com/valeronlabs/netconverter-skills/releases/tag/v1.0.0-rc.8). Download the desired archive and SHA256SUMS.txt from that same release; verify its SHA-256 before extraction. Keep one installation route to avoid duplicate skills.

- Standalone: extract one skill ZIP and copy its entire folder into the client skills directory (Claude Code: ~/.claude/skills/; Codex: ~/.agents/skills/; Cursor: ~/.cursor/skills/). Preserve existing instructions and unrelated skills.
- Markdown: merge netconverter-mcp.md into the assistant instructions you already use. Do not replace an existing AGENTS.md, CLAUDE.md or other instruction file. Do not also activate the equivalent standalone skill.
- Claude plugin: extract netconverter-private-marketplace.zip to a stable local folder, then run the commands below with that actual path. The historical `netconverter-private` catalog name remains for compatibility; it does not restrict this public download.

```sh
claude plugin validate "/absolute/toolkit-folder/plugins/claude" --strict
claude plugin marketplace add "/absolute/toolkit-folder"
claude plugin install netconverter-customer-toolkit@netconverter-private
```

Start a fresh session, inspect `/mcp`, and complete interactive NetConverter sign-in. The portable wrapper and local marketplace catalog are also supplied for clients supporting them; they are not an approved public marketplace listing. Installing an MCP connector alone does not prove skills were installed.

For upgrades, retain a backup, disable the previous managed copy and install the selected version in one scope. Verify the bundled VERSION and skill activation in a new conversation. To remove, use the client plugin manager or delete only the installed skill folder/managed instruction block. Preserve credentials, unrelated instructions and customer files.

## Hosted MCP and local files

Hosted endpoint: https://api.netconverter.ai/mcp/v1/ (keep the trailing slash). Sign in using the client OAuth flow. Hosted MCP cannot browse your computer.

For local file transfer, install [NetConverter Terminal 0.8.0](https://github.com/valeronlabs/netconverter-terminal/releases/tag/v0.8.0) separately. Choose any authorized folder; `migrations` is not required:

```sh
cd "/absolute/customer-folder"
netconverter
# Equivalent interactive mode:
netconverter --workspace "/absolute/customer-folder" run
# Configure your MCP client to launch this bridge:
netconverter mcp --workspace "/absolute/customer-folder"
```

A configured bridge retains its absolute workspace. Folder scope, disclosure level and permission to upload a selected file are separate controls. Full disclosure does not grant whole-computer access or authorize bulk uploads. Do not paste configuration files or credentials into chat.

## Verification and limitations

This is a prerelease, not a claim of universal client compatibility or deployment readiness. Earlier actual ChatGPT and Claude guided tests showed selected server-side YAML interface changes in downloaded PAN-OS output; no-op proposals preserved revision/hash and declined options stayed unchanged. Claude package activation and isolated install/upgrade/removal were exercised. RC8 adds clearer source/target routing and package-version identity; its packaging checks passed, but final installed RC8 ChatGPT acceptance is pending. Cowork UI, Cursor and native Windows workflows beyond automated checks remain unverified.

Source interface inventory is not emitted target configuration. Use the available target reader for emitted interfaces. Jobs and evidence may require manual review; conversion completion does not prove device acceptance or traffic equivalence. Available operations depend on source, target, device type, tenant permissions and server tools. Unsupported decisions must be disclosed rather than invented.

## Provenance

Customer files here are exported from the allowlisted RC8 artifacts. Private qualification traces, internal test identities, experimental candidates and Git history are excluded. Release assets remain byte-for-byte unchanged from source commit 19ecfe3be4484bf528d7a4e912743f3d8c21ee51. The original release-manifest.json and bundled README text retain their build-time private-candidate labels; this repository and GitHub prerelease record the later owner-approved public distribution. SHA256SUMS.txt covers release downloads, not GitHub's automatic source ZIP.

[Privacy](https://netconverter.ai/privacy.html) · [Terms](https://netconverter.ai/terms.html) · [LICENSE](LICENSE) · [NOTICE](NOTICE)
