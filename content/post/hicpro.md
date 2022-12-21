---
title: "HiC-Proの導入方法"
date: 2022-12-14
---
<br><br>
## HiC-Proの導入において苦戦したため日本語解説を残しておく。  
<br>

HiC-Proのインストールは、Conda, Docker, Singularityでのみ行える。そのため、自分のPCではConda、スパコン(遺伝研)ではSingularityで行うことになると思う。ここでは、Condaを用いたインストール方法について説明する。  
<br>
<br>

### 1. Pythonライブラリのインストール

Pythonライブラリであるbx-python, numpy, scipy, pysam, argparse, pandas, icedをpipでインストールする。
<br>

```
pip install bx-python numpy scipy pysam argparse pandas iced
```
<br><br>

### 2. Rの準備

Rのパッケージであるggplot2、RColorBrewer、gridをインストールする。

<br><br>

### 3. 他のツールのインストール
bowtie2とsamtoolsをインストールする
<br>

```
brew install bowtie2
brew install samtools
```
<br><br>

### 4. g++ compilerのインストール
<br>

```
brew install gcc
```	
<br>

### 5.Condaによるインストール
<br>

minicondaのインストール  
```
pyenv install miniconda3-latest
```
もしくは (https://docs.conda.io/en/latest/miniconda.html)よりそれぞれの環境に合ったものをインストールする
<br><br>

### 6. Conda環境の作成(HiC-Proのインストール)  
<br>

```
conda env create -f MY_INSTALL_PATH/HiC-Pro/environment.yml -p WHERE_TO_INSTALL_MY_ENV
conda activate WHERE_TO_INSTALL_MY_ENV
```

### テストデータの動かしかた

1. Hi-Cデータの取得
```
wget https://zerkalo.curie.fr/partage/HiC-Pro/HiCPro_testdata.tar.gz && tar -zxvf HiCPro_testdata.tar.gz
```
これをtest_dataファイルに入れる

2. [Bowtie2 manualページ](https://bowtie-bio.sourceforge.net/bowtie2/manual.shtml)からヒトhg19のindexesフォルダをダウンロードし作業ディレクトリに入れる

3. 使うconfig_test_latest.txt内の、##alignment optionsのbowtie2_IDX_PATHに2のファイルのパスを入力

4. bin/HiC-Pro -c config_test_latest.txt -i test_data -o hicpro_latest_testを実行

以上で動くはず
<br><br>

## 本番環境
本番環境では、自分で制限サイトファイル、染色体サイズファイル、bowtie2 indexフォルダを集める必要があり、用いるconfig.txtにそれぞれのpathを入力する。
