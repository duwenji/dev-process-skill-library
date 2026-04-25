---
title: 新規機能・機能変更対応 Skill - 詳細手順書（Runbook）
category: feature-implementation-unified
version: 1.0
created: 2026-04-25
---

## 新規機能・機能変更対応 Runbook（詳細手順・判定基準）

この Runbook は `feature-implementation-unified` の実行正本です。段階4以降の具体実施手順と、判定時に確認すべき基準を定義します。

## 参照優先順位

```text
実装ファイル（csproj/DDL/ログ等） > 本Runbook > SKILL.md > 実行ログ
```

SKILL.md と本 Runbook の記載が不一致の場合は、本 Runbook を正とします。

## Phase 別の参照入口

- Phase 1（要求整理・方式候補化）: `sub-skills/phase1-discovery.md`
- Phase 2（設計決定・実装）: `sub-skills/phase2-design-implementation.md`
- Phase 3（検証・受入確認）: `sub-skills/phase3-validation.md`
- Phase 4（報告）: `sub-skills/phase4-reporting.md`

## 段階4の実施方針（現行調査・差分分析）

1. 変更対象の実装実体を特定する（コード、設定、DDL、関連ログ）。
2. 要求内容と現行実装の差分を、機能・非機能の両面で明文化する。
3. 影響範囲を呼び出し元/呼び出し先で確認し、回帰対象を抽出する。
4. 調査結果をログに残し、段階5の方式案作成へ引き渡す。

## ゲート判定での確認観点

### ゲート #1（段階7）

- 方式案が複数提示されているか。
- 各案のメリット/デメリットが受入条件に紐づいているか。
- 選定案の根拠が明文化されているか。

### ゲート #2（段階11）

- 検証項目が改修点を網羅しているか。
- 正常系/異常系/境界値/回帰が明示されているか。
- 実行可能な検証手順になっているか。

### ゲート #3（段階13）

- 結果が項目単位で記録されているか。
- 不合格項目に対する対処方針があるか。
- 受入可否の判定根拠が明確か。
