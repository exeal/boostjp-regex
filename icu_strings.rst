.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


Unicode と ICU 文字列型
=======================


.. _ref.non_std_strings.icu.intro:

ICU とともに Boost.Regex を使用する
-----------------------------------

ヘッダ ::

   <boost/regex/icu.hpp>

に、Unicode 環境で正規表現を使用するのに必要なデータ型とアルゴリズムが含まれている。

このヘッダを使用する場合は `ICU ライブラリ <http://www.ibm.com/software/globalization/icu/>`_\が必要である。また :ref:`ICU サポートを有効にして <install.building_with_unicode_and_icu_su>` Boost.Regex をビルドしていなければならない。

このヘッダにより以下のことが可能となる。

* Unicode 文字列を UTF-32 コードポイントシーケンスとして扱う正規表現の作成。
* 文字分類を含む Unicode データプロパティをサポートする正規表現の作成。
* UTF-8 、UTF-16 、UTF-32 のいずれかで符号化された Unicode 文字列の透過的な検索。


.. _ref.non_std_strings.icu.unicode_types:

Unicode 正規表現型
------------------

ヘッダ :file:`<boost/regex/icu.hpp>` は UTF-32 文字を処理する正規表現特性クラスを提供する。 ::

   class icu_regex_traits;

そしてこの特性クラスを用いた正規表現型がある。 ::

   typedef basic_regex<UChar32,icu_regex_traits> u32regex;

型 :cpp:type:`!u32regex` はあらゆる Unicode 正規表現を使用するための正規表現型である。内部的には UTF-32 コードポイントを使用しているが、UTF-32 符号化文字列だけでなく、UTF-8 および UTF-16 符号化文字列による作成・検索も可能である。

:cpp:type:`!u32regex` のコンストラクタおよび :cpp:func:`~u32regex::assign` メンバ関数は UTF-32 符号化文字列を要求するが、UTF-8 、UTF-16 および UTF-32 符号化文字列から正規表現を作成する :cpp:func:`!make_u32regex` アルゴリズムの多重定義群がある。


.. cpp:function:: template <class InputIterator> \
		  u32regex make_u32regex(InputIterator i, InputIterator j, regex_constants::syntax_option_type opt)

   :効果: イテレータシーケンス [i,j) から正規表現オブジェクトを作成する。シーケンスの文字符号化形式は :code:`sizeof(*i)` により決定し、1 であれば UTF-8 、2 であれば UTF-16 、4 であれば UTF-32 となる。


.. cpp:function:: u32regex make_u32regex(const char* p, regex_constants::syntax_option_type opt = regex_constants::perl)

   :効果: null 終端 UTF-8 文字シーケンス :cpp:var:`!p` から正規表現オブジェクトを作成する。


.. cpp:function:: u32regex make_u32regex(const unsigned char* p, regex_constants::syntax_option_type opt = regex_constants::perl)

   :効果: null 終端 UTF-8 文字シーケンス :cpp:var:`!p` から正規表現オブジェクトを作成する。


.. cpp:function:: u32regex make_u32regex(const wchar_t* p, regex_constants::syntax_option_type opt = regex_constants::perl)

   :効果: null 終端文字シーケンス :cpp:var:`!p` から正規表現オブジェクトを作成する。シーケンスの文字符号化形式 :cpp:expr:`sizeof(wchar_t)` により決定し、1 であれば UTF-8 、2 であれば UTF-16 、4 であれば UTF-32 となる。


.. cpp:function:: u32regex make_u32regex(const UChar* p, regex_constants::syntax_option_type opt = regex_constants::perl)

   :効果: null 終端 UTF-16 文字シーケンス :cpp:var:`!p` から正規表現オブジェクトを作成する。


.. cpp:function:: u32regex make_u32regex(const std::basic_string<C, T, A>& s, InputIterator j, regex_constants::syntax_option_type opt)

   :効果: 文字列 :cpp:var:`!s` から正規表現オブジェクトを作成する。シーケンスの文字符号化形式は :cpp:expr:`sizeof(C)` により決定し、1 であれば UTF-8 、2 であれば UTF-16 、4 であれば UTF-32 となる。


.. cpp:function:: u32regex make_u32regex(const UnicodeString& s, regex_constants::syntax_option_type opt = regex_constants::perl)

   :効果: UTF-16 符号化文字列 :cpp:var:`!s` から正規表現オブジェクトを作成する。


.. _ref.non_std_strings.icu.unicode_algo:

Unicode 正規表現アルゴリズム
----------------------------

