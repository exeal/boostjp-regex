.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


basic_regex
===========

.. cpp:class:: template <class charT, class traits = regex_traits<charT> > \
	       basic_regex

   テンプレートクラス :cpp:class:`!basic_regex` は、正規表現の解析とコンパイルをカプセル化する。このクラスは 2 つのテンプレート引数をとる。

   :tparam charT: 文字型を決定する。すなわち :cpp:type:`!char` か :cpp:type:`!wchar_t` のいずれかである。:ref:`charT のコンセプト <ref.concepts.charT_concept>`\を見よ。
   :tparam traits: 例えばどの文字クラス名を考慮するか、といった文字型の振る舞いを決定する。既定の特性クラスとして :cpp:class:`!regex_traits<charT>` が用意されている。:ref:`traits のコンセプト <ref.concepts.traits_concept>`\を見よ。

   簡単に使用できるように、標準的な :cpp:class:`!basic_regex` インスタンスを定義する typedef が 2 つある。カスタムの特性クラスか非標準の文字型（例えば :doc:`Unicode サポート <icu_strings>`\を見よ）を使用するつもりがなければ、この 2 つだけを使用すればよい。


.. cpp:namespace-push:: basic_regex

.. _ref.basic_regex.synopsis:

概要
----

::

   #include <boost/regex.hpp>

   namespace boost{

   template <class charT, class traits = regex_traits<charT>  >
   class basic_regex;

   typedef basic_regex<char>      regex;
   typedef basic_regex<wchar_t>   wregex;

   }

以下が :cpp:class:`!basic_regex` の定義である。:cpp:class:`!basic_string` クラスに基づいており、:cpp:type:`!charT` の定数コンテナの要求事項を満足する。

