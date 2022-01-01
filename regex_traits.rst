.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_traits
============

.. cpp:struct:: template <class charT, class implementationT = sensible_default_choice> \
		regex_traits : public implementationT


.. _ref.regex_traits.synopsis:

::

   namespace boost{

   template <class charT, class implementationT = sensible_default_choice>
   struct regex_traits : public implementationT
   {
      regex_traits() : implementationT() {}
   };

   template <class charT>
   struct c_regex_traits;

   template <class charT>
   class cpp_regex_traits;

   template <class charT>
   class w32_regex_traits;

   } // namespace boost


.. _ref.regex_traits.description:

説明
----

:cpp:class:`!regex_traits` クラスは以下のいずれかである、実装クラスの薄いラッパである。

* :cpp:class:`!c_regex_traits`：このクラスは非推奨である。C ロカールをラップし、Win32 以外のプラットフォームで C++ ロカールが利用不能な場合に使用される。
* :cpp:class:`!cpp_regex_traits`：非 Win32 プラットフォームにおける既定の特性クラスである。正規表現クラスのロカールを変更するのに :cpp:class:`!std::locale` インスタンスが使用可能である。
* :cpp:class:`!w32_regex_traits`：Win32 プラットフォームにおける既定の特性クラスである。正規表現クラスのロカールを変更するのに LCID が使用可能である。

既定の動作は `boost/regex/user.hpp <http://www.boost.org/doc/libs/boost/regex/user.hpp>`_ にある以下の設定マクロのいずれかを定義することで変更可能である。

* :c:macro:`!BOOST_REGEX_USE_C_LOCALE`：:cpp:class:`!c_regex_traits` が既定となる。
* :c:macro:`!BOOST_REGEX_USE_CPP_LOCALE`：:cpp:class:`!cpp_regex_traits` が既定となる。

これらの特性クラスは\ :doc:`特性クラスの要件 <concepts>`\を満たす。
