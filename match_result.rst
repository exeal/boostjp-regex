.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


match_results
=============

.. cpp:class:: template <class BidirectionalIterator, class Allocator = std::allocator<sub_match<BidirectionalIterator> > > \
	       match_results

   正規表現が他の多くの単純なパターンマッチアルゴリズムと異なるのは、マッチを発見するだけでなく、部分式のマッチを生成する点である。各部分式はパターン中の括弧の組 :regexp:`(…)` により、その範囲が与えられる。部分式のマッチをユーザに知らせるために何らかの方法が必要である。部分式マッチの添字付きコレクションとして振舞う :cpp:class:`!match_results` クラスの定義がそれであり、各部分式マッチは :cpp:class:`sub_match` 型オブジェクトに含まれる。

   テンプレートクラス :cpp:class:`!match_results` は、正規表現マッチの結果を表す文字シーケンスのコレクションを表現する。:cpp:class:`!match_results` 型のオブジェクトは :cpp:func:`regex_match` および :cpp:func:`regex_search` アルゴリズムに渡して使用する。またイテレータ :cpp:class:`regex_iterator` がこのオブジェクトを返す。このコレクションが使用するストレージは、:cpp:class:`!match_results` のメンバ関数が必要に応じて割り当て、解放する。

   テンプレートクラス :cpp:class:`!match_results` は（lib.sequence.reqmts）が規定する Sequence の要件を満たす。ただし const 限定の操作に限られる。

   大抵の場合、クラステンプレート :cpp:class:`!match_results` を使用するときは、その typedef である :cpp:type:`!cmatch` 、:cpp:type:`!wcmatch` 、:cpp:type:`!smatch` および :cpp:type:`!wsmatch` のいずれかを用いる。


.. cpp:namespace-push:: match_results


.. _ref.match_results.synopsis:

概要
----

