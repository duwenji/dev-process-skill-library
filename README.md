# 開発プロセス標準化 Skill ライブラリ

ソフトウェア開発向けの再利用可能な Skill セットを管理するためのリポジトリです。

> 💡 ブラウザで https://duwenji.github.io/spa-quiz-app/ を開くと、関連トピックをクイズ形式で復習できます。

> 🔁 クイズ同期時の更新基準は、この README の「クイズ同期ガイド（統合版）」を参照してください。

> 🤝 **関連シリーズ:** このライブラリは `GitHub Copilot` 専用ではありませんが、Copilot 導入・運用を扱う教材群の **補完シリーズ** として併用できます。導入戦略は [GitHub Copilot Adoption Framework](https://github.com/duwenji/github-copilot-adoption-framework) も参照してください。

## 対象読者

- 開発者
- PM（プロジェクトマネージャー、[PM](skills/shared-references/glossary.md#pm)）
- 品質保証（QA、[QA](skills/shared-references/glossary.md#qa)）

省略用語の一覧は [skills/shared-references/glossary.md](skills/shared-references/glossary.md) の「略語・日本語対応表」を参照してください。

## 目的
- 開発作業を標準化し、品質のばらつきを抑える
- 調査・実装・検証・報告の抜け漏れを減らす
- 承認ゲートと証跡管理を一貫した形で運用する

## 使い方ガイド（統合版）

### 全体フロー

1. リポジトリをチームへ配布する
2. 開発タスクに合う Skill を選ぶ
3. SKILL.md を読んでフェーズを実行する
4. 承認ゲートを通過させる
5. ログ・出力物を残す
6. 改善点があれば Skill 更新を申請する

### 配布と導入

- 組織運用を前提に、GitHub リポジトリをチームへ共有する
- Owner / Reviewer / Approver / 参照者の権限を明確化する
- 初回は Pilot 導入対象 Skill を限定し、小さく検証する

### Skill 実行と承認

- 各 Skill の `SKILL.md` を正本として順番に実行する
- 承認ゲートは `未承認 / 承認済` の2値で管理する
- 承認記録は「日時 / 判断者 / 根拠」を最小単位として残す

### ログ保存規約

- 実行ログは append-only で記録する
- 推奨保存先（作業リポジトリ側）:
	- `docs/skill-logs/<category>/<skill-name>/YYYY-MM-DD_log.md`
	- `docs/skill-logs/<category>/<skill-name>/YYYY-MM-DD_output.md`

### FAQ（要約）

- どの Skill から始めるか: `requirements-refinement` と `test-strategy-unified` を優先
- 複数 Skill の同時実行: 非推奨。上流から順次適用
- 追加手順が必要な場合: Issue で Skill 追記を申請

## クイック参照（統合版）

| 目的 | 参照先 |
|---|---|
| Skill の全体像を知る | 本 README の「Skill 体系マップ」 |
| 個別 Skill を確認する | `skills/` |
| チーム導入の型を見る | `skills/shared-governance/` |
| テンプレートを再利用する | `skills/shared-templates/` |
| クイズ同期ルールを確認する | 本 README の「クイズ同期ガイド（統合版）」 |

## 公開手順（統合版）

1. `skills/` / `skills/shared-templates/` / `skills/shared-governance/` を更新する
2. README の導線と説明を最新化する
3. 本 README の「クイズ同期ガイド（統合版）」に従って同期対象を確認する
4. 本 README の「検証チェックリスト（統合版）」で最終確認する
5. レビュー後に公開する

## 検証チェックリスト（統合版）

- [ ] `README.md` から主要ディレクトリへ移動できる
- [ ] `skills/` の説明と実体が一致している
- [ ] `skills/shared-templates/` と `skills/shared-governance/` の導線が分かりやすい
- [ ] 関連教材との位置づけ説明が最新である
- [ ] README 内の運用ガイド導線が最新である

## コントリビューションガイド（統合版）

改善提案や Skill 追加は歓迎します。

基本方針:

- 再利用しやすいプロセス知識を優先する
- 具体的な利用トリガーと成果物を明記する
- 大きな変更は `Issue` で背景を共有してから進める

推奨フロー:

1. `Issue` を作成する
2. branch を切る
3. `skills/` / `skills/shared-templates/` / `skills/shared-governance/` を更新する
4. `README.md` と関連資料を更新する
5. `README.md` の「検証チェックリスト（統合版）」を確認する
6. `Pull Request` を作成する

## Skill 体系マップ

全体を以下の6カテゴリで管理します。新規 Skill 追加時は、必ずどれかのカテゴリに紐づけます。

| カテゴリ | 目的 | 該当 Skill（現在運用中） | 該当 Skill（提案） |
|---|---|---|---|
| 要件・計画 | 要件定義、優先度判断、導入計画 | requirements-refinement | なし |
| 設計・実装 | 設計判断、実装品質、契約設計 | feature-implementation-unified, data-model-design-unified, architecture-decision-record, api-contract-design, refactoring-safety, code-review-assistant | なし |
| 検証・品質 | テスト戦略、品質検証、セキュリティ検証 | defect-repair-unified, test-strategy-unified, security-hardening | なし |
| 運用・リリース | 監視、性能、リリース準備 | release-readiness, observability-and-ops-readiness, performance-investigation | なし |
| 学習・改善 | 振り返りとドキュメント同期 | incident-postmortem, documentation-sync | なし |
| 開発方法論 | 設計・実装・運用を横断する方法論の標準化 | ddd-ai-responsibility | なし |


## 利用可能 Skills 一覧

Skill体系マップの6カテゴリに基づき、現在利用可能な Skills を一覧化します。

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
			<td>ERD、データ辞書、[DDL](skills/shared-references/glossary.md#ddl)方針、移行計画</td>
			<td>利用可</td>
		</tr>
		<tr>
			<td>architecture-decision-record</td>
			<td>設計方針の比較検討が必要</td>
			<td>課題、候補案、制約、評価観点</td>
			<td>[ADR](skills/shared-references/glossary.md#adr)（判断理由、採用案、却下理由）</td>
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
			<td>[SLO](skills/shared-references/glossary.md#slo)、現行ログ/メトリクス、運用体制</td>
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
		<tr>
			<td rowspan="1">開発方法論</td>
			<td>ddd-ai-responsibility</td>
			<td>DDD と AI の責任分担を整理したい</td>
			<td>既存開発プロセス、意思決定ルール、品質責任範囲</td>
			<td>責任分担方針、運用ルール、判断基準</td>
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
- DDD と AI の責任分担を定義する: ddd-ai-responsibility

分類ルール（運用）:

- 各 Skill は必ず1カテゴリに帰属する
- 複数カテゴリに跨る場合は、主たる目的で判断する

## 役割と責任（[RACI](skills/shared-references/glossary.md#raci)簡易版）

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

用語定義は [KPI](skills/shared-references/glossary.md#kpi) を参照してください。

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
- レビュー責任: Reviewer（[PM](skills/shared-references/glossary.md#pm) + [QA](skills/shared-references/glossary.md#qa)）

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

採点ルール:

- 採点者: Owner と Reviewer（PM + QA）の合議
- 採点タイミング: 新規登録時、四半期レビュー時、重大インシデント後
- 同点時の優先: 依存関係 > 影響度 > 緊急度
- 採点記録先: `README.md`, `skills/shared-governance/quarterly-review-log.md`

## クイズ同期ガイド（統合版）

dev-process-skill-library の更新を spa-quiz-app に反映する際の基準です。

一次ソースと反映先（要約）:

| クイズセクション | 主な一次ソース | 反映先ファイル |
|---|---|---|
| 概論・運用導入 | README（対象読者、目的、全体フロー、RACI、KPI、例外運用） | `spa-quiz-app/src/data/dev-process-skill-library/intro-overview.json` |
| 要件・計画 | requirements-and-planning 配下 Skill | `spa-quiz-app/src/data/dev-process-skill-library/requirements-planning.json` |
| 設計・実装 | design-and-implementation 配下 Skill | `spa-quiz-app/src/data/dev-process-skill-library/design-implementation.json` |
| 検証・品質 | verification-and-quality 配下 Skill | `spa-quiz-app/src/data/dev-process-skill-library/verification-quality.json` |
| 運用・リリース | operations-and-release 配下 Skill | `spa-quiz-app/src/data/dev-process-skill-library/operations-release.json` |
| 学習・改善 | learning-and-improvement 配下 Skill | `spa-quiz-app/src/data/dev-process-skill-library/learning-improvement.json` |

更新手順:

1. 一次ソース更新ファイルを抽出する
2. 影響するクイズセクションを特定する
3. 対象 JSON を更新する
4. 問数変更時は metadata の `totalQuestions` と `questionCount` を更新する
5. セクション変更時は quizSets 定義を更新する
6. `validate:quiz`, `validate:metadata`, `validate:all` を実行する
7. `sync-data`, `build` を実行して表示確認する

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
- 例外事項は識別子（例: [TRK](skills/shared-references/glossary.md#trk) / [EX](skills/shared-references/glossary.md#ex)）で紐付け、追跡可能性を担保する
- 段階完了時に、要約・決定事項・未解決事項を残す

### 4) 役割分担の原則

- 方針決定と最終承認は人間（開発者 / Owner / Approver）が担う
- AI は調査、草案、実装補助、検証補助を担う
- AI は未承認の意思決定を確定扱いにしない

### 5) 参照優先順位（競合時）

- 実装実体（ソースコード、設定、[DDL](skills/shared-references/glossary.md#ddl)）
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
| 要件・計画 | 010_requirements-and-planning |
| 設計・実装 | 020_design-and-implementation |
| 検証・品質 | 030_verification-and-quality |
| 運用・リリース | 040_operations-and-release |
| 学習・改善 | 050_learning-and-improvement |
| 開発方法論 | 060_development-method |

```text
skills-for-software-development/
	README.md
	skills/
		shared-governance/
			exception-log.md
			quarterly-review-log.md
			kpi-monthly-report.md
		shared-templates/
				skill/
					SKILL.md
					runbook.md
					checklist.md
					log.md
		README.md
			010_requirements-and-planning/
			README.md
				010_requirements-refinement/
				SKILL.md
				references/
				assets/
			020_design-and-implementation/
			README.md
				050_feature-implementation-unified/
				SKILL.md
				references/
				sub-skills/
				assets/
				040_data-model-design-unified/
				SKILL.md
				runbook.md
				sub-skills/
				assets/
				020_architecture-decision-record/
				SKILL.md
				references/
				assets/
				010_api-contract-design/
				SKILL.md
				references/
				assets/
				060_refactoring-safety/
				SKILL.md
				references/
				assets/
				030_code-review-assistant/
				SKILL.md
				references/
				assets/
			030_verification-and-quality/
			README.md
				010_defect-repair-unified/
				SKILL.md
				remediation-runbook.md
				sub-skills/
				references/
				assets/
				030_test-strategy-unified/
				SKILL.md
				references/
				assets/
				020_security-hardening/
				SKILL.md
				references/
				assets/
			040_operations-and-release/
			README.md
				010_observability-and-ops-readiness/
				SKILL.md
				references/
				assets/
				020_performance-investigation/
				SKILL.md
				references/
				assets/
				030_release-readiness/
				SKILL.md
				references/
				assets/
			050_learning-and-improvement/
			README.md
				020_incident-postmortem/
				SKILL.md
				references/
				assets/
				010_documentation-sync/
				SKILL.md
				references/
				assets/
			060_development-method/
				README.md
				010_ddd-ai-responsibility/
					SKILL.md
```

運用方針:

- skills 配下は カテゴリ -> Skill の 2 階層を標準とする
- Skill フォルダ名は skill 名と一致させ、`kebab-case` を使用する
- 各 Skill フォルダには `SKILL.md` を必須とし、必要に応じて `references/`, `sub-skills/`, `assets/` を配置する
- カテゴリは親ディレクトリで管理し、`SKILL.md` のフロントマターには含めない
- カテゴリ配下の `README.md` には、配下 Skill 一覧と選択ガイドのみを記載する
- 全社共通ルール、採点、例外記録は `skills/shared-governance/` に集約する
- 横断説明資料はルート `README.md` に集約する
- 新規 Skill 追加時は `skills/shared-templates/skill/` のテンプレートを起点に作成する

移行ステップ:

1. `skills/shared-templates/` と `skills/shared-governance/` を整備する
2. skills 配下に 6 カテゴリフォルダとカテゴリ別 README.md を新設する
3. 既存 Skill を該当カテゴリへ移設し、Skill フォルダ名を `kebab-case` で統一する
4. 全 `SKILL.md` を skill schema 準拠の frontmatter に統一する
5. Pilot対象 Skill（test-strategy-unified, requirements-refinement）をカテゴリ配下で追加する
6. 四半期レビューでカテゴリ運用の妥当性を評価し、命名・配置ルールを固定化する
