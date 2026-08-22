# 成果物文書テンプレート体系化 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 15スキルそれぞれが生成する成果物文書（要件定義書、設計仕様書、テスト戦略書等）を、共通スケルトン＋種別固有詳細章の2層構造テンプレートとして体系化し、文書ID/対応元IDによるトレーサビリティを持たせる。

**Architecture:** `skills/shared-templates/document-templates/` に15個の成果物文書テンプレートと索引READMEを新設する。`skills/shared-references/traceability-id-convention.md` にIDプレフィックス規約を定義する。既存15スキルそれぞれの `SKILL.md` に、テンプレートへの参照リンク（入力リファレンス）と完了条件を追記する。段階番号・ゲート・mermaid図・sub-skills/runbook/assetsは一切変更しない。

**Tech Stack:** Markdown のみ（コード・テストランナーなし）。検証は `grep` によるセクション見出し存在確認とリンク解決確認で行う。

**Spec:** [docs/superpowers/specs/2026-08-22-deliverable-document-templates-design.md](../specs/2026-08-22-deliverable-document-templates-design.md)

## Global Constraints

- 段階番号（段階1〜14）・ゲート（段階7,11,13）・mermaid図は変更しない
- `assets/*-log-template.md`（実行ログ）は変更しない。役割は「実行記録」のまま維持する
- 新設テンプレートは共通スケルトン（文書情報／目的・背景／スコープ・非スコープ／対応元ID／方針・決定事項／種別固有詳細章／制約／未決事項・リスク／関連ドキュメント・参照リンク／変更履歴）を全15種で統一する
- IDプレフィックスは `REQ, DES, DM, ADR, API, REV, RFC, DEF, SEC, TS, OPS, PERF, REL, DOC, PMR` を厳守する（`PM` は既存glossaryで「プロジェクトマネージャー」の意味のため使わない）
- 各SKILL.mdへの追記は「入力リファレンス」1行と「完了条件」1行のみ。既存の文言・箇条書き・リンクは削除・改変しない

---

## Task 1: トレーサビリティID規約 + glossary更新

**Files:**
- Create: `skills/shared-references/traceability-id-convention.md`
- Modify: `skills/shared-references/glossary.md`

**Interfaces:**
- Produces: 15種類のIDプレフィックス規約（後続タスクの全テンプレートが参照する）

- [ ] **Step 1: `traceability-id-convention.md` を作成する**

```markdown
# 工程間トレーサビリティ ID規約

`shared-templates/document-templates/` 配下の成果物文書は、自身の「文書ID」を持ち、下流文書は
「対応元ID（トレーサビリティ）」セクションで上流の文書IDを逆参照する。中心集約のマトリクス文書は
作らず、各文書が上流への逆リンクを持つ形に留める。

## ID形式

`<プレフィックス>-<3桁連番>`（例: `REQ-001`）。連番はプロジェクト内・文書種別ごとに独立して振る。

## プレフィックス一覧

| プレフィックス | 文書種別 | 対応Skill |
|---|---|---|
| REQ | 要件定義書 | 010_requirements-refinement |
| DES | 機能設計・実装方針書 | 020_feature-implementation-unified |
| DM | データモデル設計書 | 020_data-model-design-unified |
| ADR | ADR（アーキテクチャ決定記録） | 020_architecture-decision-record |
| API | API契約書 | 020_api-contract-design |
| REV | コードレビュー実施基準書 | 020_code-review-assistant |
| RFC | リファクタリング計画書 | 020_refactoring-safety |
| DEF | 不具合調査・対応報告書 | 030_defect-repair-unified |
| SEC | セキュリティ対策方針書 | 030_security-hardening |
| TS | テスト戦略書 | 030_test-strategy-unified |
| OPS | 運用設計書 | 040_observability-and-ops-readiness |
| PERF | 性能調査・計測報告書 | 040_performance-investigation |
| REL | リリース計画書 | 040_release-readiness |
| DOC | ドキュメント更新計画書 | 050_documentation-sync |
| PMR | ポストモーテム報告書 | 050_incident-postmortem |

`PM` は [glossary.md](./glossary.md) で「プロジェクトマネージャー」として既に使われているため、
ポストモーテム報告書には `PMR` を用いる。

## 使い方

1. 文書作成時、文書情報の「文書ID」欄に `<プレフィックス>-<連番>` を採番する
2. 上流の成果物（要件定義書等）を参照する場合、「対応元ID（トレーサビリティ）」セクションに
   上流の文書IDを記載する
3. 最上流の文書（要件定義書など、本ライブラリ管理外の上位文書しか持たない文書）は
   「該当なし（最上流）」と記載する
4. 中心集約のトレーサビリティマトリクスは作成しない。追跡は各文書の逆リンクを辿って行う
```

- [ ] **Step 2: `glossary.md` に規約への相互参照を追加する**

