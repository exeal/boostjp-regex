.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

.. _format:

検索・置換書式化文字列の構文
----------------------------

.. toctree::
   :titlesonly:

   format_sed_syntax
   format_perl_syntax
   format_boost_syntax

書式化文字列は、アルゴリズム :cpp:func:`regex_replace` および :cpp:func:`match_results::format` で文字列を変換するのに使用する。

書式化文字列には :doc:`sed <format_sed_syntax>` 、:doc:`Perl <format_perl_syntax>` および :doc:`Boost 拡張 <format_boost_syntax>`\の 3 種類がある。

これとは別に、上に挙げた関数にフラグ :cpp:var:`!format_literal` を渡すと書式化文字列は直値文字列として扱われ、出力にそのままコピーされる。
