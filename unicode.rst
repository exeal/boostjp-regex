.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

Unicode と Boost.Regex
======================

Boost.Regex で Unicode 文字列を使う方法は 2 つある。


.. _unicode.rely_on_wchar_t:

wchar_t への依存
----------------

プラットフォームの :cpp:type:`!wchar_t` 型が Unicode 文字列を保持でき、かつプラットフォームの C/C++ 実行時ライブラリがワイド文字定数（が :cpp:func:`!std::iswspace` や :cpp:func:`!std::iswlower` に渡されるなどのケース）を正しく処理できるのであれば、:cpp:type:`boost::wregex` を使った Unicode 処理が可能である。しかしながら、このアプローチにはいくつか不便がある。

* 移植性がない。:cpp:type:`!wchar_t` の幅や実行時ライブラリがワイド文字を Unicode として扱うかどうかについては何の保証もない。ほとんどの Windows コンパイラは保証しているが、多くの Unix システムではそうではない。
* Unicode 固有の文字クラスはサポートされない（:regexp:`[[:Nd:]]` 、:regexp:`[[:Po:]]` など）。
* ワイド文字シーケンスで符号化された文字列しか検索できない。UTF-8 や UTF-16 でさえも多くのプラットフォームで検索できない。


.. _unicode.use_a_unicode_aware_regular_expression_type_:

Unicode 対応の正規表現型の使用
------------------------------

`ICU ライブラリ <http://www.ibm.com/software/globalization/icu/>`_\があれば :ref:`Boost.Regex から利用できるように設定 <install.building_with_unicode_and_icu_su>`\できる。これにより Unicode 固有の文字プロパティや、UTF-8 、UTF-16 、および UTF-32 で符号化された文字列の検索をサポートする特別な正規表現型（:cpp:type:`boost::u32regex`）が提供される。:doc:`ICU 文字列クラスのサポート <icu_strings>`\を見よ。
