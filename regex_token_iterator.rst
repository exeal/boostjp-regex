.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_token_iterator
====================

.. cpp:class:: template <class BidirectionalIterator, class charT = iterator_traits<BidirectionalIterator>::value_type, traits = regex_traits<charT> > \
	       regex_token_iterator

   ::

      #include <boost/regex.hpp>

   テンプレートクラス :cpp:class:`regex_token_iterator` はイテレータアダプタである。すなわち、入力テキストシーケンス内の正規表現マッチをすべて検索することで既存のシーケンス（入力テキスト）に対する新しいビューを表現し、各マッチ文字シーケンスを与える。このイテレータが列挙する各位置は、正規表現中の各部分式のマッチを表す :cpp:class:`sub_match` オブジェクトである。:cpp:class:`regex_token_iterator` クラスを使用して -1 の添字で部分式を列挙すると、イテレータはフィールド分割を行う。すなわち、指定した正規表現にマッチしない各文字コンテナシーケンスにつき 1 つの文字シーケンスを列挙する。\ [#]_

::

   template <class BidirectionalIterator,
            class charT = iterator_traits<BidirectionalIterator>::value_type,
            class traits = regex_traits<charT> >
   class regex_token_iterator
   {
   public:
      typedef          basic_regex<charT, traits>                              regex_type;
      typedef          sub_match<BidirectionalIterator>                        value_type;
      typedef typename iterator_traits<BidirectionalIterator>::difference_type difference_type;
      typedef          const value_type*                                       pointer;
      typedef          const value_type&                                       reference;
      typedef          std::forward_iterator_tag                               iterator_category;

      regex_token_iterator();
      regex_token_iterator(BidirectionalIterator a,
                           BidirectionalIterator b,
                           const regex_type& re,
                           int submatch = 0,
                           match_flag_type m = match_default);
      regex_token_iterator(BidirectionalIterator a,
                           BidirectionalIterator b,
                           const regex_type& re,
                           const std::vector<int>& submatches,
                           match_flag_type m = match_default);
      template <std::size_t N>
      regex_token_iterator(BidirectionalIterator a,
                           BidirectionalIterator b,
                           const regex_type& re,
                           const int (&submatches)[N],
                           match_flag_type m = match_default);
      regex_token_iterator(const regex_token_iterator&);
      regex_token_iterator& operator=(const regex_token_iterator&);
      bool operator==(const regex_token_iterator&)const;
      bool operator!=(const regex_token_iterator&)const;
      const value_type& operator*()const;
      const value_type* operator->()const;
      regex_token_iterator& operator++();
      regex_token_iterator operator++(int);
   };

   typedef regex_token_iterator<const char*>                   cregex_token_iterator;
   typedef regex_token_iterator<std::string::const_iterator>   sregex_token_iterator;
   #ifndef BOOST_NO_WREGEX
   typedef regex_token_iterator<const wchar_t*>                wcregex_token_iterator;
   typedef regex_token_iterator<<std::wstring::const_iterator> wsregex_token_iterator;
   #endif

   template <class charT, class traits>
   regex_token_iterator<const charT*, charT, traits>
      make_regex_token_iterator(
            const charT* p,
            const basic_regex<charT, traits>& e,
            int submatch = 0,
            regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits, class ST, class SA>
   regex_token_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits>
      make_regex_token_iterator(
            const std::basic_string<charT, ST, SA>& p,
            const basic_regex<charT, traits>& e,
            int submatch = 0,
            regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits, std::size_t N>
   regex_token_iterator<const charT*, charT, traits>
      make_regex_token_iterator(
            const charT* p,
            const basic_regex<charT, traits>& e,
            const int (&submatch)[N],
            regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits, class ST, class SA, std::size_t N>
   regex_token_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits>
      make_regex_token_iterator(
            const std::basic_string<charT, ST, SA>& p,
            const basic_regex<charT, traits>& e,
            const int (&submatch)[N],
            regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits>
   regex_token_iterator<const charT*, charT, traits>
      make_regex_token_iterator(
            const charT* p,
            const basic_regex<charT, traits>& e,
            const std::vector<int>& submatch,
            regex_constants::match_flag_type m = regex_constants::match_default);

   template <class charT, class traits, class ST, class SA>
   regex_token_iterator<
         typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits>
      make_regex_token_iterator(
            const std::basic_string<charT, ST, SA>& p,
            const basic_regex<charT, traits>& e,
            const std::vector<int>& submatch,
            regex_constants::match_flag_type m = regex_constants::match_default);


.. _ref.regex_token_iterator.description:

説明
----

.. cpp:namespace-push:: regex_token_iterator

.. cpp:function:: regex_token_iterator()

   :効果: シーケンスの終端を指すイテレータを構築する。

.. cpp:function:: regex_token_iterator(BidirectionalIterator a, BidirectionalIterator b, const regex_type& re, int submatch = 0, match_flag_type m = match_default)

   :事前条件: :cpp:expr:`!re.empty()`。オブジェクト :cpp:var:`!re` はイテレータの生涯にわたって存在しなければならない。
   :効果:
      シーケンス [a,b) 中で、式 :cpp:var:`!re` とマッチフラグ :cpp:var:`!m`\（:cpp:type:`match_flag_type` を見よ）で見つかる各正規表現マッチに対して文字列を 1 つずつ列挙する :cpp:class:`regex_token_iterator` を構築する。列挙される文字列は、見つかった各マッチに対する部分式 :samp:`{submatch}` である。:samp:`{submatch}` が -1 の場合は、式 :cpp:var:`!re` にマッチしなかったテキストシーケンスをすべて列挙する（フィールドの分割）。
   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。

.. cpp:function:: regex_token_iterator(BidirectionalIterator a, BidirectionalIterator b, const regex_type& re, const std::vector<int>& submatches, match_flag_type m = match_default)

   :事前条件:
      :cpp:expr:`submatches.size() && !re.empty()`。オブジェクト :cpp:var:`!re` はイテレータの生涯にわたって存在しなければならない。
   :効果:
      シーケンス [a,b) 中で、式 :cpp:var:`!re` とマッチフラグ :cpp:var:`!m`\（:cpp:type:`match_flag_type` を見よ）で見つかる各正規表現マッチに対して :cpp:expr:`submatches.size()` 個の文字列を列挙する :cpp:class:`regex_token_iterator` を構築する。各マッチに対して、ベクタ :cpp:var:`!submatches` 内の添字に対応する各部分式にマッチした文字列を 1 つずつ列挙する。:cpp:expr:`submatches[0]` が -1 の場合、各マッチに対して最初に列挙する文字列は、前回のマッチの終端から今回のマッチの先頭までのテキストとなり、さらにこれ以上マッチが見つからない場合に列挙する文字列（最後のマッチの終端から対象シーケンスの終端までのテキスト）が 1 つ追加される。
   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。


.. cpp:function:: template <std::size_t R> \
                  regex_token_iterator(BidirectionalIterator a, BidirectionalIterator b, const regex_type& re, const int (&submatches)[R], match_flag_type m = match_default)

   :事前条件:
      :cpp:expr:`!re.empty()`。オブジェクト :cpp:var:`!re` はイテレータの生涯にわたって存在しなければならない。
   :効果:
      シーケンス [a,b) 中で、式 :cpp:var:`!re` とマッチフラグ :cpp:var:`!m`\（:cpp:type:`match_flag_type` を見よ）で見つかる各正規表現マッチに対して R 個の文字列を列挙する :cpp:class:`regex_token_iterator` を構築する。各マッチに対して、配列 :cpp:var:`!submatches` 内の添字に対応する各部分式にマッチした文字列を 1 つずつ列挙する。:cpp:expr:`submatches[0]` が -1 の場合、各マッチに対して最初に列挙する文字列は、前回のマッチの終端から今回のマッチの先頭までのテキストとなり、さらにこれ以上マッチが見つからない場合に列挙する文字列（最後のマッチの終端から対象シーケンスの終端までのテキスト）が 1 つ追加される。
   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。


.. cpp:function:: regex_token_iterator(const regex_token_iterator& that)

   :効果: :cpp:var:`!that` のコピーを構築する。
   :事後条件: :cpp:expr:`*this == that`。


.. cpp:function:: regex_token_iterator& operator=(const regex_token_iterator& that)

   :効果: :cpp:expr:`*this` を :cpp:var:`!that` と等価にする。
   :事後条件: :cpp:expr:`*this == that`。


.. cpp:function:: bool operator==(const regex_token_iterator& that) const

   :効果: :cpp:expr:`*this` と :cpp:var:`!that` が同じ位置であれば真を返す。


.. cpp:function:: bool operator!=(const regex_token_iterator& that) const

   :効果: :cpp:expr:`!(*this == that)` を返す。


.. cpp:function:: const value_type& operator*() const

   :効果: 列挙中の現在の文字シーケンスを返す。


.. cpp:function:: const value_type* operator->() const

   :効果: :cpp:expr:`&(*this)` を返す。


.. cpp:function:: regex_token_iterator& operator++()

   :効果: 列挙中の次の文字シーケンスへ移動する。
   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、式のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。
   :returns: :cpp:expr:`*this`。


.. cpp:function:: regex_token_iterator& operator++(int)

   :効果: 戻り値用に :cpp:expr:`*this` のコピーを構築した後、:cpp:expr:`++(*this)` を呼び出す。
   :returns:	結果。


.. cpp:namespace-pop::


.. cpp:function:: template <class charT, class traits> \
                  regex_token_iterator<const charT*, charT, traits> make_regex_token_iterator(const charT* p, const basic_regex<charT, traits>& e, int submatch = 0, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class traits, class ST, class SA> \
		  regex_token_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits> make_regex_token_iterator(const std::basic_string<charT, ST, SA>& p, const basic_regex<charT, traits>& e, int submatch = 0, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class traits, std::size_t N> \
		  regex_token_iterator<const charT*, charT, traits> make_regex_token_iterator(const charT* p, const basic_regex<charT, traits>& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class traits, class ST, class SA, std::size_t N> \
		  regex_token_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits> make_regex_token_iterator(const std::basic_string<charT, ST, SA>& p, const basic_regex<charT, traits>& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class traits> \
		  regex_token_iterator<const charT*, charT, traits> make_regex_token_iterator(const charT* p,const basic_regex<charT, traits>& e, const std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class traits, class ST, class SA> \
		  regex_token_iterator<typename std::basic_string<charT, ST, SA>::const_iterator, charT, traits> make_regex_token_iterator(const std::basic_string<charT, ST, SA>& p, const basic_regex<charT, traits>& e, const std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)

   :効果:
      文字列 :cpp:var:`!p` 中から正規表現 :cpp:var:`!e` と :cpp:type:`match_flag_type` :cpp:var:`!m` を用いて見つかる各マッチに対して、:cpp:var:`!submatch` 内の値に対応する 1 つの :cpp:class:`sub_match` を列挙する :cpp:class:`regex_token_iterator` を返す。


.. _ref.regex_token_iterator.examples:

使用例
------

次の例は文字列を受け取り、トークン列に分解する。 ::

   #include <iostream>
   #include <boost/regex.hpp>

   using namespace std;

   int main(int argc)
   {
      string s;
      do{
         if(argc == 1)
            {
               cout << "分解するテキストを入力してください（\"quit\" で終了）：";
               getline(cin, s);
               if(s == "quit") break;
            }
            else
               s = "This is a string of tokens";

            boost::regex re("\\s+");
            boost::sregex_token_iterator i(s.begin(), s.end(), re, -1);
            boost::sregex_token_iterator j;

            unsigned count = 0;
            while(i != j)
            {
               cout << *i++ << endl;
               count++;
            }
            cout << "テキスト内に " << count << " 個のトークンが見つかりました。" << endl;

         }while(argc == 1);
         return 0;
      }

次の例は HTML ファイルを受け取り、リンクしているファイルのリストを出力する。 ::

   #include <fstream>
   #include <iostream>
   #include <iterator>
   #include <boost/regex.hpp>

   boost::regex e("<\\s*A\\s+[^>]*href\\s*=\\s*\"([^\"]*)\"",
                  boost::regex::normal | boost::regbase::icase);

   void load_file(std::string& s, std::istream& is)
   {
      s.erase();
      //
      // ファイルサイズに合わせて文字列バッファを拡張する。
      // 場合によっては正しく動作しない…
      s.reserve(is.rdbuf()->in_avail());
      char c;
      while(is.get(c))
      {
         // （上の）in_avail が 0 を返した場合は
         // 対数拡大法を使う：
         if(s.capacity() == s.size())
            s.reserve(s.capacity() * 3);
         s.append(1, c);
      }
   }

   int main(int argc, char** argv)
   {
      std::string s;
      int i;
      for(i = 1; i < argc; ++i)
      {
         std::cout << "次のファイルで URL を検索中 " << argv[i] << "：" << std::endl;
         s.erase();
         std::ifstream is(argv[i]);
         load_file(s, is);
         boost::sregex_token_iterator i(s.begin(), s.end(), e, 1);
         boost::sregex_token_iterator j;
         while(i != j)
         {
            std::cout << *i++ << std::endl;
         }
      }
      //
      // 別の方法：
      // 配列直値版コンストラクタのテスト。マッチ全体を
      // $1... 同様に分割する
      //
      for(i = 1; i < argc; ++i)
      {
         std::cout << "次のファイルで URL を検索中 " << argv[i] << "：" << std::endl;
         s.erase();
         std::ifstream is(argv[i]);
         load_file(s, is);
         const int subs[] = {1, 0,};
         boost::sregex_token_iterator i(s.begin(), s.end(), e, subs);
         boost::sregex_token_iterator j;
         while(i != j)
         {
            std::cout << *i++ << std::endl;
         }
      }

      return 0;
   }


.. [#] 訳注　-1 の添字は、後述するように実際には :cpp:class:`!sub_match` と同様に「前回のマッチの終端から今回のマッチの先頭まで」を表します。:cpp:class:`sub_match` の項でドキュメントされていない -2 の添字についても同様ですが、奇妙な動作をするので使用しないほうが無難です。