.. parsed-literal::

   #include <boost/regex.hpp>

   template <class BidirectionalIterator,
             class Allocator = std::allocator<sub_match<BidirectionalIterator> >
   class match_results;

   typedef match_results<const char*>              cmatch;
   typedef match_results<const wchar_t*>           wcmatch;
   typedef match_results<string::const_iterator>   smatch;
   typedef match_results<wstring::const_iterator>  wsmatch;

   template <class BidirectionalIterator,
             class Allocator = std::allocator<sub_match<BidirectionalIterator> >
   class match_results
   {
   public:
      typedef          sub_match<BidirectionalIterator>                        value_type;
      typedef          const value_type&                                       const_reference;
      typedef          const_reference                                         reference;
      typedef          :samp:`{implementation defined}`                                  const_iterator;
      typedef          const_iterator                                          iterator;
      typedef typename iterator_traits<BidirectionalIterator>::difference_type difference_type;
      typedef typename Allocator::size_type                                    size_type;
      typedef          Allocator                                               allocator_type;
      typedef typename iterator_traits<BidirectionalIterator>::value_type      char_type;
      typedef          basic_string<char_type>                                 string_type;

      // 構築、コピー、解体：
      explicit :cpp:func:`~match_results::match_results`\(const Allocator& a = Allocator());
      :cpp:func:`~match_results::match_results`\(const match_results& m);
      match_results& :cpp:func:`operator=`\(const match_results& m);
      ~match_results();

      // サイズ：
      size_type :cpp:func:`size`\() const;
      size_type :cpp:func:`max_size`\() const;
      bool :cpp:func:`empty`\() const;
      // 要素アクセス：
      difference_type :cpp:func:`length`\(int sub = 0) const;
      difference_type :cpp:func:`length`\(const char_type* sub) const;
      template <class charT>
      difference_type :cpp:func:`length`\(const charT* sub) const;
      template <class charT, class Traits, class A>
      difference_type :cpp:func:`length`\(const std::basic_string<charT, Traits, A>& sub) const;
      difference_type :cpp:func:`position`\(unsigned int sub = 0) const;
      difference_type :cpp:func:`position`\(const char_type* sub) const;
      template <class charT>
      difference_type :cpp:func:`position`\(const charT* sub) const;
      template <class charT, class Traits, class A>
      difference_type :cpp:func:`position`\(const std::basic_string<charT, Traits, A>& sub) const;
      string_type :cpp:func:`str`\(int sub = 0) const;
      string_type :cpp:func:`str`\(const char_type* sub)const;
      template <class Traits, class A>
      string_type :cpp:func:`str`\(const std::basic_string<char_type, Traits, A>& sub)const;
      template <class charT>
      string_type :cpp:func:`str`\(const charT* sub)const;
      template <class charT, class Traits, class A>
      string_type :cpp:func:`str`\(const std::basic_string<charT, Traits, A>& sub)const;
      const_reference :cpp:func:`operator[]`\(int n) const;
      const_reference :cpp:func:`operator[]`\(const char_type* n) const;
      template <class Traits, class A>
      const_reference :cpp:func:`operator[]`\(const std::basic_string<char_type, Traits, A>& n) const;
      template <class charT>
      const_reference :cpp:func:`operator[]`\(const charT* n) const;
      template <class charT, class Traits, class A>
      const_reference :cpp:func:`operator[]`\(const std::basic_string<charT, Traits, A>& n) const;

      const_reference :cpp:func:`prefix`\() const;
      const_reference :cpp:func:`suffix`\() const;
      const_iterator :cpp:func:`begin`\() const;
      const_iterator :cpp:func:`end`\() const;

      // 書式化：
      template <class OutputIterator, class Formatter>
      OutputIterator :cpp:func:`format`\(OutputIterator out,
                              Formatter& fmt,
                              match_flag_type flags = format_default) const;
      template <class Formatter>
      string_type :cpp:func:`format`\</methodname>(const Formatter fmt,
                            match_flag_type flags = format_default) const;

      allocator_type :cpp:func:`get_allocator`\() const;
      void :cpp:func:`swap`\(match_results& that);

   #ifdef BOOST_REGEX_MATCH_EXTRA
      typedef typename value_type::capture_sequence_type capture_sequence_type;
      const capture_sequence_type& :cpp:func:`captures`\(std::size_t i)const;
   #endif

   };

   template <class BidirectionalIterator, class Allocator>
   bool :cpp:func:`operator ==` (const match_results<BidirectionalIterator, Allocator>& m1,
                     const match_results<BidirectionalIterator, Allocator>& m2);
   template <class BidirectionalIterator, class Allocator>
   bool :cpp:func:`operator !=` (const match_results<BidirectionalIterator, Allocator>& m1,
                     const match_results<BidirectionalIterator, Allocator>& m2);

   template <class charT, class traits, class BidirectionalIterator, class Allocator>
   basic_ostream<charT, traits>&
      :cpp:func:`operator <<` (basic_ostream<charT, traits>& os,
                   const match_results<BidirectionalIterator, Allocator>& m);

   template <class BidirectionalIterator, class Allocator>
   void :cpp:func:`swap`\(match_results<BidirectionalIterator, Allocator>& m1,
             match_results<BidirectionalIterator, Allocator>& m2);


.. _ref.match_results.description:

説明
----

.. cpp:function:: match_results(const Allocator& a = Allocator())

   :効果: :cpp:class:`!match_results` クラスのオブジェクトを構築する。この関数の事後条件は次の表のとおりである。

   .. list-table::
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
	- :cpp:var:`!true`
      * - :cpp:expr:`size()`
        - ``0``
      * - :cpp:expr:`str()`
        - :cpp:expr:`basic_string<charT>()`

   :cpp:class:`!match_results` のすべてのコンストラクタにおける :cpp:type:`!Allocator` 引数のコピーは、オブジェクトの生涯にわたってコンストラクタとメンバ関数によるメモリ割り当てに使用される。


.. cpp:function:: match_results(const match_results& m)

   :効果: :cpp:var:`!m` をコピーして :cpp:class:`!match_results` クラスのオブジェクトを構築する。


