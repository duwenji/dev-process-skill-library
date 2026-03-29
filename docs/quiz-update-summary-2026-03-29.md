# クイズ更新サマリ（2026-03-29）

この文書は、dev-process-skill-library 更新を spa-quiz-app のクイズセットへ反映した変更内容のサマリです。

## 実施概要

- シリーズに「概論・運用導入」セクションを追加
- 総問題数を 75 問から 87 問へ拡張
- 学習・改善カテゴリへ known-how-ingestion の論点を反映
- 設計・実装カテゴリへ data-model-design-unified と code-review-assistant の論点を反映
- 要件・計画、運用・リリースカテゴリでゲート条件に合わせた設問差し替えを実施
- 同期ルール文書を追加し、README と usage-guide から参照導線を追加

## 主要変更ファイル

### spa-quiz-app 側

- src/data/dev-process-skill-library/intro-overview.json（新規）
- src/data/dev-process-skill-library/metadata.json
- src/data/dev-process-skill-library/requirements-planning.json
- src/data/dev-process-skill-library/design-implementation.json
- src/data/dev-process-skill-library/operations-release.json
- src/data/dev-process-skill-library/learning-improvement.json
- src/data/quizSets.json

### dev-process-skill-library 側

- docs/quiz-sync-guide.md（新規作成後、対応表追記）
- docs/quiz-update-summary-2026-03-29.md（本ファイル）
- README.md（同期ガイド導線を追加）
- docs/usage-guide.md（関連ドキュメントへ導線を追加）

## セクション構成（更新後）

1. 概論・運用導入: 12問
2. 要件・計画: 10問
3. 設計・実装: 20問
4. 検証・品質: 16問
5. 運用・リリース: 15問
6. 学習・改善: 14問

合計: 87問

## 検証結果

- validate:all: 成功
- build: 成功
- sync-data により public/data へ反映済み

## 補足

- 今後の更新時は docs/quiz-sync-guide.md の「設問ソース対応表」と「更新差分レビュー手順」を起点に反映する
- 横断論点は intro-overview に集約し、カテゴリ別セクションは各 Skill の実務判断に集中させる