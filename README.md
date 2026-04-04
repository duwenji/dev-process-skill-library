# 開発プロセス標準化 Skill ライブラリ

ソフトウェア開発向けの再利用可能な Skill セットを管理するためのリポジトリです。

> 💡 ブラウザで https://duwenji.github.io/spa-quiz-app/ を開くと、この教材をクイズ形式で学習できます。

> 🔁 クイズ同期時の更新基準は [docs/quiz-sync-guide.md](docs/quiz-sync-guide.md) を参照してください。

> 🤝 **関連シリーズ:** このライブラリは `GitHub Copilot` 専用ではありませんが、Copilot 導入・運用を扱う教材群の **補完シリーズ** として併用できます。導入戦略は [GitHub Copilot Adoption Framework](https://github.com/duwenji/github-copilot-adoption-framework) も参照してください。

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
| 設計・実装 | 設計判断、実装品質、契約設計 | feature-implementation-unified, data-model-design-unified, architecture-decision-record, api-contract-design, refactoring-safety, code-review-assistant | なし |
| 検証・品質 | テスト戦略、品質検証、セキュリティ検証 | defect-repair-unified, test-strategy-unified, security-hardening | なし |
| 運用・リリース | 監視、性能、リリース準備 | release-readiness, observability-and-ops-readiness, performance-investigation | なし |
| 学習・改善 | 振り返りとドキュメント同期 | incident-postmortem, documentation-sync | なし |


## 利用可能 Skills 一覧

Skill体系マップの5カテゴリに基づき、現在利用可能な Skills を一覧化します。

運用で選びやすくするために、用途起点の列（利用トリガー、主な入力、主な成果物）を追加しています。カテゴリは1列目に配置し、同一カテゴリを縦方向にマージしています。

<table>
	<thead>
		<tr>
			<th>カテゴリ</th>
			<th>Skill名</th>
			<th>利用トリガー（いつ使うか）</th>
			<th>主な入力</th>
			<th>主な成果物</th>
			<th>ステータス</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td rowspan="1">要件・計画</td>
			<td>requirements-refinement</td>
			<td>要件が曖昧、受入条件が不明確</td>
			<td>課題背景、要求一覧、制約</td>
			<td>要件定義、受入条件、非機能要件</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td rowspan="6">設計・実装</td>
			<td>feature-implementation-unified</td>
			<td>新機能追加、仕様変更対応</td>
			<td>ユーザーストーリー、既存仕様、影響範囲</td>
			<td>実装計画、変更実装、検証結果</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>data-model-design-unified</td>
			<td>要件からデータモデルを先に固めたい</td>
			<td>要件ID、エンティティ候補、データ制約、既存DB情報</td>
			<td>ERD、データ辞書、DDL方針、移行計画</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>architecture-decision-record</td>
			<td>設計方針の比較検討が必要</td>
			<td>課題、候補案、制約、評価観点</td>
			<td>ADR（判断理由、採用案、却下理由）</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>api-contract-design</td>
			<td>API新規設計、互換性維持が必要</td>
			<td>利用者要件、既存API、バージョン方針</td>
			<td>API契約、互換性方針、変更ルール</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>refactoring-safety</td>
			<td>リファクタ前の影響評価が必要</td>
			<td>変更対象コード、依存関係、テスト状況</td>
			<td>影響分析、段階的変更計画、回帰確認</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>code-review-assistant</td>
			<td>レビュー品質を均一化したい</td>
			<td>PR差分、設計意図、コーディング規約</td>
			<td>レビュー観点チェック、指摘テンプレート</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td rowspan="3">検証・品質</td>
			<td>defect-repair-unified</td>
			<td>本番/検証で不具合が発生</td>
			<td>障害報告、再現手順、ログ、関連コミット</td>
			<td>原因分析、修正実装、再発防止策</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>test-strategy-unified</td>
			<td>テスト観点が不足・重複している</td>
			<td>要件、設計情報、リスク一覧</td>
			<td>テスト方針、観点マトリクス、実行計画</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>security-hardening</td>
			<td>セキュリティ要件対応、脆弱性対策</td>
			<td>脅威モデル、構成情報、依存ライブラリ</td>
			<td>対策設計、実装チェック、検証記録</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td rowspan="3">運用・リリース</td>
			<td>release-readiness</td>
			<td>リリース可否判定、直前確認</td>
			<td>変更一覧、テスト結果、運用手順</td>
			<td>リリース判定、Go/No-Go記録、ロールバック計画</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>observability-and-ops-readiness</td>
			<td>監視不足、障害検知の遅れ</td>
			<td>SLO、現行ログ/メトリクス、運用体制</td>
			<td>監視設計、アラート定義、運用Runbook</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>performance-investigation</td>
			<td>応答遅延、負荷増加、性能劣化</td>
			<td>計測結果、再現条件、環境情報</td>
			<td>ボトルネック特定、改善案、再計測結果</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td rowspan="2">学習・改善</td>
			<td>incident-postmortem</td>
			<td>障害後の再発防止を定着したい</td>
			<td>タイムライン、影響範囲、対応履歴</td>
			<td>ポストモーテム、再発防止アクション</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>documentation-sync</td>
			<td>実装更新に対し文書が遅延</td>
			<td>変更差分、既存文書、公開対象</td>
			<td>更新文書、差分サマリ、更新履歴</td>
			<td>利用可</td>
		</tr>
	</tbody>
</table>

クイック選択（最短導線）:

- まず要件を固める: requirements-refinement
- モデルを固める: data-model-design-unified
- 実装を進める: feature-implementation-unified
- 不具合を直す: defect-repair-unified
- テストを整備する: test-strategy-unified
- リリース可否を判断する: release-readiness

分類ルール（運用）:

- 各 Skill は必ず1カテゴリに帰属する
- 複数カテゴリに跨る場合は、主たる目的で判断する

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

現時点で Pilot 実施時期は未定です。以下を確定後に実施計画を更新します。

- 対象チーム: 未定
- 初期対象 Skill: 未定
- 期間目安: 未定
- 完了条件（案）:
	- 複数案件で Skill を適用
	- レトロスペクティブで改善点を記録
	- 全社展開可否を判定

展開計画:

- Pilot 実施時期の確定後に、0-30日 / 31-60日 / 61-90日の計画を策定

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
- データモデル設計: data-model-design-unified
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
			data-model-design-unified/
				SKILL.md
				runbook.md
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