正規表現アルゴリズム :cpp:func:`regex_match` 、:cpp:func:`regex_search` および :cpp:func:`regex_replace` はすべて、処理する文字シーケンスの文字エンコーディングが正規表現オブジェクトで使われているものと同じであると想定している。この動作は Unicode 正規表現では望ましいものではない。\ [#]_ 1 データを UTF-32 の「チャンク」で処理したくでも、実際のデータは UTF-8 か UTF-16 で符号化されている場合が多い。そのためヘッダ :file:`<boost/regex/icu.hpp>` はこれらのアルゴリズムの薄いラッパ群 :cpp:func:`!u32regex_match` 、:cpp:func:`!u32regex_search` および :cpp:func:`!u32regex_replace` を提供している。これらのラッパは内部でイテレータアダプタを使って、実際は「本体の」アルゴリズムに渡すことのできる UTF-32 シーケンスであるデータを見かけ上 UTF-8 、UTF-16 としている。


.. _ref.non_std_strings.icu.unicode_algo.u32regex_match:

u32regex_match
^^^^^^^^^^^^^^

各 :cpp:func:`regex_match` アルゴリズムが :file:`<boost/regex.hpp>` で定義されているのに対し、:file:`<boost/regex/icu.hpp>` は同じ引数をとる多重定義アルゴリズム :cpp:func:`!u32regex_match` を定義する。入力として ICU の :cpp:class:`!UnicodeString` とともに UTF-8 、UTF-16 、UTF-32 符号化データを受け取る。

.. code-block::
   :caption: 例：パスワードのマッチを UTF-16 :cpp:class:`!UnicodeString` で行う。

   //
   // password が正規表現 requirements で
   // 定義したパスワードの要件を満たしているか調べる。
   //
   bool is_valid_password(const UnicodeString& password, const UnicodeString& requirements)
   {
      return boost::u32regex_match(password, boost::make_u32regex(requirements));
   }

.. code-block::
   :caption: 例：UTF-8 で符号化されたファイル名のマッチを行う。

   //
   // UTF-8 で符号化された std::string のパスからファイル名部分を抜き出し、
   // 結果を別の std::string として返す：
   //
   std::string get_filename(const std::string& path)
   {
      boost::u32regex r = boost::make_u32regex("(?:\\A|.*\\\\)([^\\\\]+)");
      boost::smatch what;
      if(boost::u32regex_match(path, what, r))
      {
         // $1 を std::string として抽出する：
         return what.str(1);
      }
      else
      {
         throw std::runtime_error("パス名が不正");
      }
   }


.. _ref.non_std_strings.icu.unicode_algo.u32regex_search:

u32regex_search
^^^^^^^^^^^^^^^

各 :cpp:func:`regex_search` アルゴリズムが :file:`<boost/regex.hpp>` で定義されているのに対し、:file:`<boost/regex/icu.hpp>` は同じ引数をとる多重定義アルゴリズム :cpp:func:`!u32regex_search` を定義する。入力として ICU の :cpp:class:`!UnicodeString` とともに UTF-8 、UTF-16 、UTF-32 符号化データを受け取る。

.. code-block::
   :caption: 例：特定の言語区画から文字シーケンスを検索する。

   UnicodeString extract_greek(const UnicodeString& text)
   {
      // UTF-16 で符号化されたテキストからギリシャ語の区画を検索する。
      // この正規表現は完全ではないが、今のところは最善の方法である。特定の
      // 用字系を検索するのは、実際は非常に難しい。
      //
      // 検索するのはギリシャ文字で始まり
      // 非アルファベット（[^[:L*:]]）かギリシャ文字ブロック
      //（[\\x{370}-\\x{3FF}]）の文字が続く文字シーケンスである。
      //
      boost::u32regex r = boost::make_u32regex(
            L"[\\x{370}-\\x{3FF}](?:[^[:L*:]]|[\\x{370}-\\x{3FF}])*");
      boost::u16match what;
      if(boost::u32regex_search(text, what, r))
      {
         // $0 を UnicodeString として抽出する:
         return UnicodeString(what[0].first, what.length(0));
      }
      else
      {
         throw std::runtime_error("ギリシャ語の部分は見つかりませんでした！");
      }
   }


.. _ref.non_std_strings.icu.unicode_algo.u32regex_replace:

u32regex_replace
^^^^^^^^^^^^^^^^

各 :cpp:func:`regex_replace` アルゴリズムが :file:`<boost/regex.hpp>` で定義されているのに対し、:file:`<boost/regex/icu.hpp>` は同じ引数をとる多重定義アルゴリズム :cpp:func:`!u32regex_replace` を定義する。入力として ICU の :cpp:class:`UnicodeString` とともに UTF-8 、UTF-16 、UTF-32 符号化データを受け取る。アルゴリズムに渡す入力シーケンスと書式化文字列の符号化形式は異なっていてもよい（一方が UTF-8 で他方が UTF-16 など）が、結果の文字列や出力イテレータは検索対象のテキストと同じ文字符号化形式でなければならない。

.. code-block::
   :caption: 例：クレジットカード番号を書式化しなおす。

   //
   // クレジットカード番号を（数字を含んだ）文字列として受け取り、
   // 4 桁ずつ "-" で区切られた可読性の高い形式に
   // 再書式化する。
   // UTF-32 の正規表現、UTF-16 の文字列、
   // UTF-8 の書式指定子を混在させているが
   // すべて正しく動作することに注意していただきたい：
   //
   const boost::u32regex e = boost::make_u32regex(
         "\\A(\\d{3,4})[- ]?(\\d{4})[- ]?(\\d{4})[- ]?(\\d{4})\\z");
   const char* human_format = "$1-$2-$3-$4";

   UnicodeString human_readable_card_number(const UnicodeString& s)
   {
      return boost::u32regex_replace(s, e, human_format);
   }


.. _ref.non_std_strings.icu.unicode_iter:

Unicode 正規表現イテレータ
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _ref.non_std_strings.icu.unicode_iter.u32regex_iterator:

.. cpp:class:: template <class BidirectionalIterator> u32regex_iterator

   型 :cpp:type:`!u32regex_iterator` はあらゆる側面で :cpp:class:`regex_iterator` と同じであるが、正規表現型が常に :cpp:type:`u32regex` であることからテンプレート引数を 1 つ（イテレータ型）だけとる点が異なる。内部で :cpp:type:`!u32regex_search` を呼び出し、UTF-8 、UTF-16 および UTF-32 のデータを正しく処理する。 ::

      template <class BidirectionalIterator>
      class u32regex_iterator
      {
         // メンバについては regex_iterator を参照
      };

      typedef u32regex_iterator<const char*>     utf8regex_iterator;
      typedef u32regex_iterator<const UChar*>    utf16regex_iterator;
      typedef u32regex_iterator<const UChar32*>  utf32regex_iterator;

   文字列から :cpp:type:`!u32regex_iterator` を簡単に構築するために、非メンバのヘルパ関数群 :cpp:func:`!make_u32regex_iterator` がある。


.. cpp:function:: u32regex_iterator<const char*> make_u32regex_iterator(const char* s, const u32regex& e, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_iterator<const wchar_t*> make_u32regex_iterator(const wchar_t* s, const u32regex& e, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_iterator<const UChar*> make_u32regex_iterator(const UChar* s, const u32regex& e, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class Traits, class Alloc> \
		  u32regex_iterator<typename std::basic_string<charT, Traits, Alloc>::const_iterator> make_u32regex_iterator(const std::basic_string<charT, Traits, Alloc>& e, const u32regex& e, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_iterator<const UChar*> make_u32regex_iterator(const UnicodeString& s, const u32regex& e, regex_constants::match_flag_type m = regex_constants::match_default)

   これらの多重定義は、テキスト :cpp:var:`!s` に対してフラグ :cpp:var:`!m` を用いて見つかる正規表現 :cpp:var:`!e` のすべてのマッチを列挙するイテレータを返す。

   .. code-block::
      :caption: 例：国際通貨記号とその金額（数値）を検索する。

      void enumerate_currencies(const std::string& text)
      {
         // 通貨記号とその金額（数値）を
         // すべて列挙、印字する：
         const char* re =
            "([[:Sc:]][[:Cf:][:Cc:][:Z*:]]*)?"
            "([[:Nd:]]+(?:[[:Po:]][[:Nd:]]+)?)?"
            "(?(1)"
               "|(?(2)"
                  "[[:Cf:][:Cc:][:Z*:]]*"
               ")"
               "[[:Sc:]]"
            ")";
         boost::u32regex r = boost::make_u32regex(re);
         boost::u32regex_iterator<std::string::const_iterator>
               i(boost::make_u32regex_iterator(text, r)), j;
         while(i != j)
         {
            std::cout << (*i)[0] << std::endl;
            ++i;
         }
      }

   次のように呼び出すと、 ::

      enumerate_currencies(" $100.23 or £198.12 ");

   以下の結果を得る。

   .. code-block:: console

      $100.23
      £198.12

   当然ながら、入力は UTF-8 で符号化したものである。


.. _ref.non_std_strings.icu.unicode_iter.u32regex_token_iterator:

.. cpp:class:: template <class BidirectionalIterator> u32regex_token_iterator

   型 :cpp:type:`!u32regex_token_iterator` はあらゆる側面で :cpp:class:`regex_token_iterator` と同じであるが、正規表現型が常に :cpp:type:`!u32regex` であることからテンプレート引数を 1 つ（イテレータ型）だけとる点が異なる。内部で :cpp:func:`!u32regex_search` を呼び出し、UTF-8 、UTF-16 および UTF-32 のデータを正しく処理する。 ::

      template <class BidirectionalIterator>
      class u32regex_token_iterator
      {
         // メンバについては regex_token_iterator を参照
      };

      typedef u32regex_token_iterator<const char*>     utf8regex_token_iterator;
      typedef u32regex_token_iterator<const UChar*>    utf16regex_token_iterator;
      typedef u32regex_token_iterator<const UChar32*>  utf32regex_token_iterator;

文字列から :cpp:type:`!u32regex_token_iterator` を簡単に構築するために、非メンバのヘルパ関数群 :cpp:func:`!make_u32regex_token_iterator` がある。


.. cpp:function:: u32regex_token_iterator<const char*> make_u32regex_token_iterator(const char* s, const u32regex& e, int sub, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const wchar_t*> make_u32regex_token_iterator(const wchar_t* s, const u32regex& e, int sub, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UChar* s, const u32regex& e, int sub, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class Traits, class Alloc> \
		  u32regex_token_iterator<typename std::basic_string<charT, Traits, Alloc>::const_iterator> make_u32regex_token_iterator(const std::basic_string<charT, Traits, Alloc>& s, const u32regex& e, int sub, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UnicodeString& s, const u32regex& e, int sub, regex_constants::match_flag_type m = regex_constants::match_default)

   これらの多重定義は、テキスト :cpp:var:`s` に対してフラグ :cpp:var:`!m` を用いて見つかる正規表現 :cpp:var:`!e` の部分式 :cpp:var:`!sub` のすべてのマッチを列挙するイテレータを返す。


.. cpp:function:: template <std::size_t N> \
		  u32regex_token_iterator<const char*> make_u32regex_token_iterator(const char* p, const u32regex& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <std::size_t N> \
		  u32regex_token_iterator<const wchar_t*> make_u32regex_token_iterator(const wchar_t* p, const u32regex& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <std::size_t N> \
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UChar* p, const u32regex& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class Traits, class Alloc, std::size_t N> \
		  u32regex_token_iterator<typename std::basic_string<charT, Traits, Alloc>::const_iterator> make_u32regex_token_iterator(const std::basic_string<charT, Traits, Alloc>& p, const u32regex& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)
		  template <std::size_t N> \
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UnicodeString& s, const u32regex& e, const int (&submatch)[N], regex_constants::match_flag_type m = regex_constants::match_default)

   これらの多重定義は、テキスト :cpp:var:`!s` に対してフラグ :cpp:var:`!m` を用いて見つかる正規表現 :cpp:var:`!e` のすべての部分式マッチを列挙するイテレータを返す。


.. cpp:function:: u32regex_token_iterator<const char*> make_u32regex_token_iterator(const char* p, const u32regex& e, std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const wchar_t*> make_u32regex_token_iterator(const wchar_t* p, const u32regex& e, std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UChar* p, const u32regex& e, std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)
		  template <class charT, class Traits, class Alloc> \
		  u32regex_token_iterator<typename std::basic_string<charT, Traits, Alloc>::const_iterator> make_u32regex_token_iterator(const std::basic_string<charT, Traits, Alloc>& p, const u32regex& e, std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)
		  u32regex_token_iterator<const UChar*> make_u32regex_token_iterator(const UnicodeString& s, const u32regex& e, std::vector<int>& submatch, regex_constants::match_flag_type m = regex_constants::match_default)

   これらの多重定義は、テキスト :cpp:var:`!s` に対してフラグ :cpp:var:`!m` を用いて見つかる正規表現 :cpp:var:`!e` の 1 つの部分式マッチを列挙するイテレータを返す。

   .. code-block::
      :caption: 例：国際通貨記号とその金額（数値）を検索する。

      void enumerate_currencies2(const std::string& text)
      {
         // 通貨記号とその金額（数値）を
         // すべて列挙、印字する：
         const char* re =
            "([[:Sc:]][[:Cf:][:Cc:][:Z*:]]*)?"
            "([[:Nd:]]+(?:[[:Po:]][[:Nd:]]+)?)?"
            "(?(1)"
               "|(?(2)"
                  "[[:Cf:][:Cc:][:Z*:]]*"
               ")"
               "[[:Sc:]]"
            ")";
         boost::u32regex r = boost::make_u32regex(re);
         boost::u32regex_token_iterator<std::string::const_iterator>
            i(boost::make_u32regex_token_iterator(text, r, 1)), j;
         while(i != j)
         {
            std::cout << *i << std::endl;
            ++i;
         }
      }


.. [#] 訳注　Unicode に限った話ではありません。日本語では従来から複数の符号化方式を使用しています。
