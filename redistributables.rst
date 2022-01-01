.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

再配布について
==============

Microsoft か Borland の C++ を使って実行時ライブラリの DLL 版にリンクしているのであれば、コードをコンパイルするときにシンボル :c:macro:`BOOST_REGEX_DYN_LINK` を定義して Boost.Regex の DLL 版にリンク可能である。これらの DLL は再配布可能だが「標準の」版というものが存在しないので、ユーザの PC にインストールする場合は、PC のディレクトリパスではなくアプリケーションの私的なディレクトリに配置するべきである。実行時ライブラリの静的版にリンクしているのであれば Boost.Regex の静的版にリンクすればよく、DLL の再配布は必要ない。Boost.Regex の DLL 、ライブラリがとり得る名前は `Getting Started <http://www.boost.org/more/getting_started.html>`_ ガイドに与えられている式から決定する。

コンパイル時にシンボル :c:macro:`BOOST_REGEX_NO_LIB` を定義すると、ライブラリの自動選択を無効にできるということに注意していただきたい。Boost.Regex を自分で IDE を使ってビルドしたい場合や Boost.Regex をデバッグする必要がある場合に役に立つ。
