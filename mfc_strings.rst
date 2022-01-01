.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


MFC 文字列とともに Boost.Regex を使用する
=========================================


.. _ref.mfc_strings.intro:

はじめに
--------

ヘッダ :file:`<boost/regex/mfc.hpp>` に、MFC 文字列型に対する Boost.Regex のサポートがある。この機能には、MFC および ATL のすべての文字列型が :cpp:class:`!CSimpleStringT` テンプレートクラスに基づいている Visual Studio .NET（Visual C++ 7）以降が必要であることに注意していただきたい。

以下の説明では、:cpp:class:`!CSimpleStringT<charT>` の箇所は次の MFC/ATL の型に置き換えて読んでかまわない（すべて :cpp:class:`!CSimpleStringT<charT>` を継承している）。

* :cpp:class:`!CString`
* :cpp:class:`!CStringA`
* :cpp:class:`!CStringW`
* :cpp:class:`!CAtlString`
* :cpp:class:`!CAtlStringA`
* :cpp:class:`!CAtlStringW`
* :cpp:class:`!CStringT<charT,traits>`
* :cpp:class:`!CFixedStringT<charT,N>`
* :cpp:class:`!CSimpleStringT<charT>`


.. _ref.mfc_strings.unicode_types:

MFC 文字列で使用する Boost.Regex の型
-------------------------------------

:cpp:type:`!TCHAR` を使用するのに便利なように、以下の typedef を提供している。 ::

   typedef basic_regex<TCHAR>                  tregex;
   typedef match_results<TCHAR const*>         tmatch;
   typedef regex_iterator<TCHAR const*>        tregex_iterator;
   typedef regex_token_iterator<TCHAR const*>  tregex_token_iterator;

:cpp:type:`!TCHAR` ではなく、ナロー文字かワイド文字を明示的に使用する場合は、通常の Boost.Regex 型である :cpp:type:`!regex` か :cpp:type:`!wregex` を使用するとよい。


.. _ref.non_std_strings.mfc_strings.mfc_regex_create:

MFC 文字列からの正規表現の作成
------------------------------

以下のヘルパ関数は MFC/ATL 文字列型からの正規表現作成を補助する。


.. cpp:function:: template <class charT> \
		  basic_regex<charT> make_regex(const ATL::CSimpleStringT<charT>& s, ::regex_constants::syntax_option_type f = regex_constants::normal)

   :効果: :cpp:expr:`basic_regex<charT>(s.GetString(), s.GetString() + s.GetLength(), f)` を返す。


.. _ref.non_std_strings.mfc_strings.mfc_algo:

MFC 文字列型に対するアルゴリズムの多重定義
------------------------------------------

:cpp:class:`!std::basic_string` 引数についてのアルゴリズムの各多重定義に対して、MFC/ATL 文字列型についての多重定義がある。これらのアルゴリズムのすべてのシグニチャは実際よりもかなり複雑に見えるが、完全性のためにここではすべて記述する。


.. _ref.non_std_strings.mfc_strings.mfc_algo.regex_match:

regex_match
^^^^^^^^^^^

2 つの多重定義がある。1 番目のものは何がマッチしたかを :cpp:class:`!match_results` 構造体で返し、2 番目は何も返さない。

:cpp:func:`regex_match` についての注意がすべてこの関数にも適用されるが、特にこのアルゴリズムは入力テキスト全体の式に対するマッチが成功したかだけを返す。この動作が希望のものでない場合は :cpp:func:`regex_search` を代わりに使用するとよい。


