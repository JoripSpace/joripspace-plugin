# Local CLI workflows

Use the host's own file editing tools to change source files. JoripSpace CLI connects the workspace, installs templates, transfers files, and deploys; it is not a source-code editor.

## Resolve the command

1. Check for `joripspace` on PATH and run `joripspace --version` if found. Keep the resolved executable and arguments separate, especially for paths with spaces.
2. If it is missing, check `node --version` and `npx --version`. With Node.js 18 or later and npm available, run `npx -y @joripspace/cli@latest --version`. Use this same prefix for later commands. npm downloads the package to its cache; no global installation or project dependency is needed.
3. If no executable is available and Node/npm is unavailable, explain that this plugin requires Node.js 18+ and npm to run the CLI on demand. Do not install a runtime or change system settings without the user's request.

Check the resolved command's `--help` before choosing flags. For deployment and template installation, also read `deploy --help` or `install-template --help`. Prefer `--json` for machine-readable results. CLI errors may be returned on stderr with a nonzero exit code; inspect both streams and never treat partial output as success. Respect host network and execution approvals.

Below, `CLI` means the existing executable or `npx -y @joripspace/cli@latest`. Replace `PROJECT` with the chosen project slug and `ROOT` with the intended absolute workspace path. Quote paths and pass user content as arguments without shell interpolation. Never print connection tokens or credential files.

## Connect and select a project

Reuse the ignored `.env.joripspace` connection when present. Connection setup and renewal use the CLI's browser connection-code flow.

For onboarding without existing credentials:

1. Run `CLI login --cwd "ROOT" --json`, show its browser connection link, and wait for the user's five-minute connection code.
2. Run `CLI login --code "CODE" --cwd "ROOT" --json`. Keep the code and returned credentials out of the reply and logs you create. This command persists connection files and may auto-link the sole accessible project.
3. Run `CLI projects --cwd "ROOT" --json`. If there are multiple projects, ask the user to choose. Reuse an explicitly requested slug when accessible; never choose a different project silently.
4. Run `CLI start "PROJECT" --cwd "ROOT" --json` only when the user requested local workspace work. It can synchronize deployed source and create connection files. Preserve conflict backups and follow the returned onboarding guide. Under Node/npm, a returned executable ending in `.js` must be launched with `node` or the same npx prefix, not treated as a native executable.

For remote-only requests, use project-targeted commands after authentication without running `start` or changing local source files unnecessarily. If credentials expire, use the browser connection flow; never extract cookies or copy OAuth tokens from another host's credential store.

## Execute the requested operation

Use these command shapes after checking the installed help and current project guide. Credentials are resolved from the workspace; do not put token values in command examples or chat.

| User task                         | CLI command after `CLI`                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Inspect project                   | `get --project "PROJECT" --cwd "ROOT" --json`                                                           |
| List templates                    | `template list --project "PROJECT" --cwd "ROOT" --json`                                                 |
| Install chosen template locally   | `install-template --project "PROJECT" --template "SLUG" --dir "ROOT" --cwd "ROOT" --files-only --json`  |
| Check template updates            | `template pull --project "PROJECT" --dir "ROOT" --cwd "ROOT" --json`                                    |
| Deploy verified local directory   | `deploy --project "PROJECT" --dir "ROOT" --entrypoint "ENTRYPOINT" --label "LABEL" --cwd "ROOT" --json` |
| Read deployment history           | `deployments --project "PROJECT" --cwd "ROOT" --json`                                                   |
| Download chosen deployment source | `deployment pull --project "PROJECT" --deployment "ID" --cwd "ROOT" --json`                             |
| Save local project files          | `checkpoint save --project "PROJECT" --label "LABEL" --cwd "ROOT" --json`                               |
| List saved checkpoints            | `checkpoint list --project "PROJECT" --cwd "ROOT" --json`                                               |
| Preview local checkpoint restore  | `checkpoint restore --project "PROJECT" --checkpoint "ID" --cwd "ROOT" --json`                          |

For DB, storage, rollback, secrets, cron, domains, mail, usage, logs, realtime, and GitHub tasks, use their existing CLI commands listed by `--help`; do not invent an API wrapper. Keep queries read-only unless the user requested a write. `storage get --out` downloads a file; it does not edit application source.

Read the current project guide and complete its deployment verification gate before any deployment. Record checks actually run; do not invent verification flags that are absent from CLI help. Check deployment history after a timeout before retrying. Report the actual URL and status after success.

Template update application uses `--yes`; local checkpoint restoration uses `--apply`. Use them only after the requested changes and target have been reviewed, and retain the CLI's safety backups. Local restore does not authorize production rollback or deployment. Do not add `--force`, `--deploy`, or delete flags merely to make a command succeed.
