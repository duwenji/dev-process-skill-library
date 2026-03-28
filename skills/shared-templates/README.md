# shared-templates

このディレクトリは、全14スキルの実装を支援するための共通テンプレートと設計ガイドです。

## ファイル一覧

| ファイル | 目的 | 対象者 |
|---------|------|--------|
| [SKILL-template.md](./SKILL-template.md) | 新規スキルの SKILL.md テンプレート（カスタマイズ ポイント埋め込み） | スキル実装担当者 |
| [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) | 3パターン（調査・検証/意思決定・設計/運用手続き）の選択と customization checklist | スキル実装 PM |
| [SUB-SKILLS-TEMPLATE.md](./SUB-SKILLS-TEMPLATE.md) | Phase 1～4 の sub-skills ファイル（phase1-*.md ～ phase4-*.md）テンプレート | スキル実装担当者 |

---

## 使用フロー（新規スキル実装時）

### Step 1: パターン選択
1. [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) を読む
2. スキルの目的から、パターン A/B/C のいずれかを選択
3. Customization Checklist を参照し、必須項目を把握

### Step 2: SKILL.md テンプレートの fill-in
1. [SKILL-template.md](./SKILL-template.md) をコピーして、スキルディレクトリに配置
2. frontmatter を埋める（name, description, argument-hint等）
3. 各 Phase のタイトル・段階説明を、選択パターンに合わせて customization
4. ゲート条件#1, #2, #3 を明確に記述

### Step 3: Sub-skills ファイルの作成
1. [SUB-SKILLS-TEMPLATE.md](./SUB-SKILLS-TEMPLATE.md) を参照
2. phase1-[KEY].md, phase2-[KEY].md, phase3-[KEY].md, phase4-reporting.md を作成
3. 各フェーズの目的・実施段階・出力物を記述

### Step 4: 参照ファイルの整備
1. assets/[SKILL_NAME]-log-template.md を作成（運用実行ログテンプレート）
2. 共通参照 `../shared-references/` を優先利用:
  - flowchart-best-practices.md
  - investigation-checklist.md
  - testcase-template.md
3. Skill固有の追加資料が必要な場合のみ references/ を作成
3. runbook.md を作成（詳細手順・判定基準）

### Step 5: QA Checklist
1. [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) の "Quality Assurance Checklist" で検証
2. frontmatter, Phase, ゲート条件, sub-skills, references が完備されているか確認

---

## リソース ファイル

### 既存テンプレートの参照

#### feature-implementation-unified から学ぶ
- Path: `skills/design-and-implementation/feature-implementation-unified/`
- Pattern: **パターン B**（意思決定・設計型）の標準例
- 参照すべき file:
  - [SKILL.md](../design-and-implementation/feature-implementation-unified/SKILL.md) - フロー定義
  - [sub-skills/](../design-and-implementation/feature-implementation-unified/sub-skills/) - Phase ごと詳細
  - [assets/implementation-log-template.md](../design-and-implementation/feature-implementation-unified/assets/implementation-log-template.md) - 実行ログ形式

#### defect-repair-unified から学ぶ
- Path: `skills/verification-and-quality/defect-repair-unified/`
- Pattern: **パターン A**（調査・検証型）の標準例
- 参照すべき file:
  - [SKILL.md](../verification-and-quality/defect-repair-unified/SKILL.md) - フロー定義
  - [sub-skills/](../verification-and-quality/defect-repair-unified/sub-skills/) - Phase ごと詳細
  - [assets/defect-log-template.md](../verification-and-quality/defect-repair-unified/assets/defect-log-template.md) - 実行ログ形式
  - [remediation-runbook.md](../verification-and-quality/defect-repair-unified/remediation-runbook.md) - 詳細手順

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
- ゲート#1（段階7）: 요지가 明確で受入条件が定量的か
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
|------|------|-----|
| SKILL.md | 固定 | SKILL.md |
| Sub-skills | phase1-[KEY].md, phase2-[KEY].md, phase3-[KEY].md, phase4-reporting.md | phase1-discovery.md |
| Assets | [SKILL_NAME]-log-template.md | test-strategy-log-template.md |
| References | {flowchart-best-practices, investigation-checklist, testcase-template}.md | flowchart-best-practices.md |
| Runbook | [runbook-[SKILL_NAME].md](runbook-[SKILL_NAME].md) または remediation-runbook.md | runbook-security-hardening.md |
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
- [ ] [トップレベル README.md](../../../README.md) の「現在運用中」/「提案」に記載されている

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
- A: 既存の2パターンで 12/14 スキルをカバーできます。パターン C（運用手続き型）も追加済み。新規パターンが必要な場合は、対応するスキルと共に提案してください。

**Q: sub-skills の 4ファイルを分割せず、SKILL.md に統合することはできるか?**
- A: 可能ですが、運用上は分割（sub-skills/）が推奨されます。理由: ファイルサイズ管理、段階ごとの更新管理、ユーザーが実施段階の手順をすぐに参照できる。

**Q: ゲート条件は段階7, 11, 13 で固定か?**
- A: はい。フレームワークの一貫性を保つため、全スキルで固定です。ゲート間の流れと出力物を customization してください。

**Q: runbook/remediation-runbook はSKILL.md に含めるか、別ファイルか?**
- A: 別ファイルを推奨。理由: 詳細手順・判定基準は頻繁に更新され、SKILL.md（概要）と分離することでメンテナンス効率が向上します。

---

**共通テンプレート公開日**: 2026-03-28  
**参照**: [PATTERN-SELECTION-GUIDE.md](./PATTERN-SELECTION-GUIDE.md) → [SKILL-template.md](./SKILL-template.md) → [SUB-SKILLS-TEMPLATE.md](./SUB-SKILLS-TEMPLATE.md)