.. parsed-literal::

   template <class charT, class traits = regex_traits<charT> >
   class basic_regex {
      public:
      // 型：
      typedef          charT                               value_type;
      typedef          implementation-specific             const_iterator;
      typedef          const_iterator                      iterator;
      typedef          charT&                              reference;
      typedef          const charT&                        const_reference;
      typedef          std::ptrdiff_t                      difference_type;
      typedef          std::size_t                         size_type;
      typedef          regex_constants:::cpp:type:`syntax_option_type` flag_type;
      typedef typename traits::locale_type                 locale_type;

      // 定数：
      // メインオプションの選択：
      static const regex_constants:: :cpp:type:`syntax_option_type` normal
                                                   = regex_constants::normal;
      static const regex_constants:: :cpp:type:`syntax_option_type` ECMAScript
                                                   = normal;
      static const regex_constants:: :cpp:type:`syntax_option_type` JavaScript
                                                   = normal;
      static const regex_constants:: :cpp:type:`syntax_option_type` Jscript
                                                   = normal;
      static const regex_constants:: :cpp:type:`syntax_option_type` basic
                                                   = regex_constants::basic;
      static const regex_constants:: :cpp:type:`syntax_option_type` extended
                                                   = regex_constants::extended;
      static const regex_constants:: :cpp:type:`syntax_option_type` awk
                                                   = regex_constants::awk;
      static const regex_constants:: :cpp:type:`syntax_option_type` grep
                                                   = regex_constants::grep;
      static const regex_constants:: :cpp:type:`syntax_option_type` egrep
                                                   = regex_constants::egrep;
      static const regex_constants:: :cpp:type:`syntax_option_type` sed
                                                   = basic = regex_constants::sed;
      static const regex_constants:: :cpp:type:`syntax_option_type` perl
                                                   = regex_constants::perl;
      static const regex_constants:: :cpp:type:`syntax_option_type` literal
                                                   = regex_constants::literal;

      // Perl 正規表現固有の修飾子：
      static const regex_constants:: :cpp:type:`syntax_option_type` no_mod_m
                                                   = regex_constants::no_mod_m;
      static const regex_constants:: :cpp:type:`syntax_option_type` no_mod_s
                                                   = regex_constants::no_mod_s;
      static const regex_constants:: :cpp:type:`syntax_option_type` mod_s
                                                   = regex_constants::mod_s;
      static const regex_constants:: :cpp:type:`syntax_option_type` mod_x
                                                   = regex_constants::mod_x;

      // POSIX 基本正規表現固有の修飾子：
      static const regex_constants:: :cpp:type:`syntax_option_type` bk_plus_qm
                                                   = regex_constants::bk_plus_qm;
      static const regex_constants:: :cpp:type:`syntax_option_type` bk_vbar
                                                   = regex_constants::bk_vbar;
      static const regex_constants:: :cpp:type:`syntax_option_type` no_char_classes
                                                   = regex_constants::no_char_classes;
      static const regex_constants:: :cpp:type:`syntax_option_type` no_intervals
                                                   = regex_constants::no_intervals;

      // 共通の修飾子：
      static const regex_constants:: :cpp:type:`syntax_option_type` nosubs
                                                   = regex_constants::nosubs;
      static const regex_constants:: :cpp:type:`syntax_option_type` optimize
                                                   = regex_constants::optimize;
      static const regex_constants:: :cpp:type:`syntax_option_type` collate
                                                   = regex_constants::collate;
      static const regex_constants:: :cpp:type:`syntax_option_type` newline_alt
                                                   = regex_constants::newline_alt;
      static const regex_constants:: :cpp:type:`syntax_option_type` no_except
                                                   = regex_constants::newline_alt;

      // 構築、コピー、解体：
      explicit :cpp:func:`~basic_regex::basic_regex` ();
      explicit :cpp:func:`~basic_regex::basic_regex`\(const charT* p, flag_type f = regex_constants::normal);
      :cpp:func:`~basic_regex::basic_regex`\(const charT* p1, const charT* p2,
                  flag_type f = regex_constants::normal);
      :cpp:func:`~basic_regex::basic_regex`\(const charT* p, size_type len, flag_type f);
      :cpp:func:`~basic_regex::basic_regex`\(const basic_regex&);

      template <class ST, class SA>
      explicit :cpp:func:`~basic_regex::basic_regex`\(const basic_string<charT, ST, SA>& p,
                           flag_type f = regex_constants::normal);

      template <class InputIterator>
      :cpp:func:`~basic_regex::basic_regex`\(InputIterator first, InputIterator last,
                  flag_type f = regex_constants::normal);

      ~basic_regex();
      basic_regex& :cpp:func:`operator=`\(const basic_regex&);
      basic_regex& :cpp:func:`operator=` (const charT* ptr);

      template <class ST, class SA>
      basic_regex& :cpp:func:`operator=` (const basic_string<charT, ST, SA>& p);
      // イテレータ：
      std::pair<const_iterator, const_iterator> :cpp:func:`subexpression`\(size_type n) const;
      const_iterator :cpp:func:`begin`\() const;
      const_iterator :cpp:func:`end`\() const;
      // 容量：
      size_type :cpp:func:`size`\() const;
      size_type :cpp:func:`max_size`\() const;
      bool :cpp:func:`empty`\() const;
      size_type :cpp:func:`mark_count`\()const;
      //
      // 変更：
      basic_regex& :cpp:func:`assign`\(const basic_regex& that);
      basic_regex& :cpp:func:`assign`\(const charT* ptr,
                          flag_type f = regex_constants::normal);
      basic_regex& :cpp:func:`assign`\(const charT* ptr, unsigned int len, flag_type f);

      template <class string_traits, class A>
      basic_regex& :cpp:func:`assign`\(const basic_string<charT, string_traits, A>& s,
                          flag_type f = regex_constants::normal);

      template <class InputIterator>
      basic_regex& :cpp:func:`assign`\(InputIterator first, InputIterator last,
                          flag_type f = regex_constants::normal);

      // const な操作：
      flag_type :cpp:func:`flags`\() const;
      int :cpp:func:`status`\()const;
      basic_string<charT> :cpp:func:`str`\() const;
      int :cpp:func:`compare`\(basic_regex&) const;
      // ロカール：
      locale_type :cpp:func:`imbue`\(locale_type loc);
      locale_type :cpp:func:`getloc`\() const;
      // 値の交換
      void :cpp:func:`swap`\(basic_regex&) throw();
   };

   template <class charT, class traits>
   bool :cpp:func:`operator ==` (const basic_regex<charT, traits>& lhs,
                     const basic_regex<charT, traits>& rhs);

   template <class charT, class traits>
   bool :cpp:func:`operator !=` (const basic_regex<charT, traits>& lhs,
                     const basic_regex<charT, traits>& rhs);

   template <class charT, class traits>
   bool :cpp:func:`operator <` (const basic_regex<charT, traits>& lhs,
                     const basic_regex<charT, traits>& rhs);

   template <class charT, class traits>
   bool :cpp:func:`operator <=` (const basic_regex<charT, traits>& lhs,
                     const basic_regex<charT, traits>& rhs);

   template <class charT, class traits>
   bool :cpp:func:`operator >=` (const basic_regex<charT, traits>& lhs,
                     const basic_regex<charT, traits>& rhs);

   template <class charT, class traits>
   bool :cpp:func:`operator >` (const basic_regex<charT, traits>& lhs,
                    const basic_regex<charT, traits>& rhs);

   template <class charT, class io_traits, class re_traits>
   basic_ostream<charT, io_traits>&
   :cpp:func:`operator <<` (basic_ostream<charT, io_traits>& os,
                const basic_regex<charT, re_traits>& e);

   template <class charT, class traits>
   void :cpp:func:`swap`\(basic_regex<charT, traits>& e1,
             basic_regex<charT, traits>& e2);

   typedef basic_regex<char> regex;
   typedef basic_regex<wchar_t> wregex;

   } // namespace boost


