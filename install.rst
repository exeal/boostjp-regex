.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

ライブラリのビルドとインストール
================================

ライブラリの zip ファイルを解凍するとき、ディレクトリの内部構造を変更しないようにする（例えば :option:`!-d` オプションを付けて解凍する）。もし変更してしまっていたら、この文書を読むのをやめて解凍したファイルをすべて削除して最初からやり直したほうがよい。

本ライブラリを使用する前に設定することは何もない。大抵のコンパイラ、標準ライブラリ、プラットフォームは何もしなくてもサポートされる。設定で何か問題がある場合や、単にあなたのコンパイラで設定をテストしてみたい場合は、やり方は他の Boost ライブラリと同じである。`ライブラリの設定ドキュメント <http://www.boost.org/libs/config/index.html>`_\を見るとよい。

ライブラリのコードはすべて名前空間 :cpp:member:`!boost` 内にある。

コンパイラに C++11 以降のサポートがあれば、このライブラリはヘッダオンリーである。C++03 コンパイラは現時点ではサポートしているが非推奨となっており、今後通知なく削除する可能性がある。

libboost_regex 外部ライブラリをビルドする必要があるのは、下記のいずれかの場合のみである。

* ライブラリを C++03 モードで使用する。もしくは
* 非推奨の POSIX C API を使用する。

このライブラリは、Boost の他のライブラリを使用しない「スタンドアロン」モードで使用できる。このためには、

* :code:`__has_include` をサポートする C++17 コンパイラを使用する。この場合 :file:`<boost/config.h>` が無ければ自動的にスタンドアロンモードとなる。もしくは
* ビルド時に :c:macro:`BOOST_REGEX_STANDALONE` を定義する。

:file:`<boost/regex/icu.hpp>` を使ってこのライブラリを ICU とともに使用する場合、提供されている CMake スクリプトを使わないのであれば自分で ICU ライブラリにリンクしなければならない。


.. _usage_with_cmake:

CMake を用いた使用
------------------

このライブラリを他の CMake スクリプトから利用できるよう、非常に基本的な :file:`CMakeLists.txt` を提供している。

:file:`CMakeLists.txt` は 2 つのターゲット定義を持つ。

* :code:`Boost::regex`\ ：通常のヘッダオンリービルドで使用するターゲット。
* :code:`Boost::regex_icu`\ ：:file:`<boost/regex/icu.hpp>` を使って ICU へ依存させるのに使用するターゲット。

設定オプションが 1 つある。

.. c:macro:: BOOST_REGEX_STANDALONE

   他の Boost ライブラリを依存ターゲットにせず、Boost.Regex をスタンドアロンモードにする場合に設定する。\ :code:`-DBOOST_REGEX_STANDALONE=on` として CMake を起動しスタンドアロンモードを有効化する。


.. _install.building_with_bjam:

C++03 ユーザ限定（非推奨）：bjam を用いたビルド
-----------------------------------------------

本ライブラリの古いバージョンをビルドおよびインストールする最適な方法である。`Getting Started ガイド <http://www.boost.org/more/getting_started.html>`_\を参照していただきたい。


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

ICU は見つからず、関連するサポートはライブラリのコンパイルに含まれない。これが期待した結果と違うという場合は、ファイル :file:`boost-root/bin.v2/config.log` の内容を見て、設定チェック時にビルドが吐き出した実際のエラーメッセージを確認すべきである。コンパイラに適切なオプションを渡してエラーを修正する必要があるだろう。\ :program:`b2` に渡す主要なオプションは、

.. option:: include=/some/path

   インクルードファイルの探索パスリストに :file:`/some/path` を追加する。大半のコンパイラにおける :option:`!-I/some/path` と等価である。

.. option:: library-path=/some/path

   外部ライブラリの探索パスリストに :file:`/some/path` を追加する。ICU バイナリが非標準的な場所にある場合に設定する。

.. option:: -sICU_ICUUC_NAME=NAME

   :file:`libicuuc` が非標準的な名前を持つ場合、リンク対象ライブラリの名前を設定する。既定は :file:`icuuc` 、\ :file:`icuucd` 、\ :file:`sicuuc` 、:file:`sicuucd` のいずれかである（ビルドオプションによる）。

.. option:: -sICU_ICUDT_NAME=NAME

   :file:`libicudata` が非標準的な名前を持つ場合、リンク対象ライブラリの名前を設定する。既定は :file:`icudt` 、\ :file:`icudata` 、\ :file:`sicudt` 、:file:`sicudata` のいずれかである（ビルドオプションによる）。

.. option:: -sICU_ICUIN_NAME=NAME

   :file:`libicui18n` が非標準的な名前を持つ場合、リンク対象ライブラリの名前を設定する。既定は :file:`icui18n` 、\ :file:`icuin` 、\ :file:`icuind` 、\ :file:`sicuin` 、:file:`sicuins` のいずれかである（ビルドオプションによる）。

.. option:: cxxstd=XX

   サポートされている C++ 標準を設定する。\ :option:`!XX` は 03 、11 、14 、17 、2a のいずれかである。

.. option:: cxxflags="FLAGS"

   :option:`!"FLAGS"` を直接コンパイラに渡す。最終手段のオプション。

.. option:: linflags="FLAGS"

   :option:`!"FLAGS"` をリンク時に直接コンパイラに渡す。最終手段のオプション。

.. important:: 設定の結果はキャッシュされる。異なるコンパイラオプションで再ビルドする場合、bjam のコマンドラインに :option:`!-a` を付けるとすべてのターゲットが強制的に再ビルドされる。

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
