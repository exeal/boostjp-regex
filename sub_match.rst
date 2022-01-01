.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


sub_match
=========

.. cpp:class:: template<class BidirectionalIterator> \
               sub_match : public std::pair<BidirectionalIterator, BidirectionalIterator>

   正規表現が他の多くの単純なパターンマッチアルゴリズムと異なるのは、マッチを発見するだけでなく、部分式のマッチを生成する点である。各部分式はパターン中の括弧の組 :regexp:`(...)` により、その範囲が与えられる。部分式マッチをユーザに知らせるために何らかの方法が必要である。部分式マッチの添字付きコレクションとして振舞う :cpp:class:`match_results` クラスの定義がそれであり、各部分式マッチは :cpp:class:`sub_match` 型オブジェクトが保持する。

   :cpp:class:`sub_match` 型のオブジェクトは :cpp:class:`match_results` 型のオブジェクトの配列要素としてのみ取得可能である。

   :cpp:class:`sub_match` 型のオブジェクトは :cpp:class:`!std::basic_string` 、:cpp:type:`!const charT*` 、:cpp:type:`!const charT` 型のオブジェクトと比較可能である。

   :cpp:class:`sub_match` 型のオブジェクトは :cpp:class:`!std::basic_string` 、:cpp:type:`!const charT*` 、:cpp:type:`!const charT` 型のオブジェクトに追加して新しい :cpp:class:`!std::basic_string` オブジェクトを生成可能である。

   :cpp:class:`sub_match` 型のオブジェクトで示されるマーク済み部分式が正規表現マッチに関与していれば :cpp:member:`~sub_match::matched` メンバは\ **真**\と評価され、メンバ :cpp:member:`~sub_match::first` と :cpp:member:`~sub_match::second` はマッチを形成する文字範囲 [first,second) を示す。それ以外の場合は :cpp:member:`~sub_match::matched` は\ **偽**\であり、メンバ :cpp:member:`~sub_match::first` と :cpp:member:`~sub_match::second` は未定義の値となる。

   :cpp:class:`sub_match` 型のオブジェクトで示されるマーク済み部分式が繰り返しになっている場合、その :cpp:class:`sub_match` オブジェクトが表現するのは\ **最後の**\繰り返しに対応するマッチである。すべての繰り返しに対応するすべての捕捉の完全なセットは :cpp:func:`!captures()` メンバ関数でアクセス可能である（効率に関して深刻な問題があり、この機能は明示的に有効にしなければならない）。

   :cpp:class:`sub_match` 型のオブジェクトが部分式 0（マッチ全体）を表現する場合、メンバ :cpp:member:`~sub_match::matched` は常に\ **真**\である。ただし正規表現アルゴリズムにフラグ :cpp:var:`!match_partial` を渡して結果が\ :doc:`部分マッチ <partial_matches>`\となる場合はこの限りではなく、メンバ :cpp:member:`~sub_match::matched` は\ **偽**\、メンバ :cpp:member:`~sub_match::first` と :cpp:member:`~sub_match::second` は部分マッチを形成する文字範囲を表現する。


.. cpp:namespace-push:: sub_match

