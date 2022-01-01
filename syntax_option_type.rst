.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


syntax_option_type
==================

.. contents::
   :depth: 1
   :local:


.. cpp:type:: implementation_specific_bitmask_type syntax_option_type

   :cpp:type:`syntax_option_type` 型は実装固有のビットマスク型で、正規表現文字列の解釈方法を制御する。利便性のために、ここに挙げる定数はすべて :cpp:class:`basic_regex` テンプレートクラスのスコープにも複製していることに注意していただきたい。


.. _ref.syntax_option_type.syntax_option_type_synopsis:

syntax_option_type の概要
-------------------------

::

   namespace std{ namespace regex_constants{

   typedef implementation_specific_bitmask_type syntax_option_type;

   // 以下のフラグは標準化されている：
   static const syntax_option_type normal;
   static const syntax_option_type ECMAScript = normal;
   static const syntax_option_type JavaScript = normal;
   static const syntax_option_type JScript = normal;
   static const syntax_option_type perl = normal;
   static const syntax_option_type basic;
   static const syntax_option_type sed = basic;
   static const syntax_option_type extended;
   static const syntax_option_type awk;
   static const syntax_option_type grep;
   static const syntax_option_type egrep;
   static const syntax_option_type icase;
   static const syntax_option_type nosubs;
   static const syntax_option_type optimize;
   static const syntax_option_type collate;

   //
   // 残りのオプションは Boost.Regex 固有のものである：
   //

   // Perl および POSIX 正規表現共通のオプション：
   static const syntax_option_type newline_alt;
   static const syntax_option_type no_except;
   static const syntax_option_type save_subexpression_location;

   // Perl 固有のオプション：
   static const syntax_option_type no_mod_m;
   static const syntax_option_type no_mod_s;
   static const syntax_option_type mod_s;
   static const syntax_option_type mod_x;
   static const syntax_option_type no_empty_expressions;

   // POSIX 拡張固有のオプション：
   static const syntax_option_type no_escape_in_lists;
   static const syntax_option_type no_bk_refs;

   // POSIX 基本のオプション：
   static const syntax_option_type no_escape_in_lists;
   static const syntax_option_type no_char_classes;
   static const syntax_option_type no_intervals;
   static const syntax_option_type bk_plus_qm;
   static const syntax_option_type bk_vbar;

   } // namespace regex_constants
   } // namespace std


.. _ref.syntax_option_type.syntax_option_type_overview:

syntax_option_type の概観
-------------------------

:cpp:type:`syntax_option_type` 型は実装固有のビットマスク型である（C++ 標準 17.3.2.1.2 を見よ）。各要素の効果は以下の表に示すとおりである。:cpp:type:`syntax_option_type` 型の値は :cpp:var:`!normal` 、:cpp:var:`!basic` 、:cpp:var:`!extended` 、:cpp:var:`!awk` 、:cpp:var:`!grep` 、:cpp:var:`!egrep` 、:cpp:var:`!sed` 、:cpp:var:`!literal` 、:cpp:var:`!perl` のいずれか 1 つの要素を必ず含んでいなければならない。

利便性のために、ここに挙げる定数はすべて :cpp:class:`basic_regex` テンプレートクラスのスコープにも複製していることに注意していただきたい。よって、次のコードは、 ::

   boost::regex_constants::constant_name

次のように書くことができる。 ::

   boost::regex::constant_name

あるいは次のようにも書ける。 ::

   boost::wregex::constant_name

以上はいずれも同じ意味である。


.. _ref.syntax_option_type.syntax_option_type_perl:

Perl 正規表現のオプション
-------------------------

