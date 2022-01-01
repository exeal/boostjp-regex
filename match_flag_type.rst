.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


match_flag_type
===============

.. cpp:type:: implemenation_specific_bitmask_type match_flag_type

   :cpp:type:`!match_flag_type` 型は実装固有のビットマスク型（C++ 標準 17.3.2.1.2）で、正規表現の文字シーケンスに対するマッチ方法を制御する。書式化フラグの動作は\ :ref:`書式化構文ガイド <format>`\に詳細を記述する。

::

   namespace boost{ namespace regex_constants{

   typedef implemenation-specific-bitmask-type match_flag_type;

   static const match_flag_type match_default = 0;
   static const match_flag_type match_not_bob;
   static const match_flag_type match_not_eob;
   static const match_flag_type match_not_bol;
   static const match_flag_type match_not_eol;
   static const match_flag_type match_not_bow;
   static const match_flag_type match_not_eow;
   static const match_flag_type match_any;
   static const match_flag_type match_not_null;
   static const match_flag_type match_continuous;
   static const match_flag_type match_partial;
   static const match_flag_type match_single_line;
   static const match_flag_type match_prev_avail;
   static const match_flag_type match_not_dot_newline;
   static const match_flag_type match_not_dot_null;
   static const match_flag_type match_posix;
   static const match_flag_type match_perl;
   static const match_flag_type match_nosubs;
   static const match_flag_type match_extra;
   static const match_flag_type format_default = 0;
   static const match_flag_type format_sed;
   static const match_flag_type format_perl;
   static const match_flag_type format_literal;
   static const match_flag_type format_no_copy;
   static const match_flag_type format_first_only;
   static const match_flag_type format_all;

   } // namespace regex_constants
   } // namespace boost


.. _ref.match_flag_type.description:

説明
----

:cpp:type:`match_flag_type` 型は実装固有のビットマスク型（C++ 標準 17.3.2.1.2）である。文字シーケンス [first, last) に対して正規表現マッチを行うとき、各要素を設定した場合の効果を以下の表に示す。

