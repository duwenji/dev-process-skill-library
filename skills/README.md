# 開発プロセス標準化 Skill ライブラリ（詳細ガイド）

このファイルは、開発プロセス標準化 Skill ライブラリに関する説明を集約した詳細ガイドです。
ルート README はディレクトリ導線、電子書籍生成導線、クイズ同期ガイドを担い、運用詳細は本書に統合します。

> 💡 ブラウザで https://duwenji.github.io/spa-quiz-app/ を開くと、関連トピックをクイズ形式で復習できます。

> 🤝 このライブラリは GitHub Copilot 専用ではありませんが、Copilot 導入・運用教材の補完シリーズとして併用できます。

## 対象読者

- 開発者
- PM（プロジェクトマネージャー、[PM](shared-references/glossary.md#pm)）
- 品質保証（QA、[QA](shared-references/glossary.md#qa)）

省略用語の一覧は [shared-references/glossary.md](shared-references/glossary.md) の「略語・日本語対応表」を参照してください。

## 目的

- 開発作業を標準化し、品質のばらつきを抑える
- 調査・実装・検証・報告の抜け漏れを減らす
- 承認ゲートと証跡管理を一貫した形で運用する

## 使い方ガイド（統合版）

### 全体フロー

1. 開発タスクに合う Skill を選ぶ
2. SKILL.md を読んでフェーズを実行する
3. 承認ゲートを通過させる
4. ログ・出力物を残す
5. 改善点があれば Skill 更新を申請する

### Skill 実行と承認

- 各 Skill の `SKILL.md` を正本として順番に実行する
- 承認ゲートは `未承認 / 承認済` の2値で管理する
- 承認記録は「日時 / 判断者 / 根拠」を最小単位として残す

### ログ保存規約

- 実行ログは append-only で記録する
- 推奨保存先（作業リポジトリ側）:
  - `docs/skill-logs/<category>/<skill-name>/YYYY-MM-DD_log.md`
  - `docs/skill-logs/<category>/<skill-name>/YYYY-MM-DD_output.md`

## Skill 体系マップ

全体を以下の6カテゴリで管理します。新規 Skill 追加時は、必ずどれかのカテゴリに紐づけます。

| カテゴリ | 目的 | 該当 Skill（現在運用中） |
|---|---|---|
| 要件・計画 | 要件定義、優先度判断、導入計画 | requirements-refinement |
| 設計・実装 | 設計判断、実装品質、契約設計 | feature-implementation-unified, data-model-design-unified, architecture-decision-record, api-contract-design, refactoring-safety, code-review-assistant |
| 検証・品質 | テスト戦略、品質検証、セキュリティ検証 | defect-repair-unified, test-strategy-unified, security-hardening |
| 運用・リリース | 監視、性能、リリース準備 | release-readiness, observability-and-ops-readiness, performance-investigation |
| 学習・改善 | 振り返りとドキュメント同期 | incident-postmortem, documentation-sync |
| 開発方法論 | 設計・実装・運用を横断する方法論の標準化 | ddd-ai-responsibility |

## 利用可能 Skills 一覧（カテゴリ別）

### 要件・計画

- requirements-refinement

### 設計・実装

- feature-implementation-unified
- data-model-design-unified
- architecture-decision-record
- api-contract-design
- refactoring-safety
- code-review-assistant

### 検証・品質

- defect-repair-unified
- test-strategy-unified
- security-hardening

### 運用・リリース

- release-readiness
- observability-and-ops-readiness
- performance-investigation

### 学習・改善

- incident-postmortem
- documentation-sync

### 開発方法論

- ddd-ai-responsibility

クイック選択（最短導線）:

- まず要件を固める: requirements-refinement
- モデルを固める: data-model-design-unified
- 実装を進める: feature-implementation-unified
- 不具合を直す: defect-repair-unified
- テストを整備する: test-strategy-unified
- リリース可否を判断する: release-readiness
- DDD と AI の責任分担を定義する: ddd-ai-responsibility

## 役割と責任（RACI 簡易版）

- Owner: Skill の内容設計と更新責任
- Reviewer: 実務妥当性と運用整合性のレビュー
- Approver: 全社標準としての承認
- Maintainer: 定期棚卸しと陳腐化防止

推奨アサイン:

- Owner: 開発リード
- Reviewer: PM + QA
- Approver: 開発マネージャー
- Maintainer: 開発プロセス担当

## KPI（導入効果測定）

用語定義は [KPI](shared-references/glossary.md#kpi) を参照してください。

- 不具合再発率
- レビュー指摘密度（件数/PR）
- 要件確定から実装完了までのリードタイム
- リリース後 30 日以内の障害件数
- ドキュメント更新遅延件数

## 優先度採点ガイド（統合版）

各 Skill を 5 観点で 1〜5 点評価し、合計点で優先度を判定します。

| 観点 | 説明 |
|---|---|
| 影響度 | 品質、納期、顧客影響への寄与 |
| 発生頻度 | 対象課題が発生する頻度 |
| 緊急度 | 早期導入の必要性 |
| 実装容易性 | 導入工数の小ささ |
| 依存関係 | 他 Skill の前提となる度合い |

判定基準:

- High: 20 点以上
- Medium: 14〜19 点
- Low: 13 点以下

## クイズ同期ガイド（統合版）

クイズ同期ガイドの正本はルートの [../README.md](../README.md) に移動しました。

- 参照先: [../README.md](../README.md) の「クイズ同期ガイド（統合版）」
- 本ファイルは Skill ライブラリ運用詳細（体系、ポリシー、カテゴリ運用）の説明を担います。

## 共通運用ポリシー

### フェーズ進行

- 段階冒頭で、実施内容と到達条件を短く提示する
- 段階は原則として 1 つずつ進行し、並行実行しない
- 段階完了ごとに、次段階へ進む前の確認を必須とする

### 承認ステータス管理

- 承認ステータスは 未承認 / 承認済 の2値で統一する
- ゲートを伴う段階は、承認者と判定根拠を必ず記録する
- 重要な判断は、日時・判断者・根拠の3点を最小記録単位とする

### 証跡管理

- 実行ログは append-only で記録し、既存履歴を削除・上書きしない
- 例外事項は識別子（例: [TRK](shared-references/glossary.md#trk) / [EX](shared-references/glossary.md#ex)）で紐付ける
- 段階完了時に、要約・決定事項・未解決事項を残す

### 参照優先順位（競合時）

1. 実装実体（ソースコード、設定、[DDL](shared-references/glossary.md#ddl)）
2. 各 Skill の runbook
3. 各 Skill の SKILL.md
4. 実行ログ

補足:

- SKILL.md と runbook が不一致の場合は runbook を正とする
- 実行ログは履歴媒体であり、手順の正本として扱わない

## ディレクトリ構成（推奨）

論理ツリー（カテゴリ別）と実体構成を一致させるため、skills 配下を「カテゴリ -> Skill」の2階層で管理します。

| カテゴリ（論理） | フォルダ名（物理） |
|---|---|
| 要件・計画 | 010_requirements-and-planning |
| 設計・実装 | 020_design-and-implementation |
| 検証・品質 | 030_verification-and-quality |
| 運用・リリース | 040_operations-and-release |
| 学習・改善 | 050_learning-and-improvement |
| 開発方法論 | 060_development-method |

運用方針:

- skills 配下は カテゴリ -> Skill の 2 階層を標準とする
- Skill フォルダ名は skill 名と一致させ、`kebab-case` を使用する
- 各 Skill フォルダには `SKILL.md` を必須とし、必要に応じて `references/`, `sub-skills/`, `assets/` を配置する
- カテゴリは親ディレクトリで管理し、`SKILL.md` のフロントマターには含めない
- カテゴリ配下の `README.md` には、配下 Skill 一覧と選択ガイドのみを記載する
- 全社共通ルール、採点、例外記録は `shared-governance/` に集約する
- 新規 Skill 追加時は `shared-templates/skill/` のテンプレートを起点に作成する

## 現在の移行ステータス

- 旧配置（直下配置）: なし（移行完了）
- 新配置（カテゴリ配下）: 全 Skill を配置済み
- カテゴリ別 README: 全6カテゴリ作成済み
- SKILL.md frontmatter の skill schema 準拠: 済み
- shared-governance / shared-templates / shared-references の整備: 済み
