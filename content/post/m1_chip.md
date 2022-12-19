---
title: "M1 macでバイオインフォマティクス講習を受けて苦労したこと"
date: 2022-12-19T17:26:29+09:00
tags: []
featured_image: ""
description: ""
---

バイオインフォマティクス講習ではさまざまなツールやパッケージを扱うが、  
いくつかがM1チップ(arm64)に対応していなかったので一応の解決策を挙げる。  
 <br><br>


## HISAT2周りの導入  

HISAT2周りのツールのインストール・実行の際にエラーが出る。  
解決策は2つあるが、後者をお勧めする。


### 1. x86_64バージョンのbrewもインストールする  


```
arch -x86_64 /bin/bash -c "$(curl -fsSL 
https://raw.githubusercontent.com/Homebrew/install/master/install.sh)
```
でx86_64バージョンのbrewをインストールする。  



このbrewを用いてパッケージをインストールするには、  
```
arch -x86_64 brew install PACKAGE
```
を使う。




[参考:古いbrewのインストールと動かし方](https://blog.mksc.jp/contents/apple-silicon/)

*ただし、この方法だと他のパッケージと組み合わせるときに、どちらのbrewを参照するかで不備が生じる可能性がある。*



### 2. バイナリ版をダウンロードし、PATHを通す。

詳しくは[先輩のページ](https://notemy.netlify.app/trouble-shooting/mac-architecture/)を参照されたし。  


<br>
<br>
<br>
<br>




## RパッケージTCC (edgeR)のインストール

<br>

BiocManagerでTCCをインストールする際に、edgeRとその他がインストールできなくなっている。  
私の環境では、gfortranをインストールすることで動くようになった。

[gfortran](https://github.com/fxcoudert/gfortran-for-macOS/releases)  
<br>
[How to install](file:///Volumes/gfortran-ARM-12.2-Ventura/README.html)

場合によっては、Xcodeのインストールも必要かもしれない。

この方法により、edgeRはインストールできたが、nlmeはできなかったため、根本的な解決にはなっていないと思われる。