.. cpp:function:: template <class charT, class T, class A> \
		  bool regex_match(const ATL::CSimpleStringT<charT>& s, match_results<const B*, A>& what, const basic_string<charT, T>& e, regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`::boost::regex_match(s.GetString(), s.GetString() + s.GetLength(), what, e, f)` を返す。
   :使用例:
      ::

         //
         // CString のパスからファイル名部分を抜き出し、
         // 結果を CString で返す：
         //
         CString get_filename(const CString& path)
         {
            boost::tregex r(__T("(?:\\A|.*\\\\)([^\\\\]+)"));
            boost::tmatch what;
            if(boost::regex_match(path, what, r))
            {
               // $1 を CString として抽出する：
               return CString(what[1].first, what.length(1));
            }
            else
            {
               throw std::runtime_error("パス名が不正");
            }
         }


.. _ref.non_std_strings.mfc_strings.mfc_algo.regex_match__second_overload_:

regex_match （第二の多重定義）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. cpp:function:: template <class charT, class T> \
		  bool regex_match(const ATL::CSimpleStringT<charT>& s, const basic_string<B, T>& e, regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`::boost::regex_match(s.GetString(), s.GetString() + s.GetLength(), e, f)` を返す。
   :使用例:
      ::

         //
         // password が正規表現 requirements で
         // 定義したパスワードの要件を満たしているか調べる。
         //
         bool is_valid_password(const CString& password, const CString& requirements)
         {
            return boost::regex_match(password, boost::make_regex(requirements));
         }


.. _ref.non_std_strings.mfc_strings.mfc_algo.regex_search:

regex_search
^^^^^^^^^^^^

:cpp:func:`regex_search` については多重定義を 2 つ追加する。1 番目のものは何がマッチしたかを返し、2 番目は何も返さない。


.. cpp:function:: template <class charT, class A, class T> \
		  bool regex_search(const ATL::CSimpleStringT<charT>& s, match_results<const charT*, A>& what, const basic_string<charT, T>& e, regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`::boost::regex_search(s.GetString(), s.GetString() + s.GetLength(), what, e, f)` を返す。
   :使用例: 住所の文字列から郵便番号を抜き出す。
      ::

         CString extract_postcode(const CString& address)
         {
            // 投函住所から英国郵便番号を検索し、結果を返す。
            // 正規表現は www.regxlib.com の Phil A. のものを用いた：
            boost::tregex r(__T("^(([A-Z]{1,2}[0-9]{1,2})|([A-Z]{1,2}[0-9][A-Z]))\\s?([0-9][A-Z]{2})$"));
            boost::tmatch what;
            if(boost::regex_search(address, what, r))
            {
               // $0 を CString として抽出する：
               return CString(what[0].first, what.length());
            }
            else
            {
               throw std::runtime_error("郵便番号は見つかりません");
            }
         }


.. _ref.non_std_strings.mfc_strings.mfc_algo.regex_search__second_overload_:

regex_search （第二の多重定義）
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. cpp:function:: template <class charT, class T> \
		  inline bool regex_search(const ATL::CSimpleStringT<charT>& s, const basic_string<charT, T>& e, regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`::boost::regex_search(s.GetString(), s.GetString() + s.GetLength(), e, f)` を返す。


.. _ref.non_std_strings.mfc_strings.mfc_algo.regex_replace:

regex_replace
^^^^^^^^^^^^^

:cpp:func:`regex_replace` については多重定義を 2 つ追加する。1 番目のものは出力イテレータに出力を送り、2 番目は何も出力しない。


.. cpp:function:: template <class OutputIterator, class BidirectionalIterator, class traits, class charT> \
		  OutputIterator regex_replace(OutputIterator out, BidirectionalIterator first, BidirectionalIterator last, const basic_regex<charT, traits>& e, const ATL::CSimpleStringT<charT>& fmt, match_flag_type flags = match_default)

   :効果: :cpp:expr:`::boost::regex_replace(out, first, last, e, fmt.GetString(), flags)` を返す。


.. cpp:function:: template <class traits, class charT> \
		  ATL::CSimpleStringT<charT> regex_replace(const ATL::CSimpleStringT<charT>& s, const basic_regex<charT, traits>& e, const ATL::CSimpleStringT<charT>& fmt, match_flag_type flags = match_default)

   :効果: :cpp:func:`regex_replace` 、および文字列 :cpp:var:`!s` と同じメモリマネージャを使って新文字列を作成し、返す。
   :使用例:
      ::

         //
         // クレジットカード番号を（数字を含んだ）文字列で受け取り、
         // 4 桁ずつ "-" で区切った可読性の高い形式に
         // 再書式化する：
         //
         const boost::tregex e(__T("\\A(\\d{3,4})[- ]?(\\d{4})[- ]?(\\d{4})[- ]?(\\d{4})\\z"));
         const CString human_format = __T("$1-$2-$3-$4");

         CString human_readable_card_number(const CString& s)
         {
            return boost::regex_replace(s, e, human_format);
         }


.. _ref.non_std_strings.mfc_strings.mfc_iter:

MFC 文字列に対するマッチの反復
------------------------------

MFC/ATL 文字列を :cpp:class:`regex_iterator` および :cpp:class:`regex_token_iterator` に簡単に変換できるように、以下のヘルパ関数を提供する。


.. _ref.non_std_strings.mfc_strings.mfc_iter.regex_iterator_creation_helper:

regex_iterator 作成ヘルパ
^^^^^^^^^^^^^^^^^^^^^^^^^

.. cpp:function:: template <class charT> \
		  regex_iterator<charT const*> make_regex_iterator(const ATL::CSimpleStringT<charT>& s, const basic_regex<charT>& e, ::regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`regex_iterator(s.GetString(), s.GetString() + s.GetLength(), e, f)` を返す。
   :使用例:
      ::

         void enumerate_links(const CString& html)
         {
            // HTML テキスト中のリンクをすべて列挙、印字する。
            // 正規表現は www.regxlib.com の Andrew Lee のものを用いた：
            boost::tregex r(
               __T("href=[\"\']((http:\\/\\/|\\.\\/|\\/)?\\w+"
                   "(\\.\\w+)*(\\/\\w+(\\.\\w+)?)*"
                   "(\\/|\\?\\w*=\\w*(&\\w*=\\w*)*)?)[\"\']"));
            boost::tregex_iterator i(boost::make_regex_iterator(html, r)), j;
            while(i != j)
            {
               std::cout << (*i)[1] << std::endl;
               ++i;
            }
         }


.. _ref.non_std_strings.mfc_strings.mfc_iter.regex_token_iterator_creation_helpers:

regex_token_iterator 作成ヘルパ
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. cpp:function:: template <class charT> \
		  regex_token_iterator<charT const*> make_regex_token_iterator(const ATL::CSimpleStringT<charT>& s, const basic_regex<charT>& e, int sub = 0, ::regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`regex_token_iterator(s.GetString(), s.GetString() + s.GetLength(), e, sub, f)` を返す。


.. cpp:function:: template <class charT> \
		  regex_token_iterator<charT const*> make_regex_token_iterator(const ATL::CSimpleStringT<charT>& s, const basic_regex<charT>& e, const std::vector<int>& subs, ::regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`regex_token_iterator(s.GetString(), s.GetString() + s.GetLength(), e, subs, f)` を返す。

.. cpp:function:: template <class charT, std::size_t N> \
		  regex_token_iterator<charT const*> make_regex_token_iterator(const ATL::CSimpleStringT<charT>& s, const basic_regex<charT>& e, const int (& subs)[N], ::regex_constants::match_flag_type f = regex_constants::match_default)

   :効果: :cpp:expr:`regex_token_iterator(s.GetString(), s.GetString() + s.GetLength(), e, subs, f)` を返す。
   :使用例:
      ::

         void enumerate_links2(const CString& html)
         {
            // HTML テキスト中のリンクをすべて列挙、印字する。
            // 正規表現は www.regxlib.com の Andrew Lee のものを用いた：
            boost::tregex r(
                  __T("href=[\"\']((http:\\/\\/|\\.\\/|\\/)?\\w+"
                      "(\\.\\w+)*(\\/\\w+(\\.\\w+)?)*"
                      "(\\/|\\?\\w*=\\w*(&\\w*=\\w*)*)?)[\"\']"));
                      boost::tregex_token_iterator i(boost::make_regex_token_iterator(html, r, 1)), j;
            while(i != j)
            {
               std::cout << *i << std::endl;
               ++i;
            }
         }
