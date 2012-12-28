=======================
Yoctoドキュメントの翻訳
=======================

下準備
======

Pokyの`Gitリポジトリ`_をクローンして、`GitHub`_に登録します。 ::

    mkdir yocto
    cd yocto
    git clone git://git.yoctoproject.org/poky
    cd poky
    git remote set-url origin git@github.com:m-shibata/poky
    git push origin master

.. _Gitリポジトリ: http://git.yoctoproject.org/cgit/cgit.cgi/poky/

.. _GitHub: https://github.com/m-shibata

翻訳用のブランチを作り、pushします。 ::

    git branch -b i18n
    git push origin i18n


TODO
====

* ドキュメントのビルド方法
* 上流との同期方法
* DocBookのi18n
* 作成したドキュメントの公開方法
