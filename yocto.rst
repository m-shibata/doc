=======================
Yoctoドキュメントの翻訳
=======================

下準備
======

ドキュメント構築に必要なパッケージをインストールします。 ::

    sudo apt-get install git xsltproc

[PokyのGitリポジトリ](http://git.yoctoproject.org/cgit/cgit.cgi/poky/)をクローンして、
[GitHub](https://github.com/m-shibata)に登録します。GitHub上のリポジトリは
あらかじめ作成しておいてください。 ::

    mkdir yocto
    cd yocto
    git clone git://git.yoctoproject.org/poky
    cd poky
    git remote set-url origin git@github.com:m-shibata/poky
    git push origin master


翻訳用のブランチを作り、pushします。 ::

    git branch -b i18n
    git push origin i18n


ドキュメントのビルド方法
========================

Poky関係のドキュメント（documentation）とBitbake関係のドキュメント
（bitbake/doc）があります。

例えばYocto Project Quick StartのHTMLを生成、表示する場合は、
次のコマンドを実行します。 ::

    cd documentation/
    make DOC=yocto-project-qs
    xdg-open yocto-project-qs/yocto-project-qs.html

TODO
====

* 上流との同期方法
* DocBookのi18n
* 作成したドキュメントの公開方法