Perl の正規表現では、以下のいずれか 1 つを必ず設定しなければならない。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!ECMAScript`
     - ○
     - 正規表現エンジンが解釈する文法が通常のセマンティクスに従うことを指定する。ECMA-262, ECMAScript Language Specification, Chapter 15 part 10, RegExp (Regular Expression) Objects (FWD.1) に与えられているものと同じである。

       これは :doc:`syntax_perl`\と機能的には等価である。

       このモードでは、Boost.Regex は Perl 互換の :regexp:`(?…)` 拡張もサポートする。
   * - :cpp:var:`!perl`
     - ×
     - 上に同じ。
   * - :cpp:var:`!normal`
     - ×
     - 上に同じ。
   * - :cpp:var:`!JavaScript`
     - ×
     - 上に同じ。
   * - :cpp:var:`!JScript`
     - ×
     - 上に同じ。

Perl スタイルの正規表現を使用する場合は、以下のオプションを組み合わせることができる。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!icase`
     - ○
     - 文字コンテナシーケンスに対する正規表現マッチにおいて、大文字小文字を区別しないことを指定する。
   * - :cpp:var:`!nosubs`
     - ○
     - 文字コンテナシーケンスに対して正規表現マッチしたときに、与えられた :cpp:class:`match_results` 構造体に部分式マッチを格納しないように指定する。
   * - :cpp:var:`!optimize`
     - ○
     - 正規表現エンジンに対し、正規表現オブジェクトの構築速度よりも正規表現マッチの速度についてより多くの注意を払うように指定する。設定しない場合でもプログラムの出力に検出可能な効果はない。Boost.Regex では現時点では何も起こらない。
   * - :cpp:var:`!collate`
     - ○
     - :regexp:`[a-b]` 形式の文字範囲がロカールを考慮するように指定する。
   * - :cpp:var:`!newline_alt`
     - ×
     - :regexp:`\n` 文字が選択演算子 :regexp:`|` と同じ効果を持つように指定する。これにより、改行で区切られたリストが選択のリストとして動作する。
   * - :cpp:var:`!no_except`
     - ×
     - 不正な式が見つかった場合に :cpp:class:`basic_regex` が例外を投げるのを禁止する。
   * - :cpp:var:`!no_mod_m`
     - ×
     - 通常 Boost.Regex は Perl の m 修飾子が設定された状態と同じ動作をし、表明 :regexp:`^` および :regexp:`$` はそれぞれ改行の直前および直後にマッチする。このフラグを設定するのは式の前に :regexp:`(?-m)` を追加するのと同じである。
   * - :cpp:var:`!no_mod_s`
     - ×
     - 通常 Boost.Regex において :regexp:`.` が改行文字にマッチするかはマッチフラグ :cpp:var:`!match_dot_not_newline` により決まる。このフラグを設定するのは式の前に :regexp:`(?-s)` を追加するのと同じであり、:regexp:`.` はマッチフラグに :cpp:var:`!match_dot_not_newline` が設定されているかに関わらず改行文字にマッチしない。
   * - :cpp:var:`!mod_s`
     - ×
     - 通常 Boost.Regex において :regexp:`.` が改行文字にマッチするかはマッチフラグ :cpp:var:`!match_dot_not_newline` により決まる。このフラグを設定するのは式の前に :regexp:`(?s)` を追加するのと同じであり、:regexp:`.` はマッチフラグに :cpp:var:`!match_dot_not_newline` が設定されているかに関わらず改行文字にマッチする。
   * - :cpp:var:`!mod_x`
     - ×
     - Perl の x 修飾子を有効にする。正規表現中のエスケープされていない空白は無視される。
   * - :cpp:var:`!no_empty_expressions`
     - ×
     - 空の部分式および選択を禁止する。
   * - :cpp:var:`!save_subexpression_location`
     - ×
     - **元の正規表現文字列**\における個々の部分式の位置に、:cpp:class:`!basic_regex` の :cpp:func:`~basic_regex::subexpression()` メンバ関数でアクセス可能になる。


.. _ref.syntax_option_type.syntax_option_type_extended:

POSIX 拡張正規表現のオプション
------------------------------

