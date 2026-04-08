# 使い方ガイド — GitHub リポジトリを活用した Skill 配布と開発プロセスへの導入

このガイドでは、`dev-process-skill-library` を GitHub リポジトリとして管理し、  
チームの開発プロセスに Skill を取り込むまでの一連の流れを説明します。

---

## 全体フロー

```text
[1] リポジトリをチームに配布
        ↓
[2] 開発タスクで適用する Skill を選ぶ
        ↓
[3] SKILL.md を読んでフェーズを実行する
        ↓
[4] 承認ゲートを通過させる
        ↓
[5] ログ・出力物を残す
        ↓
[6] 改善点があれば Skill を更新申請する
```

---

## Step 1: リポジトリをチームに配布する

### 1-1. リポジトリを Fork またはクローンする

**組織リポジトリとして使う場合（推奨）:**

```bash
# GitHub 上で組織にリポジトリを作成し、Push
git clone https://github.com/<your-org>/dev-process-skill-library.git
```

**個人・プロジェクト単位で使う場合:**

```bash
git clone https://github.com/<source-org>/dev-process-skill-library.git
cd dev-process-skill-library
git remote set-url origin https://github.com/<your-org>/dev-process-skill-library.git
git push -u origin main
```

### 1-2. チームメンバーにアクセス権を付与する

| ロール | GitHub 権限 | 対象 |
|---|---|---|
| Owner / Maintainer | Write | 開発リード、プロセス担当 |
| Reviewer | Write | PM、QA |
| Approver | Admin または別承認フロー | 開発マネージャー |
| 参照のみ | Read | チームメンバー全員 |

### 1-3. チームへの周知

- リポジトリ URL を Slack / Teams / Wiki などで共有する
- [README.md](../README.md) の「Skill 選択ガイド」を入口として案内する
- まず [pilot-guide.md](./pilot-guide.md) を対象チームに共有し、試験導入から始める

---

## Step 2: 開発タスクに合う Skill を選ぶ

タスクの性質に応じて以下を参照します。

| 状況 | 使う Skill |
|---|---|
| 不具合が発生した | `verification-and-quality/defect-repair-unified` |
| 新機能・仕様変更の実装 | `design-and-implementation/feature-implementation-unified` |
| 要件からデータモデルを設計したい | `design-and-implementation/data-model-design-unified` |
| テスト方針を整備したい | `verification-and-quality/test-strategy-unified` |
| 要件・受入条件が曖昧 | `requirements-and-planning/requirements-refinement` |
| 設計判断を記録したい | `design-and-implementation/architecture-decision-record` |
| リリース前にチェックしたい | `operations-and-release/release-readiness` |
| セキュリティ観点を強化したい | `verification-and-quality/security-hardening` |
| 性能ボトルネックを調査したい | `operations-and-release/performance-investigation` |
| 障害の振り返りをしたい | `learning-and-improvement/incident-postmortem` |

複数の状況が重なる場合は、[taxonomy.md](./taxonomy.md) の依存関係を参照して上流から適用します。

---

## Step 3: SKILL.md を読んでフェーズを実行する

各 Skill フォルダの `SKILL.md` が実行手順の正本です。

```
skills/
  <カテゴリ>/
    <skill-name>/
      SKILL.md        ← 実行手順・承認ゲート・完了条件
      runbook.md      ← 具体的操作手順（ある場合）
      references/     ← 参考資料
      assets/         ← テンプレート・チェックリスト
```

### SKILL.md の読み方

```markdown
---
name: skill-name
description: ...
category: <カテゴリ>
---

## Phase 1: ...
## Phase 2: ...
## 承認ゲート
## 完了条件
## 出力物テンプレート
```

- **Phase を上から順に実行する**（並行実行しない）
- **承認ゲートに到達したら必ずレビュワーに諮る**
- **完了条件をすべて満たしてから次のタスクに進む**

---

## Step 4: 承認ゲートを通過させる

承認ゲートは `未承認 / 承認済` の2値で管理します。

### 承認フローの例（GitHub PR を活用）

```text
1. Skill の実行ログ・出力物を作業ブランチにコミット
2. PR を作成し、Reviewer をアサイン
3. Reviewer がレビュー・コメント
4. Approver が承認（Approve）
5. main にマージ → 承認済として記録
```