.. _ref.basic_regex.description:

説明
----

:cpp:class:`!basic_regex` クラスは以下の公開メンバをもつ。

.. parsed-literal::

   // メインオプションの選択：
   static const regex_constants:: :cpp:type:`syntax_option_type` normal
                                              = regex_constants::normal;
   static const regex_constants:: :cpp:type:`syntax_option_type` ECMAScript
                                              = normal;
   static const regex_constants:: :cpp:type:`syntax_option_type` JavaScript
                                              = normal;
   static const regex_constants:: :cpp:type:`syntax_option_type` Jscript
                                              = normal;
   static const regex_constants:: :cpp:type:`syntax_option_type` basic
                                              = regex_constants::basic;
   static const regex_constants:: :cpp:type:`syntax_option_type` extended
                                              = regex_constants::extended;
   static const regex_constants:: :cpp:type:`syntax_option_type` awk
                                              = regex_constants::awk;
   static const regex_constants:: :cpp:type:`syntax_option_type` grep
                                              = regex_constants::grep;
   static const regex_constants:: :cpp:type:`syntax_option_type` egrep
                                              = regex_constants::egrep;
   static const regex_constants:: :cpp:type:`syntax_option_type` sed
                                              = basic = regex_constants::sed;
   static const regex_constants:: :cpp:type:`syntax_option_type` perl
                                              = regex_constants::perl;
   static const regex_constants:: :cpp:type:`syntax_option_type` literal
                                              = regex_constants::literal;

   // Perl 正規表現固有の修飾子：
   static const regex_constants:: :cpp:type:`syntax_option_type` no_mod_m
                                              = regex_constants::no_mod_m;
   static const regex_constants:: :cpp:type:`syntax_option_type` no_mod_s
                                              = regex_constants::no_mod_s;
   static const regex_constants:: :cpp:type:`syntax_option_type` mod_s
                                              = regex_constants::mod_s;
   static const regex_constants:: :cpp:type:`syntax_option_type` mod_x
                                              = regex_constants::mod_x;

   // POSIX 基本正規表現固有の修飾子：
   static const regex_constants:: :cpp:type:`syntax_option_type` bk_plus_qm
                                              = regex_constants::bk_plus_qm;
   static const regex_constants:: :cpp:type:`syntax_option_type` bk_vbar
                                              = regex_constants::bk_vbar;
   static const regex_constants:: :cpp:type:`syntax_option_type` no_char_classes
                                              = regex_constants::no_char_classes;
   static const regex_constants:: :cpp:type:`syntax_option_type` no_intervals
                                              = regex_constants::no_intervals;

   // 共通の修飾子：
   static const regex_constants:: :cpp:type:`syntax_option_type` nosubs
                                              = regex_constants::nosubs;
   static const regex_constants:: :cpp:type:`syntax_option_type` optimize
                                              = regex_constants::optimize;
   static const regex_constants:: :cpp:type:`syntax_option_type` collate
                                              = regex_constants::collate;
   static const regex_constants:: :cpp:type:`syntax_option_type` newline_alt
                                              = regex_constants::newline_alt;
   static const regex_constants:: :cpp:type:`syntax_option_type` no_except
                                              = regex_constants::newline_alt;