.. cpp:function:: match_results& operator=(const match_results& m)

   :効果: :cpp:var:`!m` を :cpp:expr:`*this` に代入する。この関数の事後条件は次の表のとおりである。

   .. list-table::
      :header-rows: 1

      * - 要素
        - 値
      * - :cpp:expr:`empty()`
        - :cpp:expr:`m.empty()`
      * - :cpp:expr:`size()`
        - :cpp:expr:`m.size()`
      * - :cpp:expr:`str(n)`
        - :cpp:expr:`n < m.size()` であるすべての整数で :cpp:expr:`m.str(n)`
      * - :cpp:expr:`prefix()`
        - :cpp:expr:`m.prefix()`
      * - :cpp:expr:`suffix()`
        - :cpp:expr:`m.suffix()`
      * - :cpp:expr:`(*this)[n]`
        - :cpp:expr:`n < m.size()` であるすべての整数で :cpp:expr:`m[n]`
      * - :cpp:expr:`length(n)`
        - :cpp:expr:`n < m.size()` であるすべての整数で :cpp:expr:`m.length(n)`
      * - :cpp:expr:`position(n)`
        - :cpp:expr:`n < m.size()` であるすべての整数で :cpp:expr:`m.position(n)`


.. cpp:function:: size_type size() const

   :効果: :cpp:expr:`*this` 中の :cpp:class:`sub_match` 要素数を返す。これは正規表現中でマッチしたマーク済み部分式の数に 1 を足したものである。


.. cpp:function:: size_type max_size() const

   :効果: :cpp:expr:`*this` に格納可能な :cpp:class:`sub_match` 要素の最大数を返す。


.. cpp:function:: bool empty() const

   :効果: :cpp:expr:`size() == 0` を返す。


.. cpp:function:: difference_type length(int sub = 0) const
		  difference_type length(const char_type* sub) const
		  template <class charT> \
		  difference_type length(const charT* sub) const
		  template <charT, class Traits, class A> \
		  difference_type length(const std::basic_string<charT, Traits, A>& sub) const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: 部分式 :cpp:var:`!sub` の長さを返す。:cpp:expr:`(*this)[sub].length()` と同じである。

          文字列を引数に取る多重定義は :samp:`{n}` 番目の名前付き部分式を参照する。指定した名前をもつ部分式がない場合は 0 を返す。

          この関数のテンプレート多重定義に渡す文字列・文字の型は、オブジェクトが保持するシーケンスや正規表現の文字型と異なっていてもよい。この場合、文字列は正規表現が保持する文字型に変換される。引数の文字型が正規表現が保持するシーケンスの文字型より幅が大きい場合はコンパイルエラーとなる。これらの多重定義は、マッチを行う正規表現の文字型が Unicode 文字型のような変り種の場合であっても、通常の幅の小さい C 文字列リテラルを引数として渡せるようにしてある。


.. cpp:function:: difference_type position(unsigned int sub = 0) const
		  difference_type position(const char_type* sub) const
		  template <class charT> \
		  difference_type position(const charT* sub) const
                  template <class charT, class Traits, class A> \
		  difference_type position(const std::basic_string<charT, Traits, A>& sub) const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: 部分式 :cpp:var:`!sub` の開始位置を返す。:cpp:var:`!sub` がマッチしなかった場合は -1 を返す。部分マッチの場合は :cpp:expr:`(*this)[0].matched` は偽であるが、:cpp:expr:`position()` は部分マッチの位置を返す。

          文字列を引数に取る多重定義は :samp:`{n}` 番目の名前付き部分式を参照する。指定した名前をもつ部分式がない場合は -1 を返す。

	  この関数のテンプレート多重定義に渡す文字列・文字の型は、オブジェクトが保持するシーケンスや正規表現の文字型と異なっていてもよい。この場合、文字列は正規表現が保持する文字型に変換される。引数の文字型が正規表現が保持するシーケンスの文字型より幅が大きい場合はコンパイルエラーとなる。これらの多重定義は、マッチを行う正規表現の文字型が Unicode 文字型のような変り種の場合であっても、通常の幅の小さい C 文字列リテラルを引数として渡せるようにしてある。


