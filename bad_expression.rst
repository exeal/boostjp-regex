.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


bad_expression
==============

.. cpp:class:: regex_error : public std::runtime_error

   :cpp:class:`regex_error` クラスは、正規表現を表す文字列を有限状態マシンに変換する際に発生したエラーを報告するのに投げられる例外オブジェクトの型を定義する。


.. _ref.bad_expression.synopsis:

概要
----

::

   #include <boost/pattern_except.hpp>

   namespace boost{

   class regex_error : public std::runtime_error
   {
   public:
      explicit regex_error(const std::string& s, regex_constants::error_type err, std::ptrdiff_t pos);
      explicit regex_error(boost::regex_constants::error_type err);
      boost::regex_constants::error_type code()const;
      std::ptrdiff_t position()const;
   };

   typedef regex_error bad_pattern; // 後方互換のため
   typedef regex_error bad_expression; // 後方互換のため

   } // namespace boost


.. _ref.bad_expression.description:

説明
----

.. cpp:namespace-push:: regex_error

.. cpp:function:: regex_error(const std::string& s, regex_constants::error_type err, std::ptrdiff_t pos)
		  regex_error(boost::regex_constants::error_type err)

   :効果: :cpp:class:`!regex_error` クラスのオブジェクトを構築する。


.. cpp:function:: boost::regex_constants::error_type code() const

   :効果: 発生した解析エラーを表すエラーコードを返す。


.. cpp:function:: std::ptrdiff_t position() const

   :効果: 解析が停止した正規表現内の位置を返す。


.. cpp:namespace-pop::


.. _ref.bad_expression.footnotes:

補足
----

:cpp:class:`!regex_error` の基本クラスに :cpp:class:`!std::runtime_error` を選択したことについては議論の余地がある。ライブラリの使い方という点では、例外は論理エラー（プログラマが正規表現を与える）、実行時エラー（ユーザが正規表現を与える）のいずれでもよいと考えられる。このライブラリは以前はエラーに :cpp:class:`!bad_pattern` と :cpp:class:`!bad_expression` を使っていたが、`Technical Report on C++ Library Extension <http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1836.pdf>`_ と同期をとるために :cpp:class:`!regex_error` クラスに一本化した。
