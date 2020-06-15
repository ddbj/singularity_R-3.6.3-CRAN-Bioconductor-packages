# ubuntu20.04 LTS + R-3.6.3 + CRAN all packages の singularityイメージレシピファイル
* Singularity.0 : ubuntu-20.04 LTSにaptでRをインストール後、Rを削除
* Singularity.1 : Singularity.0で作成したイメージにR-3.6.3をソースからインストール
* Singularity.2 : Singularity.1で作成したイメージにCRANパッケージのインストールに必要なライブラリをインストール
* Singularity.3 : Singularity.2で作成したイメージにCRANパッケージをインストール
* Singularity.update : Singularity.3で作成したイメージ内のCRANパッケージを更新・新規追加パッケージをインストール

## イメージのビルド
```
$ sudo singularity build ubuntu-20.04-R-install-base.simg Singularity.0 2>&1 | tee log.0
$ sudo singularity build ubuntu-20.04-R-3.6.3.simg Singularity.1 2>&1 | tee log.1
$ sudo singularity build ubuntu-20.04-R-3.6.3-2.simg Singularity.2 2>&1 | tee log.2
$ sudo singularity build ubuntu-20.04-R-3.6.3-CRAN-packages.simg Singularity.3 2>&1 | tee log.3
$ sudo singularity build ubuntu-20.04-R-3.6.3-CRAN-packages-update.simg Singularity.update 2>&1 | tee log.4
```
Singularity.updateを使ってイメージをビルドした際のログで、インストールに失敗したパッケージを把握する。
不足しているライブラリのインストールをSingularity.updateに追加し、再度イメージのビルドを行う。

## 生成したイメージのサイズ
```
$ ls -lh
-rwxr-xr-x  1 root  root  2.1G  6月  5 14:29 ubuntu-20.04-R-3.6.3-2.simg
-rwxr-xr-x  1 okuda okuda  17G  6月  8 10:59 ubuntu-20.04-R-3.6.3-CRAN-packages.simg
-rwxr-xr-x  1 root  root  1.4G  5月 28 14:56 ubuntu-20.04-R-3.6.3.simg
-rwxr-xr-x  1 root  root  562M  5月 28 12:34 ubuntu-20.04-R-install-base.simg
```
## インストール結果
[インストールに成功・失敗したパッケージについて](https://ddbj-dev.atlassian.net/wiki/spaces/~161140260/pages/383221789/CRAN+package+singularity+image)