これらのオプションの意味は :cpp:type:`syntax_option_type` の節にある。

静的定数メンバは名前空間 :cpp:member:`!boost::regex_constants` 内で宣言した定数の別名として提供している。名前空間 :cpp:member:`!boost::regex_constants` 内で宣言されている :cpp:type:`syntax_option_type` 型の各定数については、:cpp:class:`!basic_regex` のスコープで同じ名前・型・値で宣言している。


.. cpp:function:: basic_regex()

   :効果: :cpp:class:`!basic_regex` クラスのオブジェクトを構築する。

   .. list-table:: :cpp:class:`!basic_regex` デフォルトコンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``true``
      * - :cpp:expr:`size()`
        - ``0``
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>()`


.. cpp:function:: basic_regex(const chartT* p, flag_type f = regex_constants::normal)

   :要件: :cpp:var:`!p` は null ポインタ以外。
   :throws bad_expression: :cpp:var:`!s` が正しい正規表現でない場合（:cpp:var:`!f` にフラグ :cpp:var:`!no_except` が設定されていない場合）。
   :効果: :cpp:class:`basic_regex` クラスのオブジェクトを構築する。:cpp:var:`!f` で指定した\ :doc:`オプションフラグ <syntax_option_type>`\にしたがって null 終端文字列 :cpp:var:`!p` の正規表現を解釈し、オブジェクトの内部有限状態マシンを構築する。

   .. list-table:: :cpp:class:`!basic_regex` デフォルトコンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:expr:`char_traits<charT>::length(p)`
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>(p)`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count()`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: basic_regex(const charT* p1, const charT* p2, flag_type f = regex_constants::normal)

   :要件: :cpp:var:`!p1` と :cpp:var:`!p2` は null ポインタ以外、かつ :cpp:expr:`p1 < p2`。
   :throws bad_expression: [p1,p2) が正しい正規表現でない場合（:cpp:var:`!f` に :cpp:var:`!no_except` が設定されていない場合）。
   :効果: クラス :cpp:class:`basic_regex` のオブジェクトを構築する。:cpp:var:`!f` で指定した\ :doc:`オプションフラグ <syntax_option_type>`\にしたがって文字シーケンス [p1,p2) の正規表現を解釈し、オブジェクトの内部有限状態マシンを構築する。

   .. list-table:: :cpp:class:`!basic_regex` デフォルトコンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:expr:`std::distance(p1,p2)`
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>(p1,p2)`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count()`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: basic_regex(const charT* p, size_type len, flag_type f)

   :要件: :cpp:var:`!p` は null ポインタ以外、かつ :cpp:expr:`len < max_size()`。
   :throws bad_expression: :cpp:var:`!p` が正しい正規表現でない場合（:cpp:var:`!f` に :cpp:var:`!no_except` が設定されていない場合）。
   :効果: クラス :cpp:class:`basic_regex` のオブジェクトを構築する。:cpp:var:`!f` で指定したオプションフラグにしたがって文字シーケンス [p,p+len) の正規表現を解釈し、オブジェクトの内部有限状態マシンを構築する。

   .. list-table:: :cpp:class:`!basic_regex` デフォルトコンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:var:`!len`
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>(p, len)`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count()`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: basic_regex(const basic_regex& e)

   :効果: オブジェクト :cpp:var:`!e` をコピーしてクラス :cpp:class:`basic_regex` オブジェクトを構築する。


.. cpp:function:: template <class ST, class SA> \
		  basic_regex(const basic_string<charT, ST, SA>& s, type_flag f = regex_constants::normal)

   :throws bad_expression: :cpp:var:`!s` が正しい正規表現でない場合（:cpp:var:`!f` に :cpp:var:`!no_except` が設定されていない場合）。
   :効果: :cpp:class:`basic_regex` クラスのオブジェクトを構築する。:cpp:var:`!f` で指定した\ :doc:`オプションフラグ <syntax_option_type>`\にしたがって文字列 :cpp:var:`!s` の正規表現を解釈し、オブジェクトの内部有限状態マシンを構築する。

   .. list-table:: :cpp:class:`!basic_regex` コンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:expr:`s.size()`
      * - :cpp:expr:`str()`
        - :cpp:var:`!s`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count()`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: template <class ForwardIterator> \
		  basic_regex(ForwardIterator first, ForwardIterator last, flag_type f = regex_constants::normal)

   :throws bad_expression: [first,last) が正しい正規表現でない場合（:cpp:var:`!f` に :cpp:var:`!no_except` が設定されていない場合）。
   :効果: :cpp:class:`basic_regex` クラスのオブジェクトを構築する。:cpp:var:`!f` で指定した\ :doc:`オプションフラグ <syntax_option_type>`\にしたがって文字シーケンス [first,last) の正規表現を解釈し、オブジェクトの内部有限状態マシンを構築する。

   .. list-table:: :cpp:class:`!basic_regex` コンストラクタの事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:expr:`distance(first,last)`
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>(first,last)`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count()`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: basic_regex& operator=(const basic_regex& e)

   :効果: :cpp:expr:`assign(e.str(), e.flags())` の結果を返す。


