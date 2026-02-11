# Power Grid Visualization

電力グリッドシミュレーション結果の可視化・分析ツール

## 概要

このプロジェクトは、配電系統シミュレーションの結果（MATファイル）を読み込み、充電ステーション（CS）や太陽光発電（PV）、家庭負荷などの電力データを可視化・分析するためのツールです。

## ディレクトリ構造

```
power_grid_visualization/
├── README.md
└── shikata/
    ├── csPosition_dss_shikata.csv        # 充電ステーション位置情報
    ├── df_location_processed.csv          # ノード位置情報（処理済み）
    ├── Hnode.csv                          # 配電系統ノード情報
    ├── plot_summarized_load_cstype.ipynb  # 負荷の日間変動グラフ作成ノートブック
    └── read_matdata_summarizedload.ipynb  # MATファイル読み込みノートブック
```

## データファイル

### csPosition_dss_shikata.csv
充電ステーション（CS）の配置情報を含むファイル
- **type**: CSの種類（F: Fast充電、N: Normal充電）
- **Node**: ノード名
- **Name**: CS名
- **Csid**: CS識別子
- **Cgrid**: グリッド番号
- **Cap**: 容量（kW）
- **CgrNum**: グループ番号

### df_location_processed.csv
配電系統の各ノードの位置情報（処理済み）
- **Node**: ノード名
- **Latitude**: 緯度
- **Longitude**: 経度
- **Section**: セクション番号
- **Feeder**: フィーダー番号

### Hnode.csv
配電系統の全ノード情報（変電所含む）
- **Node**: ノード名（SS: 変電所）
- **Latitude**: 緯度
- **Longitude**: 経度
- **Section**: セクション番号
- **Feeder**: フィーダー番号

## Jupyterノートブック

### read_matdata_summarizedload.ipynb
MATファイルからシミュレーション結果を読み込み、負荷データを集計するノートブック

**主な処理内容:**
- `results_shikata.mat`からシミュレーションデータを読み込み
- 充電ステーション、太陽光発電、家庭負荷のデータを抽出
- ノード別・時間別の負荷データを集計

**必要なライブラリ:**
- scipy
- pandas
- matplotlib
- numpy

### plot_summarized_load_cstype.ipynb
負荷の日間変動グラフを作成するノートブック

**主な処理内容:**
- CSタイプ別（Fast/Normal）の負荷データを可視化
- 日間の負荷変動パターンをグラフ化
- CS、PV、家庭負荷の比較分析

**対象ケース:**
- case_list: シミュレーションケース（例: '50'）
- cstype: CSの種類（'N': Normal、'F': Fast）
- load_list: 負荷種類（'cs', 'pv', 'homeload'）

## 使用方法

### 1. セットアップ・クローン方法

シミュレーションを行った地域ディレクトリ内の`result/`ディレクトリ直下に、分析用のファイルをクローンします。

#### クローン手順

1. シミュレーションを実行した地域のresultディレクトリに移動（例: shikataの場合）:
```bash
cd result/
```

2. リポジトリをクローンし、必要なファイルのみを取得:
```bash
# 一時的にリポジトリをクローン
git clone <repository-url> temp_clone

# 分析対象地域のディレクトリから必要なファイルをコピー
cp temp_clone/shikata/*.csv ./
cp temp_clone/shikata/*.ipynb ./

# 一時ディレクトリを削除
rm -rf temp_clone
```

#### ディレクトリ構成例

```
shikata/                                        # 分析対象の地域ディレクトリ
└── result/                                     # シミュレーション結果ディレクトリ
    ├── results_shikata.mat                     # シミュレーション結果ファイル（既存）
    ├── csPosition_dss_shikata.csv              # クローンしたファイル
    ├── df_location_processed.csv               # クローンしたファイル
    ├── Hnode.csv                               # クローンしたファイル
    ├── plot_summarized_load_cstype.ipynb       # クローンしたファイル
    └── read_matdata_summarizedload.ipynb       # クローンしたファイル
```

**注意**: 
- ファイルは`result/`直下に配置され、MATファイルと同じディレクトリに存在します
- 他の地域で分析する場合も同様に、その地域の`result/`ディレクトリにファイルをコピーしてください

### 2. データ読み込み

`read_matdata_summarizedload.ipynb`を実行してMATファイルを読み込み:
```python
matname = 'results_shikata.mat'  # MATファイル名を指定
filename = '50'                   # 出力ファイル名を指定
```

### 3. グラフ作成

`plot_summarized_load_cstype.ipynb`を実行して可視化:
```python
case_list = ['50']               # 分析するケースを指定
cstype = ['N','F']               # CSタイプを指定
load_list = ['cs','pv','homeload']  # 分析する負荷種類を指定
```

## 分析項目

- 充電ステーション（CS）の負荷パターン分析
- 太陽光発電（PV）の発電量変動分析
- 家庭負荷の時間変動分析
- CSタイプ別（Fast/Normal）の比較
- ノード別・フィーダー別の負荷分布

## 注意事項

- MATファイル（`results_shikata.mat`）は別途シミュレーションツールで生成する必要があります
- CSVファイルのノード名は小文字に統一されます
- グラフ出力にはmatplotlibのインライン表示を使用しています

## ライセンス

このプロジェクトは研究用途で作成されています。

## 更新履歴

- 2026-02-11: プロジェクト初版作成