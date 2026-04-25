# Mermaid かように作成ガイドライン（Flowchart Best Practices）

本ガイドは、Phase 1「調査・分析・対応案決定」（段階5）で使用する処理フロー図（Mermaid形式）の作成基準です。

## 利用方法
- **対象**: 段階5「AI が処理フロー図を生成」で、統一的な図表を作成
- **出力形式**: Mermaid Flowchart（Markdown コード ブロック内）
- **粒度**: 「不具合発生点⇔対応前後の差」レベル

---

## フロー設計の基本原則

### 1. 目的の明確化

#### 対応前フロー（問題がある処理）
```
目的: 不具合が発生する処理パスを可視化
内容: 
  - 入力から不具合発生点まで
  - 分岐・条件判定
  - 例外ハンドリング（あれば）
```

#### 対応後フロー（改修後の処理）
```
目的: 不具合を回避する改善方法を可視化
内容:
  - 対応前フロー との差分
  - 追加の判定ステップ
  - エラーハンドリングの強化
```

---

## ノード・シェイプの使い分け

### ノード種別と記号

| ノード種別 | Mermaid 記号 | 用途 | 例 |
|----------|------------|------|-----|
| **処理** | `[テキスト]` | 通常の処理ステップ | `[DB読取]` |
| **判定** | `{テキスト}` | 条件分岐 Yes/No | `{NULL判定}` |
| **データ** | `([テキスト])` | 入出力データ | `([DS Telegram])` |
| **開始/終了** | `([テキスト])` / `([終了])` | フロー始終 | `([開始])` |
| **エラー** | `❌ テキスト` | 例外発生 | `❌ InvalidCastException` |
| **結果** | `✓ テキスト` | 正常終了 | `✓ Result出力` |

### カラーリング（視認性向上）

```
スタイル付与例:
- 問題がある箇所: 赤
- 改修後の追加処理: 青/緑
- 判定ポイント: 黄
```

**Mermaid CSS サンプル**:
```mermaid
flowchart TD
    A["開始"] --> B{判定}
    B -->|Yes| C["✓ 正常"]
    B -->|No| D["❌ エラー"]
    
    style D fill:#FF6B6B
    style C fill:#51CF66
```

---

## 対応前フロー（Problem Flow）の設計

### 例：DBNull 起因のキャスト例外

```mermaid
flowchart TD
    A["DS Telegram受信"]
    B{Parse成功?}
    C["ログ出力<br/>skip"]
    D["DataRecord取得"]
    E["直接キャスト<br/>(string)dataRecord"]
    F["❌ InvalidCastException<br/>DISPLAY_NAME=NULL時"]
    G["エラーログ出力"]
    H["プロセス異常終了"]
    
    A --> B
    B -->|No| C
    B -->|Yes| D
    D --> E
    E -->|NULLを含む| F
    F --> G
    G --> H
    
    style F fill:#FF6B6B
    style H fill:#FF6B6B
```

### ポイント

1. **エントリーポイント明示** — 「DS Telegram受信」から開始
2. **分岐の可視化** — Parse成功/失敗 の分岐
3. **問題箇所の強調** — ❌ マーク + 赤色で不具合点を明示
4. **結果の表示** — 「プロセス異常終了」で終了状態を明確化

---

## 対応後フロー（Solution Flow）の設計

### 同じシナリオで改修後

```mermaid
flowchart TD
    A["DS Telegram受信"]
    B{Parse成功?}
    C["ログ出力<br/>skip"]
    D["DataRecord取得"]
    E["DataRecordExtensions<br/>使用"]
    F["GetStringOrNull()<br/>で読取"]
    G{NULL?}
    H["デフォルト値<br/>を適用<br/>eg: '(未設定)'"]
    I["✓ 正常処理"]
    J["Result出力"]
    
    A --> B
    B -->|No| C
    B -->|Yes| D
    D --> E
    E --> F
    F --> G
    G -->|Yes| H
    G -->|No| I
    H --> I
    I --> J
    
    style E fill:#4ECDC4
    style H fill:#4ECDC4
    style J fill:#51CF66
```

### ポイント

1. **対応前との差分を強調** — 青色で新規ステップ（DataRecordExtensions, NULL判定, デフォルト値）
2. **分岐の詳細化** — NULL判定による分岐を明示
3. **正常終了の明確化** — ✓ マーク + 緑色
4. **入出力の一貫性** — 対応前フローと同じ入出力

---

## 複数対応案のフロー表現

対応案が複数ある場合、各案の差分を並べて比較することが効果的です。

### 対応案1: DataRecordExtensions置換

```mermaid
flowchart TD
    D["DataRecord取得"]
    E1["DataRecordExtensions<br/>使用"]
    F1["GetStringOrNull()"]
    G1{NULL?}
    H1["既定値適用"]
    
    D --> E1
    E1 --> F1
    F1 --> G1
    G1 -->|Yes| H1
    
    style E1 fill:#4ECDC4
    style H1 fill:#4ECDC4
```

### 対応案2: 層厚い NULL判定

```mermaid
flowchart TD
    D["DataRecord取得"]
    E2["ValidateDataRecord()<br/>新規メソッド"]
    F2["全列の NULL判定"]
    G2{不正判定}
    H2["ログ出力<br/>skip"]
    
    D --> E2
    E2 --> F2
    F2 --> G2
    G2 -->|Yes| H2
    
    style E2 fill:#FFD93D
    style F2 fill:#FFD93D
```

---

## フロー図に含むべき要素

