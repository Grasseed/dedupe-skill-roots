# Dedupe Skill Roots Workflow

## Purpose

Use this workflow to consolidate duplicate skill discovery roots into one canonical source and prevent repeated scans from multiple visible copies.

## Inspect

1. List roots and identify real directories vs symlinks.
2. Check for nested `skills` directories inside the active workspace.
3. If the user reports repeated VS Code scans, review recent renderer logs for:
   - `computeSkillDiscoveryInfo`
   - `ChatSessionsService`
   - `Auto updating outdated extensions`
4. Treat tool-specific roots as examples, not hard requirements. Use the roots that exist on the machine.

Example commands:

```bash
find ~/.claude/skills -maxdepth 1 -mindepth 1 -print
find ~/.codex/skills -maxdepth 1 -mindepth 1 -print
find ~/.agents/skills -maxdepth 1 -mindepth 1 -print
rg -n "computeSkillDiscoveryInfo|ChatSessionsService|Auto updating outdated extensions" \
  "$HOME/Library/Application Support/Code/logs"
```

## Choose Canonical Source

- Prefer the fuller or more correct implementation when two versions differ.
- If the contents are identical, keep one canonical tree and replace the others with symlinks.
- If a skill intentionally differs by product, do not force a merge.

## Backup

Create a timestamped backup before moving anything:

```bash
backup_root="$HOME/.skill-backups/$(date +%Y%m%dT%H%M%S)"
mkdir -p "$backup_root"
```

Save both the old canonical tree and any tree you plan to replace.

## Replace

1. Move the chosen canonical content into the canonical directory.
2. Replace duplicate directories with symlinks to the canonical path.
3. Keep tool-specific roots as thin entry points only.

Example:

```bash
mv ~/.agents/skills/<skill-name> "$backup_root/agents/skills/<skill-name>"
cp -a ~/.claude/skills/<skill-name> ~/.agents/skills/<skill-name>
ln -sfn ~/.agents/skills/<skill-name> ~/.claude/skills/<skill-name>
ln -sfn ~/.agents/skills/<skill-name> ~/.codex/skills/<skill-name>
```

## Verify

1. Confirm the canonical directory contains the intended files.
2. Confirm the entry points resolve to the canonical path.
3. Relaunch the app and confirm repeated scan warnings do not recur.

Example checks:

```bash
readlink ~/.claude/skills/<skill-name>
readlink ~/.codex/skills/<skill-name>
diff -qr ~/.claude/skills/<skill-name> ~/.agents/skills/<skill-name>
```

## 繁體中文

- 先盤點哪些 skill root 實際存在，哪些只是 symlink。
- 先比對內容差異，再決定 canonical source。
- 用備份保護搬移與回滾，不要直接覆蓋。
- 驗證時確認每個入口都指向同一份內容，且重複掃描警告消失或明顯下降。

## 简体中文

- 先盘点哪些 skill root 实际存在，哪些只是 symlink。
- 先比对内容差异，再决定 canonical source。
- 用备份保护搬移和回滚，不要直接覆盖。
- 验证时确认每个入口都指向同一份内容，且重复扫描警告消失或明显下降。

## Roll Back

If the replacement changes behavior or chooses the wrong canonical source:

1. Remove the symlinked entry points.
2. Restore the backed-up directories.
3. Re-run verification before leaving the machine in the new state.
