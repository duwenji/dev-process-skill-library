# shared-templates

省略用語（RACI, KPI, ADR, DDL, SLO, QA, PM, TRK, EX）は [../shared-references/glossary.md](../shared-references/glossary.md) の『略語・日本語対応表』を参照してください。

このディレクトリは、skills 配下の各スキル実装を支援するための共通テンプレートと設計ガイドです。スキル総数はカテゴリ追加に伴い変動するため、最新の一覧は [skills/README.md](../README.md) の「利用可能 Skills 一覧」を正とします。

共通運用ルールと copilot-instructions.md のひな形は [copilot-instructions.md](./copilot-instructions.md) に集約しています。

## ファイル一覧

| ファイル | 目的 | 対象者 |
| --- | --- | --- |
| [SKILL.md](./skill/SKILL.md) | 新規スキルの SKILL.md テンプレート（カスタマイズ ポイント埋め込み） | スキル実装担当者 |
| [copilot-instructions.md](./copilot-instructions.md) | Copilot 運用ルールと copilot-instructions.md ひな形の集約先 | 運用設計担当者 |
| [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) | 3パターン（調査・検証/意思決定・設計/運用手続き）の選択と customization checklist | スキル実装 PM |
| [sub-skills.md](./skill/sub-skills.md) | Phase 1～4 の sub-skills ファイル（phase1-*.md ～ phase4-*.md）テンプレート | スキル実装担当者 |
| [data-dictionary-template.md](./data-dictionary-template.md) | 要件IDから属性・API・DDLへのトレーサビリティを残すデータ辞書テンプレート | 設計担当者 |
| [test-case-csv-template.md](./test-case-csv-template.md) | テストチェック項目を CSV で管理・共有し、Excel で確認するためのテンプレート | テスト実施担当者 |
| [../shared-references/test-check-aggregation-template.md](../shared-references/test-check-aggregation-template.md) | テストチェック項目と実施状況（ステータス/実施方法/分類）の集計テンプレート | 開発者、レビュアー |

---

## 使用フロー（新規スキル実装時）

### Step 1: パターン選択

1. [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) を読む
2. スキルの目的から、パターン A/B/C のいずれかを選択
3. Customization Checklist を参照し、必須項目を把握

### Step 2: SKILL.md テンプレートの fill-in

1. [SKILL.md](./skill/SKILL.md) をコピーして、スキルディレクトリに配置
2. frontmatter を埋める（name, description, argument-hint等）
3. 各 Phase のタイトル・段階説明を、選択パターンに合わせて customization
4. ゲート条件#1, #2, #3 を明確に記述

### Step 3: Sub-skills ファイルの作成

1. [sub-skills.md](./skill/sub-skills.md) を参照
2. phase1-[KEY].md, phase2-[KEY].md, phase3-[KEY].md, phase4-reporting.md を作成
3. 各フェーズの目的・実施段階・出力物を記述

### Step 4: 参照ファイルの整備

1. assets/[SKILL_NAME]-log-template.md を作成（運用実行ログテンプレート）
2. 共通参照 `../shared-references/` を優先利用:

- flowchart-best-practices.md
- investigation-checklist.md
- testcase-template.md

1. Skill固有の追加資料が必要な場合のみ references/ を作成
2. runbook.md を作成（詳細手順・判定基準）

### Step 5: QA Checklist

1. [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) の "Quality Assurance Checklist" で検証
2. frontmatter, Phase, ゲート条件, sub-skills, references が完備されているか確認

---

## リソース ファイル

### 既存テンプレートの参照

#### feature-implementation-unified から学ぶ

- Path: `skills/020_design-and-implementation/050_feature-implementation-unified/`
- Pattern: **パターン B**（意思決定・設計型）の標準例
- 参照すべき file:

  - [SKILL.md](../020_design-and-implementation/050_feature-implementation-unified/SKILL.md) - フロー定義
  - [sub-skills/](../020_design-and-implementation/050_feature-implementation-unified/sub-skills/) - Phase ごと詳細
  - [assets/implementation-log-template.md](../020_design-and-implementation/050_feature-implementation-unified/assets/implementation-log-template.md) - 実行ログ形式

#### defect-repair-unified から学ぶ

- Path: `skills/030_verification-and-quality/010_defect-repair-unified/`
- Pattern: **パターン A**（調査・検証型）の標準例
- 参照すべき file:

  - [SKILL.md](../030_verification-and-quality/010_defect-repair-unified/SKILL.md) - フロー定義
  - [sub-skills/](../030_verification-and-quality/010_defect-repair-unified/sub-skills/) - Phase ごと詳細
  - [assets/defect-log-template.md](../030_verification-and-quality/010_defect-repair-unified/assets/defect-log-template.md) - 実行ログ形式
  - [runbook.md](../030_verification-and-quality/010_defect-repair-unified/runbook.md) - 詳細手順

