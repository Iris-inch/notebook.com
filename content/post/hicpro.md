---
title: "HiC-Proの導入方法"
date: 2022-12-14
---
<br><br>

## 1. Pythonライブラリのインストール

Pythonライブラリであるbx-python, numpy, scipy, pysam, argparse, pandas, icedをpipでインストールする。  
```
pip install bx-python numpy scipy pysam argparse pandas iced
```
<br><br>
## 2. Rの準備

Rのパッケージであるggplot2、RColorBrewer、gridをインストールする。

<br><br>

## 3. 他のツールのインストール

bowtie2とsamtoolsをインストールする
```
brew install bowtie2
brew install samtools
```
<br><br>
## 4. g++ compilerのインストール
```
brew install gcc
```	
<br><br>

3,4はやらなくてもよい?
<br>

## HiC-Proのインストール
HiC-ProのインストールはConda, Docker, Singularityで行える。  
私はCondaで導入した。  

インストール可能なanacondaの検索  
```
pyenv install -l | grep anaconda
```
インストール  
```
pyenv install anaconda3
```
Conda環境の作成(HiC-Proのインストール)  
```
conda env create -f MY_INSTALL_PATH/HiC-Pro/environment.yml -p WHERE_TO_INSTALL_MY_ENV
conda activate WHERE_TO_INSTALL_MY_ENV
```

## テストデータの動かしかた
