# 開発プロセス標準化 Skill ライブラリ

ソフトウェア開発向けの再利用可能な Skill セットを管理するためのリポジトリです。

> 💡 ブラウザで https://duwenji.github.io/spa-quiz-app/ を開くと、この教材をクイズ形式で学習できます。

## 対象読者

- 開発者
- PM（プロジェクトマネージャー）
- 品質保証（QA）

## 目的

- 開発作業を標準化し、品質のばらつきを抑える
- 調査・実装・検証・報告の抜け漏れを減らす
- 承認ゲートと証跡管理を一貫した形で運用する

## Skill 体系マップ

全体を以下の5カテゴリで管理します。新規 Skill 追加時は、必ずどれかのカテゴリに紐づけます。

| カテゴリ | 目的 | 該当 Skill（現在運用中） | 該当 Skill（提案） |
|---|---|---|---|
| 要件・計画 | 要件定義、優先度判断、導入計画 | requirements-refinement | なし |
| 設計・実装 | 設計判断、実装品質、契約設計 | feature-implementation-unified, architecture-decision-record, api-contract-design, refactoring-safety, code-review-assistant | なし |
| 検証・品質 | テスト戦略、品質検証、セキュリティ検証 | defect-repair-unified, test-strategy-unified, security-hardening | なし |
| 運用・リリース | 監視、性能、リリース準備 | release-readiness, observability-and-ops-readiness, performance-investigation | なし |
| 学習・改善 | 振り返りとドキュメント同期 | incident-postmortem, documentation-sync | なし |

論理ツリー（カテゴリ別）:

```text
Skill体系
	要件・計画
		現在運用中
			- requirements-refinement
		提案
			- なし
	設計・実装
		現在運用中
			- feature-implementation-unified
			- architecture-decision-record
			- api-contract-design
			- refactoring-safety
			- code-review-assistant
		提案
			- なし
	検証・品質
		現在運用中
			- defect-repair-unified
			- test-strategy-unified
			- security-hardening
		提案
			- なし
	運用・リリース
		現在運用中
			- release-readiness
			- performance-investigation
			- observability-and-ops-readiness
		提案
			- なし
	学習・改善
		現在運用中
			- incident-postmortem
			- documentation-sync
		提案
			- なし
```

## 提案 Skills 一覧

優先度は全社共通の基準で運用します。

### 優先度 High

| Skill名 | 目的 | 期待効果 |
|---|---|---|
| test-strategy-unified | 単体・結合・E2Eのテスト観点を統合管理 | 回帰不具合の削減、検証品質の安定化 |
| requirements-refinement | 要件・受入条件・非機能要件の明確化 | 手戻り削減、開発着手の判断精度向上 |
| architecture-decision-record | 設計判断をADR形式で記録 | 判断根拠の共有、保守性向上 |
| release-readiness | リリース前チェックとロールバック計画の標準化 | 本番障害リスク低減 |
| security-hardening | セキュリティ観点の設計・実装・検証チェック | 脆弱性混入の抑制 |

実装状況（2026-03-28 時点）:

- 実装済み: test-strategy-unified, requirements-refinement, architecture-decision-record, release-readiness, security-hardening
- 次の対象: api-contract-design, refactoring-safety, performance-investigation, observability-and-ops-readiness, code-review-assistant

### 優先度 Medium

| Skill名 | 目的 | 期待効果 |
|---|---|---|
| api-contract-design | API契約・エラー設計・互換性維持の標準化 | 連携不整合の削減 |
| refactoring-safety | リファクタ時の影響分析と安全実行 | 品質維持しながら内部改善 |
| performance-investigation | 計測主導の性能ボトルネック調査と改善 | 根拠のある性能改善 |
| observability-and-ops-readiness | ログ・メトリクス・アラート運用の整備 | 障害検知と復旧の迅速化 |
| code-review-assistant | レビュー観点をテンプレート化 | レビュー品質の平準化 |

実装状況（2026-03-28 時点）:

- 実装済み: api-contract-design, refactoring-safety, performance-investigation, observability-and-ops-readiness, code-review-assistant
- 次の対象: incident-postmortem, documentation-sync

### 優先度 Low

| Skill名 | 目的 | 期待効果 |
|---|---|---|
| incident-postmortem | 障害振り返りと再発防止の標準化 | 組織学習の定着 |
| documentation-sync | 実装変更に追従した文書更新フロー | ドキュメント陳腐化の防止 |

実装状況（2026-03-28 時点）:

- 実装済み: incident-postmortem, documentation-sync
- 提案 Skill 12件すべて実装完了

## 推奨導入順

1. test-strategy-unified
2. requirements-refinement
3. architecture-decision-record
4. release-readiness
5. security-hardening
6. api-contract-design
7. refactoring-safety
8. performance-investigation
9. observability-and-ops-readiness
10. code-review-assistant
11. incident-postmortem
12. documentation-sync

