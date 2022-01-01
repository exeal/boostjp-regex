.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_match
===========

::

   #include <boost/regex.hpp>

アルゴリズム :cpp:func:`regex_match` は、与えられた正規表現が双方向イテレータの組で示された文字シーケンス\ **全体**\にマッチするか判定する。このアルゴリズムの定義は以下に示すとおりである。この関数の主な用途は入力データの検証である。

.. important::

   結果が真となるのは式が入力シーケンス\ **全体**\にマッチする場合のみということに注意していただきたい。シーケンス内で式を検索するには :cpp:func:`regex_search` を使用する。文字列の先頭でマッチを行う場合は、フラグ :cpp:var:`!match_continuous` を設定して :cpp:func:`regex_search` を使用する。

::

   template <class BidirectionalIterator, class Allocator, class charT, class traits>
   bool regex_match(BidirectionalIterator first, BidirectionalIterator last,
                    match_results<BidirectionalIterator, Allocator>& m,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);

   template <class BidirectionalIterator, class charT, class traits>
   bool regex_match(BidirectionalIterator first, BidirectionalIterator last,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);

   template <class charT, class Allocator, class traits>
   bool regex_match(const charT* str, match_results<const charT*, Allocator>& m,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);

   template <class ST, class SA, class Allocator, class charT, class traits>
   bool regex_match(const basic_string<charT, ST, SA>& s,
                    match_results<typename basic_string<charT, ST, SA>::const_iterator, Allocator>& m,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);

   template <class charT, class traits>
   bool regex_match(const charT* str,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);

   template <class ST, class SA, class charT, class traits>
   bool regex_match(const basic_string<charT, ST, SA>& s,
                    const basic_regex <charT, traits>& e,
                    match_flag_type flags = match_default);


.. _ref.regex_match.description:

説明
----

.. cpp:function:: template <class BidirectionalIterator, class Allocator, class charT, class traits> \
		  bool regex_match(BidirectionalIterator first, BidirectionalIterator last, match_results<BidirectionalIterator, Allocator>& m, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :要件: 型 :cpp:type:`!BidirectionalIterator` が双方向イテレータの要件（24.1.4）を満たす。
   :効果: 正規表現 :cpp:var:`!e` と文字シーケンス [first, last) 全体の間に完全なマッチが存在するか判定する。引数 :cpp:var:`!flags`\（:cpp:type:`match_flag_type` を見よ）は、正規表現が文字シーケンスに対してどのようにマッチするかを制御するのに使用する。完全なマッチが存在する場合は真を、それ以外の場合は偽を返す。
   :throws std\:\:runtime_error: 長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、正規表現のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）
   :事後条件:
      関数が偽を返した場合、引数 :cpp:var:`!m` の状態は未定義である。それ以外の場合は次の表のとおりである。

      .. list-table::
	 :header-rows: 1

	 * - 要素
	   - 値
	 * - :cpp:expr:`m.size()`
	   - :cpp:expr:`1 + e.mark_count()`
	 * - :cpp:expr:`m.empty()`
	   - :cpp:var:`!false`
	 * - :cpp:expr:`m.prefix().first`
	   - :cpp:var:`!first`
	 * - :cpp:expr:`m.prefix().last`
	   - :cpp:var:`!first`
	 * - :cpp:expr:`m.prefix().matched`
	   - :cpp:var:`!false`
	 * - :cpp:expr:`m.suffix().first`
	   - :cpp:var:`!last`
	 * - :cpp:expr:`m.suffix().last`
	   - :cpp:var:`!last`
	 * - :cpp:expr:`m.suffix().matched`
	   - :cpp:var:`!false`
	 * - :cpp:expr:`m[0].first`
	   - :cpp:var:`!first`
	 * - :cpp:expr:`m[0].second`
	   - :cpp:var:`last`
	 * - :cpp:expr:`m[0].matched`
	   - 完全マッチが見つかった場合は真、（:cpp:var:`!match_partial` フラグを設定した結果）部分マッチが見つかった場合は偽。
	 * - :cpp:expr:`m[n].first`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの先頭。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は :cpp:var:`!last`。
	 * - :cpp:expr:`m[n].second`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの終端。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は :cpp:var:`!last`。
	 * - :cpp:expr:`m[n].matched`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` にマッチした場合は真、それ以外は偽。

   
.. cpp:function:: template <class BidirectionalIterator, class charT, class traits> \
		  bool regex_match(BidirectionalIterator first, BidirectionalIterator last, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:class:`!match_results<BidirectionalIterator>` のインスタンス :cpp:var:`!what` を構築し、:cpp:expr:`regex_match(first, last, what, e, flags)` の結果を返す。


.. cpp:function:: template <class charT, class Allocator, class traits> \
		  bool regex_match(const charT* str, match_results<const charT*, Allocator>& m, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_match(str, str + char_traits<charT>::length(str), m, e, flags)` の結果を返す。


.. cpp:function:: template <class ST, class SA, class Allocator, class charT, class traits> \
		  bool regex_match(const basic_string<charT, ST, SA>& s, match_results<typename basic_string<charT, ST, SA>::const_iterator, Allocator>& m, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_match(s.begin(), s.end(), m, e, flags)` の結果を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool regex_match(const charT* str, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_match(str, str + char_traits<charT>::length(str), e, flags)` の結果を返す。


.. cpp:function:: template <class ST, class SA, class charT, class traits> \
		  bool regex_match(const basic_string<charT, ST, SA>& s, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_match(s.begin(), s.end(), e, flags)` の結果を返す。


.. _ref.regex_match.examples:

使用例
------

以下は FTP 応答を処理する例である。 ::

   #include <stdlib.h>
   #include <boost/regex.hpp>
   #include <string>
   #include <iostream>

   using namespace boost;

   regex expression("([0-9]+)"
                    "(\\-| |$)"
		    "(.*)");  // 訳注　原文の文字列は Sphinx が解釈できないため改行しました

   // process_ftp：
   // 成功時は FTP 応答コードを返し、
   // msg に応答メッセージを書き込む。
   int process_ftp(const char* response, std::string* msg)
   {
      cmatch what;
      if(regex_match(response, what, expression))
      {
         // what[0] には文字列全体が入る
         // what[1] には応答コードが入る
         // what[2] には区切り文字が入る
         // what[3] にはテキストメッセージが入る。
         if(msg)
            msg->assign(what[3].first, what[3].second);
         return std::atoi(what[1].first);
      }
      // マッチしなかったら失敗
      if(msg)
         msg->erase();
      return -1;
   }