`skills/shared-references/glossary.md` を読み、「略語・日本語対応表」の表の直後（表が終わる空行の後）に以下を追記する。

```markdown
### 成果物文書トレーサビリティID

`REQ, DES, DM, ADR, API, REV, RFC, DEF, SEC, TS, OPS, PERF, REL, DOC, PMR` は成果物文書の
トレーサビリティID規約です。詳細は [traceability-id-convention.md](./traceability-id-convention.md)
を参照してください。`PM`（プロジェクトマネージャー）とは異なる用法です。
```

- [ ] **Step 3: 検証する**

Run: `grep -c "^|" skills/shared-references/traceability-id-convention.md`
Expected: `17`（ヘッダ行1 + セパレータ行1 + プレフィックス15行）

Run: `grep -n "traceability-id-convention" skills/shared-references/glossary.md`
Expected: 1行以上ヒットする

- [ ] **Step 4: コミット**

```bash
git add skills/shared-references/traceability-id-convention.md skills/shared-references/glossary.md
git commit -m "docs: 成果物文書トレーサビリティIDの規約を新設"
```

---

## Task 2: 成果物文書テンプレート 索引README（共通スケルトン定義）

**Files:**
- Create: `skills/shared-templates/document-templates/README.md`

**Interfaces:**
- Consumes: Task 1の `traceability-id-convention.md`
- Produces: 全15テンプレートが従う共通スケルトンの正本

- [ ] **Step 1: `README.md` を作成する**

