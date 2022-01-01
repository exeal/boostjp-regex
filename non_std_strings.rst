.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

非標準文字列型に対するインターフェイス
======================================

.. toctree::
   :maxdepth: 2
   :titlesonly:

   icu_strings
   mfc_strings


Boost.Regex のアルゴリズムおよびイテレータはすべてイテレータベースである。また、標準ライブラリの文字列型を内部でイテレータの組に変換する、便利なアルゴリズムの多重定義を提供する。標準以外の文字列型に対して検索を行うのであれば、その文字列をイテレータの組に変換すればよい。イテレータベースでないとされているものも含めて、私はこの方法で処理不能な文字列型を見たことがない。もちろん、内部バッファと長さへのアクセスを提供する文字列型は、すべて（イテレータの組として使用可能である）ポインタの組に変換可能である。

標準以外の文字列型の中でも、広く使われているため既にラッパが用意されているものがある。現在は ICU と MFC の文字列クラス型にラッパがある。
