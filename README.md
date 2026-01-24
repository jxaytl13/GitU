# GitU (Git commits inside the Unity Editor)

GitU brings the full flow—"review changes → pick files → stage → commit/push"—into the Unity Editor, so you can ship a clean commit without leaving Unity.

> Recommended location: `Assets/Editor/GitU`

## Key highlights

- **Safer by default**: If you don't pick a target asset/folder, GitU shows nothing—reducing the chance of accidentally committing the entire repo.
- **Selective commits, fast**: Unstaged on the left, Staged on the right; drag & drop to stage/unstage; supports multi-select and filtering.
- **Optimized for Unity workflows**: Open directly from Project/Hierarchy; understands Unity asset types (Prefab/Scene/Texture...); focuses changes by target scope.
- **Doesn't freeze the Editor**: Refresh, stage/unstage, commit, and push run asynchronously with status feedback.
- **Works with multiple repos**: If multiple `.git` roots exist under the project tree, changes are grouped per repo and operations run per repo.
- **Commit messages made easy**: Keeps a local commit message history for quick reuse.
- **Optional external Git client handoff**: After commit/push, clicking "OK" in the result dialog can open your configured Git client (UGit / SourceTree / GitHub Desktop, etc.).

## Requirements

- The Unity project must be inside a Git repository (a `.git` exists in a parent directory).
- Git must be installed and `git` must be available in `PATH`.
- If you use "Commit & Push", an upstream should be configured (otherwise `git push` may fail).

## 3‑minute quick start (recommended)

1. **Pick a target**: Select a file/folder in Project, or select a GameObject in Hierarchy (GitU will try to map it to Prefab/Scene assets).
2. **Open GitU**: Menu `Window/T·L NEXUS/GitU`, or right-click:
   - `Assets/T·L NEXUS/GitU`
   - `GameObject/T·L NEXUS/GitU`
3. **Refresh & review**: Confirm Unstaged changes on the left are what you want (filter by Added/Modified/Deleted + asset type + search).
4. **Drag to stage**: Drag items from Unstaged → Staged (typically include the `.meta` file when applicable).
5. **Commit**:
   - `Commit`: commit locally
   - `Commit & Push`: commit + push

## High-frequency features

### 1) Target scope

- **No target**: shows no changes (safety-first).
- **File target**: prioritizes changes related to that asset (may include dependencies/references; exact behavior depends on GitU’s calculation).
- **Folder target**: shows changes under that folder (and subpaths).

### 2) Two lists: Unstaged ↔ Staged

- Left: Unstaged
- Right: Staged (what actually goes into `git commit`)

Common interactions:

- Click to locate; double-click to copy filename/path (Unity paths like `Assets/...`).
- Right-click `Discard Changes`: discards selected changes (high-risk; see notes below).

### 3) Filter + sort

- Change type: Added / Modified / Deleted
- Asset type: Prefab / Scene / Texture ... (and All)
- Search: filters by filename/path (applies to both lists)
- Sort: by status/type/name/time/path (handy for large commits)

### 4) Settings

You can enable/configure:

- **Auto save before opening GitU (Ctrl+S)**: Saves once when opening GitU (`Save Assets + Save Open Scenes`, and attempts to save PrefabStage) to avoid missing changes that aren’t written to disk yet.
- **Open Git client after commit**: After Commit/Commit & Push, clicking "OK" in the result dialog opens the external Git client; you can also set the client executable path.
- **CN/EN switch**: The settings panel supports language switching.

## Notes (recommended reading)

- `Discard Changes` is **irreversible**: it may involve reset / checkout / clean (deleting untracked files). Confirm you don’t need the changes; back up or `git stash` first if necessary.
- `Commit & Push` is essentially `git push`: it does not automatically `pull/rebase`. If you hit "rejected", resolve sync/conflicts in your external client/CLI first.

## FAQ

- **Git root not found**: Ensure the Unity project is inside a Git repo; verify with `git rev-parse --show-toplevel` from the project folder.
- **Git not available**: Install Git and make sure `git` is in `PATH`.

## Storage (local to this machine & project)

- Commit message history: `Library/GitUCommitHistory.json`
- Staged allowlist (for handling "externally staged" scenarios): `Library/GitUStagedAllowList.json`

> `Library` is typically ignored by Git (via `.gitignore`), so this data usually won’t sync to other machines.
