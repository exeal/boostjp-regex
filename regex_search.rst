.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_search
============

::

   #include <boost/regex.hpp>

アルゴリズム :cpp:func:`regex_search` は、双方向イテレータの組で示される範囲から与えられた正規表現を検索する。このアルゴリズムは様々な発見的方法を用いて検索時間を短縮する。そのために、個々の位置からマッチが開始する可能性があるかチェックのみを行う。このアルゴリズムの定義は以下のとおりである。 ::

   template <class BidirectionalIterator,
            class Allocator, class charT, class traits>
   bool regex_search(BidirectionalIterator first, BidirectionalIterator last,
                     match_results<BidirectionalIterator, Allocator>& m,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);

   template <class ST, class SA,
            class Allocator, class charT, class traits>
   bool regex_search(const basic_string<charT, ST, SA>& s,
                     match_results<
                        typename basic_string<charT, ST,SA>::const_iterator,
                        Allocator>& m,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);

   template<class charT, class Allocator, class traits>
   bool regex_search(const charT* str,
                     match_results<const charT*, Allocator>& m,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);

   template <class BidirectionalIterator, class charT, class traits>
   bool regex_search(BidirectionalIterator first, BidirectionalIterator last,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);

   template <class charT, class traits>
   bool regex_search(const charT* str,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);

   template<class ST, class SA, class charT, class traits>
   bool regex_search(const basic_string<charT, ST, SA>& s,
                     const basic_regex<charT, traits>& e,
                     match_flag_type flags = match_default);


.. _ref.regex_search.description:

説明
----

.. cpp:function:: template <class BidirectionalIterator, class Allocator, class charT, class traits> \
		  bool regex_search(BidirectionalIterator first, BidirectionalIterator last, match_results<BidirectionalIterator, Allocator>& m, const basic_regex<charT, traits>& e, match_flag_type flags = match_default)

   :要件: 型 :cpp:type:`!BidirectionalIterator` が双方向イテレータの要件（24.1.4）を満たす。
   :効果: [first, last) 中に正規表現 :cpp:var:`!e` にマッチする部分シーケンスが存在するか判定する。引数 :cpp:var:`!flags` は、式が文字シーケンスに対してどのようにマッチするかを制御するのに使用する。完全なマッチが存在する場合は真を、それ以外の場合は偽を返す。
   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、正規表現のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。
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
	   - :cpp:expr:`m[0].first`
	 * - :cpp:expr:`m.prefix().matched`
	   - :cpp:expr:`m.prefix().first != m.prefix.second`
	 * - :cpp:expr:`m.suffix().first`
	   - :cpp:expr:`m[0].second`
	 * - :cpp:expr:`m.suffix().last`
	   - :cpp:var:`!last`
	 * - :cpp:expr:`m.suffix().matched`
	   - :cpp:expr:`m.suffix().first != m.suffix().second`
	 * - :cpp:expr:`m[0].first`
	   - 正規表現にマッチした文字シーケンスの先頭
	 * - :cpp:expr:`m[0].second`
	   - 正規表現にマッチした文字シーケンスの終端
	 * - :cpp:expr:`m[0].matched`
	   - 完全マッチが見つかった場合は真、（:cpp:var:`!match_partial` フラグを設定した結果）部分マッチが見つかった場合は偽。
	 * - :cpp:expr:`m[n].first`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの先頭。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は :cpp:var:`!last`。
	 * - :cpp:expr:`m[n].second`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの終端。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は :cpp:var:`last`。
	 * - :cpp:expr:`m[n].matched`
	   - :cpp:expr:`n < m.size()` であるすべての整数について部分式 :samp:`{n}` がマッチした場合は真、それ以外は偽。


.. cpp:function:: template <class charT, class Allocator, class traits> \
		  bool regex_search(const charT* str, match_results<const charT*, Allocator>& m, const basic_regex<charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_search(str, str + char_traits<charT>::length(str), m, e, flags)` の結果を返す。

.. cpp:function:: template <class ST, class SA, class Allocator, class charT, class traits> \
		  bool regex_search(const basic_string<charT, ST, SA>& s, match_results<typename basic_string<charT, ST, SA>::const_iterator, Allocator>& m, const basic_regex<charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_search(s.begin(), s.end(), m, e, flags)` の結果を返す。


.. cpp:function:: template <class iterator, class charT, class traits> \
		  bool regex_search(iterator first, iterator last, const basic_regex<charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:class:`!match_results<BidirectionalIterator>` のインスタンス :cpp:var:`!what` を構築し、:cpp:expr:`regex_search(first, last, what, e, flags)` の結果を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool regex_search(const charT* str, const basic_regex<charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_search(str, str + char_traits<charT>::length(str), e, flags)` の結果を返す。


.. cpp:function:: template <class ST, class SA, class charT, class traits> \
		  bool regex_search(const basic_string<charT, ST, SA>& s, const basic_regex <charT, traits>& e, match_flag_type flags = match_default)

   :効果: :cpp:expr:`regex_search(s.begin(), s.end(), e, flags)` の結果を返す。


.. _ref.regex_search.examples:

使用例
------

以下の例は、ファイルの内容を 1 つの文字列として読み取り、ファイル内の C++ クラス宣言をすべて検索する。このコードは :cpp:class:`!std::string` の実装方法に依存しない。例えば SGI の :cpp:class:`!rope` クラス（不連続メモリバッファが使われている）を使うように容易に修正できる。 ::

   #include <string>
   #include <map>
   #include <boost/regex.hpp>

   // 目的：
   // ファイルの内容を単一の文字列として受け取り、
   // C++ クラス宣言をすべて検索し、それらの位置を
   // 文字列対整数の辞書に保存する
   typedef std::map<std::string, int, std::less<std::string> > map_type;

   boost::regex expression(
      "^(template[[:space:]]*<[^;:{]+>[[:space:]]*)?"
      "(class|struct)[[:space:]]*"
      "(\\<\\w+\\>([[:blank:]]*\\([^)]*\\))?"
      "[[:space:]]*)*(\\<\\w*\\>)[[:space:]]*"
      "(<[^;:{]+>[[:space:]]*)?(\\{|:[^;\\{()]*\\{)");

   void IndexClasses(map_type& m, const std::string& file)
   {
      std::string::const_iterator start, end;
      start = file.begin();
      end = file.end();
      boost::match_results<std::string::const_iterator> what;
      boost::match_flag_type flags = boost::match_default;
      while(regex_search(start, end, what, expression, flags))
      {
         // what[0] には文字列全体が入り
         // what[5] にはクラス名が入る。
         // what[6] にはテンプレートの特殊化（あれば）が入り、
         // クラス名と位置を辞書に入れて対応させる：
         m[std::string(what[5].first, what[5].second)
               + std::string(what[6].first, what[6].second)]
            = what[5].first - file.begin();
         // 検索位置を更新する：
         start = what[0].second;
         // flags を更新する：
         flags |= boost::match_prev_avail;
         flags |= boost::match_not_bob;
      }
   }
