.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

正規表現の構文
==============

.. toctree::
   :titlesonly:

   syntax_perl
   syntax_extended
   syntax_basic
   character_class_names
   collating_names
   leftmost_longest

本節では本ライブラリで使用可能な正規表現構文について述べる。本文書はプログラミングガイドであり、あなたのプログラムのユーザが触れる実際の構文は正規表現をコンパイルするときに用いるフラグによる。

正規表現オブジェクトの構築方法により、主に 3 つの構文が用意されている。

* :doc:`Perl（既定の動作）。 <syntax_perl>`
* :doc:`POSIX 拡張（egrep および awk の変種も含む）。 <syntax_extended>`
* :doc:`POSIX 基本（grep および emacs の変種も含む）。 <syntax_basic>`

すべての文字を直値（リテラル）として扱う正規表現も構築可能である（実際のところ「構文」とはいえないが）。
