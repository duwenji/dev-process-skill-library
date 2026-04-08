# 実装ログテンプレート（Implementation Log Template）

省略用語（RACI, KPI, ADR, DDL, SLO, QA, PM, TRK, EX）は [../../../shared-references/glossary.md](../../../shared-references/glossary.md) の『略語・日本語対応表』を参照してください。


本テンプレートは、新規機能・機能変更対応の監査最小セットを定義します。

## ログファイル命名規則

```text
AI改善/feature_implementation_${DATE}.md
例: AI改善/feature_implementation_20260327.md
```

## Append-Only運用ルール
1. 対応開始時に新規ファイルを作成
2. 段階完了ごとに末尾へ追記
3. 既存記録の削除・上書きは禁止
4. 誤記修正は「訂正」セクションを追記

## ログファイルヘッダ

```markdown
# 機能対応ログ（YYYY-MM-DD）

## 基本情報
| 項目 | 内容 |
|------|------|
| 要求ID | |
| タイトル | |
| 対象 | |
| 開始日時 | |
| 担当 | |
| ステータス | |
```

## 段階別テンプレート

```markdown
## 段階X: [名称]（YYYY-MM-DD HH:mm:ss +09:00）

### 実施内容
- 

### 判定根拠
- 

### 承認ステータス
- 未承認 / 承認済

### TRK/EX
- TRK-xxx
```

## ゲート判定テンプレート

```markdown
## ゲート条件 #N 判定（日時）

### 判定項目
- [ ] 項目1
- [ ] 項目2

### 判定結果
- 承認 / 差戻し

### 承認者
- [開発者名]
```