---

## カスタマイズ例

### 例1: test-strategy-unified（パターン A）

選択: **パターン A - 調査・検証型**

- phase1-assessment.md
- phase2-implementation.md
- phase3-testing.md
- phase4-reporting.md

主要ゲート:

- ゲート#1（段階7）: テスト戦略案が複数提示されているか
- ゲート#2（段階11）: テストケースが改修点を網羅しているか
- ゲート#3（段階13）: テスト結果が合格基準を満たすか

### 例2: requirements-refinement（パターン B）

選択: **パターン B - 意思決定・設計型**

- phase1-refinement.md
- phase2-design-implementation.md
- phase3-validation.md
- phase4-reporting.md

主要ゲート:

- ゲート#1（段階7）: 要件が明確で受入条件が定量的か
- ゲート#2（段階11）: 検証項目が受入条件を網羅しているか
- ゲート#3（段階13）: 検証結果が受入基準を満たすか

### 例3: code-review-assistant（パターン C）

選択: **パターン C - 運用手続き型**

- phase1-guideline-definition.md
- phase2-execution-planning.md
- phase3-feedback-and-adjustment.md
- phase4-continuous-improvement.md

主要ゲート:

- ゲート#1（段階7）: レビューチェックリストが定義されたか
- ゲート#2（段階11）: 実際のレビュー結果が集約されたか
- ゲート#3（段階13）: 改善提案が次期運用に反映される予定か

---

## ファイル命名規約

| 項目 | 規約 | 例 |
| --- | --- | --- |
| SKILL.md | 固定 | SKILL.md |
| Sub-skills | phase1-[KEY].md, phase2-[KEY].md, phase3-[KEY].md, phase4-reporting.md | phase1-discovery.md |
| Assets | [SKILL_NAME]-log-template.md | test-strategy-log-template.md |
| References | {flowchart-best-practices, investigation-checklist, testcase-template}.md | flowchart-best-practices.md |
| Runbook | runbook.md | runbook.md |
| Category README | 固定 | README.md（カテゴリディレクトリ直下） |

---

## チェックポイント（実装終了時）

- [ ] SKILL.md frontmatter が正しい
- [ ] Phase 1～4 が定義されている
- [ ] ゲート条件 #1/#2/#3 が明確である
- [ ] sub-skills/ に 4つのファイルが存在する
- [ ] assets/, references/ が整備されている
- [ ] runbook.md またはそれに相当する詳細手順書が作成されている
- [ ] [カテゴリ]/README.md に新規スキルが追記されている
- [ ] 対象カテゴリの `README.md`（例: `skills/020_design-and-implementation/README.md`）の Skill 一覧に「現在運用中」として記載されている

---

## 進め方のコツ

1. **パターン選択を大切に**: 時間をかけてパターンを選択すると、後の実装が加速します
2. **既存例を参照**: feature-impl と defect-repair の実装を事前に読むと、構造が理解しやすくなります
3. **Phase ごとに補助ファイルを作成**: テンプレートと並行して、チェックリスト・参考資料を準備
4. **ゲート条件から逆算**: 各ゲート条件から、その段階で何が必要かを把握し、sub-skills を充実させる
5. **運用を意識**: 机上の計画ではなく、実際に運用されることを想定した記述

---

## FAQ

**Q: パターン A/B 以外に新しいパターンを追加したい場合は?**

- A: 既存の3パターンで標準形スキル15件すべてをカバーできます。標準形に当てはまらない場合は、まず [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) の「パターン外: 構造例外」を確認してください（known-how-ingestion, ddd-ai-responsibility が該当）。新規パターンが必要な場合は、対応するスキルと共に提案してください。

**Q: sub-skills の 4ファイルを分割せず、SKILL.md に統合することはできるか?**

- A: 可能ですが、運用上は分割（sub-skills/）が推奨されます。理由: ファイルサイズ管理、段階ごとの更新管理、ユーザーが実施段階の手順をすぐに参照できる。

**Q: ゲート条件は段階7, 11, 13 で固定か?**

- A: はい。フレームワークの一貫性を保つため、全スキルで固定です。ゲート間の流れと出力物を customization してください。

**Q: runbook はSKILL.md に含めるか、別ファイルか?**

