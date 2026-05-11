# Kaggle初心者向け pandas 超入門

## pandasとは？

pandasは、

> 「表データを操作するためのPythonライブラリ」

ExcelをPythonで超強化して扱うイメージ。

Kaggleではほぼ必須。

---

# 1. DataFrame（表）

```python
import pandas as pd

df = pd.DataFrame({
    "名前": ["もも", "ゆい", "あい"],
    "年齢": [25, 22, 30]
})
```

結果：

|   | 名前 | 年齢 |
|---|---|---|
|0|もも|25|
|1|ゆい|22|
|2|あい|30|

この「表」が DataFrame。

---

# 2. Series（1列）

```python
df["年齢"]
```

結果：

|   | 年齢 |
|---|---|
|0|25|
|1|22|
|2|30|

1列だけ取り出したものを Series という。

- 表全体 → DataFrame
- 1列 → Series

---

# 3. CSVを読み込む

Kaggleではまずこれ。

```python
import pandas as pd

train = pd.read_csv("/kaggle/input/xxx/train.csv")
```

CSVファイルをDataFrameとして読み込む。

---

# 4. データの確認

## head()

最初の5行を見る。

```python
train.head()
```

---

## columns

列名一覧を見る。

```python
train.columns
```

---

## shape

データサイズを見る。

```python
train.shape
```

例：

```python
(1000, 20)
```

意味：

- 1000行
- 20列

---

# 5. 列を取り出す

## 1列だけ

```python
train["Age"]
```

---

## 複数列

```python
train[["Age", "Sex"]]
```

※ `[]` が2重になる。

---

# 6. 条件で絞り込み

## 年齢20以上だけ取得

```python
train[train["Age"] >= 20]
```

意味：

```python
Age >= 20 の行だけ取得
```

---

# 7. 欠損値処理（超重要）

Kaggleでかなり重要。

## 欠損数確認

```python
train.isnull().sum()
```

各列の空欄数を見る。

---

## 平均値で埋める

```python
train["Age"] = train["Age"].fillna(train["Age"].mean())
```

意味：

```python
Age列の空欄を平均値で埋める
```

---

# 8. カテゴリ変換

機械学習は文字を扱えない。

## male / female を数字化

```python
train["Sex"] = train["Sex"].map({
    "male": 0,
    "female": 1
})
```

結果：

| Sex |
|---|
|0|
|1|

---

# 9. 新しい列を作る

## 特徴量エンジニアリング

```python
train["FamilySize"] = train["SibSp"] + train["Parch"]
```

新しい特徴量を追加。

---

# 10. describe()

統計情報を見る。

```python
train.describe()
```

確認できるもの：

- 平均
- 最大値
- 最小値
- 標準偏差

など。

---

# 11. groupby（超重要）

集計処理。

## 性別ごとの平均年齢

```python
train.groupby("Sex")["Age"].mean()
```

意味：

```python
Sexごとに
Ageの平均を計算
```

---

# 12. Kaggleでの典型的な流れ

```python
import pandas as pd

train = pd.read_csv("train.csv")

# データ確認
train.head()

# 欠損確認
train.isnull().sum()

# 欠損補完
train["Age"] = train["Age"].fillna(train["Age"].mean())

# 文字を数値化
train["Sex"] = train["Sex"].map({
    "male": 0,
    "female": 1
})

# 学習用データ作成
X = train[["Pclass", "Sex", "Age"]]
y = train["Survived"]
```

---

# 13. pandasで最重要の考え方

コード暗記より、

> 「表をどう加工したいか」

を考える。

---

## やりたいこと → pandas

| やりたいこと | pandas |
|---|---|
| 列を見る | `df["列"]` |
| 条件抽出 | `df[df["列"] > 0]` |
| 空欄埋める | `fillna()` |
| 新列作る | `df["新列"] = ...` |
| 集計する | `groupby()` |

---

# 14. 初心者におすすめの学習法

## 最優先

Titanicコンペ。

理由：

- pandas練習に最適
- データ量がちょうどいい
- 解説が大量にある

---

## 学び方

### NG

```text
コード丸暗記
```

### OK

```text
この1行は何をしてる？
```

を理解する。

---

# 15. まず覚えるべきpandas TOP10

```python
pd.read_csv()

.head()

.shape

.columns

.describe()

.isnull().sum()

.fillna()

.map()

.groupby()

.merge()
```

まずはこれだけでかなり戦える。
