# テストケーステンプレート（Test Case Template）

本テンプレートは、Phase 3「テスト・チェック」（段階10-13）で使用するテストケース記載フォーマットです。

## 利用方法
- **対象**: 段階10「AI がチェック項目を生成」で、統一的なテストケース仕様書を作成
- **출력形式**: Markdown テーブル + C# コード例
- **記録方法**: Phase 3 Sub-Skill および AI改善/ ログに記入

---

## テストケース基本フォーマット

### ヘッダ（共通情報）

```markdown
## テストケース群: [機能名 / メソッド名]

**対象**: [改修点の簡潔な説明]

**実施日**: [テスト実行予定日]

**テスト環境**: Windows Server 2022, .NET 8.0, PostgreSQL 15.x

**担当**: AI (自動 / ダミー実装 / 手動別の実施者)

**参照**:
- [業務仕様書...]
- [DDL: テーブル名...]
- [改修PR...]
```

---

## テストケーステーブル形式

```markdown
| TRK | 分類 | テストケース名 | 前提条件 | 入力値 | 期待結果 | 実施方法 | 備考 |
|-----|------|--------------|--------|--------|---------|---------|------|
| TRK-001 | 正常系 | [テストケース] | [前提] | [入力] | [期待] | 自動/手動 | |
| TRK-002 | 境界値 | [テストケース] | [前提] | [入力] | [期待] | 自動/手動 | |
```

### カラム説明

| カラム | 説明 |
|--------|------|
| **TRK** | Tracking ID（段階14まで追跡するための一意ID） |
| **分類** | 正常系 / 境界値 / 異常系 / 統合 / 回帰 のいずれか |
| **テストケース名** | テスト内容を簡潔に（例: "NULL値で処理") |
| **前提条件** | DB 状態やモック設定等、テスト前の準備 |
| **入力値** | テスト時に与えるパラメータ値 |
| **期待結果** | テスト合格の条件（具体的に） |
| **実施方法** | 自動（ユニットテスト）/ ダミー実装 / 手動 のいずれか |
| **備考** | テスト対象のメソッド名、特記事項等 |

---

## テストケース設計の観点

### 1. 正常系（Happy Path）

```markdown
| TRK-001 | 正常系 | 全フィールド有効値で処理実行 | 
| 前提: DB に有効なデータが存在 | 
| 入力: OwnerName="太郎", RegNumber="ABC-123", ResidenceCode=100 | 
| 期待結果: 車両情報が正常に処理される, ログに「処理成功」出力 | 
| 実施方法: 自動（既存テスト利用可） | 
| 備考: 回帰テスト対象 |
```

**ユニットテスト（C#）例**:
```csharp
[Test]
public void ProcessVehicleData_ValidData_Success()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r["OWNER_NAME"]).Returns("太郎");
    mockRecord.Setup(r => r["REG_NUMBER"]).Returns("ABC-123");
    mockRecord.Setup(r => r["RESIDENCE_CODE"]).Returns(100);
    
    var processor = new VehicleProcessor(_mockLogger);
    
    // Act
    var result = processor.ProcessVehicleData(mockRecord.Object);
    
    // Assert
    Assert.That(result.Status, Is.EqualTo(ProcessStatus.Success));
    Assert.That(result.Vehicle.OwnerName, Is.EqualTo("太郎"));
}
```

---

### 2. 境界値（Boundary Values）

境界値テストでは、**NULL / 空文字列 / 最大値 / 最小値** 等を検証します。

#### 例1: NULL 値テスト

```markdown
| TRK-002 | 境界値 | OWNER_NAME が NULL の場合の処理 | 
| 前提: DB の OWNER_NAME がNULL | 
| 入力: OwnerName=NULL (DBNull), RegNumber="XYZ-789", ResidenceCode=50 | 
| 期待結果: デフォルト値「不明」を適用、正常処理継続 | 
| 実施方法: 自動（新規テスト） | 
| 備考: DataRecordExtensions.GetStringOrNull() の動作確認 |
```

