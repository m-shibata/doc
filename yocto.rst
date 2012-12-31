=======================
Yoctoドキュメントの翻訳
=======================

下準備
======

ドキュメント構築に必要なパッケージをインストールします。 ::

    sudo apt-get install git xsltproc po4a

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


ドキュメントのビルド方法
========================

Poky関係のドキュメント（documentation）とBitbake関係のドキュメント
（bitbake/doc）があります。

例えばYocto Project Quick StartのHTMLを生成、表示する場合は、
次のコマンドを実行します。 ::

    cd documentation/
    make DOC=yocto-project-qs
    xdg-open yocto-project-qs/yocto-project-qs.html


DocBookの国際化対応
===================

PokyのドキュメントはDocBookを使っています。そこで`po4a`_を使って
国際化対応を行います。

.. _po4a: http://po4a.alioth.debian.org/man/man7/po4a.7.php

ここで説明した内容は、i18nブランチでは対応済です。
単純に作業を行いたいだけであれば、次章に移動してください。

potファイルを作成する
---------------------

最初に翻訳テンプレートであるpotファイルを作成します。 ::

    cd documentation/yocto-project-qs
    mkdir po
    po4a-gettextize -f docbook -m yocto-project-qs.xml -M UTF-8 -p po/yocto-project-qs.pot

yocto-project-qs.xmlはASCII文字のみで構成されていないようなので、
文字コードを指定しています。


各言語ごとのpoファイルを作成する
--------------------------------

翻訳する言語のpoファイルがまだ存在しなければ、言語コードを使って
potファイルをコピーします。 ::

    cp po/yocto-project-qs.pot po/ja.po

さらにヘッダーを適切に設定します。

    # Translation for "The Yocto Project Quick Start"
    # Copyright (C) 2012-2013 Linux Foundation
    # This file is distributed under the same license as the Poky.
    # Mitsuya Shibata <mty.shibata@gmail.com>, 2012-2013.
    #
    msgid ""
    msgstr ""
    "Project-Id-Version: poky master\n"
    "POT-Creation-Date: 2012-12-29 20:43+0900\n"
    "PO-Revision-Date: 2012-12-29 20:43+0900\n"
    "Last-Translator: Mitsuya Shibata <mty.shibata@gmail.com>\n"
    "Language-Team: Japanese <ja@li.org>\n"
    "Language: ja\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Plural-Forms: nplurals=1; plural=0;\n"

あとは、ja.poを翻訳していくだけです。


翻訳したデータを反映してドキュメントを作成する
----------------------------------------------

翻訳したデータを反映したドキュメントは次のコマンドで作成できます。 ::

    po4a-translate -f docbook -m yocto-project-qs.xml -M UTF-8 \
        -p po/ja.po -k 0 -l yocto-project-qs.ja.xml

オプション`-k`を指定しないと、翻訳率が80%以下なら翻訳が反映されなくなります。
これは不用意に品質の低いpoファイルを取り込むことを防止するためのしきい値ですが、
翻訳開始当初は不便なのでしきい値を0に下げています。

例えば、yocto-project-qs.xmlに名前を変更すれば、既存のmakeコマンドで
日本語化されたドキュメントを作成できます。


最新の原文にあわせてpoをアップデートする
----------------------------------------

原文が更新されたら、それにあわせてpoファイルをアップデートする必要があります。 ::

    po4a-updatepo -f docbook -m yocto-project-qs.xml -M UTF-8 -p po/ja.po

多少表現が変わった程度であれば、以下のようにfuzzyタグがつくだけなので、
fuzzyと元の文章を削除すればいいだけです。例えば、"Welcome!"の
エクスクラメーションマークが削除された場合は、次のように
poファイルが更新されます。

    #,fuzzy
    #| msgid "Welcome!"
    msgid "Welcome"
    msgstr "ようこそ！"


i18nブランチを使った翻訳方法
============================

i18nブランチのPokyのMakefileでは、po4aを使った翻訳ファイルの管理方法が
統合されています。そのためmakeコマンドで、翻訳の反映やpoのアップデートが
できます。

まず、翻訳したいドキュメントフォルダーにpoフォルダーが存在しない場合は、
作成しておきます。 ::

    mkdir documentation/yocto-project-qs/po

updatepoターゲットを使ってpotファイルを作成します。 ::

    make DOC=yocto-project-qs updatepo

作成されたpotファイルを、翻訳したい言語コードを使ってpoファイルに
コピーします。詳しいことは「各言語ごとのpoファイルを作成する」を
参照してください。

作成されたpoファイルを使って翻訳が完了したら、それを反映した
ドキュメントを作成します。通常のドキュメント作成時のコマンドに、
LN変数に翻訳したい言語コードを付与するだけです。
例えば日本語の場合は言語コードが"ja"なので、次のようにします。 ::

    make DOC=yocto-project-qs LN=ja
    xdg-open yocto-project-qs/yocto-project-qs.html

これで翻訳されたドキュメントが作成されるはずです。


TODO
====

* 上流との同期方法
* 作成したドキュメントの公開方法
