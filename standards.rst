.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

標準への適合
============

.. _background.standards.c__:

C++
---

Boost.Regex は `Technical Report on C++ Library Extensions <http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1836.pdf>`_ への適合を意図している。


.. _background.standards.ecmascript___javascript:

ECMAScript / JavaScript
-----------------------

以下を除く ECMAScript 正規表現構文のすべての機能をサポートする。

エスケープシーケンス :regexp:`\\u` は Unicode エスケープシーケンスではなく、大文字にマッチする（:regexp:`[[:upper:]]` と同じ）。Unicode エスケープには :regexp:`\\x{DDDD}` を使用する。


.. _background.standards.perl:

Perl
----

以下を除く Perl のほとんどすべての機能をサポートする。

* :regexp:`(?{code})` はコンパイルの必要な型付けの強い言語では実装不可能である。
* :regexp:`(??{code})` はコンパイルの必要な型付けの強い言語では実装不可能である。
* `特殊なバックトラック制御記号 <http://perldoc.perl.org/perlre.html#Special-Backtracking-Control-Verbs>`_\（:regexp:`(*VERB)`） [#]_
* また、:regexp:`^` :regexp:`$` :regexp:`\\Z` の機能は Perl とは少し動作が異なる。これらは :regexp:`\\n` 以外の改行シーケンスも取り扱う。後述の Unicode の要件を見よ。


.. _background.standards.posix:

POSIX
-----

以下を除く POSIX 標準および拡張のすべての機能をサポートする。

C ロカールでは特性クラスを使って明示的に登録しない限り、POSIX 標準で指定されているもの以外の照合名は解釈されない。

文字等価クラス（:regexp:`[[=a=]]` など）は Win32 以外ではおそらくバグがある。この機能の実装には、システムが生成する文字列ソートキーについての情報が必要である。この機能が必要で、既定の実装が使用しているプラットフォームで動作しない場合は、カスタムの特性クラスを用意する必要がある。


.. _background.standards.unicode:

Unicode
-------

以下のコメントは `Unicode Technical Report #18: Unicode Regular Expressions version 11 <http://www.unicode.org/reports/tr18/tr18-11.html>`_ に対するものである。

.. list-table::
   :header-rows: 1

   * - 項目
     - 機能
     - サポート
   * - 1.1
     - 16 進表記
     - サポート。コードポイント U+DDDD を表すには :regexp:`\\x{DDDD}` を使用する。
   * - 1.2
     - 文字プロパティ
     - 一般分類の名前はすべてサポートする。用字系およびその他の名前は現時点ではサポートしない。
   * - 1.3
     - 差および交差
     - 前方先読みにより間接的にサポートする。:regexp:`(?=[[:X:]])[[:Y:]]` は文字プロパティ :samp:`{X}` と :samp:`{Y}` の交差を与える。:regexp:`(?![[:X:]])[[:Y:]]` は :samp:`{Y}` の中で :samp:`{X}` に含まれないもの（差）を与える。
   * - 1.4
     - 単純な単語境界
     - 適合する。送りなし記号は単語構成文字として扱う。
   * - 1.5
     - 大文字小文字を区別しないマッチ
     - サポートする。この水準では大文字小文字の対応は一対一の変換であり、多対多のケースフォールディング（“ß” から “SS” への変換）はサポートしない。
   * - 1.6
     - 行境界
     - :regexp:`.` が :regex-input:`\\r\\n` の 1 文字ずつにしかマッチしないということ以外はサポートする。単語境界以外は正しくマッチする（:regex-input:`\\r\\n` シーケンスの中間にマッチしない）。
   * - 1.7
     - コードポイント
     - サポートする。:cpp:func:`!u32*` アルゴリズムを使用して UTF-8 、UTF-16 および UTF-32 をすべて 32 ビットコードポイント列として扱うことができる。
   * - 2.1
     - 正規等価
     - サポートしない。ライブラリのユーザがテキストを正規表現と同じ正規形に変換するしかない。
   * - 2.2
     - 既定の書記素
     - サポートしない。
   * - 2.3
     - 既定の単語境界
     - サポートしない。
   * - 2.4
     - 既定のあいまいマッチ
     - サポートしない。
   * - 2.5
     - 名前付きプロパティ
     - サポートする。正規表現 :regexp:`[[:name:]]` 、:regexp:`\\N{Name}` は名前付き文字 :samp:`{“name”}` にマッチする。
   * - 2.6
     - プロパティ名中のワイルドカード
     - サポートしない。
   * - 3.1
     - 区切り文字のテーラリング
     - サポートしない。
   * - 3.2
     - 書記素のテーラリング
     - サポートしない。
   * - 3.3
     - 単語境界のテーラリング
     - サポートしない。
   * - 3.4
     - テーラリングを用いたあいまいなマッチ
     - 部分的にサポートする。:regexp:`[[=c=]]` は :samp:`{c}` と同じ第 1 位の等価クラスを持つ文字にマッチする。
   * - 3.5
     - 範囲のテーラリング
     - サポートする。:cpp:var:`!collate` フラグを設定して式を構築した場合、:regexp:`[a-b]` は a から b の範囲に照合される文字にマッチする。
   * - 3.6
     - 文脈を考慮したマッチ
     - サポートしない。
   * - 3.7
     - インクリメンタルマッチ
     - サポートする。正規表現アルゴリズムにフラグ :cpp:var:`!match_partial` を渡す。
   * - 3.8
     - コンパイル済み文字集合の共有
     - サポートしない。
   * - 3.9
     - 可能なマッチの集合
     - サポートしない。しかしながらこの情報は正規表現のマッチを最適化するために内部的に使用し、可能なマッチが存在しない場合に高速に処理を返すようになっている。
   * - 3.10
     - フォールディング付きのマッチ
     - サポートしない。カスタムの正規表現特性クラスを用いることで似たような効果を得ることができる。
   * - 3.11
     - カスタムマッチ
     - サポートしない。


.. [#] 訳注　“Special backtracking control verbs”。Perl の新しい実験的機能のため、定訳らしきものがないのかもしれません。例えば http://fleur.hio.jp/perldoc/perl/5.9.5/pod/perlre.ja.html を参照してください。