.. parsed-literal::

   #include <boost/regex.hpp>

   namespace boost{

   template <class BidirectionalIterator>
   class sub_match;

   typedef sub_match<const char*>                    csub_match;
   typedef sub_match<const wchar_t*>                 wcsub_match;
   typedef sub_match<std::string::const_iterator>    ssub_match;
   typedef sub_match<std::wstring::const_iterator>   wssub_match;

   template <class BidirectionalIterator>
   class sub_match : public std::pair<BidirectionalIterator, BidirectionalIterator>
   {
   public:
      typedef typename iterator_traits<BidirectionalIterator>::value_type       :cpp:type:`value_type`;
      typedef typename iterator_traits<BidirectionalIterator>::difference_type  :cpp:type:`difference_type`;
      typedef          BidirectionalIterator                                    :cpp:type:`iterator`;

      bool :cpp:member:`matched`;

      difference_type :cpp:func:`length`\()const;
      :cpp:func:`operator basic_string\<value_type>`\()const;
      basic_string<value_type> :cpp:func:`str`\()const;

      int :cpp:func:`compare`\(const sub_match& s)const;
      int :cpp:func:`compare`\(const basic_string<value_type>& s)const;
      int :cpp:func:`compare`\(const value_type* s)const;
   #ifdef BOOST_REGEX_MATCH_EXTRA
      typedef :samp:`{implementation-private` :cpp:type:`capture_sequence_type`;
      const capture_sequence_type& :cpp:func:`captures`\()const;
   #endif
   };
   //
   // sub_match 同士の比較：
   //
   template <class BidirectionalIterator>
   bool :cpp:func:`operator ==` (const sub_match<BidirectionalIterator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator !=` (const sub_match<BidirectionalIterator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <` (const sub_match<BidirectionalIterator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <=` (const sub_match<BidirectionalIterator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >=` (const sub_match<BidirectionalIterator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >` (const sub_match<BidirectionalIterator>& lhs,
                    const sub_match<BidirectionalIterator>& rhs);

   //
   // basic_string との比較：
   //
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator ==` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator !=` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator <` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                            traits,
                                            Allocator>& lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator >` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                            traits,
                                            Allocator>& lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator >=` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator <=` (const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator ==` (const sub_match<BidirectionalIterator>& lhs,
                     const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator !=` (const sub_match<BidirectionalIterator>& lhs,
                     const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator <` (const sub_match<BidirectionalIterator>& lhs,
                    const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                            traits,
                                            Allocator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator >` (const sub_match<BidirectionalIterator>& lhs,
                    const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                            traits,
                                            Allocator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator >=` (const sub_match<BidirectionalIterator>& lhs,
                     const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& rhs);
   template <class BidirectionalIterator, class traits, class Allocator>
   bool :cpp:func:`operator <=` (const sub_match<BidirectionalIterator>& lhs,
                     const std::basic_string<iterator_traits<BidirectionalIterator>::value_type,
                                             traits,
                                             Allocator>& rhs);

   //
   // 文字列ポインタとの比較：
   //
   template <class BidirectionalIterator>
   bool :cpp:func:`operator ==` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator !=` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >=` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <=` (typename iterator_traits<BidirectionalIterator>::value_type const* lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator ==` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const* rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator !=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const* rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <` (const sub_match<BidirectionalIterator>& lhs,
                    typename iterator_traits<BidirectionalIterator>::value_type const* rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >` (const sub_match<BidirectionalIterator>& lhs,
                    typename iterator_traits<BidirectionalIterator>::value_type const* rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const* rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const* rhs);

   //
   // 1 文字との比較：
   //
   template <class BidirectionalIterator>
   bool :cpp:func:`operator ==` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator !=` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                    const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >=` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <=` (typename iterator_traits<BidirectionalIterator>::value_type const& lhs,
                     const sub_match<BidirectionalIterator>& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator ==` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator !=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator >=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);
   template <class BidirectionalIterator>
   bool :cpp:func:`operator <=` (const sub_match<BidirectionalIterator>& lhs,
                     typename iterator_traits<BidirectionalIterator>::value_type const& rhs);

   //
   // 加算演算子：
   //
   template <class BidirectionalIterator, class traits, class Allocator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>
      :cpp:func:`operator +` (const std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type,
                                          traits,
                                          Allocator>& s,
               const sub_match<BidirectionalIterator>& m);
   template <class BidirectionalIterator, class traits, class Allocator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>
      :cpp:func:`operator +` (const sub_match<BidirectionalIterator>& m,
                  const std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type,
                                          traits,
                                          Allocator>& s);
   template <class BidirectionalIterator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type>
      :cpp:func:`operator +` (typename iterator_traits<BidirectionalIterator>::value_type const* s,
                  const sub_match<BidirectionalIterator>& m);
   template <class BidirectionalIterator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type>
      :cpp:func:`operator +` (const sub_match<BidirectionalIterator>& m,
                  typename iterator_traits<BidirectionalIterator>::value_type const * s);
   template <class BidirectionalIterator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type>
      :cpp:func:`operator +` (typename iterator_traits<BidirectionalIterator>::value_type const& s,
                  const sub_match<BidirectionalIterator>& m);
   template <class BidirectionalIterator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type>
      :cpp:func:`operator +` (const sub_match<BidirectionalIterator>& m,
                  typename iterator_traits<BidirectionalIterator>::value_type const& s);
   template <class BidirectionalIterator>
   std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type>
      :cpp:func:`operator +` (const sub_match<BidirectionalIterator>& m1,
                  const sub_match<BidirectionalIterator>& m2);

   //
   // ストリーム挿入子：
   //
   template <class charT, class traits, class BidirectionalIterator>
   basic_ostream<charT, traits>&
      :cpp:func:`operator <<` (basic_ostream<charT, traits>& os,
                   const sub_match<BidirectionalIterator>& m);

   } // namespace boost


.. _ref.sub_match.description:

説明
----

.. _ref.sub_match.members:

メンバ
^^^^^^

.. cpp:type:: typename std::iterator_traits<iterator>::value_type value_type

   イテレータが指す型。


.. cpp:type:: typename std::iterator_traits<iterator>::difference_type difference_type

   2 つのイテレータの差を表す型。


.. cpp:type:: BidirectionalIterator iterator

   イテレータ型。


.. cpp:member:: iterator first

   マッチの先頭位置を示すイテレータ。


.. cpp:member:: iterator second

   マッチの終端位置を示すイテレータ。


.. cpp:member:: bool matched

   この部分式がマッチしているかを示す論理値。


.. cpp:function:: difference_type length() const

   :効果: マッチした部分式の長さを返す。この部分式がマッチしなかった場合は 0 を返す。:code:`matched ?`\ :cpp:expr:`distance(first, second)`\ :code:`: 0` と同じ。


.. cpp:function:: operator basic_string<value_type>() const

   :効果: :cpp:expr:`*this` を文字列に変換する。:code:`(matched ?`\ :cpp:expr:`basic_string<value_type>(first, second)`\ :code:`:`\ :cpp:expr:`basic_string<value_type>()`\ :code:`)` を返す。


.. cpp:function:: basic_string str() const

   :効果: :cpp:expr:`*this` の文字列表現を返す。:code:`(matched ?`\ :cpp:expr:`basic_string<value_type>(first, second)`\ :code:`:`\ :cpp:expr:`basic_string<value_type>()`\ :code:`)` と同じ。


.. cpp:function:: int compare(const sub_match& s) const

   :効果: :cpp:expr:`*this` と :cpp:var:`!d` と字句的比較を行う。:cpp:expr:`str().compare(s.str())` を返す。


.. cpp:function:: int compare(const basic_string<value_type>& s) const

   :効果: :cpp:expr:`*this` と文字列 :cpp:var:`!s` を比較する。:cpp:expr:`str().compare(s)` を返す。


.. cpp:function:: int compare(const value_type* s) const

   :効果: :cpp:expr:`*this` と null 終端文字列 :cpp:var:`!s` を比較する。:cpp:expr:`str().compare(s)` を返す。


.. cpp:type:: implementation_private capture_sequence_type

   :効果: 標準ライブラリ Sequence の要件（21.1.1 および表 68 の操作）を満たす実装固有の型を定義する。その :cpp:type:`!value_type` は :cpp:class:`!sub_match<BidirectionalIterator>` である。この型が :cpp:class:`!std::vector<sub_match<BidirectionalIterator> >` となる可能性もあるが、それに依存すべきではない。


.. cpp:function:: const capture_sequence_type& captures() const

   :効果: この部分式に対するすべての捕捉を格納したシーケンスを返す。
   :事前条件: :c:macro:`BOOST_REGEX_MATCH_EXTRA` を使ってライブラリをビルドしていなければ、このメンバ関数は定義されない。また正規表現マッチ関数（:cpp:func:`regex_match` 、:cpp:func:`regex_search` 、:cpp:class:`regex_iterator` 、:cpp:class:`regex_token_iterator`）にフラグ :cpp:var:`!match_extra` を渡していなければ、有用な情報を返さない。
   :根拠: この機能を有効にするといくつか影響がある。

	  * :cpp:class:`!sub_match` がより多くのメモリを占有し、複雑な正規表現をマッチする場合にすぐにメモリやスタック空間の不足に陥る。
	  * :cpp:var:`!match_extra` を使用しない場合であっても、処理する機能（例えば独立部分式）によってはマッチアルゴリズムの効率が落ちる。
	  * :cpp:var:`!match_extra` を使用するとさらに効率が落ちる（速度が低下する）。ほとんどの場合、さらに必要なメモリ割り当てが起こる。


.. cpp:namespace-pop::


.. _ref.sub_match.sub_match_non_member_operators:

sub_match 非メンバ演算子
^^^^^^^^^^^^^^^^^^^^^^^^

.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator ==(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) == 0` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator !=(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) != 0` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <<(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) < 0` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <=(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) <= 0` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >=(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) >= 0` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >(const sub_match<BidirectionalIterator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs.compare(rhs) > 0` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator ==(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs == rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator !=(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs != rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator <<(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs < rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator >(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs > rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator >=(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs >= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator <=(const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs <= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator ==(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() == rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator !=(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() != rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator <(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() < rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator >(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() > rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator >=(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() >= rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  bool operator <=(const sub_match<BidirectionalIterator>& lhs, const std::basic_string<iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& rhs)

   :効果: :cpp:expr:`lhs.str() <= rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator ==(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs == rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator !=(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs != rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs < rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs > rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >=(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs >= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <=(typename iterator_traits<BidirectionalIterator>::value_type const* lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs <= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator ==(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() == rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator !=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() != rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() < rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() > rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() >= rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const* rhs)

   :効果: :cpp:expr:`lhs.str() <= rhs` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator ==(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs == rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator !=(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs != rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs < rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs > rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >=(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs >= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <=(typename iterator_traits<BidirectionalIterator>::value_type const& lhs, const sub_match<BidirectionalIterator>& rhs)

   :効果: :cpp:expr:`lhs <= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator ==(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs == rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator !=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs != rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <<(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs < rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs > rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator >=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs >= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  bool operator <=(const sub_match<BidirectionalIterator>& lhs, typename iterator_traits<BidirectionalIterator>::value_type const& rhs)

   :効果: :cpp:expr:`lhs <= rhs.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator> operator +(const std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& s, const sub_match<BidirectionalIterator>& m)

   :cpp:class:`sub_match` の加算演算子により、:cpp:class:`!basic_string` に追加可能な型に対して :cpp:class:`sub_match` を追加することができ、結果として新しい文字列を得る。

   :効果: :cpp:expr:`s + m.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator, class traits, class Allocator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator> operator +(const sub_match<BidirectionalIterator>& m, const std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type, traits, Allocator>& s)

   :効果: :cpp:expr:`m.str() + s` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type> operator +(typename iterator_traits<BidirectionalIterator>::value_type const* s, const sub_match<BidirectionalIterator>& m)

   :効果: :cpp:expr:`s + m.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type> operator +(const sub_match<BidirectionalIterator>& m, typename iterator_traits<BidirectionalIterator>::value_type const* s)

   :効果: :cpp:expr:`m.str() + s` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type> operator +(typename iterator_traits<BidirectionalIterator>::value_type const& s, const sub_match<BidirectionalIterator>& m)

   :効果: :cpp:expr:`s + m.str()` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type> operator +(const sub_match<BidirectionalIterator>& m, typename iterator_traits<BidirectionalIterator>::value_type const& s)

   :効果: :cpp:expr:`m.str() + s` を返す。


.. cpp:function:: template <class BidirectionalIterator> \
		  std::basic_string<typename iterator_traits<BidirectionalIterator>::value_type> operator +(const sub_match<BidirectionalIterator>& m1, const sub_match<BidirectionalIterator>& m2)

   :効果: :cpp:expr:`m1.str() + m2.str()` を返す。


.. _ref.sub_match.stream_inserter:

ストリーム挿入子
^^^^^^^^^^^^^^^^

.. cpp:function:: template <class charT, class traits, class BidirectionalIterator> \
		  basic_ostream<charT, traits>& operator << (basic_ostream<charT, traits>& os, const sub_match<BidirectionalIterator>& m)

   :効果: :cpp:expr:`(os << m.str())` を返す。