**テストコード**:
```csharp
[Test]
public void ProcessVehicleData_NullOwnerName_ApplyDefault()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r["OWNER_NAME"]).Returns(DBNull.Value);
    mockRecord.Setup(r => r["REG_NUMBER"]).Returns("XYZ-789");
    mockRecord.Setup(r => r["RESIDENCE_CODE"]).Returns(50);
    
    var processor = new VehicleProcessor(_mockLogger);
    
    // Act
    var result = processor.ProcessVehicleData(mockRecord.Object);
    
    // Assert
    Assert.That(result.Vehicle.OwnerName, Is.EqualTo("不明"));
    Assert.That(result.Status, Is.EqualTo(ProcessStatus.Success));
}
```

#### 例2: 複合 NULL テスト

```markdown
| TRK-005 | 境界値 | 複数フィールドが NULL の場合の処理 | 
| 前提: DB で複数列が NULL | 
| 入力: OwnerName=NULL, RegNumber=NULL, ResidenceCode=0 | 
| 期待結果: 各フィールドにデフォルト値を適用、正常処理継続 | 
| 実施方法: 自動（新規テスト） | 
| 備考: 複合条件の検証 |
```

---

### 3. 異常系（Error Cases）

エラーハンドリングが適切に機能することを検証します。

#### 例1: 型不一致エラー

```markdown
| TRK-006 | 異常系 | RESIDENCE_CODE に文字列が入っている場合 | 
| 前提: 不正な型データが DB に挿入されている（想定外） | 
| 入力: ResidenceCode="InvalidNumber" (文字列) | 
| 期待結果: InvalidOperationException を throw / ログに「型エラー」出力 | 
| 実施方法: 自動（新規テスト） | 
| 備考: GetInt32OrNull() の型チェック動作確認 |
```

**テストコード**:
```csharp
[Test]
public void ProcessVehicleData_InvalidResidenceCodeType_ThrowException()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r["OWNER_NAME"]).Returns("太郎");
    mockRecord.Setup(r => r["REG_NUMBER"]).Returns("ABC-123");
    mockRecord.Setup(r => r["RESIDENCE_CODE"])
        .Throws(new InvalidOperationException("Cannot convert string to int"));
    
    var processor = new VehicleProcessor(_mockLogger);
    
    // Act & Assert
    var ex = Assert.Throws<InvalidOperationException>(() => {
        processor.ProcessVehicleData(mockRecord.Object);
    });
    Assert.That(ex.Message, Contains.Substring("Cannot convert"));
}
```

#### 例2: DB 接続エラー（ダミー実装）

```markdown
| TRK-007 | 異常系 | DB 接続エラー時の Exception ハンドリング | 
| 前提: Mock で SqlException を投出するよう設定 | 
| 入力: DB 接続時に SqlException が発生 | 
| 期待結果: Exception をキャッチ、エラーログ出力、上位へ伝播 | 
| 実施方法: ダミー実装（Mock利用） | 
| 備考: 実際の DB エラーシナリオを Mock で安全に再現 |
```

**テストコード（ダミー実装）**:
```csharp
[Test]
public void ProcessVehicleData_DBConnectionError_HandleException()
{
    // Arrange
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(r => r["OWNER_NAME"])
        .Throws(new SqlException("Connection timeout"));
    
    var mockLogger = new Mock<ILogger>();
    var processor = new VehicleProcessor(mockLogger);
    
    // Act & Assert
    var ex = Assert.Throws<SqlException>(() => {
        processor.ProcessVehicleData(mockRecord.Object);
    });
    
    // ログが正常に出力されたか確認
    mockLogger.Verify(
        l => l.Error(It.Is<string>(s => s.Contains("Connection timeout"))),
        Times.Once
    );
}
```

---

### 4. 統合テスト（Integration）

複数のコンポーネント / プロセス間の連携を検証します。

```markdown
| TRK-008 | 統合 | 複数の VR telegram を連続処理 | 
| 前提: テスト環境で複数 telegram がキューに溜まっている | 
| 入力: 100件の VR telegram を連続送信 | 
| 期待結果: 全 telegram が独立に処理される、メモリリークなし、処理時間 <= 5秒 | 
| 実施方法: 手動（ストレステスト） / 自動（ロード生成） | 
| 備考: パフォーマンス観点の検証 |
```