:doc:`POSIX 拡張正規表現 <syntax_extended>`\では、以下のいずれか1つを必ず設定しなければならない。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!extended`
     - ○
     - 正規表現エンジンが IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Base Definitions and Headers, Section 9, Regular Expressions (FWD.1) の POSIX 拡張正規表現で使用されているものと同じ文法に従うことを指定する。

       詳細は\ :doc:`POSIX 拡張正規表現ガイド <syntax_extended>`\を参照せよ。

       Perl スタイルのエスケープシーケンスもいくつかサポートする（POSIX 標準の定義では「特殊な」文字のみがエスケープ可能であり、他のエスケープシーケンスを使用したときの結果は未定義である）。
   * - :cpp:var:`!egrep`
     - ○
     - 正規表現エンジンが IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, Utilities, grep (FWD.1) の POSIX ユーティリティに :option:`!-E` オプションを与えた場合と同じ文法に従うことを指定する。

       つまり :doc:`POSIX 拡張構文 <syntax_extended>`\と同じであるが、改行文字が :regexp:`|` と同じく選択文字として動作する。
   * - :cpp:var:`!awk`
     - ○
     - 正規表現エンジンが IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, awk (FWD.1) の POSIX ユーティリティ :program:`awk` の文法に従うことを指定する。

       つまり :doc:`POSIX 拡張構文 <syntax_extended>`\と同じであるが、文字クラス中のエスケープシーケンスが許容される。

       さらに Perl スタイルのエスケープシーケンスもいくつかサポートする（実際には :program:`awk` の構文は :regexp:`\\a` 、:regexp:`\\b` 、:regexp:`\\t` 、:regexp:`\\v` 、:regexp:`\\f` 、:regexp:`\\n` および :regexp:`\\r` のみを要求しており、他のすべての Perl スタイルのエスケープシーケンスを使用したときの動作は未定義であるが、Boost.Regex では実際には後者も解釈する）。

POSIX 拡張正規表現を使用する場合は、以下のオプションを組み合わせることができる。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!icase`
     - ○
     - 文字コンテナシーケンスに対する正規表現マッチにおいて、大文字小文字を区別しないことを指定する。
   * - :cpp:var:`!nosubs`
     - ○
     - 文字コンテナシーケンスに対して正規表現マッチしたときに、与えられた :cpp:class:`match_results` 構造体に部分式マッチを格納しないように指定する。
   * - :cpp:var:`!optimize`
     - ○
     - 正規表現エンジンに対し、正規表現オブジェクトの構築速度よりも正規表現マッチの速度についてより多くの注意を払うように指定する。設定しない場合でもプログラムの出力に検出可能な効果はない。Boost.Regex では現時点では何も起こらない。
   * - :cpp:var:`!collate`
     - ○
     - :regexp:`[a-b]` 形式の文字範囲がロカールを考慮するように指定する。このビットは POSIX 拡張正規表現では既定でオンであるが、オフにして範囲をコードポイントのみで比較するようにすることが可能である。
   * - :cpp:var:`!newline_alt`
     - ×
     - :regexp:`\\n` 文字が選択演算子 :regexp:`|` と同じ効果を持つように指定する。これにより、改行で区切られたリストが選択のリストとして動作する。
   * - :cpp:var:`!no_escape_in_lists`
     - ×
     - 設定するとエスケープ文字はリスト内で通常の文字として扱われる。よって :regexp:`[\b]` は :regex-input:`\\` か :regex-input:`b` にマッチする。このビットは POSIX 拡張正規表現では既定でオンであるが、オフにしてリスト内でエスケープが行われるようにすることが可能である。
   * - :cpp:var:`!no_bk_refs`
     - ×
     - 設定すると後方参照が無効になる。このビットは POSIX 拡張正規表現では既定でオンであるが、オフにして後方参照を有効にすることが可能である。
   * - :cpp:var:`!no_except`
     - ×
     - 不正な式が見つかった場合に :cpp:class:`basic_regex` が例外を投げるのを禁止する。
   * - :cpp:var:`!save_subexpression_location`
     - ×
     - **元の正規表現文字列**\における個々の部分式の位置に、:cpp:class:`!basic_regex` の :cpp:func:`~basic_regex::subexpression()` メンバ関数でアクセス可能になる。


.. _ref.syntax_option_type.syntax_option_type_basic:

POSIX 基本正規表現のオプション
------------------------------

