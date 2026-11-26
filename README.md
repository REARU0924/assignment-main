# 変圧器オイル温度予測 PoC

ETT（Electricity Transformer Temperature）公開データセットを使い、変圧器のオイル温度（`OT`）を予測する機械学習PoCです。探索的データ分析（EDA）でデータの傾向や負荷特徴量との関係を調べ、その結果をもとに複数の時系列モデルを構築・比較します。ノートブックはGoogle Colabでの実行を想定しています。

## 目的

経験則による閾値監視を補う予測型保全の可能性を、公開ベンチマーク上で検討します。対象はETTの4系列 `ETTh1`、`ETTh2`、`ETTm1`、`ETTm2` です。基本的な予測タスクは次の観測時刻の `OT` 予測で、ETThは1時間先、ETTmは15分先を対象とします。

## ディレクトリ構成

| パス | 内容 |
| --- | --- |
| `data/` | ETTのCSVデータ（4系列） |
| `EDA_ETT_Colab.ipynb` | ETTデータの基本的な探索的分析 |
| `EDA_ETT_Colab2.ipynb` | OT・負荷特徴量やラグ相関に関する仮説検証 |
| `EDA_ETT_correlation_heatmaps_remaining.ipynb` | ETTh2、ETTm1、ETTm2の相関ヒートマップ作成 |
| `model_ETT_colab.ipynb` | LSTMとTCNを使った1ステップ先予測の比較 |
| `model_ETT_nlinear_colab.ipynb` | NLinearによる予測実験 |
| `model_ETT_tcn_all_colab.ipynb` | TCNで4系列を処理する実験 |
| `model_ETT_tcn_all_colab_modefied.ipynb` | 負荷特徴量選択とTCNの設定比較を含む4系列実験 |
| `eda_output/` | EDAの図表・集計CSV |
| `model_output/`、`model_output_nlinear/`、`model_output_tcn_all/`、`model_output_tcn_all_modified/` | モデルごとの予測結果・評価指標・図表 |
| `ATHENA TECHNOLOGIES PoC報告.pdf` | PoC報告資料 |

各ノートブックには実験条件や個別の準備手順があります。特に `model_ETT_tcn_all_colab_modefied.ipynb` では、学習期間のデータで負荷特徴量の相関を調べ、負荷なしモデルと選択した負荷特徴量を加えたモデルを比較します。

## Google Colabでの実行

1. 対象の `.ipynb` をGoogle Colabで開きます。
2. ノートブック内の準備セルを実行し、必要なライブラリをインストールしてGoogle Driveをマウントします。
3. `data/` のCSVをノートブックが参照する場所に配置します。現在のノートブックでは `/content/drive/Othercomputers/マイ Mac/assignment-main/data` を使うものがあります。自身のDrive構成が異なる場合は、ノートブック内のデータパスを変更してください。
4. セルを上から順に実行します。モデルの学習には時間がかかる場合があります。

実行後の図表やCSVは、ノートブックに設定されたGoogle Drive上の出力先へ保存されます。リポジトリ内の `eda_output/` と `model_output*/` は、実験結果を整理して保持するためのディレクトリです。

## 評価と解釈

モデル実験では時系列順に学習・検証・テスト期間を分け、主にMAEとRMSEで予測誤差を比較します。評価設定はノートブックごとに異なるため、結果を比較するときは各ノートブックの分割方法、予測対象、入力特徴量を確認してください。

相関やラグ相関は特徴量間の関連を調べるための探索的な指標であり、因果関係や物理的な応答時間を示すものではありません。また、単一の時系列分割による結果は探索的な比較です。実運用への適用判断には、複数の期間・予測 horizon での再検証と、各予測時点で利用可能な情報だけを使った評価が必要です。

## データ出典

ETTデータセット（Electricity Transformer Temperature）。データの詳細と原典については、[zhouhaoyi/ETDataset](https://github.com/zhouhaoyi/ETDataset) を参照してください。
