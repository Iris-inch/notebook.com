---
title: "HiC-Proの導入方法"
date: 2023-02-17
---
<br><br>
## HiC-Proの導入において苦戦したため解説を残しておく。  
<br>

HiC-Proのインストールは、Conda, Docker, Singularityを用いることで楽に行える。
基本的にはcondaで行うことになるであろうが、既存のpython環境とごっちゃになってしまうらしいので、パッケージ管理等に注意が必要である。
ローカル環境ではスペック不足で何十時間もかかってしまう上に、ストレージ容量が足りないため、Hi-C解析は遺伝研でのみ行うことになるであろう。
今回はローカルでの導入方法も載せるが、遺伝研スパコンで行うことを進める。
<br>
<br>

## 遺伝研スパコンの場合(https://sc.ddbj.nig.ac.jp/software/python)

### python
(https://conda.io/projects/conda/en/latest/user-guide/install/linux.html)
このリンクからMinicondaを選び、好きなバージョンのリンクを選ぶ。
```
wget 'link'
```
で遺伝研上の好きなところに落としてくる。

```
bash Miniconda3-py39_4.10.3-Linux-x86_64.sh
```
を実行。conda initはNO。
あとは、condaにpathを通す。
.bashrcに
```
PATH='~/miniconda3/bin:$PATH'
```
を書く。
また、
```
$ conda config --add channels conda-forge
$ conda config --set channel_priority strict
$ vim ~/.condarc
```
でconda-forgeレポジトリをデフォルトに設定し、.condarcのトップに追加されたことを確認。
これでcondaの導入は終了。

### 2. 環境の作成
```
git clone https://github.com/nservant/HiC-Pro.git
```
でgithubからHiC-Proのレポジトリを好きな場所に落としてくる。

```
conda env create -f MY_INSTALL_PATH/HiC-Pro/environment.yml -p WHERE_TO_INSTALL_MY_ENV
conda activate WHERE_TO_INSTALL_MY_ENV
```
でconda環境を作る。（大文字は自分で指定）
すると、WHERE_TO_INSTALL_MY_ENVに必要なもの全てがインストールされている。

### 3. インストール
まずは、githubから落として来たディレクトリの中にあるconfig-install.txtを編集する。
PREFIX	HiC-Pro本体を落とす場所
BOWTIE2_PATH    WHERE_TO_INSTALL_MY_ENV/bin/bowtie2
SAMTOOLS_PATH   WHERE_TO_INSTALL_MY_ENV/bin/samtools
R_PATH	Full    WHERE_TO_INSTALL_MY_ENV/bin/R
PYTHON_PATH	    WHERE_TO_INSTALL_MY_ENV/python(version)
CLUSTER_SYS	    SGE

編集が終わったら、
```
make configure
make install
```
を行う。
このとき、pythonのパッケージエラーが発生することがある。
これは、最初に導入したcondaのpythonとHiC-Proから落として来たpythonの2つが存在するためである。
scripts/install/install_dependencies.shの225行目のpythonをHiC-Proのバージョンで指定すれば解決する。

以上でインストールは完了。

## ローカルの場合
ローカルの場合も遺伝研とほとんど変わりない。

### 1. Condaの導入
condaは2つの方法で導入できる。すでに、pyenv環境があるならpyenvでの方法で行うのが良いだろう。

minicondaのインストール  
```
pyenv install miniconda3-latest
```
もしくは (https://docs.conda.io/en/latest/miniconda.html)よりそれぞれの環境に合ったものをインストールする
<br><br>

### 2. configの設定  
<br>
PREFIX  HiC-Pro本体を落とす場所
BOWTIE2_PATH    WHERE_TO_INSTALL_MY_ENV/bin/bowtie2
SAMTOOLS_PATH   WHERE_TO_INSTALL_MY_ENV/bin/samtools
R_PATH	Full    WHERE_TO_INSTALL_MY_ENV/bin/R
PYTHON_PATH	    WHERE_TO_INSTALL_MY_ENV/python(version)
CLUSTER_SYS	    



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
