# Skill Pattern Selection Guide

省略用語（RACI, KPI, ADR, DDL, SLO, QA, PM, TRK, EX）は [../shared-references/glossary.md](../shared-references/glossary.md) の『略語・日本語対応表』を参照してください。


新規スキルを実装する際に、既存パターンから選択して customization する手順です。

## 3つの実装パターン

### パターン A: 調査・検証型（defect-repair-unified をベース）

**特徴**:
- 問題や課題の「原因調査」「分析」 → 「複数案検討」 → 「実装」 → 「テスト・検証」のフロー
- Phase 1: 調査・分析・対応案決定
- Phase 2: 実装決定・実施（ゲート#1で案を決定）
- Phase 3: テスト・チェック（ゲート#2で項目を承認、ゲート#3で結果を承認）
- Phase 4: 報告

**対象スキル**:
- test-strategy-unified
- security-hardening
- performance-investigation

**phase1 名前**: investigation / analysis / assessment
**phase2 名前**: implementation
**phase3 名前**: testing / validation
**phase4 名前**: reporting

**特有の出力物**:
- 調査報告書（原因、該当箇所）
- チェック項目リスト
- テスト結果レポート

---

### パターン B: 意思決定・設計型（feature-implementation-unified をベース）

**特徴**:
- 要件・要求の「整理」「分析」 → 「実装方針決定」 → 「実装」 → 「検証」のフロー
- Phase 1: 要件整理・方式候補化
- Phase 2: 設計決定・実装（ゲート#1で方針を決定）
- Phase 3: 検証・受入確認（ゲート#2で検証項目を承認、ゲート#3で結果を承認）
- Phase 4: 報告

**対象スキル**:
- requirements-refinement
- architecture-decision-record
- api-contract-design
- refactoring-safety
- release-readiness

**phase1 名前**: discovery / planning / refinement
**phase2 名前**: design-implementation
**phase3 名前**: validation
**phase4 名前**: reporting

**特有の出力物**:
- 要件整理シート or ADR ドキュメント
- 複数の実装方針案（メリット/デメリット）
- 検証項目リスト
- 最終レポート

---

### パターン C: 運用手続き型（新規パターン）

**特徴**:
- ガイドライン・チェックリスト化 → 実施 → フィードバック → 改善の運用継続フロー
- Phase 1: ガイドライン準備・基準明確化
- Phase 2: 実施計画・最初の試行（ゲート#1で方針確認）
- Phase 3: フィードバック・調整・継続検証（ゲート#2で項目承認、ゲート#3で改善方針承認）
- Phase 4: 改善まとめ・次シーズンへの引き継ぎ

**対象スキル**:
- code-review-assistant
- observability-and-ops-readiness
- incident-postmortem
- documentation-sync

**phase1 名前**: preparation / guideline-definition
**phase2 名前**: execution-planning
**phase3 名前**: feedback-and-adjustment
**phase4 名前**: continuous-improvement

**特有の出力物**:
- チェックリスト or テンプレート
- 運用手順書
- 改善提案書
- lessons learned レポート

---

## パターン選択フロー

```
スキルの目的は?

  ├─ [問題/障害/脆弱性の原因を調査し、対策を試行検証したい]
  │   → **パターン A: 調査・検証型**
  │
  ├─ [新規要件を整理し、実装方針を決定してから実装したい]
  │   → **パターン B: 意思決定・設計型**
  │
  └─ [継続的な運用ガイドラインやチェックリストを確立し、改善し続けたい]
      → **パターン C: 運用手続き型**
```

---

## Customization Checklist

各パターンを選択した後、以下のカスタマイズ ポイントを埋めてください。

### 必須（全パターン共通）

- [ ] スキル名（英語 kebab-case）: `[SKILL_NAME]`
- [ ] スキル名（日本語）: `[スキル日本語名]`
- [ ] 配置カテゴリ: `[CATEGORY]`（ディレクトリで管理）
- [ ] 説明（日本語1文）: `[DESCRIPTION]`
- [ ] argument-hint（ユーザー初期入力の指示）: `[HINT]`

### Phase タイトル・段階説明

**パターン A** の場合:
- [ ] Phase 1: 調査・分析・対応案決定
  - [ ] S1-S6 の段階説明を埋める
  - [ ] Phase 1 概要説明を調査/分析に特化させる
  - [ ] phase1-investigation.md or phase1-assessment.md を作成
- [ ] Phase 2: 実装決定・実施
  - [ ] S7: 対応案決定ゲート
  - [ ] S8: AI 実装
  - [ ] S9: 開発者コードレビュー
  - [ ] phase2-implementation.md を作成