.. cpp:function:: string_type str(int sub = 0) const
		  string_type str(const char_type* sub) const
		  template <class Traits, class A> \
		  string_type str(const std::basic_string<char_type, Traits, A>& sub) const
		  template <class charT> \
		  string_type str(const charT* sub) const
		  template <class charT, class Traits, class A> \
		  string_type str(const std::basic_string<charT, Traits, A>& sub) const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: 部分式 :cpp:var:`!sub` の文字列を返す。:cpp:expr:`string_type((*this)[sub])` と同じである。

          文字列を引数に取る多重定義は :samp:`{n}` 番目の名前付き部分式を参照する。指定した名前をもつ部分式がない場合は空文字列を返す。

          この関数のテンプレート多重定義に渡す文字列・文字の型は、オブジェクトが保持するシーケンスや正規表現の文字型と異なっていてもよい。この場合、文字列は正規表現が保持する文字型に変換される。引数の文字型が正規表現が保持するシーケンスの文字型より幅が大きい場合はコンパイルエラーとなる。これらの多重定義は、マッチを行う正規表現の文字型が Unicode 文字型のような変り種の場合であっても、通常の幅の小さい C 文字列リテラルを引数として渡せるようにしてある。


.. cpp:function:: const_reference operator[](int n) const
		  const_reference operator[](const char_type* n) const
		  template <class Traits, class A> \
		  const_reference operator[](const std::basic_string<char_type, Traits, A>& n) const
		  template <class charT> \
		  const_reference operator[](const charT* n) const
		  template <class charT, class Traits, class A> \
		  const_reference operator[](const std::basic_string<charT, Traits, A>& n) const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: マーク済み部分式 :samp:`{n}` にマッチした文字シーケンスを表す :cpp:class:`sub_match` オブジェクトへの参照を返す。:cpp:expr:`n == 0` の場合は、正規表現全体にマッチした文字シーケンスを表す :cpp:class:`sub_match` オブジェクトへの参照を返す。:samp:`{n}` が範囲外であるかマッチしなかった部分式を指している場合は、:cpp:member:`~sub_match::matched` メンバが偽である :cpp:class:`sub_match` オブジェクトを返す。

          文字列を引数に取る多重定義は :samp:`{n}` 番目の名前付き部分式にマッチした文字シーケンスを表す :cpp:class:`sub_match` オブジェクトへの参照を返す。指定した名前をもつ部分式がない場合は :cpp:member:`~sub_match::matched` メンバが偽である :cpp:class:`sub_match` オブジェクトを返す。

          この関数のテンプレート多重定義に渡す文字列・文字の型は、オブジェクトが保持するシーケンスや正規表現の文字型と異なっていてもよい。この場合、文字列は正規表現が保持する文字型に変換される。引数の文字型が正規表現が保持するシーケンスの文字型より幅が大きい場合はコンパイルエラーとなる。これらの多重定義は、マッチを行う正規表現の文字型が Unicode 文字型のような変り種の場合であっても、通常の幅の小さい C 文字列リテラルを引数として渡せるようにしてある。


.. cpp:function:: const_reference prefix() const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: マッチ・検索を行う文字列の先頭から見つかったマッチの先頭までの文字シーケンスを表す :cpp:class:`sub_match` オブジェクトへの参照を返す。


.. cpp:function:: const_reference suffix() const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: 見つかったマッチの終端からマッチ・検索を行う文字列の終端までの文字シーケンスを表す :cpp:class:`sub_match` オブジェクトへの参照を返す。


.. cpp:function:: const_iterator begin() const

   :効果: :cpp:expr:`*this` に格納されたすべてのマーク済み部分式を列挙する開始イテレータを返す。


.. cpp:function:: const_iterator end() const

   :効果: :cpp:expr:`*this` に格納されたすべてのマーク済み部分式を列挙する終了イテレータを返す。


