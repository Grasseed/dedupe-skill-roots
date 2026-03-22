# Dedupe Skill Roots

Consolidate duplicate skill discovery roots into one canonical source.

This repository contains a Codex skill for:

- finding duplicate skill roots
- choosing one canonical source
- replacing duplicate trees with symlinks
- verifying that repeated scans stop
- rolling back safely if needed

## When to use

Use this skill when Codex, VS Code, Claude Code, or related tools keep scanning the same skills repeatedly, or when multiple `skills` roots contain the same content.

## Typical workflow

1. Inspect the current roots.
2. Choose the canonical source.
3. Normalize duplicate roots.
4. Verify the result.
5. Roll back if needed.

## 中文說明

這是一個用來整理重複 skills root 的 Codex skill。

它適合處理以下情況：

- 找出重複的 skill 根目錄
- 選出單一 canonical source
- 把重複目錄改成 symlink
- 驗證是否還有重複掃描
- 必要時安全回滾

## 何時使用

當 Codex、VS Code、Claude Code 或相關工具反覆掃描同一批 skills，或多個 `skills` root 內容重複時使用。

## 基本流程

1. 盤點目前的 root。
2. 選定 canonical source。
3. 收斂重複 root。
4. 驗證結果。
5. 需要時回滾。

## Skill files

- `SKILL.md` - primary instructions for Codex
- `references/workflow.md` - inspection and rollback workflow
- `agents/openai.yaml` - UI metadata