```markdown
# 成果物文書テンプレート

各Skillが生成する成果物文書（要件定義書、設計仕様書、テスト戦略書 等）の型を定義する。
`assets/*-log-template.md`（Skill実行ログ・振り返り用）とは役割が異なり、こちらは
**対象プロジェクトの docs/ 配下等に配置される成果物文書そのもの**の型である。

IDプレフィックス規約は [../../shared-references/traceability-id-convention.md](../../shared-references/traceability-id-convention.md) を参照。

## 共通スケルトン

全テンプレートは以下の章立てを共通で持つ。種別固有の詳細章は「方針・決定事項」の直後に挿入する。

```markdown
## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | |
| ドキュメント種別 | |
| 対象システム/機能 | |
| 関連Skill | |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

## 方針・決定事項

<!-- 種別固有の詳細章はここに挿入 -->

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

## 索引（15種類）

| # | ファイル | 文書種別 | IDプレフィックス | 対応Skill |
|---|---|---|---|---|
| 1 | `requirements-definition-document-template.md` | 要件定義書 | REQ | 010_requirements-refinement |
| 2 | `feature-design-document-template.md` | 機能設計・実装方針書 | DES | 020_feature-implementation-unified |
| 3 | `data-model-design-document-template.md` | データモデル設計書 | DM | 020_data-model-design-unified |
| 4 | `adr-template.md` | ADR | ADR | 020_architecture-decision-record |
| 5 | `api-contract-document-template.md` | API契約書 | API | 020_api-contract-design |
| 6 | `code-review-standard-document-template.md` | コードレビュー実施基準書 | REV | 020_code-review-assistant |
| 7 | `refactoring-plan-document-template.md` | リファクタリング計画書 | RFC | 020_refactoring-safety |
| 8 | `defect-investigation-report-template.md` | 不具合調査・対応報告書 | DEF | 030_defect-repair-unified |
| 9 | `security-hardening-policy-template.md` | セキュリティ対策方針書 | SEC | 030_security-hardening |
| 10 | `test-strategy-document-template.md` | テスト戦略書 | TS | 030_test-strategy-unified |
| 11 | `operability-design-document-template.md` | 運用設計書 | OPS | 040_observability-and-ops-readiness |
| 12 | `performance-investigation-report-template.md` | 性能調査・計測報告書 | PERF | 040_performance-investigation |
| 13 | `release-plan-document-template.md` | リリース計画書 | REL | 040_release-readiness |
| 14 | `documentation-update-plan-template.md` | ドキュメント更新計画書 | DOC | 050_documentation-sync |
| 15 | `postmortem-report-template.md` | ポストモーテム報告書 | PMR | 050_incident-postmortem |

<!-- リンク化はTask18で全ファイル作成後に行う -->
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^| [0-9]* | \`" skills/shared-templates/document-templates/README.md`
Expected: `15`

- [ ] **Step 3: コミット**

```bash
git add skills/shared-templates/document-templates/README.md
git commit -m "docs: 成果物文書テンプレートの索引READMEと共通スケルトンを新設"
```

---

## Task 3: 要件定義書テンプレート + 010_requirements-refinement 配線

**Files:**
- Create: `skills/shared-templates/document-templates/requirements-definition-document-template.md`
- Modify: `skills/010_requirements-and-planning/010_requirements-refinement/SKILL.md`

**Interfaces:**
- Consumes: Task 1 (`traceability-id-convention.md`), Task 2 (共通スケルトン)

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# 要件定義書テンプレート

対象Skill: [010_requirements-refinement](../../010_requirements-and-planning/010_requirements-refinement/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `REQ-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | REQ- |
| ドキュメント種別 | 要件定義書 |
| 対象システム/機能 | |
| 関連Skill | 010_requirements-refinement |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

要件定義書は本ライブラリの最上流の成果物文書のため、通常は「該当なし（最上流）」と記載する。
上位計画書（本ライブラリ管理外）がある場合はそのID・名称を記載する。

## 方針・決定事項

## 機能要件
| 要件ID | 内容 | 優先度 |
|---|---|---|

## 非機能要件
| 区分 | 内容 |
|---|---|
| 性能 | |
| セキュリティ | |
| 可用性 | |

## 受入条件
1.

## 用語定義
| 用語 | 定義 |
|---|---|

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: テンプレートの必須見出しを検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/requirements-definition-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

`skills/010_requirements-and-planning/010_requirements-refinement/SKILL.md` の以下の行を検索する。

Old:
```
- 記録テンプレート: assets/requirements-refinement-log-template.md
```

New:
```
- 記録テンプレート: assets/requirements-refinement-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/requirements-definition-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `REQ-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が requirements-definition-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 配線を検証する**

Run: `grep -n "requirements-definition-document-template" skills/010_requirements-and-planning/010_requirements-refinement/SKILL.md`
Expected: 2行ヒット（入力リファレンス行、完了条件行）

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/requirements-definition-document-template.md skills/010_requirements-and-planning/010_requirements-refinement/SKILL.md
git commit -m "docs: 要件定義書テンプレートを新設しrequirements-refinementに配線"
```

---

## Task 4: 機能設計・実装方針書テンプレート + 020_feature-implementation-unified 配線

**Files:**
- Create: `skills/shared-templates/document-templates/feature-design-document-template.md`
- Modify: `skills/020_design-and-implementation/050_feature-implementation-unified/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# 機能設計・実装方針書テンプレート

対象Skill: [050_feature-implementation-unified](../../020_design-and-implementation/050_feature-implementation-unified/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `DES-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | DES- |
| ドキュメント種別 | 機能設計・実装方針書 |
| 対象システム/機能 | |
| 関連Skill | 020_feature-implementation-unified |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書のIDを記載する（例: REQ-001）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 現行仕様との差分分析

## 方式案比較
| 案 | 概要 | メリット | デメリット | 採否 |
|---|---|---|---|---|

## 実装方針

## 変更ファイル一覧・実装根拠
| ファイル | 変更内容 | 根拠 |
|---|---|---|

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/feature-design-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/implementation-log-template.md
```

New:
```
- 記録テンプレート: assets/implementation-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/feature-design-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `DES-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が feature-design-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
- 対応元IDが上流の要件定義書のIDで埋まっている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "feature-design-document-template" skills/020_design-and-implementation/050_feature-implementation-unified/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/feature-design-document-template.md skills/020_design-and-implementation/050_feature-implementation-unified/SKILL.md
git commit -m "docs: 機能設計・実装方針書テンプレートを新設しfeature-implementation-unifiedに配線"
```

---

## Task 5: データモデル設計書テンプレート + 020_data-model-design-unified 配線

**Files:**
- Create: `skills/shared-templates/document-templates/data-model-design-document-template.md`
- Modify: `skills/020_design-and-implementation/040_data-model-design-unified/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# データモデル設計書テンプレート

対象Skill: [040_data-model-design-unified](../../020_design-and-implementation/040_data-model-design-unified/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `DM-xxx`）
関連ガイド: [erd-best-practices.md](../../shared-references/erd-best-practices.md), [data-dictionary-template.md](../data-dictionary-template.md)

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | DM- |
| ドキュメント種別 | データモデル設計書 |
| 対象システム/機能 | |
| 関連Skill | 020_data-model-design-unified |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書のIDを記載する（例: REQ-001）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 概念モデル（ERD）
参照: erd-best-practices.md の手順で作成した ERD へのリンクまたは埋め込み

## 論理モデル

## 物理モデル

## データ辞書
参照: data-dictionary-template.md への記入結果へのリンク

## DDL方針

## 移行・互換性メモ

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/data-model-design-document-template.md`
Expected: `15`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/model-design-log-template.md
```

New:
```
- 記録テンプレート: assets/model-design-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/data-model-design-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `DM-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が data-model-design-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "data-model-design-document-template" skills/020_design-and-implementation/040_data-model-design-unified/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/data-model-design-document-template.md skills/020_design-and-implementation/040_data-model-design-unified/SKILL.md
git commit -m "docs: データモデル設計書テンプレートを新設しdata-model-design-unifiedに配線"
```

---

## Task 6: ADRテンプレート + 020_architecture-decision-record 配線

**Files:**
- Create: `skills/shared-templates/document-templates/adr-template.md`
- Modify: `skills/020_design-and-implementation/020_architecture-decision-record/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# ADR（アーキテクチャ決定記録）テンプレート

対象Skill: [020_architecture-decision-record](../../020_design-and-implementation/020_architecture-decision-record/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `ADR-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | ADR- |
| ドキュメント種別 | ADR |
| 対象システム/機能 | |
| 関連Skill | 020_architecture-decision-record |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 判断テーマ

## 検討した代替案と比較軸
| 案 | 比較軸 | 評価 |
|---|---|---|

## 採用案と採否理由

## 影響範囲・追従タスク

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/adr-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/architecture-decision-record-log-template.md
```

New:
```
- 記録テンプレート: assets/architecture-decision-record-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/adr-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `ADR-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が adr-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "adr-template" skills/020_design-and-implementation/020_architecture-decision-record/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/adr-template.md skills/020_design-and-implementation/020_architecture-decision-record/SKILL.md
git commit -m "docs: ADRテンプレートを新設しarchitecture-decision-recordに配線"
```

---

## Task 7: API契約書テンプレート + 020_api-contract-design 配線

**Files:**
- Create: `skills/shared-templates/document-templates/api-contract-document-template.md`
- Modify: `skills/020_design-and-implementation/010_api-contract-design/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# API契約書テンプレート

対象Skill: [010_api-contract-design](../../020_design-and-implementation/010_api-contract-design/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `API-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | API- |
| ドキュメント種別 | API契約書 |
| 対象システム/機能 | |
| 関連Skill | 020_api-contract-design |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## エンドポイント一覧
| メソッド | パス | 概要 |
|---|---|---|

## リクエスト・レスポンス仕様

## エラー表
| エラーコード | 発生条件 | レスポンス内容 |
|---|---|---|

## 互換性方針・移行メモ

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/api-contract-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/api-contract-design-log-template.md
```

New:
```
- 記録テンプレート: assets/api-contract-design-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/api-contract-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `API-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が api-contract-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "api-contract-document-template" skills/020_design-and-implementation/010_api-contract-design/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/api-contract-document-template.md skills/020_design-and-implementation/010_api-contract-design/SKILL.md
git commit -m "docs: API契約書テンプレートを新設しapi-contract-designに配線"
```

---

## Task 8: コードレビュー実施基準書テンプレート + 020_code-review-assistant 配線

**Files:**
- Create: `skills/shared-templates/document-templates/code-review-standard-document-template.md`
- Modify: `skills/020_design-and-implementation/030_code-review-assistant/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# コードレビュー実施基準書テンプレート

対象Skill: [030_code-review-assistant](../../020_design-and-implementation/030_code-review-assistant/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `REV-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | REV- |
| ドキュメント種別 | コードレビュー実施基準書 |
| 対象システム/機能 | |
| 関連Skill | 020_code-review-assistant |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する機能設計・実装方針書のIDを記載する（例: DES-001）。プロジェクト共通基準の場合は「該当なし（共通基準）」と記載する。

## 方針・決定事項

## レビュー観点一覧
| 観点 | 確認内容 |
|---|---|

## 指摘分類基準
| 分類 | 基準 | 対応要否 |
|---|---|---|

## 確認手順

## 改善候補リスト

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/code-review-standard-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/code-review-assistant-log-template.md
```

New:
```
- 記録テンプレート: assets/code-review-assistant-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/code-review-standard-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `REV-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が code-review-standard-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "code-review-standard-document-template" skills/020_design-and-implementation/030_code-review-assistant/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/code-review-standard-document-template.md skills/020_design-and-implementation/030_code-review-assistant/SKILL.md
git commit -m "docs: コードレビュー実施基準書テンプレートを新設しcode-review-assistantに配線"
```

---

## Task 9: リファクタリング計画書テンプレート + 020_refactoring-safety 配線

**Files:**
- Create: `skills/shared-templates/document-templates/refactoring-plan-document-template.md`
- Modify: `skills/020_design-and-implementation/060_refactoring-safety/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# リファクタリング計画書テンプレート

対象Skill: [060_refactoring-safety](../../020_design-and-implementation/060_refactoring-safety/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `RFC-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | RFC- |
| ドキュメント種別 | リファクタリング計画書 |
| 対象システム/機能 | |
| 関連Skill | 020_refactoring-safety |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する機能設計・実装方針書または不具合調査・対応報告書のIDを記載する（例: DES-001, DEF-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 対象範囲・分割単位

## 変更順序と確認方法

## 保証観点（回帰確認範囲）

## 差分リスクと戻し方

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/refactoring-plan-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/refactoring-safety-log-template.md
```

New:
```
- 記録テンプレート: assets/refactoring-safety-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/refactoring-plan-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `RFC-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が refactoring-plan-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "refactoring-plan-document-template" skills/020_design-and-implementation/060_refactoring-safety/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/refactoring-plan-document-template.md skills/020_design-and-implementation/060_refactoring-safety/SKILL.md
git commit -m "docs: リファクタリング計画書テンプレートを新設しrefactoring-safetyに配線"
```

---

## Task 10: 不具合調査・対応報告書テンプレート + 030_defect-repair-unified 配線

**Files:**
- Create: `skills/shared-templates/document-templates/defect-investigation-report-template.md`
- Modify: `skills/030_verification-and-quality/010_defect-repair-unified/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# 不具合調査・対応報告書テンプレート

対象Skill: [010_defect-repair-unified](../../030_verification-and-quality/010_defect-repair-unified/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `DEF-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | DEF- |
| ドキュメント種別 | 不具合調査・対応報告書 |
| 対象システム/機能 | |
| 関連Skill | 030_defect-repair-unified |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 事象・再現手順

## 原因調査結果

## 対応方針と実装内容

## 検証結果（正常系・境界値・異常系・回帰）
| 区分 | ケース | 結果 |
|---|---|---|

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/defect-investigation-report-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/defect-log-template.md
```

New:
```
- 記録テンプレート: assets/defect-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/defect-investigation-report-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `DEF-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 決定・判定根拠がすべて追跡可能である
```

New:
```
- 決定・判定根拠がすべて追跡可能である
- 成果物文書が defect-investigation-report-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "defect-investigation-report-template" skills/030_verification-and-quality/010_defect-repair-unified/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/defect-investigation-report-template.md skills/030_verification-and-quality/010_defect-repair-unified/SKILL.md
git commit -m "docs: 不具合調査・対応報告書テンプレートを新設しdefect-repair-unifiedに配線"
```

---

## Task 11: セキュリティ対策方針書テンプレート + 030_security-hardening 配線

**Files:**
- Create: `skills/shared-templates/document-templates/security-hardening-policy-template.md`
- Modify: `skills/030_verification-and-quality/020_security-hardening/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# セキュリティ対策方針書テンプレート

対象Skill: [020_security-hardening](../../030_verification-and-quality/020_security-hardening/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `SEC-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | SEC- |
| ドキュメント種別 | セキュリティ対策方針書 |
| 対象システム/機能 | |
| 関連Skill | 030_security-hardening |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 資産・脅威一覧
| 資産 | 脅威 |
|---|---|

## 脅威分析

## 対策方針と適用範囲

## 残余リスク台帳
| リスク | 影響度 | 対応方針 |
|---|---|---|

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/security-hardening-policy-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/security-hardening-log-template.md
```

New:
```
- 記録テンプレート: assets/security-hardening-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/security-hardening-policy-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `SEC-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が security-hardening-policy-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "security-hardening-policy-template" skills/030_verification-and-quality/020_security-hardening/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/security-hardening-policy-template.md skills/030_verification-and-quality/020_security-hardening/SKILL.md
git commit -m "docs: セキュリティ対策方針書テンプレートを新設しsecurity-hardeningに配線"
```

---

## Task 12: テスト戦略書テンプレート + 030_test-strategy-unified 配線

**Files:**
- Create: `skills/shared-templates/document-templates/test-strategy-document-template.md`
- Modify: `skills/030_verification-and-quality/030_test-strategy-unified/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# テスト戦略書テンプレート

対象Skill: [030_test-strategy-unified](../../030_verification-and-quality/030_test-strategy-unified/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `TS-xxx`）
関連テンプレート: [testcase-template.md](../../shared-references/testcase-template.md), [test-case-csv-template.md](../test-case-csv-template.md)

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | TS- |
| ドキュメント種別 | テスト戦略書 |
| 対象システム/機能 | |
| 関連Skill | 030_test-strategy-unified |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 影響分析・既存テスト棚卸し

## テスト層別方針（単体/結合/受入等）
| 層 | 方針 | 担当 |
|---|---|---|

## 優先順位表
| 項目 | 優先度 | 理由 |
|---|---|---|

## 実行チェックリスト
参照: testcase-template.md / test-case-csv-template.md への記入結果へのリンク

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/test-strategy-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/test-strategy-log-template.md
```

New:
```
- 記録テンプレート: assets/test-strategy-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/test-strategy-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `TS-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が test-strategy-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "test-strategy-document-template" skills/030_verification-and-quality/030_test-strategy-unified/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/test-strategy-document-template.md skills/030_verification-and-quality/030_test-strategy-unified/SKILL.md
git commit -m "docs: テスト戦略書テンプレートを新設しtest-strategy-unifiedに配線"
```

---

## Task 13: 運用設計書テンプレート + 040_observability-and-ops-readiness 配線

**Files:**
- Create: `skills/shared-templates/document-templates/operability-design-document-template.md`
- Modify: `skills/040_operations-and-release/010_observability-and-ops-readiness/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# 運用設計書テンプレート

対象Skill: [010_observability-and-ops-readiness](../../040_operations-and-release/010_observability-and-ops-readiness/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `OPS-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | OPS- |
| ドキュメント種別 | 運用設計書 |
| 対象システム/機能 | |
| 関連Skill | 040_observability-and-ops-readiness |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 観測性現状評価と課題

## 監視・アラート設計
| 監視項目 | 閾値 | アラート先 |
|---|---|---|

## 初動手順

## 引き継ぎ事項

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/operability-design-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/observability-and-ops-readiness-log-template.md
```

New:
```
- 記録テンプレート: assets/observability-and-ops-readiness-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/operability-design-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `OPS-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで引き継ぎ可能な状態である
```

New:
```
- 最終報告書が作成済みで引き継ぎ可能な状態である
- 成果物文書が operability-design-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "operability-design-document-template" skills/040_operations-and-release/010_observability-and-ops-readiness/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/operability-design-document-template.md skills/040_operations-and-release/010_observability-and-ops-readiness/SKILL.md
git commit -m "docs: 運用設計書テンプレートを新設しobservability-and-ops-readinessに配線"
```

---

## Task 14: 性能調査・計測報告書テンプレート + 040_performance-investigation 配線

**Files:**
- Create: `skills/shared-templates/document-templates/performance-investigation-report-template.md`
- Modify: `skills/040_operations-and-release/020_performance-investigation/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# 性能調査・計測報告書テンプレート

対象Skill: [020_performance-investigation](../../040_operations-and-release/020_performance-investigation/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `PERF-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | PERF- |
| ドキュメント種別 | 性能調査・計測報告書 |
| 対象システム/機能 | |
| 関連Skill | 040_performance-investigation |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する要件定義書・機能設計書のIDを記載する（例: REQ-001, DES-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 症状整理・ボトルネック仮説

## 計測計画と条件
| 項目 | 条件 |
|---|---|

## 比較結果
| 指標 | 改善前 | 改善後 |
|---|---|---|

## 改善提案

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/performance-investigation-report-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/performance-investigation-log-template.md
```

New:
```
- 記録テンプレート: assets/performance-investigation-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/performance-investigation-report-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `PERF-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、改善根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、改善根拠が追跡可能
- 成果物文書が performance-investigation-report-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "performance-investigation-report-template" skills/040_operations-and-release/020_performance-investigation/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/performance-investigation-report-template.md skills/040_operations-and-release/020_performance-investigation/SKILL.md
git commit -m "docs: 性能調査・計測報告書テンプレートを新設しperformance-investigationに配線"
```

---

## Task 15: リリース計画書テンプレート + 040_release-readiness 配線

**Files:**
- Create: `skills/shared-templates/document-templates/release-plan-document-template.md`
- Modify: `skills/040_operations-and-release/030_release-readiness/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# リリース計画書テンプレート

対象Skill: [030_release-readiness](../../040_operations-and-release/030_release-readiness/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `REL-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | REL- |
| ドキュメント種別 | リリース計画書 |
| 対象システム/機能 | |
| 関連Skill | 040_release-readiness |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する機能設計・実装方針書・不具合調査・対応報告書のIDを記載する（例: DES-001, DEF-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## リリース条件・依存関係

## 投入手順とロールバック手順

## 周知事項

## 最終確認結果
| 項目 | 結果 |
|---|---|

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/release-plan-document-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/release-readiness-log-template.md
```

New:
```
- 記録テンプレート: assets/release-readiness-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/release-plan-document-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `REL-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
```

New:
```
- 最終報告書が作成済みで、判定根拠が追跡可能
- 成果物文書が release-plan-document-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "release-plan-document-template" skills/040_operations-and-release/030_release-readiness/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/release-plan-document-template.md skills/040_operations-and-release/030_release-readiness/SKILL.md
git commit -m "docs: リリース計画書テンプレートを新設しrelease-readinessに配線"
```

---

## Task 16: ドキュメント更新計画書テンプレート + 050_documentation-sync 配線

**Files:**
- Create: `skills/shared-templates/document-templates/documentation-update-plan-template.md`
- Modify: `skills/050_learning-and-improvement/010_documentation-sync/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# ドキュメント更新計画書テンプレート

対象Skill: [010_documentation-sync](../../050_learning-and-improvement/010_documentation-sync/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `DOC-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | DOC- |
| ドキュメント種別 | ドキュメント更新計画書 |
| 対象システム/機能 | |
| 関連Skill | 050_documentation-sync |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

更新の起点となった変更（機能設計書・不具合報告書等）のIDを記載する（例: DES-001, DEF-002）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 影響文書一覧
| 文書 | 影響内容 |
|---|---|

## 更新対象分析

## 更新順序・計画

## 未更新リスク

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/documentation-update-plan-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/documentation-sync-log-template.md
```

New:
```
- 記録テンプレート: assets/documentation-sync-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/documentation-update-plan-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `DOC-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が作成済みで、読者への影響が説明可能
```

New:
```
- 最終報告書が作成済みで、読者への影響が説明可能
- 成果物文書が documentation-update-plan-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "documentation-update-plan-template" skills/050_learning-and-improvement/010_documentation-sync/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/documentation-update-plan-template.md skills/050_learning-and-improvement/010_documentation-sync/SKILL.md
git commit -m "docs: ドキュメント更新計画書テンプレートを新設しdocumentation-syncに配線"
```

---

## Task 17: ポストモーテム報告書テンプレート + 050_incident-postmortem 配線

**Files:**
- Create: `skills/shared-templates/document-templates/postmortem-report-template.md`
- Modify: `skills/050_learning-and-improvement/020_incident-postmortem/SKILL.md`

- [ ] **Step 1: テンプレートファイルを作成する**

```markdown
# ポストモーテム報告書テンプレート

対象Skill: [020_incident-postmortem](../../050_learning-and-improvement/020_incident-postmortem/SKILL.md)
IDプレフィックス規約: [traceability-id-convention.md](../../shared-references/traceability-id-convention.md)（本文書は `PMR-xxx`）

## 文書情報
| 項目 | 内容 |
|---|---|
| 文書ID | PMR- |
| ドキュメント種別 | ポストモーテム報告書 |
| 対象システム/機能 | |
| 関連Skill | 050_incident-postmortem |
| 作成日 | |
| 作成者 | |
| 承認者 | |
| ステータス | 未承認 / 承認済 |
| 版 | |

## 目的・背景

## スコープ・非スコープ

## 対応元ID（トレーサビリティ）
| 対応元ID | 内容 | 対応状況 |
|---|---|---|

対応する不具合調査・対応報告書のIDを記載する（例: DEF-001）。存在しない場合は「該当なし」と記載する。

## 方針・決定事項

## 事実整理・時系列

## 原因分析（仮説と検証）

## 再発防止アクション
| アクション | 担当 | 期限 |
|---|---|---|

## 共有事項

## 制約

## 未決事項・リスク

## 関連ドキュメント・参照リンク

## 変更履歴
| 日付 | 版 | 変更内容 | 変更者 |
|---|---|---|---|
```

- [ ] **Step 2: 検証する**

Run: `grep -c "^## " skills/shared-templates/document-templates/postmortem-report-template.md`
Expected: `13`

- [ ] **Step 3: SKILL.md の入力リファレンスに追記する**

Old:
```
- 記録テンプレート: assets/incident-postmortem-log-template.md
```

New:
```
- 記録テンプレート: assets/incident-postmortem-log-template.md
- 成果物文書テンプレート: ../../shared-templates/document-templates/postmortem-report-template.md
- トレーサビリティID規約: ../../shared-references/traceability-id-convention.md（本Skillは `PMR-xxx`）
```

- [ ] **Step 4: SKILL.md の完了条件に追記する**

Old:
```
- 最終報告書が組織学習として共有可能な形になっている
```

New:
```
- 最終報告書が組織学習として共有可能な形になっている
- 成果物文書が postmortem-report-template.md の必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 5: 検証する**

Run: `grep -n "postmortem-report-template" skills/050_learning-and-improvement/020_incident-postmortem/SKILL.md`
Expected: 2行ヒット

- [ ] **Step 6: コミット**

```bash
git add skills/shared-templates/document-templates/postmortem-report-template.md skills/050_learning-and-improvement/020_incident-postmortem/SKILL.md
git commit -m "docs: ポストモーテム報告書テンプレートを新設しincident-postmortemに配線"
```

---

## Task 18: 索引READMEのリンク化 + 全体整合性の最終検証

**Files:**
- Modify: `skills/shared-templates/document-templates/README.md`

**Interfaces:**
- Consumes: Task 3〜17で作成された15個のテンプレートファイル

- [ ] **Step 1: 索引テーブルをリンク化する**

`skills/shared-templates/document-templates/README.md` の「索引（15種類）」テーブルの `ファイル` 列を、
プレーンテキストの `` `<filename>` `` から Markdown リンク `` [`<filename>`](<filename>) `` に置き換える。
15行すべてを同様に変更する（例: `` `requirements-definition-document-template.md` `` →
`` [`requirements-definition-document-template.md`](./requirements-definition-document-template.md) ``）。

- [ ] **Step 2: 索引直下の注記コメントを削除する**

Old:
```
<!-- リンク化はTask18で全ファイル作成後に行う -->
```

New: （この行を削除する）

- [ ] **Step 3: 15ファイルすべての実在を検証する**

Run:
```bash
for f in requirements-definition-document-template feature-design-document-template data-model-design-document-template adr-template api-contract-document-template code-review-standard-document-template refactoring-plan-document-template defect-investigation-report-template security-hardening-policy-template test-strategy-document-template operability-design-document-template performance-investigation-report-template release-plan-document-template documentation-update-plan-template postmortem-report-template; do
  test -f "skills/shared-templates/document-templates/${f}.md" || echo "MISSING: ${f}.md"
done
```
Expected: 出力なし（すべて存在する）

- [ ] **Step 4: 索引リンクの解決を検証する**

Run: `grep -o "](\./[^)]*\.md)" skills/shared-templates/document-templates/README.md | wc -l`
Expected: `15`

- [ ] **Step 5: コミット**

```bash
git add skills/shared-templates/document-templates/README.md
git commit -m "docs: 成果物文書テンプレート索引をリンク化"
```

---

## Task 19: `.github/SKILL-template.md` に成果物文書テンプレート雛形を追加

**Files:**
- Modify: `.github/SKILL-template.md`

**Interfaces:**
- Consumes: Task 1, Task 2（今後の新規スキルが従う雛形）

- [ ] **Step 1: 入力リファレンス節に行を追加する**

`.github/SKILL-template.md` の以下の箇所を探す。

Old:
```
- **記録テンプレート**: [[SKILL_NAME]-log-template.md](./assets/[SKILL_NAME]-log-template.md)
```

New:
```
- **記録テンプレート**: [[SKILL_NAME]-log-template.md](./assets/[SKILL_NAME]-log-template.md)
- **成果物文書テンプレート**: [../../shared-templates/document-templates/README.md](../../shared-templates/document-templates/README.md) から該当種別を選び、`shared-templates/document-templates/[DOCUMENT_TEMPLATE_NAME].md` として追加する
- **トレーサビリティID規約**: ../../shared-references/traceability-id-convention.md（本Skillのプレフィックスは規約に追記する）
```

- [ ] **Step 2: 完了条件節に行を追加する**

Old:
```
- 決定・判定根拠がすべて追跡可能である
```

New:
```
- 決定・判定根拠がすべて追跡可能である
- 成果物文書が該当テンプレートの必須章（文書情報／目的・背景／対応元ID／方針・決定事項／未決事項・リスク／関連ドキュメント）を満たしている
```

- [ ] **Step 3: 検証する**

Run: `grep -n "成果物文書テンプレート" .github/SKILL-template.md`
Expected: 1行以上ヒット

- [ ] **Step 4: コミット**

```bash
git add .github/SKILL-template.md
git commit -m "docs: SKILL-template.mdに成果物文書テンプレート雛形を追加"
```

---

## Task 20: `skills/README.md` と `skills/VALIDATION_CHECKLIST.md` の更新

**Files:**
- Modify: `skills/README.md`
- Modify: `skills/VALIDATION_CHECKLIST.md`

**Interfaces:**
- Consumes: Task 1〜18（全体の完了を前提に説明・検証項目を書く）

- [ ] **Step 1: `skills/README.md` の「共通運用ポリシー」に新節を追加する**

「### 参照優先順位（競合時）」の直前に以下を挿入する。

```markdown
### 成果物文書テンプレート

- 各Skillは、Skill実行の記録（`assets/*-log-template.md`）とは別に、対象プロジェクトに配置する
  成果物文書（要件定義書、設計仕様書、テスト戦略書 等）を持つ
- 成果物文書の型は [shared-templates/document-templates/README.md](shared-templates/document-templates/README.md)
  に15種類を索引化している
- 各文書は文書ID（例: `REQ-001`）を持ち、下流の文書は「対応元ID」セクションで上流IDを逆参照する。
  規約は [shared-references/traceability-id-convention.md](shared-references/traceability-id-convention.md) を参照
- 中心集約のトレーサビリティマトリクスは作らない。追跡は各文書の逆リンクを辿って行う

```

- [ ] **Step 2: `skills/VALIDATION_CHECKLIST.md` の Layer A に項目を追加する**

Old:
```
- [ ] 完了条件が判定可能な文で記述されている
```

New:
```
- [ ] 完了条件が判定可能な文で記述されている
- [ ] 対象Skillの場合、入力リファレンスに成果物文書テンプレートへの参照リンクが存在する
- [ ] 完了条件に成果物文書の必須章充足（対応元ID含む）が含まれている
```

- [ ] **Step 3: 検証する**

Run: `grep -n "成果物文書テンプレート" skills/README.md skills/VALIDATION_CHECKLIST.md`
Expected: 両ファイルで1行以上ずつヒット

- [ ] **Step 4: コミット**

```bash
git add skills/README.md skills/VALIDATION_CHECKLIST.md
git commit -m "docs: skills/READMEとVALIDATION_CHECKLISTに成果物文書テンプレートの運用ルールを追加"
```