- A: 別ファイルを推奨。理由: 詳細手順・判定基準は頻繁に更新され、SKILL.md（概要）と分離することでメンテナンス効率が向上します。

---

## 成果物の構造ベストプラクティス

スキル実行時に生成される成果物（実行ログ・フェーズ出力物）の命名規則・保存場所・ファイル内構成を統一するためのガイドラインです。

> **関連ドキュメント**: 保存手順の詳細は [skills/README.md の「使い方ガイド（統合版）」](../README.md) を参照。

---

### 用語定義

| 用語 | 定義 | 例 |
| --- | --- | --- |
| **成果物** (deliverable) | スキル実行全体を通じて最終的に提出・共有するドキュメント | API契約書、テスト計画書、ADR |
| **出力物** (phase output) | 各フェーズ完了時に必須となる中間生成物。ゲート審査の対象 | Phase 1 調査レポート、差分サマリ |
| **実行ログ** (execution log) | AI・開発者が各段階で記録するトレーサビリティ情報 | TRK-YYYY-MM-DD-NNN エントリ |

---

### 保存場所の規約

実行ログは、**作業リポジトリ（スキルライブラリではなくプロジェクト側）** の `docs/skill-logs/` 直下に、スキルごとに1ファイルで保存します。

```text
docs/
  skill-logs/
    <skill_name>_${DATE}.md          ← 実行ログ（スキル単位・追記型）
```

`<skill_name>` はスキルフォルダ名（kebab-case）を snake_case に変換した文字列です。`${DATE}` の書式や複数対象を区別するための追加セグメント（例: `${CATEGORY}`）は各 Skill の SKILL.md「記録・証跡」節が確定名を持ちます（例: `docs/skill-logs/defect_repair_${CATEGORY}_${DATE}.md`、`docs/skill-logs/test_strategy_${DATE}.md`）。

> **ファイル作成タイミング**: スキルを**初めて実行するとき**に対象ファイルを作成する。事前に空ファイルを用意する必要はない。  
> **原則**: 実行ログは **append-only**。既存エントリを削除・上書きしてはならない。

---

### 命名規則

| ファイル種別 | パターン | 例 |
| --- | --- | --- |
| 実行ログ | `<skill_name>_${DATE}.md` | `defect_repair_DB_20260327.md` |
| 日付 | スキル実行を**開始した日付**（`YYYYMMDD`） | — |
| 完全パス例 | `docs/skill-logs/<skill_name>_${DATE}.md` | `docs/skill-logs/api_contract_design_20260329.md` |

---

### ファイル内の必須構成

実行ログは1ファイルにヘッダ情報＋段階ごとの記録＋ゲート判定記録を集約します（別ファイルへの分離は行わない）。具体的な構成は各 Skill の `assets/<skill-name>-log-template.md` を正としますが、共通の最小セットは以下の通りです。

```markdown
# <Skill名> ログ（YYYY-MM-DD）

## 基本情報
| 項目 | 内容 |
|------|------|
| 対象 | |
| 開始日時 | |
| 担当 | |
| ステータス | |

## 段階X: [名称]（YYYY-MM-DD HH:mm:ss +09:00）

### 実施内容
- 

### 判定根拠
- 

### 承認ステータス
- 未承認 / 承認済

### TRK/EX
- TRK-xxx

## ゲート条件 #N 判定（日時）

### 判定項目
- [ ] 項目1

### 判定結果
- 承認 / 差戻し

### 承認者
- [開発者名]
```

各ゲート承認（段階7, 11, 13）では「承認ステータス: 承認済」と「承認者」を必ず記録する。

---

### フェーズ別最小出力物

各フェーズ完了時に ✅ を埋めることで、ゲート通過の可否を判断する。  
具体的な出力物チェックリストは [sub-skills.md](./skill/sub-skills.md) の「Phase N 完了時の最小必須出力」セクションを参照。

| フェーズ | 最小必須出力物（代表例） | ゲート |
| --- | --- | --- |
| Phase 1 | 調査・分析レポート、現状まとめ、対応案（3案以上） | — |
| Phase 2 | 方針決定ドキュメント、変更ファイル一覧、差分サマリ | ゲート#1（段階7） |
| Phase 3 | テスト項目承認記録、テスト実行結果レポート | ゲート#2（段階11）、ゲート#3（段階13） |
| Phase 4 | 最終報告書、改善提案、ナレッジ登録 | — |

---

**共通テンプレート公開日**: 2026-03-28  
**参照**: [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) → [SKILL.md](./skill/SKILL.md) → [sub-skills.md](./skill/sub-skills.md)