## 優先度判定基準（全社共通）

各 Skill は以下の5観点を 1 から 5 で評価し、合計点で優先度を判定します。

- 影響度: 品質、納期、顧客影響への寄与
- 発生頻度: 対象課題が発生する頻度
- 緊急度: 早期導入の必要性
- 実装容易性: 導入工数の小ささ
- 依存関係: 他 Skill の前提となる度合い

採点ルール:

- 採点者: Owner と Reviewer（PM + QA）の合議
- 採点タイミング: 新規登録時、四半期レビュー時、重大インシデント発生後
- 記録先: 本 README の優先度一覧と運用ログ
- 同点時の優先: 依存関係 > 影響度 > 緊急度

判定目安:

- High: 20点以上
- Medium: 14点から19点
- Low: 13点以下

注記:

- 全社共通運用を原則とし、個別プロダクトで優先度を変更しない
- 例外適用時のみ、下記の「優先度例外運用」を適用する

## 優先度例外運用

以下のいずれかを満たす場合のみ、期限付きで例外を許可します。

- 法令、監査、契約上の必須対応がある
- P1 障害など、業務継続に直結する緊急事象がある
- セキュリティインシデント対応で即時性が必要

運用ルール:

- 申請者: Owner
- 承認者: Approver
- 必須記録: 例外理由、適用期間、解除条件、影響範囲
- 有効期限: 原則 30 日（延長は再承認必須）

## 役割と責任（RACI簡易版）

- Owner: Skill の内容設計と更新責任
- Reviewer: 実務妥当性と運用整合性のレビュー
- Approver: 全社標準としての承認
- Maintainer: 定期棚卸しと陳腐化防止

推奨アサイン:

- Owner: 開発リード
- Reviewer: PM + QA
- Approver: 開発マネージャー
- Maintainer: 開発プロセス担当

## 試験導入（Pilot）

- 対象チーム: iee
- 初期対象 Skill: test-strategy-unified, requirements-refinement
- 期間目安: 4 週間
- 完了条件:
	- 2 つの案件で Skill を適用
	- レトロスペクティブで改善点を記録
	- 全社展開可否を判定

90日展開計画:

- 0-30日: iee で Pilot 実施、運用課題を収集
- 31-60日: 改訂版を2チームへ横展開
- 61-90日: 全社標準化判断と運用版 1.0 確定

## KPI（導入効果測定）

- 不具合再発率
- レビュー指摘密度（件数/PR）
- 要件確定から実装完了までのリードタイム
- リリース後 30 日以内の障害件数
- ドキュメント更新遅延件数

定義式とデータソース:

- 不具合再発率: 再発不具合件数 / 全不具合件数（データソース: Issue 管理）
- レビュー指摘密度: レビュー指摘件数 / PR 件数（データソース: PR レビュー履歴）
- リードタイム: 実装完了日 - 要件確定日 の中央値（データソース: チケット管理）
- 30日以内障害件数: リリース日から30日以内の障害起票件数（データソース: 障害管理台帳）
- ドキュメント更新遅延件数: 実装完了から 5 営業日超で未更新の文書件数（データソース: リポジトリ履歴）

計測ルール:

- ベースライン: 導入前 1 か月
- 比較期間: 導入後 1 か月、3 か月
- 判定: KPI 改善率と現場フィードバックを合わせて評価
- 集計責任: Maintainer
- レビュー責任: Reviewer（PM + QA）

## Skill 選択ガイド

- 不具合起点の対応: defect-repair-unified
- 新規要求、仕様変更: feature-implementation-unified
- テスト方針の整備: test-strategy-unified
- 性能劣化調査: performance-investigation
- セキュリティ観点強化: security-hardening

## Skill 設計ガイド（共通）

- フロントマターに name と description を必須で定義する
- description は発見しやすいトリガー語を含める
- 実行フローは Phase と承認ゲートを明示する
- 競合時の参照優先順位を明記する
- 出力物テンプレートと完了条件を定義する

## 共通運用ポリシー（現行 Skill から統合）

以下は個別 Skill に依存しないため、全 Skill の基本運用ルールとして採用します。

### 1) フェーズ進行ルール

- 段階冒頭で、実施内容と到達条件を短く提示する
- 段階は原則として 1 つずつ進行し、並行実行しない
- 段階完了ごとに、次段階へ進む前の確認を必須とする

### 2) 承認ステータス管理

- 承認ステータスは 未承認 / 承認済 の2値で統一する
- ゲートを伴う段階は、承認者と判定根拠を必ず記録する
- 重要な判断は、日時・判断者・根拠の3点を最小記録単位とする

### 3) 証跡管理ルール

- 実行ログは append-only で記録し、既存履歴を削除・上書きしない
- 例外事項は識別子（例: TRK / EX）で紐付け、追跡可能性を担保する
- 段階完了時に、要約・決定事項・未解決事項を残す

