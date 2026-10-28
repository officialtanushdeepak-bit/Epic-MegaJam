# Pre-jam baseline — 2026 Epic AI MegaJam

Written 28 Oct 2026. Jam opens **12 Nov 2026, 10:30 AM IST**. Deadline **16 Nov 2026, 7:29 AM IST**.
Full rules digest: `MEGAJAM_2026_CONTEXT.md` (team context file, kept outside this repo).
itch page re-read 28 Oct: matches the context file. AI tool credits still "coming soon". Official Rules link on the page: `epicgames.box.com/s/a7xcgl07fqoieeb99sjtd3juhj4agc4z`. In-person tickets: Eventbrite (n/a for us).

Rule that shapes everything below: **no game content before 12 Nov 10:30 AM IST.** Tooling, agent configs, MCP settings, prompt libraries and scripts are allowed and must be declared. Epic may audit this repo's history, so everything committed before that time is tooling or docs only.

---

## 1. Baseline: what we have today

| Item | State (verified 28 Oct) |
|---|---|
| Machine (Tanush) | MacBook Air M4, 16 GB RAM, 10 cores, 253 GB free, macOS 26.6.2 |
| Unreal Engine / Epic Launcher | **Not installed** |
| Xcode | **Not installed** (only Command Line Tools). UE on Mac needs full Xcode. |
| git, git-lfs, gh, node, ffmpeg | Installed. gh logged in. |
| Repo | `officialtanushdeepak-bit/Epic-MegaJam`, one initial commit |
| Team | 3 people: Tanush (Mac), Windows teammate, third member. Both Windows PCs (3070 Ti and 5060) belong to the same teammate; third member's machine unknown. |
| Windows build PC | RTX 3070 Ti 8 GB, 32 GB RAM (chosen over the RTX 5060 / 24 GB option: RAM is the tighter limit for UE). UE install and git/LFS setup not yet confirmed. |

UE 5.8 macOS requirements (Epic docs): Xcode **26.0 min, 26.1.1 recommended, 26.4 incompatible**; macOS Sonoma 14.5+; M1/M2 min, M3 recommended; **16 GB min, 32 GB recommended**. macOS 26 (Tahoe) is not listed — must be smoke-tested.

---

## 2. Hard findings that change the plan

1. **UEFN does not run on macOS.** Windows only. On this Mac, UEFN is impossible without a teammate's Windows PC.
2. **UE cannot package a Windows build from a Mac.** No Mac→Windows cross-compiler. The itch page explicitly accepts "Windows, macOS, Android, or iOS", so a **macOS package is a valid entry** and needs no extra machine. But the PDF rules say "packaged for Windows", and most judges will play on Windows, so a Mac-only build risks fewer people being able to play it. Plan: ship macOS for sure, and add a Windows package from a teammate's PC or a cloud Windows VM (a few hours on day 2 and day 4). Submit both in the one zip if under 1 GB.
3. **This Mac is at UE's floor.** 16 GB is the minimum and the Air has no fan, so it throttles under long editor sessions. Plan a stylized look (no heavy Lumen/Nanite scenes) and keep levels small. Render Royalty is not a realistic modifier from this machine.
4. **Install Xcode 26.1.1 specifically.** The App Store will hand out a newer Xcode, which may be incompatible (26.4 is known-bad). Get 26.1.1 from developer.apple.com/download/all.
5. **Unreal MCP runs tool calls serially on the game thread.** One agent drives the editor at a time; other agents work on files (C++, Python, docs, audio) in parallel.

---

## 3. Decision: Unreal Engine 5.8 (decided 28 Oct)

A teammate has a Windows PC with a GPU. So we go **UE 5.8**:
- Both machines can run the UE editor. With UEFN only the Windows teammate could open the editor; the Mac would be limited to Verse files and side work.
- The Windows PC is the Windows build machine and handles heavy scenes. The Mac builds the macOS package. Two packaged builds, no cloud VM needed.
- What we give up: Walkie-Talkie ($2,500, UEFN-only) and UEFN's easy mobile path. Best Team ($5,000) and Best Student are engine-neutral, so we lose little.
- Revisit only if the Windows teammate drops out before 12 Nov.

Original options, for the record:

| If… | Then | Why |
|---|---|---|
| Nobody on the team has a Windows PC | **UE 5.8**, macOS package (guaranteed) + Windows package on a cloud Windows VM | Only option that runs on Macs |
| A teammate has a strong Windows PC and wants to lead | Still **UE 5.8** (recommended) — Windows PC becomes the build + heavy-scene machine | Total gameplay freedom, Best Student + Best Team are engine-neutral |
| The team wants Walkie-Talkie (LLM NPC narrative) | UEFN, but Tanush can only do non-editor work (Verse files, prompts, audio, video, devlog) | UEFN is Windows-only |

**Stack: UE 5.8, Blueprints + C++ where agents help. macOS package from the Mac, Windows package from the teammate's PC.** Both machines must run the exact same UE 5.8.x patch version, or assets saved on one won't open on the other.

Prize target: **Best Student Project** (main) + **Best Team** ($5k, automatic for 2+ people) or **Glass Box** (dev log) as the modifier. Requires every member enrolled in higher education.

---

## 4. Skills audit — what we will actually use