PR の説明欄には以下を記載します：

```markdown
## 実施した Skill
- skills/<カテゴリ>/<skill-name>

## 承認ゲート通過確認
- [ ] Phase X 完了
- [ ] 受入条件との対応関係を確認済み
- [ ] 出力物テンプレート準拠

## 判断根拠（日時・判断者・根拠）
- 日時: YYYY-MM-DD
- 判断者: @username
- 根拠: ...
```

---

## Step 5: ログ・出力物を残す

実行ログは append-only で記録し、既存履歴の削除・上書きは禁止します。  
詳細は [glossary.md](/skills/shared-references/glossary.md#append-only) の用語定義を参照してください。

### 推奨ファイル配置

```
（作業リポジトリ側）
docs/skill-logs/
  <category>/                      ← スキルカテゴリ名（taxonomy と同じ5カテゴリ）
    <skill-name>/                  ← スキルディレクトリ名
      YYYY-MM-DD_log.md            ← 実行ログ
      YYYY-MM-DD_output.md         ← 出力物（報告書・ADR・テスト計画など）
```

例: `docs/skill-logs/design-and-implementation/api-contract-design/2026-03-29_log.md`

フォルダはスキルを**初めて実行するとき**に作成する（事前に空フォルダを用意しない）。  
`<category>` の値は [taxonomy.md](./taxonomy.md) の5カテゴリ名を使用する。

### ログの最小記録単位

```markdown
## 実行概要
- 日時: YYYY-MM-DD
- 担当: @username
- 使用 Skill: skills/<カテゴリ>/<skill-name>

## 決定事項
- ...

## 未解決事項
- ...

## 次アクション
- ...
```

---

## Step 6: Skill を更新申請する

実運用で改善点が見つかった場合は、`dev-process-skill-library` リポジトリに Issue または PR を作成します。

### Issue で申請する場合

1. リポジトリの **Issues > New Issue** を開く
2. 申請内容を記載する（改善理由・変更箇所・期待効果）
3. ラベル `skill:update` と優先度ラベルを付与する
4. Owner / Reviewer をアサインする

### PR で直接変更する場合

```bash
# 作業ブランチを作成
git checkout -b update/<skill-name>-<内容の要約>

# SKILL.md を編集
# ...

# コミット・プッシュ
git add skills/<カテゴリ>/<skill-name>/SKILL.md
git commit -m "feat(<skill-name>): <変更内容の要約>"
git push origin update/<skill-name>-<内容の要約>
```

PR 作成後、Reviewer のレビューと Approver の承認を経てマージします。

---

## よくある質問（FAQ）

**Q. どの Skill から使い始めればよいですか？**  
A. [README.md の「推奨導入順」](../README.md#推奨導入順) に従い、  
まず `test-strategy-unified` と `requirements-refinement` を試験導入することを推奨します。

**Q. 複数の Skill を同時に使ってもよいですか？**  
A. 同時並行は避け、タスクごとに適用 Skill を1つ選んで完了させてから次に進んでください。  
ただし、上流 Skill の出力を下流 Skill のインプットとして使う連鎖適用は問題ありません。

**Q. Skill に書かれていない手順が必要になったときは？**  
A. Skill の参照優先順位（実装実体 > runbook > SKILL.md > ログ）に従い判断します。  
必要であれば Issue で Skill への追記を申請してください。

**Q. 承認者がすぐに対応できない場合は？**  
A. [README.md の「優先度例外運用」](../README.md#優先度例外運用) を参照し、  
例外申請として Owner が承認・記録した上で暫定実行できます。

---

## 関連ドキュメント

- [taxonomy.md](./taxonomy.md) — Skill 分類体系
- [scoring-guide.md](./scoring-guide.md) — 優先度採点ガイド
- [pilot-guide.md](./pilot-guide.md) — 試験導入ガイド
- [quiz-sync-guide.md](./quiz-sync-guide.md) — spa-quiz-app のクイズ同期基準
- [quiz-update-summary-2026-03-29.md](./quiz-update-summary-2026-03-29.md) — 2026-03-29 のクイズ反映サマリ
- [../governance/exception-log.md](../governance/exception-log.md) — 例外運用ログ
- [../README.md](../README.md) — ライブラリ全体概要