### 必須要素
- [ ] 開始ポイント（入力データ）
- [ ] 主要業務ロジック（核となる処理）
- [ ] 判定・分岐ポイント
- [ ] エラーハンドリング箇所
- [ ] 終了ポイント（出力 / 正常 / 異常）

### 推奨要素
- [ ] ポイントごとのアノテーション（重要な条件等）
- [ ] データの型情報（必要に応じて）
- [ ] スループット / タイムアウト情報（パフォーマンス観点）
- [ ] インタフェース先（他プロセス、DB等）

### 省略してよい要素
- [ ] 内部ルーチン詳細（別途フロー図があれば）
- [ ] 変数名（処理内容で十分）
- [ ] UI要素（非ビジュアルシステムの場合）

---

## Mermaid コード例（テンプレート）

### 最小限のテンプレート

```mermaid
flowchart TD
    A["開始"]
    B["処理1"]
    C{判定}
    D["処理2A"]
    E["処理2B"]
    F["終了"]
    
    A --> B
    B --> C
    C -->|条件1| D
    C -->|条件2| E
    D --> F
    E --> F
```

### リッチな装飾例

```mermaid
flowchart TD
    Start(["◆ 入力: Telegram"]) --> Parse["Parse処理"]
    Parse --> Check{フォーマット<br/>OK?}
    
    Check -->|Yes| Extract["DataRecord抽出"]
    Check -->|No| Error1["❌ Parse失敗"]
    
    Extract --> NullCheck{NULL値<br/>チェック}
    NullCheck -->|含む| Apply["デフォルト値<br/>適用"]
    NullCheck -->|なし| Process["正常処理"]
    
    Apply --> Process
    Process --> Output(["◆ 出力: Result"])
    
    Error1 --> Log["ログ出力"]
    Log --> End(["◆ 終了"])
    
    style Start fill:#E8F4F8
    style Output fill:#E8F4F8
    style End fill:#FFE8E8
    style Error1 fill:#FF6B6B
    style Apply fill:#4ECDC4
    style Process fill:#51CF66
```

---

## よくある間違いと改善例

### ❌ 間違い 1: ノードが大きすぎる

```mermaid
flowchart TD
    A["DB アクセス、DataRecord 取得、NULL判定、キャスト実行、エラーハンドリング"]
```

### ✓ 改善: 適切な粒度に分割

```mermaid
flowchart TD
    A["DB アクセス"]
    B["DataRecord 取得"]
    C{NULL判定}
    D["キャスト実行"]
    E["エラーハンドリング"]
    
    A --> B
    B --> C
    C -->|Yes| E
    C -->|No| D
```

---

### ❌ 間違い 2: 線が複雑に絡んでいる

```mermaid
flowchart TD
    A --> B
    A --> C
    B --> D
    C --> D
    D --> E
    E --> B  # 循環
    E --> F
```

### ✓ 改善: 階層を明確に

```mermaid
flowchart TD
    A["入力"]
    B{条件A}
    C{条件B}
    D["出力"]
    
    A --> B
    B -->|Yes| C
    B -->|No| D
    C -->|Yes| D
    C -->|No| D
```

---

### ❌ 間違い 3: 凡例がない

（フロー図内に色や記号の意味が不明）

### ✓ 改善: 凡例を添付

```
凡例:
- 赤（❌）= エラー発生点
- 青/緑 = 改修後の追加処理
- 黄 = 判定ポイント
- ✓ = 正常終了
```

---

## フロー図の命名・保管

### ファイル形式
- **Markdown コード ブロック内**に `mermaid` 言語タグで記載
- **docs/skill-logs/ ログファイル**に embed、または **separate markdown ファイル**として保管

### 命名規則

```
flowchart_[対象モジュール]_[対応前or対応后].md

例:
flowchart_ProcDataSync_before.md
flowchart_ProcDataSync_after_plan1.md
```

### 参照関係

```
Phase 1 Sub-Skill (phase1-investigation.md)
  └─> 段階5 で 対応前フロー + 対応後フロー(複数案) を生成
  └─> docs/skill-logs/ ログ に embed
```

---

## チェックリスト: フロー図の品質確認

デプロイ前に以下をチェックしてください:

- [ ] 開始ポイント（入力）が明示されているか
- [ ] 終了ポイント（出力 or エラー）が明示されているか
- [ ] 不具合発生点が ❌ で強調されているか（対応前フロー）
- [ ] 改修内容が色分けで強調されているか（対応後フロー）
- [ ] 判定ポイント が全て {Yes/No} で明示されているか
- [ ] 線が複雑に絡んでいないか（メンテナンス性）
- [ ] ノードの粒度が統一されているか
- [ ] 凡例（色、記号の意味）が明確か
- [ ] Markdown がレンダリングされるか（確認）
- [ ] 対応案複数の場合、各案の差分が可視化されているか

---

## 参考リソース

### Mermaid 公式ドキュメント
- https://mermaid.js.org/syntax/flowchart.html
- Flowchart 構文、スタイリング、サブグラフ等の詳細

### よくある Mermaid スタイル

```mermaid
flowchart TD
    subgraph 正常系["正常系処理"]
        A["入力"]
        B["処理"]
        C["✓ 出力"]
    end
    
    subgraph 異常系["異常系処理"]
        D["❌ エラー"]
        E["ログ出力"]
    end
    
    A --> B
    B --> C
    B --> D
    D --> E
    
    style 正常系 fill:#E8F8E8
    style 異常系 fill:#FFE8E8
```

---

**最終更新**: 2026-03-27  
**ガイドライン版**: 1.0
