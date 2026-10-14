---
name: joripspace-cli
description: Connect, build, deploy, and operate existing JoripSpace projects using the CLI in Codex or Claude Code. A global CLI installation is not required.
---

# JoripSpace CLI

Use this skill when the user asks to connect, inspect, build, deploy, or operate a JoripSpace project. Match the user's language.

## Start a task

1. Read [Local CLI workflows](references/local-cli.md) and resolve the CLI command. Use an existing executable or `npx -y @joripspace/cli@latest`; do not require a global installation.
2. Run `CLI knowledge search --query "USER_REQUEST" --json` once at the start of a new, distinct JoripSpace task. Reuse that result during follow-ups on the same topic.
3. Reuse the workspace's existing connection or complete the browser connection-code workflow in the reference. Never ask for an API token or expose credentials.
4. A short request such as "JoripSpace plugin-test 시작해줘" means start the named project in the current workspace: run `CLI start "plugin-test" --cwd "ROOT" --json`, read its returned onboarding guide and agent documents, confirm the project status, and continue the user's task. If no further task was given, report readiness and ask what to build or change.
5. If no project was named and multiple projects are accessible, show their names and slugs and ask the user to choose. If no project exists, direct the user to create one in the JoripSpace web UI.

## Route operations

Use the CLI for project status, DB, storage, deployments, rollback, checkpoints, internal Secret generation, cron, domains, mail, usage, logs, realtime v2, templates, and GitHub workflow preparation. Check current CLI help rather than guessing flags. Use the host's own file editing tools for local source changes, then the CLI for file transfer and deployment.

Execute the requested workflow yourself using the host's shell and file editing tools. The user should not have to run CLI commands manually when these tools are available. Run from the intended project root and preserve `.env.joripspace`, `.joripspace/project`, and existing user-authored guidance. Do not add the CLI as a project dependency.

When no local shell or file access exists, explain the limitation in one sentence and provide the exact CLI command for the user to run locally. Do not substitute an unrelated remote action.

## Boundaries and safety

- Project creation and deletion, billing, token administration, member administration, webhook administration, and setting or deleting external secret values remain web-only. Link to the relevant JoripSpace web screen instead of bypassing that boundary.
- Never request secret values, browser cookies, SMTP passwords, API keys, or OAuth tokens in chat. Generated secret values stay in the project Worker binding and are not returned.
- Run write, delete, rollback, deployment, migration, and restore operations only when they are within the user's explicit request. Keep confirmations and host approval prompts intact.
- Before a database schema change, inspect the schema and design indexes for the actual query path. Use the database's query plan after migration when index use matters.
- Read the current project guide and complete its deployment checks. Do not invent successful checks or bypass a failed gate.
- On a timeout, retry the same read operation at most once. If a deployment reports unknown state, list deployments before considering one retry so duplicate deployments are not created.
- Do not change or reinterpret JoripSpace plans, pricing, terms, retention, or permission policy. Report the contract returned by the server.
