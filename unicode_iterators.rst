.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


Unicode イテレータ
==================

.. _ref.internals.uni_iter.synopsis:

概要
----

::

   #include <boost/regex/pending/unicode_iterator.hpp>

.. cpp:class:: template <class BaseIterator, class U16Type = ::boost::uint16_t> \
	       u32_to_u16_iterator
	       template <class BaseIterator, class U32Type = ::boost::uint32_t> \
	       u16_to_u32_iterator
	       template <class BaseIterator, class U8Type = ::boost::uint8_t> \
	       u32_to_u8_iterator
	       template <class BaseIterator, class U32Type = ::boost::uint16_t> \
	       u8_to_u32_iterator
	       template <class BaseIterator> \
	       utf16_output_iterator
	       template <class BaseIterator> \
	       utf8_output_iterator


.. _ref.internals.uni_iter.description:

説明
----

このヘッダに含まれるのは、あるエンコーディングの文字シーケンスを別のエンコーディングの読み取り専用文字シーケンス「のように見せる」イテレータアダプタ群である。 ::

   template <class BaseIterator, class U16Type = ::boost::uint16_t>
   class u32_to_u16_iterator
   {
      u32_to_u16_iterator();
      u32_to_u16_iterator(BaseIterator start_position);

      // 他の標準双方向イテレータのメンバが続く...
   };

UTF-32 文字シーケンスを（読み取り専用の）UTF-16 文字シーケンスに変換する双方向イテレータアダプタである。UTF-16 文字はプラットフォーム標準のバイト順で符号化する。 ::

   template <class BaseIterator, class U32Type = ::boost::uint32_t>
   class u16_to_u32_iterator
   {
      u16_to_u32_iterator();
      u16_to_u32_iterator(BaseIterator start_position);
      u16_to_u32_iterator(BaseIterator start_position, BaseIterator start_range, BaseIterator end_range);

      // 他の標準双方向イテレータのメンバが続く...
   };

UTF-16 文字シーケンス（バイト順はプラットフォーム標準）を（読み取り専用の）UTF-32 文字シーケンスに変換する双方向イテレータアダプタである。

このクラスの 3 引数のコンストラクタは、元のシーケンスの開始・終了とともに走査開始位置を取る。このコンストラクタは元のシーケンスの終端が正しく符号化されているか確認する。これにより、元の範囲の終端に不正な UTF-16 コードシーケンスがあった場合にシーケンスの終端を越えて誤って前進・後退するのを防止する。 ::

   template <class BaseIterator, class U8Type = ::boost::uint8_t>
   class u32_to_u8_iterator
   {
      u32_to_u8_iterator();
      u32_to_u8_iterator(BaseIterator start_position);

      // 他の標準双方向イテレータのメンバが続く...
   };

UTF-32 文字シーケンスを（読み取り専用の）UTF-8 文字シーケンスに変換する双方向イテレータアダプタである。 ::

   template <class BaseIterator, class U32Type = ::boost::uint32_t>
   class u8_to_u32_iterator
   {
      u8_to_u32_iterator();
      u8_to_u32_iterator(BaseIterator start_position);
      u8_to_u32_iterator(BaseIterator start_position, BaseIterator start_range, BaseIterator end_range);

      // 他の標準双方向イテレータのメンバが続く...
   };

UTF-8 文字シーケンスを（読み取り専用の）UTF-32 文字シーケンスに変換する双方向イテレータアダプタである。

このクラスの 3 引数のコンストラクタは、元のシーケンスの開始・終了とともに走査開始位置を取る。このコンストラクタは元のシーケンスの終端が正しく符号化されているか確認する。これにより、元の範囲の終端に不正な UTF-8 コードシーケンスがあった場合にシーケンスの終端を越えて誤って前進・後退するのを防止する。 ::

   template <class BaseIterator>
   class utf16_output_iterator
   {
      utf16_output_iterator(const BaseIterator& b);
      utf16_output_iterator(const utf16_output_iterator& that);
      utf16_output_iterator& operator=(const utf16_output_iterator& that);

      // 他の標準出力イテレータのメンバが続く...
   };

UTF-32 値を入力として受け取り、:cpp:type:`!BaseIterator` :cpp:var:`!b` にUTF-16 で出力する単純な出力イテレータアダプタである。UTF-32 および UTF-16 値はプラットフォーム標準のバイト順でなければならない。 ::

   template <class BaseIterator>
   class utf8_output_iterator
   {
      utf8_output_iterator(const BaseIterator& b);
      utf8_output_iterator(const utf8_output_iterator& that);
      utf8_output_iterator& operator=(const utf8_output_iterator& that);

      // 他の標準出力イテレータのメンバが続く...
   };

UTF-32 値を入力として受け取り、:cpp:type:`!BaseIterator` :cpp:var:`!b` に UTF-8 で出力する単純な出力イテレータアダプタである。UTF-32 入力値はプラットフォーム標準のバイト順でなければならない。