Out of ~150 installed skills, these map to judging criteria or submission requirements. The rest (SEO, Instagram, trading, founder-pack) are irrelevant to the jam.

| Jam need | Skill / tool | How |
|---|---|---|
| Theme → concept (day 1, first 2 h) | `idea-refine`, `interview-me` | Diverge on the theme, then converge on one scoped concept |
| Stress-test the concept | `doubt-driven-development` | Kill concepts that can't ship in 93 h |
| Scope + task split | `planning-and-task-breakdown`, `spec-driven-development` | One-page game spec, tasks per person |
| Build loop | `incremental-implementation`, `debugging-and-error-recovery`, `anthropic-skills:systematic-debugging` | Small playable increments, root-cause crash fixes |
| Git + LFS hygiene (auditable history) | `git-workflow-and-versioning` | Small honest commits, binary assets via LFS |
| Audio (1/7 of the score) | `earshot`, `real-sound`, Higgsfield `generate_audio`, `voicestudio` | Generate/pick SFX + music, then actually "listen" to judge mix and loudness |
| Art / 3D assets (AI-made = scored as ours) | Higgsfield `generate_image`, `generate_3d`, `remove_background`, `upscale_image` | Concept art, textures, props; import into UE |
| HUD / menu mockups | Figma MCP | Mock UI before building in UMG |
| Gameplay video (30–60 s, ≥1080p) | `ffmpeg` (installed), `hyperframes` / `remotion-*`, `media-use` | Capture real gameplay, cut, add title card + music, export 1920×1080 |
| Dev log (Glass Box) | `documentation-and-adrs`, `claude-mem:timeline-report`, `claude-mem:standup`, Artifact pages | Turn `DEVLOG.md` + commit history into a published dev log |
| Watch Epic's MCP setup video/livestreams | `watch` | Needs `yt-dlp` + an engine choice (local or Gemini key) — not set up yet |
| Ship checklist | `shipping-and-launch` | Final submission pass |

Third-party "UEFN dream-bot team" kit: skip unless we go UEFN; if used, declare it.

---

## 5. Pre-jam checklist (owner: whole team)

**This week (by 2 Nov)**
- [x] Every member 18+ (confirmed 28 Oct).
- [ ] Confirm team: names, every member currently enrolled (proof handy), Epic account emails.
- [x] Confirm who has a Windows PC + GPU. Decide UE vs UEFN → UE 5.8 (Section 3).
- [x] Windows PC chosen: RTX 3070 Ti + 32 GB RAM.
- [ ] Windows teammate: confirm ~100 GB free disk for UE + project.
- [ ] Pick Team Owner (receives prize, W-8BEN, Hyperwallet) and write down the prize split.
- [ ] Team name.
- [ ] Every member joins the jam on itch.io.

**Suggested roles (3 people, 3 editors = 3 agents driving UE in parallel)**
- Tanush (Mac): gameplay code + UI, audio, gameplay video, dev log, macOS package.
- Windows teammate (3070 Ti / 32 GB): levels, lighting, visuals, Windows package (build owner).
- Third member: role depends on their machine (TBD). The Windows teammate's spare 5060 PC could be lent as a second editor seat.

**Next week (3–9 Nov)**

Mac (Tanush):
- [ ] Install Xcode 26.1.1 (not latest) → `sudo xcode-select -s /Applications/Xcode.app` → open once to accept licence.
- [ ] Install Epic Games Launcher → UE 5.8 (~60 GB).

Windows PC (teammate):
- [ ] Install Epic Games Launcher → the **same** UE 5.8.x version as the Mac.
- [ ] Install Visual Studio 2022 with "Game development with C++" workload (needed for C++ and for packaging C++ projects).
- [ ] Install Git + Git LFS, `git lfs install`, clone this repo, check that `git lfs env` works.
- [ ] Install Claude Code (or their agent of choice); confirm it connects to the editor's MCP server.

Both:
- [ ] Throwaway project (outside this repo): enable Unreal MCP + All Toolsets, Auto Start Server, run `ModelContextProtocol.GenerateClientConfig ClaudeCode`, smoke test "What actors do I have selected?".
- [ ] Throwaway C++ project: package for macOS on the Mac and for Windows on the PC. Time both. Then write the packaging scripts from the commands that worked.
- [ ] Round-trip test: save a `.uasset` on one machine, push, pull and open it on the other.
- [ ] Record 30 s of screen at 1920×1080 and confirm the file is ≥1080p.
- [ ] Re-check itch page + blog for AI tool credits.
- [ ] Watch the UE MCP setup video and livestream (links in the context file).

**10–11 Nov**
- [ ] Freeze tooling. Update the declaration (Section 6). Tag `prejam-baseline`.
- [ ] Kickoff stream 12 Nov 12:30 AM IST (or replay in the morning).

---

## 6. Pre-jam declaration (submit with entry)

Everything committed before 12 Nov 2026 10:30 AM IST is tooling only:
- `CLAUDE.md` — agent rules for this project
- `.mcp.json` — Unreal MCP client config (Epic's default)
- `.gitignore`, `.gitattributes` — UE ignore rules and Git LFS tracking
- `docs/PREJAM.md`, `DEVLOG.md` — planning docs and an empty dev log

No gameplay, levels, art or audio exist before the jam opens.
