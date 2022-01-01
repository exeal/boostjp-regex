.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_iterator
==============

.. cpp:class:: template <class BidirectionalIterator, \
                         class charT = iterator_traits<BidirectionalIterator>::value_type, \
                         class traits = regex_traits<charT> > \
	       boost::regex_iterator

   ::

      #include <boost/regex.hpp>

   イテレータ型 :cpp:class:`regex_iterator` はシーケンス中で見つかった正規表現マッチをすべて列挙する。:cpp:class:`regex_iterator` を逆参照すると :cpp:class:`match_results` オブジェクトへの参照が得られる。


.. cpp:namespace-push:: regex_iterator


.. parsed-literal::

   template <class BidirectionalIterator,
             class charT = iterator_traits<BidirectionalIterator>::value_type,
             class traits = regex_traits<charT> >
   class regex_iterator
   {
   public:
      typedef          basic_regex<charT, traits>                              regex_type;
      typedef          match_results<BidirectionalIterator>                    value_type;
      typedef typename iterator_traits<BidirectionalIterator>::difference_type difference_type;
      typedef          const value_type*                                       pointer;
      typedef          const value_type&                                       reference;
      typedef          std::forward_iterator_tag                               iterator_category;

      :cpp:func:`~regex_iterator::regex_iterator`\();
      :cpp:func:`~regex_iterator::regex_iterator`\(BidirectionalIterator a, BidirectionalIterator b,
                     const regex_type& re,
                     match_flag_type m = match_default);
      :cpp:func:`~regex_iterator::regex_iterator`\(const regex_iterator&);
      regex_iterator& :cpp:func:`operator=`\(const regex_iterator&);
      bool :cpp:func:`operator==`\(const regex_iterator&)const;
      bool :cpp:func:`operator!=`\(const regex_iterator&)const;
      const value_type& :cpp:func:`operator*`\()const;
      const value_type* :cpp:func:`operator->`\()const;
      regex_iterator& :cpp:func:`operator++`\();
      regex_iterator :cpp:func:`operator++`\(int);
   };

   typedef regex_iterator<const char*>                  cregex_iterator;
   typedef regex_iterator<std::string::const_iterator>  sregex_iterator;

   #ifndef BOOST_NO_WREGEX
   typedef regex_iterator<const wchar_t*>               wcregex_iterator;
   typedef regex_iterator<std::wstring::const_iterator> wsregex_iterator;
   #endif

   template <class charT, class traits> regex_iterator<const charT*, charT, traits>
      :cpp:func:`make_regex_iterator`\(const charT* p, const basic_regex<charT, traits>& e,
                          regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits, class ST, class SA>
   regex_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits>
      :cpp:func:`make_regex_iterator`\(const std::basic_string<charT, ST, SA>& p,
                          const basic_regex<charT, traits>& e,
                          regex_constants::match_flag_type m = regex_constants::match_default);


.. _ref.regex_iterator.description:

説明
----

.. cpp:function:: regex_iterator()

   :cpp:class:`regex_iterator` はイテレータの組で構築され、イテレータ範囲の正規表現マッチをすべて列挙する。

   :効果: シーケンスの終了を指す :cpp:class:`regex_iterator` を構築する。

.. cpp:function:: regex_iterator(BidirectionalIterator a, BidirectionalIterator b, const regex_type& re, match_flag_type m = match_default)

   :効果: シーケンス [a,b) 内で正規表現 :cpp:var:`!re` と :cpp:type:`match_flag_type` :cpp:var:`!m` を使って見つかるすべてのマッチを列挙する :cpp:class:`regex_iterator` を構築する。オブジェクト :cpp:var:`!re` は :cpp:class:`regex_iterator` の生涯にわたって存在していなければならない。
   :throws std\:\:runtime_error: 長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。


.. cpp:function:: regex_iterator(const regex_iterator& that)

   :効果: :cpp:var:`!that` のコピーを構築する。
   :事後条件: :cpp:expr:`*this == that`。


.. cpp:function:: regex_iterator& operator=(const regex_iterator& that)

   :効果: :cpp:expr:`*this` を :cpp:var:`!that` と等価にする。
   :事後条件: :cpp:expr:`*this == that`。


.. cpp:function:: bool operator==(const regex_iterator& that) const

   :効果: :cpp:expr:`*this` を :cpp:var:`!that` が等価であれば真を返す。


.. cpp:function:: bool operator!=(const regex_iterator& that) const

   :効果: :cpp:expr:`!(*this == that)` を返す。


.. cpp:function:: const value_type& operator*() const

   :効果: :cpp:class:`regex_iterator` の逆参照は :cpp:class:`match_results` オブジェクトへの参照である。そのメンバは次のとおりである。

   .. list-table::
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`(*it).size()`
        - :cpp:expr:`1 + re.mark_count()`
      * - :cpp:expr:`(*it).empty()`
        - :cpp:var:`!false`
      * - :cpp:expr:`(*it).prefix().first`
        - 最後に見つかったマッチの終端。最初の列挙の場合は対象シーケンスの先頭。
      * - :cpp:expr:`(*it).prefix().last`
        - 見つかったマッチの先頭と同じ。:cpp:expr:`(*it)[0].first`
      * - :cpp:expr:`(*it).prefix().matched`
        - マッチ全体より前の部分が空文字列でなければ真。:cpp:expr:`(*it).prefix().first != (*it).prefix().second`
      * - :cpp:expr:`(*it).suffix().first`
        - 見つかったマッチの終端と同じ。:cpp:expr:`(*it)[0].second`
      * - :cpp:expr:`(*it).suffix().last`
        - 対象シーケンスの終端。
      * - :cpp:expr:`(*it).suffix().matched`
        - マッチ全体より後ろの部分が空文字列でなければ真。:cpp:expr:`(*it).suffix().first != (*it).suffix().second`
      * - :cpp:expr:`(*it)[0].first`
        - 正規表現にマッチした文字シーケンスの先頭。
      * - :cpp:expr:`(*it)[0].second`
        - 正規表現にマッチした文字シーケンスの終端。
      * - :cpp:expr:`(*it)[0].matched`
        - 完全マッチが見つかった場合は真、（:cpp:var:`!match_partial` フラグを設定した結果）部分マッチが見つかった場合は偽。
      * - :cpp:expr:`(*it)[n].first`
        - :cpp:expr:`n < (*it).size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの先頭。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は last。
      * - :cpp:expr:`(*it)[n].second`
        - :cpp:expr:`n < (*it).size()` であるすべての整数について部分式 :samp:`{n}` にマッチしたシーケンスの終端。それ以外で部分式 :samp:`{n}` がマッチしなかった場合は last。
      * - :cpp:expr:`(*it)[n].matched`
        - :cpp:expr:`n < (*it).size()` であるすべての整数について部分式 :samp:`{n}` がマッチした場合は真、それ以外は偽。
      * - :cpp:expr:`(*it).position(n)`
        - :cpp:expr:`n < (*it).size()` であるすべての整数について、対象シーケンスの先頭から部分式 :samp:`{n}` の先頭までの距離。


.. cpp:function:: const value_type* operator->() const

   :効果: :cpp:expr:`&(*this)` を返す。


.. cpp:function:: regex_iterator& operator++()

   :効果: イテレータを対象シーケンス中の次のマッチに移動する。何も見つからない場合はシーケンスの終端に移動する。最後のマッチが長さ 0 の文字列へのマッチである場合は、:cpp:class:`regex_iterator` は以下の要領で次のマッチを検索する。非 0 長のマッチが最後のマッチと同じ位置から始まっている場合は、そのマッチを返す。それ以外の場合は次のマッチ（再び長さが 0 ということもありうる）を最後のマッチの 1 つ右の位置から検索する。
   :throws std\:\:runtime_error: 長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。
   :returns: :cpp:expr:`*this`。


.. cpp:function:: regex_iterator operator++(int)

   :効果: 戻り値用に :cpp:expr:`*this` のコピーを構築した後、:cpp:expr:`++(*this)` を呼び出す。
   :returns: 結果。


.. cpp:namespace-pop::


.. cpp:function:: template <class charT, class traits> \
                  regex_iterator<const charT*, charT, traits> make_regex_iterator(const charT* p, const basic_regex<charT, traits>& e, regex_constants::match_flag_type m = regex_constants::match_default)
                  template <class charT, class traits, class ST, class SA> \
                  regex_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits> make_regex_iterator(const std::basic_string<charT, ST, SA>& p, const basic_regex<charT, traits>& e, regex_constants::match_flag_type m = regex_constants::match_default)

   :効果: 式 :cpp:var:`!e` と :cpp:type:`match_flag_type` :cpp:var:`!m` を用いてテキスト :cpp:var:`!p` 中で見つかるすべてのマッチを列挙するイテレータを返す。


.. _ref.regex_iterator.examples:

使用例
------

次の例は C++ ソースファイルを受け取り、クラス名とそのクラスのファイル内での位置を含んだ索引を作成する。 ::

   #include <string>
   #include <map>
   #include <fstream>
   #include <iostream>
   #include <boost/regex.hpp>

   using namespace std;

   // 目的：
   // ファイルの内容を 1 つの文字列として受け取り
   // C++ クラス定義をすべて検索し、それらの位置を
   // 文字列対整数の辞書に保存する。

   typedef std::map<std::string, std::string::difference_type, std::less<std::string> > map_type;

   const char* re =
      // 前に空白があってもよい：
      "^[[:space:]]*"
      // テンプレート宣言があってもよい：
      "(template[[:space:]]*<[^;:{]+>[[:space:]]*)?"
      // class か struct：
      "(class|struct)[[:space:]]*"
      // declspec マクロなど：
      "("
         "\\<\\w+\\>"
         "("
            "[[:blank:]]*\\([^)]*\\)"
         ")?"
         "[[:space:]]*"
      ")*"
      // クラス名
      "(\\<\\w*\\>)[[:space:]]*"
      // テンプレート特殊化引数
      "(<[^;:{]+>)?[[:space:]]*"
      // { か : で終了
      "(\\{|:[^;\\{()]*\\{)";

   boost::regex expression(re);
   map_type class_index;

   bool regex_callback(const boost::match_results<std::string::const_iterator>& what)
   {
      // what[0] には文字列全体が入り
      // what[5] にはクラス名が入る。
      // what[6] にはテンプレートの特殊化が入る（あれば）。
      // クラス名と位置を辞書に入れる：
      class_index[what[5].str() + what[6].str()] = what.position(5);
      return true;
   }

   void load_file(std::string& s, std::istream& is)
   {
      s.erase();
      s.reserve(is.rdbuf()->in_avail());
      char c;
      while(is.get(c))
      {
         if(s.capacity() == s.size())
            s.reserve(s.capacity() * 3);
         s.append(1, c);
      }
   }

   int main(int argc, const char** argv)
   {
      std::string text;
      for(int i = 1; i < argc; ++i)
      {
         cout << "次のファイルを処理中 " << argv[i] << endl;
         std::ifstream fs(argv[i]);
         load_file(text, fs);
         // イテレータを構築しておく：
         boost::sregex_iterator m1(text.begin(), text.end(), expression);
         boost::sregex_iterator m2;
         std::for_each(m1, m2, &regex_callback);
         // 結果をコピーする：
         cout << class_index.size() << " 個のマッチが見つかりました" << endl;
         map_type::iterator c, d;
         c = class_index.begin();
         d = class_index.end();
         while(c != d)
         {
            cout << "クラス \"" << (*c).first << "\" が次の位置で見つかりました：" << (*c).second << endl;
            ++c;
         }
         class_index.erase(class_index.begin(), class_index.end());
      }
      return 0;
   }
