# Output Path Strategy

Use this reference to decide where the markdown archive should be written.

## Goal

Keep the archive path tool-neutral so the skill can be used across Codex, Claude, and other agents.

Do not write archives into tool-owned directories such as `.codex/` or `.claude/` by default.

## Default Output Directory

Use this default directory when no override is present:

`./.ai/knowledge-blindspots/`

This path is relative to the current working project or workspace.

## Override Precedence

Resolve the output directory in this order:

1. explicit path provided in the current user request
2. environment variable `AI_KNOWLEDGE_BLINDSPOTS_DIR`
3. project config file `./.ai/knowledge-blindspot-journal.json`
4. user-global config file in the home directory
5. default directory `./.ai/knowledge-blindspots/`

If multiple sources exist, the highest-precedence source wins.

## Config File Locations

Project-level config:

`./.ai/knowledge-blindspot-journal.json`

User-global config:

- Windows: `%USERPROFILE%\\.ai\\knowledge-blindspot-journal.json`
- macOS/Linux: `~/.ai/knowledge-blindspot-journal.json`

If the skill cannot reliably detect the home-directory convention, use the platform-appropriate home directory for the current shell environment.

## Supported Config Keys

The config file may define:

- `output_dir`: absolute or workspace-relative directory for archives
- `filename_pattern`: file naming template
- `default_mode`: `strict` or `review`

Example:

```json
{
  "output_dir": "D:/ai-notes/knowledge-blindspots",
  "filename_pattern": "{date}_{topic}.md",
  "default_mode": "review"
}
```

## Filename Guidance

Default filename pattern:

`{date}_{topic}.md`

Recommended substitutions:

- `date`: ISO local date such as `2026-05-24`
- `topic`: short slug derived from the thread topic

Example resolved path:

`./.ai/knowledge-blindspots/2026-05-24_login-build-failure.md`

## Path Handling Rules

- Prefer workspace-relative paths for project-local archives.
- Accept absolute paths only when explicitly configured or requested.
- Normalize path separators for the current platform.
- Create missing parent directories before writing the file.
- If the configured path is invalid or inaccessible, fall back to the next lower-precedence source and state that fallback clearly.

## User Guidance

If the user asks how to create a personal global override, tell them to create the user-global config file and set `output_dir`.

If the user wants a one-off destination, prefer an explicit path in the current request or the environment variable.