.. cpp:function:: template <class OutputIterator, class Formatter> \
		  OutputIterator format(OutputIterator out, Formatter fmt, match_flag_type flags = format_default)

   :要件: 型 :cpp:type:`!OutputIterator` が出力イテレータの要件（C++ 標準 24.1.2）を満たす。

          型 :cpp:type:`!Formatter` は :cpp:type:`!char_type[]` 型の null 終端文字列へのポインタ、:cpp:type:`!char_type` 型のコンテナ（例えば :cpp:class:`!std::basic_string<char_type>`）、あるいは関数呼び出しにより置換文字列を生成する単項・二項・三項関数子のいずれかでなければならない。関数子の場合、:cpp:expr:`fmt(*this)` は置換テキストと使用する :cpp:type:`!char_type` のコンテナを返さなければならず、:cpp:expr:`fmt(*this, out)` および :cpp:expr:`fmt(*this, out, flags)` はいずれも置換テキストを :cpp:expr:`*out` に出力し :cpp:type:`OutputIterator` の新しい位置を返さなければならない。書式化子が関数子の場合は\ **値渡し**\となることに注意していただきたい。内部状態を持つ関数オブジェクトを渡す場合、`Boost.Ref <http://www.boost.org/libs/ref.html>`_ を使用してオブジェクトを参照渡しするとよい。
   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: :cpp:var:`!fmt` が null 終端文字列か :cpp:type:`char_type` のコンテナであれば、文字シーケンス [\ :cpp:expr:`fmt.begin()`\ , :cpp:expr:`fmt.end()`\ ) を :cpp:type:`OutputIterator` :cpp:var:`!out` にコピーする。:cpp:var:`!fmt` 中の各書式指定子とエスケープシーケンスは、シーケンスをそれぞれが表す文字（列）か、参照する :cpp:expr:`*this` 中の文字シーケンスで置換する。:cpp:var:`!flags` で指定したビットマスクはどの書式指定子・エスケープシーケンスを使用するか決定し、既定では ECMA-262 、ECMAScript 言語仕様、15 章 5.4.11 String.prototype.replace で使用されている書式である。

          :cpp:var:`!fmt` が関数オブジェクトであれば、関数オブジェクトが受け取った引数の数により以下のようになる。

          * :cpp:expr:`fmt(*this)` を呼び出し、結果を :cpp:type:`!OutputIterator` :cpp:var:`out` にコピーする。
          * :cpp:expr:`fmt(*this, out)` を呼び出す。
          * :cpp:expr:`fmt(*this, out, flags)` を呼び出す。

          すべての場合で :cpp:type:`!OutputIterator` の新しい位置が返される。

          詳細は\ :doc:`書式化構文ガイド <format_syntax>`\を見よ。
   :returns: :cpp:var:`!out`


.. cpp:function:: template <class Formatter> \
		  string_type format(Formatter fmt, match_flag_type flags = format_default)

   :要件: 型 :cpp:type:`!Formatter` は :cpp:type:`!char_type[]` 型の null 終端文字列へのポインタ、:cpp:type:`!char_type` 型のコンテナ（例えば :cpp:class:`!std::basic_string<char_type>`）、あるいは関数呼び出しにより置換文字列を生成する単項・二項・三項関数子のいずれかでなければならない。関数子の場合、:cpp:expr:`fmt(*this)` は置換テキストと使用する :cpp:type:`!char_type` のコンテナを返さなければならず、:cpp:expr:`fmt(*this, out)` および :cpp:expr:`fmt(*this, out, flags)` はいずれも置換テキストを :cpp:expr:`*out` に出力し :cpp:type:`OutputIterator` の新しい位置を返さなければならない。
   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`!std::logic_error` が発生する。
   :効果: :cpp:var:`!fmt` が null 終端文字列か :cpp:type:`!char_type` のコンテナであれば、文字列 :cpp:var:`!fmt` をコピーする。:cpp:var:`!fmt` 中の各書式指定子とエスケープシーケンスは、シーケンスをそれぞれが表す文字（列）か、参照する :cpp:expr:`*this` 中の文字シーケンスで置換する。:cpp:var:`!flags` で指定したビットマスクはどの書式指定子・エスケープシーケンスを使用するか決定し、既定では ECMA-262 、ECMAScript 言語仕様、15 章 5.4.11 String.prototype.replace で使用されている書式である。

          :cpp:var:`!fmt` が関数オブジェクトであれば、関数オブジェクトが受け取った引数の数により以下のようになる。

          * :cpp:expr:`fmt(*this)` を呼び出し、結果を返す。
          * :cpp:expr:`fmt(*this, unspecified_output_iterator)` を呼び出す。:samp:`{unspecified_output_iterator}` は出力を結果文字列にコピーする指定なしの :cpp:type:`!OutputIterator` 型である。
          * :cpp:expr:`fmt(*this, unspecified_output_iterator, flags)` を呼び出す。:samp:`{unspecified_output_iterator}` は出力を結果文字列にコピーする指定なしの :cpp:type:`!OutputIterator` 型である。

          すべての場合で :cpp:type:`!OutputIterator` の新しい位置が返される。

          詳細は\ :doc:`書式化構文ガイド <format_syntax>`\を見よ。