.. cpp:function:: basic_regex& operator=(const charT* ptr)

   :要件: :cpp:var:`!ptr` は null ポインタ以外。
   :効果: :cpp:expr:`assign(ptr)` の結果を返す。


.. cpp:function:: template <class ST, class SA> \
		  basic_regex& operator=(const basic_regex<charT, ST, SA>& p)

   :効果: :cpp:expr:`assign(p)` の結果を返す。


.. cpp:function:: std::pair<const_iterator, const_iterator> subexpression(size_type n) const

   :効果: 元の正規表現文字列内のマーク済み部分式 :cpp:var:`!n` の位置を表すイテレータのペアを返す。戻り値のイテレータは :cpp:func:`!begin()` および :cpp:func:`!end()` からの相対位置である。
   :要件: 正規表現は :cpp:type:`syntax_option_type` :cpp:var:`!save_subexpression_location` を設定してコンパイルしていなければならない。引数 :cpp:var:`!n` は ``0 <= n < mark_count()`` の範囲になければならない。


.. cpp:function:: const_iterator begin() const

   :効果: 正規表現を表す文字シーケンスの開始イテレータを返す。


.. cpp:function:: const_iterator end() const

   :効果: 正規表現を表す文字シーケンスの終了イテレータを返す。


.. cpp:function:: size_type size() const

   :効果: 正規表現を表す文字シーケンスの長さを返す。


.. cpp:function:: size_type max_size() const

   :効果: 正規表現を表す文字シーケンスの最大長さを返す。


.. cpp:function:: bool empty() const

   :効果: オブジェクトが正しい正規表現を保持していない場合に真を返す。それ以外の場合は偽を返す。


.. cpp:function:: unsigned mark_count() const

   :効果: 正規表現中のマーク済み部分式の数を返す。


.. cpp:function:: basic_regex& assign(const basic_regex& that)

   :効果: :cpp:expr:`assign(that.str(), that.flags())` を返す。


.. cpp:function:: basic_regex& assign(const charT* ptr, flag_type f)

   :効果: :cpp:expr:`assign(string_type(ptr), f)` を返す。


.. cpp:function:: basic_regex& assign(const charT* ptr, unsigned int len, flag_type f)

   :効果: :cpp:expr:`assign(string_type(ptr, len), f)` を返す。


.. cpp:function:: template <class string_traits, class A> \
		  basic_regex& assign(const basic_string<charT, string_traits, A>& s, flag_type f)

   :throws bad_expression: :cpp:var:`!s` が正しい正規表現でない場合（:cpp:var:`!f` に :cpp:var:`!no_except` が設定されていない場合）。
   :returns: :cpp:expr:`*this`。
   :効果: :cpp:var:`!f` で指定した\ :doc:`オプションフラグ <syntax_option_type>`\にしたがって文字列 :cpp:var:`!s` の正規表現を解釈し代入する。

   .. list-table:: :cpp:func:`!basic_regex::assign` の事後条件
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - ``false``
      * - :cpp:expr:`size()`
        - :cpp:expr:`s.size()`
      * - :cpp:expr:`str()`
        - :cpp:var:`!s`
      * - :cpp:expr:`flags()`
        - :cpp:var:`!f`
      * - :cpp:expr:`mark_count`
        - 正規表現中に含まれるマーク済み部分式の総数


