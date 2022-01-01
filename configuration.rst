.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

構成
====

.. contents::
   :depth: 1
   :local:


.. _configuration.compiler:

コンパイラセットアップ
----------------------

`Boost.Config サブシステム <http://www.boost.org/libs/config/index.html>`_\があるため、Boost.Regex を使うのに特別な構成は必要ない。問題がある場合（あるいは一般的でないコンパイラやプラットフォームを使う場合）は `Boost.Config <http://www.boost.org/libs/config/index.html>`_ に構成スクリプトがあるので、そちらを使うとよい。


.. _configuration.standalone:

（他の Boost ライブラリを使用せず）スタンドアロンモードで使用する
-----------------------------------------------------------------

このライブラリは、Boost の他のライブラリを使用しない「スタンドアロン」モードで使用可能となった。このためには以下のいずれかでなければならない。

* :code:`__has_include` をサポートする C++17 コンパイラを利用する。この場合、:file:`<boost/config.hpp>` が\ **無ければ**\ 自動的にスタンドアロンモードになる。もしくは
* ビルド時に :c:macro:`BOOST_REGEX_STANDALONE` を定義する。

2 つのモードの主な違いは、Boost.Config が与えられたときにライブラリがコンパイラの欠陥を回避して自身を自動的に調整することである。特に例外サポートをオフにしてライブラリを使用するには、インクルードパスに Boost.Config のコピーを置くか、ビルド時に :c:macro:`BOOST_NO_EXCEPTIONS` を手動で定義する必要がある。


.. _configuration.locale:

ロカールおよび特性クラスの選択
------------------------------

ユーザのロカール（locale）を Boost.Regex がどのように処理するか制御するには、以下のマクロ（:file:`user.hpp` を見よ）を使う。


.. c:macro:: BOOST_REGEX_USE_C_LOCALE

   Boost.Regex が、特性クラス中で大域 C ロカールを使うように強制する。C++ ロカールがあるのでこの設定は現在非推奨となっている。


.. c:macro:: BOOST_REGEX_USE_CPP_LOCALE

   Boost.Regex が、既定特性クラス中で :cpp:class:`!std::locale` を使うように強制する。各正規表現はインスタンス固有のロカールにより :cpp:func:`~std::locale::imbue` される。これは Windows 以外のプラットフォームにおける既定の動作である。


.. c:macro:: BOOST_REGEX_NO_W32

   Boost.Regex は（利用可能な場合でも）あらゆる Win32 API を使用しない（:c:macro:`BOOST_REGEX_USE_C_LOCALE` が設定されない限り :c:macro:`BOOST_REGEX_USE_CPP_LOCALE` が暗黙に有効になる）。


.. _configuration.tuning:

アルゴリズムの調整
------------------

.. c:macro:: BOOST_REGEX_BLOCKSIZE

   Boost.Regex は状態マシンのスタックのために大きめのメモリブロックを使う。ブロックのサイズが大きいほどメモリ確保の回数は少なくなる。既定は 4096 バイトであり、大抵の正規表現マッチでメモリの再確保が必要ない値である。しかしながらプラットフォームの特性を見た上で、別の値を選択することも可能である。


.. c:macro:: BOOST_REGEX_MAX_BLOCKS

   サイズ :c:macro:`BOOST_REGEX_BLOCKSIZE` のブロックをいくつ使用できるか設定する。この値を超えると Boost.Regex はマッチの検索を停止し、:cpp:class:`!std::runtime_error` を投げる。既定値は 1024 である。:c:macro:`BOOST_REGEX_BLOCKSIZE` を変更した場合、この値にも微調整が必要である。


.. c:macro:: BOOST_REGEX_MAX_CACHE_BLOCKS

   内部キャッシュに格納するメモリブロック数を設定する。メモリブロックは :cpp:func:`!::operator new` 呼び出しではなくこのキャッシュから割り当てられる。一般的にこの方法はメモリブロック要求のたびに :cpp:func:`!::operator new` を呼び出すよりも数段高速だが、巨大なメモリチャンク（サイズが :c:macro:`BOOST_REGEX_BLOCKSIZE` のブロックが最大 16 個）をキャッシュしなければならないという欠点がある。メモリの制限が厳しい場合は、この値を 0 に設定し（キャッシュはまったく行われない）、それで遅すぎる場合は 1 か 2 にするとよい。逆に巨大なマルチプロセッサ、マルチスレッドのシステムでは大きな値のほうがよい。