### 4) 役割分担の原則

- 方針決定と最終承認は人間（開発者 / Owner / Approver）が担う
- AI は調査、草案、実装補助、検証補助を担う
- AI は未承認の意思決定を確定扱いにしない

### 5) 参照優先順位（競合時）

- 実装実体（ソースコード、設定、DDL）
- 各 Skill の runbook
- 各 Skill の SKILL.md
- 実行ログ

補足:

- SKILL.md と runbook が不一致の場合は runbook を正とする
- 実行ログは履歴媒体であり、手順の正本として扱わない

### 6) 最小完了条件（全 Skill 共通）

- 定義された承認ゲートをすべて通過している
- 必須出力物がテンプレート準拠で揃っている
- 未解決事項がある場合は、例外承認または次アクションが明記されている
- 変更理由と検証結果の追跡可能性が確保されている

### 7) 要求・受入条件の管理

- 要求起点の Skill では、背景、スコープ、非機能要件、受入条件を最初に明確化する
- 方式案や実装内容は、受入条件との対応関係を説明可能にする
- 受入条件の変更は、変更理由と影響範囲を記録した上で承認を得る

### 8) 検証観点の標準化

- 検証項目は 正常系 / 境界値 / 異常系 / 回帰 の4観点を基本セットとする
- 不合格項目がある場合は、再実施条件または例外承認を明記する
- ビルド、静的チェック、回帰確認を最小必須の品質確認に含める

## ディレクトリ構成（推奨）

論理ツリー（カテゴリ別）と実体構成を一致させるため、`skills` 配下を「カテゴリ -> Skill」の2階層で管理します。

カテゴリ名とフォルダ名の対応:

| カテゴリ（論理） | フォルダ名（物理） |
|---|---|
| 要件・計画 | requirements-and-planning |
| 設計・実装 | design-and-implementation |
| 検証・品質 | verification-and-quality |
| 運用・リリース | operations-and-release |
| 学習・改善 | learning-and-improvement |

```text
skills-for-software-development/
	README.md
	docs/
		taxonomy.md
		scoring-guide.md
		pilot-guide.md
	governance/
		exception-log.md
		quarterly-review-log.md
		kpi-monthly-report.md
	templates/
		skill/
			SKILL.template.md
			runbook.template.md
			checklist.template.md
			log.template.md
	skills/
		README.md
		requirements-and-planning/
			README.md
			requirements-refinement/
				SKILL.md
				references/
				assets/
		design-and-implementation/
			README.md
			feature-implementation-unified/
				SKILL.md
				references/
				sub-skills/
				assets/
			architecture-decision-record/
				SKILL.md
				references/
				assets/
			api-contract-design/
				SKILL.md
				references/
				assets/
			refactoring-safety/
				SKILL.md
				references/
				assets/
			code-review-assistant/
				SKILL.md
				references/
				assets/
		verification-and-quality/
			README.md
			defect-repair-unified/
				SKILL.md
				remediation-runbook.md
				sub-skills/
				references/
				assets/
			test-strategy-unified/
				SKILL.md
				references/
				assets/
			security-hardening/
				SKILL.md
				references/
				assets/
		operations-and-release/
			README.md
			observability-and-ops-readiness/
				SKILL.md
				references/
				assets/
			performance-investigation/
				SKILL.md
				references/
				assets/
			release-readiness/
				SKILL.md
				references/
				assets/
		learning-and-improvement/
			README.md
			incident-postmortem/
				SKILL.md
				references/
				assets/
			documentation-sync/
				SKILL.md
				references/
				assets/
```

運用方針:

- skills 配下は カテゴリ -> Skill の 2 階層を標準とする
- Skill フォルダ名は skill 名と一致させ、`kebab-case` を使用する
- 各 Skill フォルダには `SKILL.md` を必須とし、必要に応じて `references/`, `sub-skills/`, `assets/` を配置する
- カテゴリは親ディレクトリで管理し、`SKILL.md` のフロントマターには含めない
- カテゴリ配下の `README.md` には、配下 Skill 一覧と選択ガイドのみを記載する
- 全社共通ルール、採点、例外記録は `governance/` に集約する
- 横断説明資料は `docs/` に集約し、ルート `README.md` には要約のみを残す
- 新規 Skill 追加時は `templates/skill` のテンプレートを起点に作成する

移行ステップ:

1. docs, governance, templates を新設
2. skills 配下に 5 カテゴリフォルダとカテゴリ別 README.md を新設する
3. 既存 Skill を該当カテゴリへ移設し、Skill フォルダ名を `kebab-case` で統一する
4. 全 `SKILL.md` を skill schema 準拠の frontmatter に統一する
5. Pilot対象 Skill（test-strategy-unified, requirements-refinement）をカテゴリ配下で追加する
6. 四半期レビューでカテゴリ運用の妥当性を評価し、命名・配置ルールを固定化する
