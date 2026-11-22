# Queenstown

Queenstown is an MCP server that lets an AI assistant (Claude Code, Claude Desktop or any MCP client) work with
ServiceNow instances safely. It runs locally, connects to as many instances as you configure, and ships with a set
of best practices learned from real builds.

**The source code is in a private repository.** This page describes the tool and explains how to request access.

**[➜ Request access](https://github.com/barjeshjain-coder/queenstown-info/issues/new?template=access-request.yml)**

> Personal project. Not an official ServiceNow product and not supported by ServiceNow.

---

## What it does

| Area | Tools |
|---|---|
| Read | `query`, `get_record`, `count`, `aggregate`, `table_schema`, `search_syslog`, `export_records` (large result sets to CSV or JSON) |
| Change | `run_script`: server-side scripts with GlideRecord, with safety guards |
| Verify | `blast_radius_check`: deleted records and deleted configuration since a given time |
| Update sets | `update_set`: list, summary, create, complete. Capture a single script run in an update set |
| Operations | `job_status` (scheduler health), `run_pa_collection` (run a Performance Analytics job now) |
| Attachments | `attachment_list`, `attachment_download`, `attachment_upload` |
| Setup and help | `list_instances`, `doctor` (checks login, roles, timezone and safety settings), `best_practices` |

## Safety built in

- **Reads use the REST API.** Writes happen only through server-side scripts, never through generic record writes.
- **Destructive calls are blocked by default**, including `deleteMultiple`, `deleteRecord`, `updateMultiple` and
  `setWorkflow(false)`. They need an explicit flag. A `deleteMultiple` with no query is always blocked.
- **Production instances are protected.** Scripts are turned off for instances marked `prod`.
- **Blast-radius check after writes**, so unintended deletes are caught straight away.
- **Credentials are stored in the operating system keychain**, never in config files. Each instance uses a
  dedicated integration user with web-service access only.
- **Runs locally over stdio.** No network port is opened.
- **Pinned, audited dependencies.** It runs on the latest Python and MCP SDK. A built-in `audit` command checks for
  known vulnerabilities.

## Best practices included

Queenstown sends its core working rules to the AI assistant when it connects:
- read-only by default, and approval before any change;
- validate field names and count before bulk changes;
- re-query and check the blast radius after every write;
- keep output compact.

It also ships topic guides, which can be searched by symptom (for example *"delete returns false"*):

| Guide | Covers |
|---|---|
| core-rules | Working rules for every task |
| bg-scripts | Server-side script gotchas, a safe script template, timing and side effects |
| writes-and-deletes | Why writes and deletes fail silently, and how to fix it |
| tables-and-forms | New-table checklist, fields, form sections, UI policies |
| workspace-ui | Workspace buttons, declarative actions, related lists, record page layout |
| fluent-sdk | Now SDK (Fluent) gotchas |
| ai-agents | AI Agent Studio, document extraction, AI Control Tower |
| grc | GRC / IRM data model and automation gotchas |
| scheduler-and-upgrades | Scheduled jobs, decision tables, document templates, upgrade analysis |

## Requirements

- macOS, Windows or Linux, with [`uv`](https://docs.astral.sh/uv/) installed. `uv` downloads Python 3.15 itself.
- An MCP client, such as Claude Code or Claude Desktop.
- Admin access to each instance you connect, needed once to create the integration user.
- A GitHub account. Access to the private repository is granted per person.

## How to get access

1. Open an **[access request](https://github.com/barjeshjain-coder/queenstown-info/issues/new?template=access-request.yml)**
   and fill in the short form.
2. If the request is approved, you receive a GitHub invitation to the private repository. Accept it.
3. Follow `SETUP.md` in that repository. Setup takes about 10 minutes.

Access requests are public issues. **Do not include passwords, instance URLs, customer names or other
confidential details.**

## Versions

Current release: **v0.3.0**. Releases are tagged, so you can install a specific version or roll back.