.. list-table::
   :header-rows: 1

   * - 要素
     - 設定した場合の効果
   * - :cpp:var:`!match_default`
     - 正規表現マッチを ECMA-262, ECMAScript Language Specification, Chapter 15 part 10, RegExp (Regular Expression) Objects (FWD.1) で使用されている通常の規則にそのまま従うことを指定する。
   * - :cpp:var:`!match_not_bob`
     - 正規表現 :regexp:`\\A` および :regexp:`\\\`` が部分シーケンス [first,first) にマッチしないことを指定する。
   * - :cpp:var:`!match_not_eob`
     - 正規表現 :regexp:`\\'` 、:regexp:`\\z` および :regexp:`\\Z` が部分シーケンス [last,last) にマッチしないことを指定する。
   * - :cpp:var:`!match_not_bol`
     - 正規表現 :regexp:`^` が部分シーケンス [first,first) にマッチしないことを指定する。
   * - :cpp:var:`!match_not_eol`
     - 正規表現 :regexp:`$` が部分シーケンス [last,last) にマッチしないことを指定する。
   * - :cpp:var:`!match_not_bow`
     - 正規表現 :regexp:`\\<` および :regexp:`\\b` が部分シーケンス [first,first) にマッチしないことを指定する。
   * - :cpp:var:`!match_not_eow`
     - 正規表現 :regexp:`\\>` および :regexp:`\\b` が部分シーケンス [last,last) にマッチしないことを指定する。
   * - :cpp:var:`!match_any`
     - 複数のマッチが可能な場合に、それらのいずれでも結果として適合することを指定する。結果が最左マッチとなることには変わりないが、当該位置における最良マッチは保証されない。何がマッチするかよりも速度を優先する場合（マッチがあるかないかのみを調べる場合）にこのフラグを使用するとよい。
   * - :cpp:var:`!match_not_null`
     - 正規表現が空のシーケンスにマッチしないことを指定する。
   * - :cpp:var:`!match_continuous`
     - 正規表現が、先頭から始まる部分シーケンスにのみマッチすることを指定する。
   * - :cpp:var:`!match_partial`
     - マッチが見つからない場合に :cpp:expr:`from != last` であるマッチ [from, last) を結果として返すことを指定する（[from,last] を接頭辞とするより長い文字シーケンス [from,to) が完全マッチの結果として存在する可能性がある場合）。テキストが不完全であるか非常に長い場合に、このフラグを使用するとよい。詳細は部分マッチの項を見よ。
   * - :cpp:var:`!match_extra`
     - 有効な捕捉情報をすべて格納するように正規表現エンジンに指示する。捕捉グループが繰り返しになっている場合、:cpp:func:`!match_results::captures()` および :cpp:func:`!sub_match::captures()` を用いて各繰り返しに対する情報にアクセスできる。
   * - :cpp:var:`!match_single_line`
     - Perl の m 修飾子の反転と同様で、:regexp:`^` が組み込みの改行文字の直後に、:regexp:`$` が組み込みの改行文字の直前にマッチしないことを指定する（よって、この 2 つのアンカーはそれぞれマッチ対象テキストの先頭、終端にのみマッチする）。
   * - :cpp:var:`!match_prev_avail`
     - :cpp:expr:`--first` が合法なイテレータ位置であることを指定する。このフラグを設定した場合、正規表現アルゴリズム（RE.7）およびイテレータ（RE.8）はフラグ :cpp:var:`!match_not_bol` と :cpp:var:`!match_not_bow` を無視する。 [#]_
   * - :cpp:var:`!match_not_dot_newline`
     - 正規表現 :regexp:`.` が改行文字にマッチしないことを指定する。Perl の s 修飾子の反転と同じである。
   * - :cpp:var:`!match_not_dot_null`
     - 正規表現 :regexp:`.` が null 文字 :regexp:`\\0` にマッチしないことを指定する。
   * - :cpp:var:`!match_posix`
     - コンパイル済み正規表現の種類に関わらず、POSIX の\ :ref:`最左規則 <syntax.leftmost_longest_rule>`\にしたがって式のマッチを行うことを指定する。貪欲でない繰り返しなどの Perl 固有の多くの機能を使用する場合、これらの規則は正しく動作しないことに注意していただきたい。
   * - :cpp:var:`!match_perl`
     - コンパイル済み正規表現の種類に関わらず、:ref:`Perl のマッチ規則 <syntax.perl_syntax.what_gets_matched>`\にしたがって式のマッチを行うことを指定する。
   * - :cpp:var:`!match_nosubs`
     - 実際に捕捉グループが与えられていても、マーク済み部分式が存在しないとして正規表現を扱う。:cpp:class:`match_results` クラスにはマッチ全体に関する情報のみ含まれ、部分式については記録されない。
   * - :cpp:var:`!format_default`
     - 正規表現マッチを新文字列で置換するとき、ECMA-262, ECMAScript Language Specification, Chapter 15 part 5.4.11 String.prototype.replace. (FWD.1) の ECMAScript replace 関数で使用されている規則を用いて新文字列を構築する。

       機能的には :ref:`Perl の書式化文字列の規則 <format.perl_format>`\と等価である。

       検索・置換操作時に指定すると、正規表現は互いに重複しない位置でマッチし、置換する。正規表現にマッチしなかったテキスト部分はそのまま出力文字列にコピーする。
   * - :cpp:var:`!format_sed`
     - 正規表現マッチを新文字列で置換するとき、IEEE Std 1003.1-2001, Portable Operating SystemInterface (POSIX), Shells and UtilitiesのUnix sed ユーティリティで使用されている規則を用いて新文字列を構築する。:ref:`sed の書式化文字列リファレンス <format.sed_format>`\も見よ。
   * - :cpp:var:`!format_perl`
     - 正規表現マッチを新文字列で置換するとき、:ref:`Perl 5 と同じ規則 <format.perl_format>`\を用いて新文字列を生成する。
   * - :cpp:var:`!format_literal`
     - 正規表現マッチを新文字列で置換するとき、置換テキストの直値コピーを新文字列とする。
   * - :cpp:var:`!format_all`
     - 条件置換 :regexp:`(?ddexpression1:expression2)` を含むすべての構文拡張を有効にする。詳細は\ :doc:`書式化文字列のガイド <format_boost_syntax>`\を見よ。
   * - :cpp:var:`!format_no_copy`
     - 検索・置換操作時に指定すると、検索対象の文字コンテナシーケンスの正規表現にマッチしない部分を出力文字列にコピーしない。
   * - :cpp:var:`!format_first_only`
     - 検索・置換操作時に指定すると、最初の正規表現マッチのみを置換する。


.. [#] 訳注　“RE.n” は N1429 の節番号（http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2003/n1429.htm）。
