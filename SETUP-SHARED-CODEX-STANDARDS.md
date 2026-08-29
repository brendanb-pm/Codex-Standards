# Shared Codex Standards Checkout

Use one local checkout shared by Atlas, Nexus, All-Aboard, and future repositories.

## Recommended location

`$HOME/.codex/Codex-Standards`

Optionally set `CODEX_STANDARDS_HOME` to a different absolute path.

## Windows PowerShell — first-time setup

```powershell
$StandardsHome = Join-Path $HOME ".codex\Codex-Standards"
New-Item -ItemType Directory -Force (Split-Path $StandardsHome) | Out-Null
git clone https://github.com/brendanb-pm/Codex-Standards $StandardsHome
[Environment]::SetEnvironmentVariable("CODEX_STANDARDS_HOME", $StandardsHome, "User")
```

Open a new terminal after setting the environment variable.

## Manual refresh

```powershell
$StandardsHome = if ($env:CODEX_STANDARDS_HOME) { $env:CODEX_STANDARDS_HOME } else { Join-Path $HOME ".codex\Codex-Standards" }
git -C $StandardsHome status --short
git -C $StandardsHome pull --ff-only origin main
```

The checkout is a read-only runtime dependency. Do not make standards edits there. Make standards changes in the canonical `brendanb-pm/Codex-Standards` repository, push them to `main`, and let project sessions refresh the shared checkout.

## Repository deployment

- Replace `Project-Atlas/AGENTS.md` with the provided Atlas file.
- Add the provided `AGENTS.md` to the root of `project-nexus`.
- Add the provided `AGENTS.md` to the root of `All-Aboard` when you are ready for agent execution there.
- Do not add a Codex-Standards submodule or copied standards directory to those repositories.