---

### 5. 回帰テスト（Regression）

既存機能が改修により影響を受けていないことを確認します。

```markdown
| TRK-009 | 回帰 | 既存テストスイートの全実行 | 
| 前提: ProcForEtgsTest.csproj に存在する全テスト | 
| 入力: `dotnet test ProcForEtgsTest.csproj` | 
| 期待結果: 全テスト PASSED (既存42件 + 新規テスト) | 
| 実施方法: 自動 | 
| 備考: 改修による副作用がないことを確認 |
```

---

## テストケース群の纏めフォーマット

```markdown
## テストケース完全一覧

### テストケース集計

| 分類 | 件数 | 実施方法別内訳 | 
|------|------|--------------|
| 正常系 | 2 | 自動2 |
| 境界値 | 3 | 自動3 |
| 異常系 | 2 | 自動1, ダミー実装1 |
| 統合 | 1 | 手動1 |
| 回帰 | 1 | 自動1 |
| **合計** | **9** | **自動7, ダミー実装1, 手動1** |

### テストケース優先度（実施順序）

**優先度高（最初に実施）**:
- TRK-001 (正常系, 既存テスト)
- TRK-009 (回帰テスト)

**優先度中（次に実施）**:
- TRK-002, TRK-003, TRK-004, TRK-005 (境界値)
- TRK-006 (異常系: 型不一致)

**優先度低（最後に実施）**:
- TRK-007 (ダミー実装, DB接続エラー)
- TRK-008 (統合テスト, 時間が掛かる)

### テストケース実施計画

**Day 1 - 自動テスト実行**:
```powershell
dotnet test ProcForEtgsTest.csproj --verbosity=detailed
# 期待: 9項目全PASSED, 処理時間 < 5分
```

**Day 2 - ダミー実装テスト**：
- TRK-007 を手動で確認
- Mock が正常に Exception を投出するか、ログが出力されるか

**Day 3 - 手動統合テスト**（必要に応じて）:
- TRK-008 を本番環境に類似した環境で実施
- パフォーマンス計測、メモリ監視
```

---

## AAA パターン（Arrange-Act-Assert）

全テストコードは以下の構造に従います:

```csharp
[Test]
public void TestMethodName_Condition_ExpectedBehavior()
{
    // ===== ARRANGE (準備) =====
    // Mock の設定、入力データの準備
    var mockRecord = new Mock<IDataRecord>();
    mockRecord.Setup(...).Returns(...);
    
    var processor = new VehicleProcessor(_mockLogger);
    
    // ===== ACT (実行) =====
    // テスト対象メソッドを呼び出し
    var result = processor.ProcessVehicleData(mockRecord.Object);
    
    // ===== ASSERT (検証) =====
    // 期待結果と実際の結果を比較
    Assert.That(result.Status, Is.EqualTo(ProcessStatus.Success));
    Assert.That(result.Vehicle.OwnerName, Is.EqualTo("期待値"));
}
```

---

## テストケース命名規則

テストメソッド名は以下の形式に従います:

```
Test[対象メソッド]_[条件]_[期待結果]

例:
- ProcessVehicleData_ValidData_Success
- ProcessVehicleData_NullOwnerName_ApplyDefault
- ProcessVehicleData_InvalidResidenceCodeType_ThrowException
- ProcessVehicleData_DBConnectionError_HandleException
```

---

## テストケース管理

### バージョン管理
```markdown
| 版 | 変更内容 | 日付 |
|----|---------|------|
| 1.0 | 初版作成（9項目） | 2026-03-27 |
```

### 追加・変更時のルール
1. 新規テストケース追加時は TRK を採番（序番で連番）
2. テストケース削除時は「削除」と明記（TRK の再利用を避ける）
3. 既存テストケースの修正時は、変更内容を「更新」で記載

---

**最終更新**: 2026-03-27  
**テンプレート版**: 1.0
