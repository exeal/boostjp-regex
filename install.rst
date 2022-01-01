.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

ライブラリのビルドとインストール
================================

ライブラリの zip ファイルを解凍するとき、ディレクトリの内部構造を変更しないようにする（例えば :option:`!-d` オプションを付けて解凍する）。もし変更してしまっていたら、この文書を読むのをやめて解凍したファイルをすべて削除して最初からやり直したほうがよい。

本ライブラリを使用する前に設定することは何もない。大抵のコンパイラ、標準ライブラリ、プラットフォームは何もしなくてもサポートされる。設定で何か問題がある場合や、単にあなたのコンパイラで設定をテストしてみたい場合は、やり方は他の Boost ライブラリと同じである。`ライブラリの設定ドキュメント <http://www.boost.org/libs/config/index.html>`_\を見るとよい。

ライブラリのコードはすべて名前空間 :cpp:member:`!boost` 内にある。

他のテンプレートライブラリとは異なり、本ライブラリはテンプレートコード（ヘッダ中）と、静的コード・データ（cpp ファイル中）が混在している。したがってライブラリを使用する前に、ライブラリのサポートコードをビルドしてライブラリかアーカイブファイルを作成する必要がある。これについて各プラットフォームにおける方法を以下に述べる。


.. _install.building_with_bjam:

bjam を用いたビルド
-------------------

本ライブラリをビルドおよびインストールする最適な方法である。`Getting Started ガイド <http://www.boost.org/more/getting_started.html>`_\を参照していただきたい。


.. _install.building_with_unicode_and_icu_su:

Unicode および ICU サポートビルド [#]_
--------------------------------------

Boost.Regex は、ICU がコンパイラの検索パスにインストールされているか設定をチェックするようになった。ビルドを始めると次のようなメッセージが現れるはずである：

.. code-block:: console

   Performing configuration checks

       - has_icu builds           : yes

これは ICU が見つかり、ライブラリのビルドでサポートされるということを表している。

.. tip:: 正規表現ライブラリで ICU を使用したくない場合は :option:`!--disable-icu` コマンドラインオプションを使用してビルドするとよい。

仮に次のような表示が出た場合、

.. code-block:: console

   Performing configuration checks

       - has_icu builds           : no

ICU は見つからず、関連するサポートはライブラリのコンパイルに含まれない。これが期待した結果と違うという場合は、ファイル :file:`boost-root/bin.v2/config.log` の内容を見て、設定チェック時にビルドが吐き出した実際のエラーメッセージを確認すべきである。コンパイラに適切なオプションを渡してエラーを修正する必要があるだろう。例えば、:file:`{some-include-path}` をコンパイラのヘッダインクルードパスに追加するには次のようにする。

.. code-block:: console

   bjam include=some-include-path --toolset=toolset-name install

あるいは ICU のバイナリが非標準的な名前でビルドされている場合に、ライブラリのリンク時に既定のICUバイナリ名の代わりに :samp:`{linker-options-for-icu}` を使用するには次のようにする。

.. code-block:: console

   bjam -sICU_LINK="linker-options-for-icu" --toolset=toolset-name install

オプション :option:`!cxxflags=option` および :option:`!linkflags=-option` でコンパイラやリンカに固有のオプションを設定する必要があるかもしれない。

.. important:: 設定の結果はキャッシュされる。異なるコンパイラオプションで再ビルドする場合、bjam のコマンドラインに :option:`!-a` を付けるとすべてのターゲットが強制的に再ビルドされる。

ICU がコンパイラのパスに入っておらず、ヘッダ・ライブラリ・バイナリがそれぞれ :file:`{path-to-icu/include}` 、:file:`{path-to-icu/lib}` 、:file:`{path-to-icu/bin}` にあるのであれば、環境変数 ICU_PATH でインストールした ICU のルートディレクトリを指定する必要がある。典型的なのは MSVC でビルドする場合である。例えば ICU を :file:`c:\\download\\icu` にインストールした場合は、次のようにする。

.. code-block:: console

   bjam -sICU_PATH=c:\download\icu --toolset=toolset-name install

.. important:: ICU も Boost と同様に C++ ライブラリであり、ICU のコピーが Boost のビルドに使用したものと同じ C++ コンパイラ（およびバージョン）でビルドされていなければならないということに注意していただきたい。そうでない場合 Boost.Regex は正しく動作しない。

結局のところ、複数のコンパイラのバージョンで異なる ICU ビルド使用してビルド・テストするのであれば、設定の段階で ICU が自動的に検出されるよう各ツールセットに適切なコンパイラ・リンカオプションを設定するよう（ICU バイナリが標準的な名前を使っているのであれば、適切なヘッダとリンカの検索パスを追加するだけでよい）user-config.jam を修正するのが現時点で唯一の方法である。


.. _install.building_from_source:

メイクファイルを使ったビルド
----------------------------

Regex ライブラリは「ただのソースファイル群」であり、ビルドに特に必要なことはない。

:file:`<boost のパス>/libs/regex/src*.cpp` のファイルをライブラリとしてビルドするか、これらのファイルをあなたのプロジェクトに追加するとよい。既定の Boost ビルドでサポートされていない個々のコンパイラオプションを使う必要がある場合に特に有用である。

以下の 2 つの #define を知っておく必要がある。

* ICU サポートを有効にしてコンパイルする場合は :c:macro:`BOOST_HAS_ICU` を定義しなければならない。
* Windows で DLL をビルドする場合は :c:macro:`BOOST_REGEX_DYN_LINK` を定義しなければならない。


.. [#] 訳注　Unicode を用いた正規表現ライブラリは ICU にもあります。Unicode に関する機能は ICU 版のほうが豊富です。