.. cpp:function:: template <class InputIterator> \
		  basic_regex& assign(InputIterator first, InputIterator last, flag_type f)

   :要件: :cpp:type:`!InputIterator` 型は\ `入力イテレータの要件（24.1.1） <http://input_iterator/>`_\を満たす。
   :効果: :cpp:expr:`assign(string_type(first, last), f)` を返す。


.. cpp:function:: flag_type flags() const

   :効果: オブジェクトのコンストラクタ、あるいは最後の :cpp:func:`!assign` の呼び出しで渡した\ :doc:`正規表現構文のフラグ <syntax_option_type>`\のコピーを返す。


.. cpp:function:: int status() const

   :効果: 正規表現が正しい正規表現であれば 0、それ以外の場合はエラーコードを返す。このメンバ関数は例外処理を使用できない環境のために用意されている。


.. cpp:function:: basic_string<charT> str() const

   :効果: オブジェクトのコンストラクタ、あるいは最後の :cpp:func:`!assign` の呼び出しで渡した文字シーケンスのコピーを返す。


.. cpp:function:: int compare(basic_regex& e) const

   :効果: :cpp:expr:`flags() == e.flags()` であれば :cpp:expr:`str().compare(e.str())` を、それ以外の場合は :cpp:expr:`flags() - e.flags()` を返す。


.. cpp:function:: locale_type imbue(locale_type l)

   :効果: :cpp:expr:`traits_inst.imbue(l)` の結果を返す。:cpp:var:`!traits_inst` はオブジェクト内の、テンプレート引数 :cpp:type:`!traits` のインスタンス（をデフォルトコンストラクタで初期化したもの）である。
   :事後条件: :cpp:expr:`empty() == true`。


.. cpp:function:: locale_type getloc() const

   :効果: :cpp:expr:`traits_inst.getloc()` の結果を返す。:cpp:var:`!traits_inst` はオブジェクト内の、テンプレート引数 :cpp:type:`!traits` のインスタンス（をデフォルトコンストラクタで初期化したもの）である。


.. cpp:function:: void swap(basic_regex& e) noexcept

   :効果: 2 つの正規表現の内容を交換する。
   :事後条件: :cpp:expr:`*this` は :cpp:var:`e` にあった正規表現を保持し、:cpp:var:`e` は :cpp:expr:`*this` にあった正規表現を保持する。
   :計算量: 一定。


.. cpp:namespace-pop::


.. cpp:function:: template <class charT, class traits> \
		  bool operator ==(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   .. note::
      :cpp:class:`basic_regex` オブジェクト間の比較は実験的なものである。`Technical Report on C++ Libraries <http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1836.pdf>`_ には記述がなく、:cpp:class:`basic_regex` の他の実装に移植する必要がある場合は注意していただきたい。

   :効果: :cpp:expr:`lhs.compare(rhs) == 0` を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool operator !=(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) != 0` を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool operator <(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) < 0` を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool operator <=(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) <= 0` を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool operator >=(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) >= 0` を返す。


.. cpp:function:: template <class charT, class traits> \
		  bool operator >(const basic_regex<charT, traits>& lhs, const basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) > 0` を返す。


.. cpp:function:: template <class charT, class io_traits, class re_traits> \
		  basic_ostream<charT, io_traits>& operator <<(basic_ostream<charT, io_traits>& os, const basic_regex<charT, re_traits>& e)

   .. note:: :cpp:class:`!basic_regex` のストリーム挿入子は実験的なものであり、正規表現のテキスト表現をストリームに出力する。

   :効果: :cpp:expr:`(os << e.str())` を返す。


.. cpp:function:: void swap(basic_regex<charT, traits>& lhs, basic_regex<charT, traits>& rhs)

   :効果: :cpp:expr:`lhs.swap(rhs)` を呼び出す。