.. cpp:function:: allocator_type get_allocator() const

   :効果: オブジェクトのコンストラクタで渡した :cpp:type:`!Allocator` のコピーを返す。


.. cpp:function:: void swap(match_results& that)

   :効果: 2 つのシーケンスの内容を交換する。
   :事後条件: :cpp:expr:`*this` は、:cpp:var:`!that` が保持していた、部分式にマッチしたシーケンスを保持する。:cpp:var:`!that` は、:cpp:expr:`*this` が保持していた、部分式にマッチしたシーケンスを保持する。
   :計算量: 一定。


.. cpp:type:: typename value_type::capture_sequence_type capture_sequence_type

   標準ライブラリ Sequence の要件（21.1.1 および表 68 の操作）を満たす実装固有の型を定義する。その :cpp:type:`!value_type` は :cpp:class:`!sub_match<BidirectionalIterator>` である。この型が :cpp:class:`!std::vector<sub_match<BidirectionalIterator> >` となる可能性もあるが、それに依存すべきではない。


.. cpp:function:: const capture_sequence_type& captures(std::size_t i) const

   :要件: :cpp:class:`!match_results` オブジェクトが :cpp:func:`regex_search` か :cpp:func:`regex_match` の呼び出し結果で初期化された、または :cpp:class:`regex_iterator` が返したもので、かつそのイテレータが無効状態でない。:cpp:class:`!match_results` オブジェクトが未初期化の場合、:cpp:class:`std::logic_error` が発生する。
   :効果: 部分式 :cpp:var:`!i` に対するすべての捕捉を格納したシーケンスを返す。
   :returns: :cpp:expr:`(*this)[i].captures()`
   :事前条件: :c:macro:`!BOOST_REGEX_MATCH_EXTRA` を使ってライブラリをビルドしていなければ、このメンバ関数は定義されない。また正規表現マッチ関数（:cpp:func:`regex_match` 、:cpp:func:`regex_search` 、:cpp:class:`regex_iterator` 、:cpp:class:`regex_token_iterator`）にフラグ :cpp:var:`!match_extra` を渡していなければ、有用な情報を返さない。
   :根拠: この機能を有効にするといくつか影響がある。

          * :cpp:class:`!sub_match` がより多くのメモリを占有し、複雑な正規表現をマッチする場合にすぐにメモリやスタック空間の不足に陥る。
          * :cpp:var:`!match_extra` を使用しない場合であっても、処理する機能（例えば独立部分式）によってはマッチアルゴリズムの効率が落ちる。
          * :cpp:var:`!match_extra` を使用するとさらに効率が落ちる（速度が低下する）。ほとんどの場合、さらに必要なメモリ割り当てが起こる。


.. cpp:function:: template <class BidirectionalIterator, class Allocator> \
		  bool operator ==(const match_results<BidirectionalIterator, Allocator>& m1, const match_results<BidirectionalIterator, Allocator>& m2)

   :効果: 2 つのシーケンスの等価性を比較する。


.. cpp:function:: template <class BidirectionalIterator, class Allocator> \
		  bool operator !=(const match_results<BidirectionalIterator, Allocator>& m1, const match_results<BidirectionalIterator, Allocator>& m2)

   :効果: 2 つのシーケンスの非等価性を比較する。


.. cpp:function:: template <class charT, class traits, class BidirectionalIterator, class Allocator> \
		  basic_ostream<charT, traits>& operator <<(basic_ostream<charT, traits>& os, const match_results<BidirectionalIterator, Allocator>& m)


   :効果: :cpp:expr:`os << m.str()` の要領で :cpp:var:`!m` の内容をストリーム :cpp:var:`!os` に書き込む。:cpp:var:`!os` を返す。


.. cpp:function:: template <class BidirectionalIterator, class Allocator> \
		  void swap(match_results<BidirectionalIterator, Allocator>& m1, match_results<BidirectionalIterator, Allocator>& m2)

   :効果: 2 つのシーケンスの内容を交換する。