POSIX 基本正規表現では、以下のいずれか 1 つを必ず設定しなければならない。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!basic`
     - ○
     - 正規表現エンジンが IEEE　Std 1003.1-2001, Portable Operating System Interface　(POSIX), Base Definitions and Headers, Section 9, Regular Expressions (FWD.1) の :doc:`POSIX 基本正規表現 <syntax_basic>`\で使用されているものと同じ文法に従うことを指定する。
   * - :cpp:var:`!sed`
     - ×
     - 上に同じ。
   * - :cpp:var:`!grep`
     - ○
     - 正規表現エンジンが IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, Utilities, grep (FWD.1) の POSIX :program:`grep` ユーティリティで使用されているものと同じ文法に従うことを指定する。

       つまり :doc:`POSIX 基本構文 <syntax_basic>`\と同じであるが、改行文字が選択文字として動作する。式は改行区切りの選択リストとして扱われる。
   * - :cpp:var:`!emacs`
     - ×
     - 使用する文法が emacs プログラムで使われている :doc:`POSIX 基本構文 <syntax_basic>`\のスーパーセットであることを指定する。

POSIX 基本正規表現を使用する場合は、以下のオプションを組み合わせることができる。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!icase`
     - ○
     - 文字コンテナシーケンスに対する正規表現マッチにおいて、大文字小文字を区別しないことを指定する。
   * - :cpp:var:`!nosubs`
     - ○
     - 文字コンテナシーケンスに対して正規表現マッチしたときに、与えられた :cpp:class:`match_results` 構造体に部分式マッチを格納しないように指定する。
   * - :cpp:var:`!optimize`
     - ○
     - 正規表現エンジンに対し、正規表現オブジェクトの構築速度よりも正規表現マッチの速度についてより多くの注意を払うように指定する。設定しない場合でもプログラムの出力に検出可能な効果はない。Boost.Regex では現時点では何も起こらない。
   * - :cpp:var:`!collate`
     - ○
     - :regexp:`[a-b]` 形式の文字範囲がロカールを考慮するように指定する。このビットは :doc:`POSIX 基本正規表現 <syntax_basic>`\では既定でオンであるが、オフにして範囲をコードポイントのみで比較するようにすることが可能である。
   * - :cpp:var:`!newline_alt`
     - ○
     - :regexp:`\\n` 文字が選択演算子 :regexp:`|` と同じ効果を持つように指定する。これにより、改行で区切られたリストが選択のリストとしてはたらく。:cpp:var:`!grep` オプションの場合はこのビットは常にオンである。
   * - :cpp:var:`!no_char_classes`
     - ×
     - 設定すると :regexp:`[[:alnum:]]` のような文字クラスは認められないようになる。
   * - :cpp:var:`!no_escape_in_lists`
     - ×
     - 設定するとエスケープ文字はリスト内で通常の文字として扱われる。よって :regexp:`[\\b]` は :regex-input:`\\` か :regex-input:`b` にマッチする。このビットは :doc:`POSIX 基本正規表現 <syntax_basic>`\では既定でオンであるが、オフにしてリスト内でエスケープが行われるようにすることが可能である。
   * - :cpp:var:`!no_intervals`
     - ×
     - 設定すると :regexp:`{2,3}` のような境界付き繰り返しは認められないようになる。
   * - :cpp:var:`!bk_plus_qm`
     - ×
     - 設定すると :regexp:`\\?` が 0 か 1 回の繰り返し演算子、:regexp:`\\+` が 1 回以上の繰り返し演算子として動作する。
   * - :cpp:var:`!bk_vbar`
     - ×
     - 設定すると :regexp:`\\|` が選択演算子として動作する。
   * - :cpp:var:`!no_except`
     - ×
     - 不正な式が見つかった場合に :cpp:class:`basic_regex` が例外を投げるのを禁止する。
   * - :cpp:var:`!save_subexpression_location`
     - ×
     - **元の正規表現文字列**\における個々の部分式の位置に、:cpp:class:`!basic_regex` の :cpp:func:`~basic_regex::subexpression()` メンバ関数でアクセス可能になる。


.. _ref.syntax_option_type.syntax_option_type_literal:

直値文字列のオプション
----------------------

直値文字列では、以下のいずれか 1 つを必ず設定しなければならない。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!literal`
     - ○
     - 文字列を直値として扱う（特殊文字が存在しない）。

:cpp:var:`!literal` フラグを使用する場合は、以下のオプションを組み合わせることができる。

.. list-table::
   :header-rows: 1

   * - 要素
     - 標準か
     - 設定した場合の効果
   * - :cpp:var:`!icase`
     - ○
     - 文字コンテナシーケンスに対する正規表現マッチにおいて、大文字小文字を区別しないことを指定する。
   * - :cpp:var:`!optimize`
     - ○
     - 正規表現エンジンに対し、正規表現オブジェクトの構築速度よりも正規表現マッチの速度についてより多くの注意を払うように指定する。設定しない場合でもプログラムの出力に検出可能な効果はない。Boost.Regex では現時点では何も起こらない。
