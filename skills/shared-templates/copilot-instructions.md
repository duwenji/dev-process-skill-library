# Copilot Instructions ガイド兼テンプレート

このファイルは、skills 配下で共通利用する Copilot Instructions のガイド兼テンプレートです。
共通ルール、運用判断、ひな形の出発点をこのファイルに集約します。

## 位置づけ

- 共通ルールとテンプレートの集約先: このファイル
- リポジトリ最小ルール: ../../.github/copilot-instructions.md

使い分け:

- 共通方針、保存先、命名、参照優先順位を確認したい場合はこのファイルを参照する
- 新しい copilot-instructions.md を作成する場合はこのファイルを出発点にし、対象リポジトリ向けに具体化する

## テンプレートの使い方
- まずこのファイルを読んで、適用すべき共通ルールを確定する
- 次にこのテンプレートを複製し、対象リポジトリや対象ディレクトリに合わせて具体化する
- テンプレートにない独自運用を追加する場合も、共通ルールと矛盾しないことを確認する

## 目的・重要性
Copilot Instructions（copilot-instructions.md）は、リポジトリや Skill の自動化・再利用性・品質向上のための明確な指示文です。曖昧な指示や属人的な運用を防ぎ、誰でも同じ成果を得られることを目指します。

## 運用方針
- 新規リポジトリ作成時や大幅改訂時に必ず記載
- 実装者・レビュワー双方が内容を確認し、必要に応じて改善
- 記載内容は簡潔かつ具体的に
- フォルダ構成や命名規則も明記し、初見者が迷わないようにする
- 構成変更時は README と .github/copilot-instructions.md の導線も合わせて更新する
- 一般論ではなく、skills 配下の運用で再利用する共通事項を優先的に記載する

## skills配下で共通化する項目

- 対象スコープ: このリポジトリの skills 配下資材と、それを利用する作業リポジトリ側の運用手順
- 実行ログ保存先: docs/skill-logs/<category>/<skill-name>/
- 参照優先順位: 実装実体 > runbook.md > SKILL.md > 実行ログ
- 命名規約: カテゴリは番号付き、Skill 名は kebab-case、runbook は runbook.md 固定
- 役割分担: 方針決定と最終承認は人間、AI は調査・整理・下書きを担当
- 更新導線: skills/README.md で概要、shared-templates で共通ルールと雛形、.github で最小ルールを確認する

## サンプル構成記載
```
リポジトリ構成例（深い階層・番号付き）:

- 010_requirements/
    - 010_business-requirements.md
    - 020_use-cases/
        - 010_use-case-01.md
        - 020_use-case-02.md
- 020_basic-design/
    - 010_domain-analysis.md
    - 020_data-model/
        - 010_er-diagram.png
        - 020_data-dictionary.md
    - 030_ui-design/
        - 010_screen-list.md
        - 020_screen-spec/
            - 010_screenA/
                - 010_elements.md
                - 020_initial-process.md
                - 030_event-handling.md
                - 040_sequence-diagram.md
                - 050_step-description.md
    - 040_external-interfaces.md
- 030_detail-design/
    - 010_api-spec/
        - 010_apiA.md
        - 020_apiB.md
    - 020_logic-design/
        - 010_flowcharts/
        - 020_pseudo-code/
- 040_implementation/
- 050_test/
- 060_release/
- README.md            # ガイド・運用資料（集約）
- shared-templates/    # テンプレート集
- shared-references/   # ベストプラクティス・用語集
- skills/shared-governance/  # KPI・例外・レビュー記録
```

### 各成果物の記載場所・内容例
- 画面仕様書（020_basic-design/030_ui-design/020_screen-spec/010_screenA/）
    - 010_elements.md：画面要素一覧・説明
    - 020_initial-process.md：初期化処理の流れ
    - 030_event-handling.md：各イベントの処理仕様
    - 040_sequence-diagram.md：画面内・外部連携のシーケンス図
    - 050_step-description.md：ユーザー操作ごとの詳細説明
- 基本設計
    - 010_domain-analysis.md：ドメインモデル・用語定義
    - 020_data-model/：ER図・データ辞書
    - 030_ui-design/：画面一覧・画面遷移図・画面仕様
    - 040_external-interfaces.md：外部システムIF仕様
- 詳細設計
    - 010_api-spec/：API仕様書
    - 020_logic-design/：ロジック設計（フローチャート・疑似コード等）

※ 番号は工程・成果物の流れを明確にし、並び順を安定させるために付与します。

## ベストプラクティス
- 曖昧な表現を避ける
- 例外処理やエッジケースも明記
- 定期的な見直し・改善を推奨
- 主要ディレクトリごとに「用途」「主なファイル」「関連ドキュメント」などを整理
- Mermaid記法によるシーケンス図・フローチャートの活用を推奨（画面仕様書やAPI仕様書、処理フロー説明などで視覚的に共有）

### Mermaid記法サンプル
```mermaid
sequenceDiagram
    participant User
    participant UI
    participant API
    User->>UI: ボタン押下
    UI->>API: データ送信
    API-->>UI: 結果返却
    UI-->>User: 結果表示
```

```mermaid
flowchart TD
    A[開始] --> B{条件判定}
    B -- Yes --> C[処理1]
    B -- No  --> D[処理2]
    C --> E[終了]
    D --> E
```

## 参考
- flowchart-best-practices.md
- ERD-best-practices.md
- skills/README.md
