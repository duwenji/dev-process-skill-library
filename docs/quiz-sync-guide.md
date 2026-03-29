# クイズ同期ガイド

この文書は、dev-process-skill-library の更新を spa-quiz-app のクイズセットへ反映するときの同期基準をまとめたものです。

## 目的

- 一次ソースとクイズセクションの対応を固定する
- README や usage-guide の更新漏れを防ぐ
- クイズ更新時に metadata と quizSets の整合を崩さない

## 一次ソースと反映先

| クイズセクション | 主な一次ソース | 反映先ファイル | 更新時の確認点 |
|---|---|---|---|
| 概論・運用導入 | README.md, docs/usage-guide.md, docs/taxonomy.md | spa-quiz-app/src/data/dev-process-skill-library/intro-overview.json | 対象読者、目的、Skill 選択、全体フロー、RACI、KPI、例外運用が反映されているか |
| 要件・計画 | skills/requirements-and-planning/README.md, requirements-refinement/SKILL.md | spa-quiz-app/src/data/dev-process-skill-library/requirements-planning.json | 要件明確化、受入条件、非機能要件、スコープ境界、承認ゲートが最新か |
| 設計・実装 | skills/design-and-implementation/README.md, 各 Skill の SKILL.md と runbook.md | spa-quiz-app/src/data/dev-process-skill-library/design-implementation.json | 方式案比較、データモデル、ADR、API 契約、レビュー観点、リファクタリング安全性が最新か |
| 検証・品質 | skills/verification-and-quality/README.md, 各 Skill の SKILL.md と runbook.md | spa-quiz-app/src/data/dev-process-skill-library/verification-quality.json | テスト層の役割、影響分析、残余リスク、セキュリティ観点が最新か |
| 運用・リリース | skills/operations-and-release/README.md, 各 Skill の SKILL.md と runbook.md | spa-quiz-app/src/data/dev-process-skill-library/operations-release.json | リリース判定、監視、ロールバック、性能調査、例外管理が最新か |
| 学習・改善 | skills/learning-and-improvement/README.md, 各 Skill の SKILL.md と runbook.md, .github/copilot/skills/known-how-ingestion/SKILL.md | spa-quiz-app/src/data/dev-process-skill-library/learning-improvement.json | 事実と推測の分離、再発防止、文書同期、ノウハウ取り込み、継続改善が最新か |

## 更新手順

1. dev-process-skill-library 側で更新対象の一次ソースを確定する。
2. 上の対応表に従って、更新対象のクイズセクションを決める。
3. spa-quiz-app 側の JSON を更新する。
4. 問数が変わる場合は metadata.json の totalQuestions と sections の questionCount を更新する。
5. セクション追加、削除、名称変更がある場合は quizSets.json の親子セット定義も更新する。
6. spa-quiz-app で validate:quiz、validate:metadata、validate:all を実行する。
7. sync-data と build を実行し、表示確認まで行う。

## 変更時のチェックリスト

- README.md の運用ルール更新が intro-overview.json に反映されている
- docs/usage-guide.md の承認、ログ、例外運用が intro-overview.json に反映されている
- docs/taxonomy.md のカテゴリ依存関係が構成と説明文に反映されている
- 各カテゴリの questionCount と実際の問題数が一致している
- 親セットの questionCount と metadata.json の totalQuestions が一致している
- quizSets.json の order と parentId が UI 期待どおりである

## 保守方針

- README.md と docs/usage-guide.md の横断論点は、既存5カテゴリへ分散させず intro-overview に集約する
- 各カテゴリセクションは、カテゴリ内の Skill 内容に集中させる
- 将来の自動化を見据え、設問ごとではなくトピック単位で対応関係を維持する

## 設問ソース対応表（2026-03-29 時点）

この表は、現行クイズの設問群がどの一次ソースに基づくかをレビューしやすくするための対応表です。

| セクション | 設問ID範囲 | 主な一次ソース | 重点確認ポイント |
|---|---|---|---|
| 概論・運用導入 | 1-12 | README.md, docs/usage-guide.md, docs/taxonomy.md | 全体フロー、RACI、承認記録、KPI、例外運用 |
| 要件・計画 | 1-10 | skills/requirements-and-planning/requirements-refinement/SKILL.md | 段階4-6、段階7/11/13 ゲート、未確定事項管理 |
| 設計・実装 | 1-20 | feature-implementation-unified/SKILL.md, data-model-design-unified/SKILL.md, architecture-decision-record/SKILL.md, api-contract-design/SKILL.md, refactoring-safety/SKILL.md, code-review-assistant/SKILL.md | 方式案比較、ERD と制約、ADR、契約互換性、レビュー基準 |
| 検証・品質 | 1-16 | defect-repair-unified/SKILL.md, test-strategy-unified/SKILL.md, security-hardening/SKILL.md | 影響分析、層別テスト戦略、残余リスク、セキュリティ検証 |
| 運用・リリース | 1-15 | release-readiness/SKILL.md, observability-and-ops-readiness/SKILL.md, performance-investigation/SKILL.md | 段階7/11/13 ゲート、監視設計、仮説と計測対応 |
| 学習・改善 | 1-14 | incident-postmortem/SKILL.md, documentation-sync/SKILL.md, .github/copilot/skills/known-how-ingestion/SKILL.md | 事実と推測の分離、文書同期、ノウハウ取り込み、機密区分 |

## 更新差分レビュー手順（推奨）

1. 一次ソース更新ファイルを抽出する。
2. 上記対応表で影響セクションと設問ID範囲を特定する。
3. セクション内の説明文と設問を更新する。
4. metadata.json と quizSets.json の説明文、問数、順序を確認する。
5. validate:all と build を実行し、表示確認まで行う。