---
name: dedupe-skill-roots
description: Consolidate duplicate skill discovery roots and replace duplicate skill trees with one canonical source. Use when Codex, VS Code, Claude Code, or related tools repeatedly scan the same skills, when ~/.claude/skills, ~/.codex/skills, or ~/.agents/skills contain duplicate entries, when symlinked skill trees need to be normalized, or when repeated computeSkillDiscoveryInfo / duplicate-skill warnings need to be reduced.
---

# Dedupe Skill Roots

## Goal

Keep one canonical skill tree and make other tool-specific roots thin entry points. Do not merge skill trees blindly if their contents differ.

## Workflow

1. Inspect the current roots.
   - List real directories vs symlinks under the active skill roots, for example `~/.claude/skills`, `~/.codex/skills`, and `~/.agents/skills`.
   - Check for extra `skills` roots inside the active workspace.
   - If VS Code is involved, inspect recent renderer logs for repeated discovery lines.

2. Choose the canonical source.
   - Prefer the version with the intended, complete behavior.
   - If two copies differ, keep the better or fuller one as canonical.
   - Do not symlink one version over another if the files differ materially.

3. Normalize the roots.
   - Move the canonical content into one source directory.
   - Replace duplicate directories with symlinks to that canonical source.
   - Keep special-case skills separate when they are intentionally different.

4. Verify the result.
   - Confirm each visible root resolves to the same canonical source.
   - Confirm only one content tree exists for each shared skill.
   - Reopen or recheck the app and make sure repeated scan warnings stop or drop to a single startup pass.

5. Roll back if needed.
   - Restore from backup if the wrong version was canonicalized or behavior regresses.
   - Do not leave partial replacements in place.

## Safety Rules

- Back up before moving or deleting directories.
- Never overwrite a skill that differs without checking the diff first.
- Preserve tool-specific roots only as entry points, not as separate content copies.

## References

- See [workflow.md](references/workflow.md) for the inspection, backup, replacement, verification, and rollback sequence.

## 繁體中文

- 用於整理重複的 skill 來源，保留一份 canonical source，其他路徑只保留成 symlink 或薄入口。
- 當 `~/.claude/skills`、`~/.codex/skills`、`~/.agents/skills` 或專案內的 `skills` 目錄出現重複內容時使用。
- 優先比對內容差異，再決定要合併、保留或回滾。

## 简体中文

- 用于整理重复的 skill 来源，保留一份 canonical source，其他路径只保留为 symlink 或薄入口。
- 当 `~/.claude/skills`、`~/.codex/skills`、`~/.agents/skills` 或项目内的 `skills` 目录出现重复内容时使用。
- 先比对内容差异，再决定合并、保留或回滚。
