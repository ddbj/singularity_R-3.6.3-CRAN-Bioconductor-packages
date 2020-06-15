# ubuntu20.04 LTS + R-3.6.3 + CRAN packages + Bioconductor packages の singularityイメージレシピファイル
* Singularity.0 : ubuntu-20.04 LTSにaptでRをインストール後、Rを削除
* Singularity.1 : Singularity.0で作成したイメージにR-3.6.3をソースからインストール
* Singularity.2 : Singularity.1で作成したイメージにCRAN, Bioconductorパッケージのインストールに必要なライブラリをインストール
* Singularity.3 : Singularity.2で作成したイメージにCRANパッケージをインストール
* Singularity.4 : Singularity.3で作成したイメージにCRANパッケージの残りとBioconductorパッケージをインストール
* Singularity.update : Singularity.4で作成したイメージ内のRパッケージを更新・新規追加パッケージをインストール

## イメージのビルド
```
$ sudo singularity build ubuntu-20.04-R-install-base.simg Singularity.0 2>&1 | tee log.0
$ sudo singularity build ubuntu-20.04-R-3.6.3.simg Singularity.1 2>&1 | tee log.1
$ sudo singularity build ubuntu-20.04-R-3.6.3-2.simg Singularity.2 2>&1 | tee log.2
$ sudo singularity build ubuntu-20.04-R-3.6.3-CRAN-packages.simg Singularity.3 2>&1 | tee log.3
$ sudo singularity build ubuntu-20.04-R-3.6.3-CRAN-Bioconductor-packages.simg Singularity.4 2>&1 | tee log.4
$ sudo singularity build ubuntu-20.04-R-3.6.3-CRAN-Bioconductor-packages-update.simg Singularity.update 2>&1 | tee log.update
```
Singularity.updateを使ってイメージをビルドした際のログで、インストールに失敗したパッケージを把握する。
不足しているライブラリのインストールをSingularity.updateに追加し、再度イメージのビルドを行う。

## 生成したイメージのサイズ
```
$ ls -lh
-rwxr-xr-x  1 root  root  2.1G  6月  5 14:29 ubuntu-20.04-R-3.6.3-2.simg
-rwxr-xr-x  1 root  root  143G  6月 12 15:17 ubuntu-20.04-R-3.6.3-CRAN-Bioconductor-packages.simg
-rwxr-xr-x  1 root  root   17G  6月  8 10:59 ubuntu-20.04-R-3.6.3-CRAN-packages.simg
-rwxr-xr-x  1 root  root  1.4G  5月 28 14:56 ubuntu-20.04-R-3.6.3.simg
-rwxr-xr-x  1 root  root  562M  5月 28 12:34 ubuntu-20.04-R-install-base.simg
```
## インストールできなかったRパッケージ
- パッケージがダウンロードされない
    - Bioconductor (1)
        - charm : Bioconductor 3.10に含まれていないため
- 依存ライブラリの不足
    - CRAN (11)
        - BALD：JAGS (http://mcmc-jags.sourceforge.net)が必要。
        - BRugs：OpenBUGS (http://www.openbugs.net/w/FrontPage)が必要。
        - OpenCL：NVIDIA CUDA等でのOpenCLランタイムのインストールが必要
        - ROracle：Oracle Instant Client or Oracle Database Clientが必要。
        - Rcplex：IBM ILOG CPLEXが必要
        - Rsymphony：SYMPHONY (https://projects.coin-or.org/SYMPHONY)が必要。
        - cplexAPI：IBM ILOG CPLEXが必要
        - kmcudaR：NVIDIA CUDAが必要。
        - qtbase：Qt 4.xが必要。ubuntu 20.04のaptリポジトリに入っていない。
        - rLindo：Lindo API（https://www.lindo.com/）が必要。LINDOAPI_HOMEを設定せよ。
        - runjags：JAGS (http://mcmc-jags.sourceforge.net)が必要。
    - Bioconductor（11）
        - ChemineOB：Open Babel (http://openbabel.org/wiki/Main_Page) が必要。
        - SharedObject：原因不明
        - mlm4omics：原因不明
        - rsbml：libsbmlが必要（libsbml5-devをインストールしたが違うようだ）。
        - xps：root_v5.34.36（https://root.cern.ch/releases）が必要。
        - Rcwl：cwltoolをインストールしたが、cwlversionの判定に失敗している。
        - permGPU：NVIDIA CUDAが必要。
        - MSGFplus：原因不明
        - scAlign：tensorflowパッケージを使ってtensorflowをインストールする必要がある。
        - MoonlightR：原因不明
        - Rariant：原因不明
- 依存Rパッケージの不足 (20)
    - BANOVA：runjagsが必要
    - BayesPostEst：runjagsが必要
    - IsotopeR：runjagsが必要
    - PortfolioOptim：Rsymphonyが必要
    - ROI.plugin.cplex：Rcplexが必要
    - ROI.plugin.symphony：Rsymphonyが必要
    - RcmdrPlugin.RMTCJags：runjagsが必要
    - TreeBUGS：runjagsが必要
    - bayescount：runjagsが必要
    - bfw：runjagsが必要
    - ora：ROracleが必要
    - pivmet：runjagsが必要
    - qtpaint：qtbaseが必要
    - BiGGR：rsbmlが必要
    - MPTmultiverse：TreeBUGS, runjagsが必要
    - RcwlPipelines：Rcwlが必要
    - MSGFgui：MSGFplusが必要
    - Replication：runjagsが必要
    - charmData：charmが必要
    - proteomics：MSGFplusが必要
