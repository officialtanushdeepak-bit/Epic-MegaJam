# Epic-MegaJam — agent rules

Entry for the 2026 Epic AI MegaJam. Jam window: 12 Nov 2026 10:30 AM IST → 16 Nov 2026 7:29 AM IST. Background and checklists: `docs/PREJAM.md`.

## Hard rules (break one = disqualification risk)
- **No game content before 12 Nov 2026 10:30 AM IST.** Before then only tooling, configs, scripts and docs. Epic can audit git history.
- Everything player-facing is **English** and **13+ (ESRB Teen)**.
- **No trademarks or logos** except the Unreal Engine logo per Epic branding rules. No other IP (characters, brands, music).
- Any asset we didn't make (Fab, free packs) goes in the "Sourced assets" table in `DEVLOG.md` with its licence. AI-generated assets are ours and don't need listing, but log the tool used.
- Keep the build packageable at all times. Unreferenced assets don't get cooked.

## Working with the editor (Unreal MCP)
- MCP server: `http://127.0.0.1:8000/mcp` (see `.mcp.json`). Start the agent from the repo root, after the editor is running.
- Tool calls run serially on the game thread: **one agent drives the editor at a time.** Other agents work on files only.
- Before a multi-step editor change, state the plan. Keep each change small and reviewable.
- New C++ `UFUNCTION`s need a full editor restart; Live Coding won't pick them up.

## Git
- Small commits, honest messages, commit often (history is evidence for judges and for the Glass Box dev log).
- Binary assets go through Git LFS (`.gitattributes`). Never commit `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/`.
- Work on a branch per feature; merge to `main` only when it opens in the editor and runs.

## Dev log
After each meaningful change, append a line to `DEVLOG.md` (time IST, what, which AI tool/prompt helped).