- [ ] Phase 3: テスト・チェック
  - [ ] S10: AI がチェック項目生成
  - [ ] S11: 開発者が項目承認 (ゲート#2)
  - [ ] S12: AI がテスト実施
  - [ ] S13: 開発者が結果承認 (ゲート#3)
  - [ ] phase3-testing.md を作成
- [ ] Phase 4: 報告
  - [ ] phase4-reporting.md を作成

**パターン B** の場合:
- [ ] Phase 1: 要件整理・方式候補化
  - [ ] S1-S6 の段階説明を埋める
  - [ ] S3: 開発者が要件入力
  - [ ] S4-S6: AI が分析・提案
  - [ ] phase1-discovery.md or phase1-refinement.md を作成
- [ ] Phase 2: 設計決定・実装
  - [ ] S7: 方針決定ゲート (ゲート#1)
  - [ ] S8: AI 実装
  - [ ] S9: 開発者コードレビュー
  - [ ] phase2-design-implementation.md を作成
- [ ] Phase 3: 検証・受入確認
  - [ ] S10: AI が検証項目生成
  - [ ] S11: 開発者が項目承認 (ゲート#2)
  - [ ] S12: AI が検証実施
  - [ ] S13: 開発者が結果承認 (ゲート#3)
  - [ ] phase3-validation.md を作成
- [ ] Phase 4: 報告
  - [ ] phase4-reporting.md を作成

**パターン C** の場合:
- [ ] Phase 1: ガイドライン準備・基準明確化
  - [ ] S1-S6 の段階説明をガイドラインとチェックリスト中心に埋める
  - [ ] phase1-preparation.md or phase1-guideline-definition.md を作成
- [ ] Phase 2: 実施計画・試行
  - [ ] S7: 方針確認ゲート (ゲート#1)
  - [ ] S8: 実施計画作成 or 第1回試行
  - [ ] S9: 開発者がフィードバック確認
  - [ ] phase2-execution-planning.md を作成
- [ ] Phase 3: フィードバック・調整
  - [ ] S10: AI がフィードバック収集
  - [ ] S11: 開発者が調整内容承認 (ゲート#2)
  - [ ] S12: AI が改善適用
  - [ ] S13: 開発者が次フェーズ方針承認 (ゲート#3)
  - [ ] phase3-feedback-and-adjustment.md を作成
- [ ] Phase 4: 改善まとめ
  - [ ] phase4-continuous-improvement.md を作成

### 実行モード説明

- [ ] strict モード: [STRICT 説明]
- [ ] speed モード: [SPEED 説明]
- [ ] balance モード: [BALANCE 説明]

### ゲート条件

- [ ] ゲート#1（段階7）: 判定条件 A, B, C を埋める
- [ ] ゲート#2（段階11）: 判定条件 A, B, C を埋める
- [ ] ゲート#3（段階13）: 判定条件 A, B, C を埋める

### リファレンス・アセット

- [ ] runbook.md または remediation-runbook.md を作成（詳細手順）
- [ ] assets/[SKILL_NAME]-log-template.md を作成（実行ログテンプレート）
- [ ] references/ 配下の共通ファイルを確認:
  - [ ] flowchart-best-practices.md
  - [ ] investigation-checklist.md
  - [ ] testcase-template.md

---

## Customization 例: security-hardening（パターン A）

```yaml
name: security-hardening
description: セキュリティ観点の設計・実装・検証を統合的に進め、脆弱性混入を抑制する。
argument-hint: 対象スコープ（モジュール名、実装パターン等）とセキュリティ懸念事項を記述してください。

# Phase 1 のカスタマイズ
Phase_1_Title: セキュリティ分析・対策案決定
S1: 準備（セキュリティ要件・脅威モデル収集）
S4: AI が脅威パターン（OWASP等）で分析
S5: AI が脅威とリスク評価のマトリクス生成
S6: AI が複数の対策案（インパクト/実装難度）を提示

# ゲート#1 のカスタマイズ
Gate_1_Condition_A: 脅威分析で重要な脅威が特定されているか
Gate_1_Condition_B: 複数の対策案が提示されプリオライズされているか
Gate_1_Condition_C: 対策案が既知のセキュリティベストプラクティスと合致しているか
```

---

## 実装ステップ（段階的）

1. **パターン選択**: 上記フロー図から該当パターンを選ぶ
2. **テンプレートコピー**: SKILL.md を新規スキルディレクトリにコピー
3. **Customization Checklist** の必須項目を埋める
4. **Phase ごとの sub-skills** ファイルを作成（phase1-*.md ~ phase4-*.md）
5. **ゲート条件** を明確に記述
6. **参照ファイル**（runbook, assets, references）を作成
7. **本 README.md**（カテゴリREADME）に新規スキルを追加

---

## 注意事項

- **Phase・段階の順序は固定**: 全スキルで Phase 1～4、段階1～14の構造を保つ
- **ゲート条件は3つ必須**: 段階7, 11, 13 は変更不可（運用一貫性のため）
- **sub-skills ファイル**: phase1-*.md, phase2-*.md, phase3-*.md, phase4-*.md の4ファイルは必須
- **配置カテゴリ**: category は frontmatter に書かず、親ディレクトリで管理する
- **argument-hint**: ユーザーが何を入力すべきか明確に（日本語推奨）

---

## Quality Assurance Checklist

各スキル実装後、以下を確認してください:

- [ ] SKILL.md の frontmatter が正しい（name, description, argument-hint, user-invocable, disable-model-invocation）
- [ ] Phase 1～4 + ゲート条件#1-3 がすべて定義されている
- [ ] sub-skills/ に phase1-*.md, phase2-*.md, phase3-*.md, phase4-*.md が存在する
- [ ] references/, assets/ が最低限整備されている（flowchart, checklist, template, log-template）
- [ ] 運用ルール セクションが記述されている
- [ ] 完了条件が明確である
- [ ] runbook.md または remediation-runbook.md が作成されている

---

**作成日**: 2026-03-28  
**参照パターン数**: 3 (A: 調査・検証型, B: 意思決定・設計型, C: 運用手続き型)
