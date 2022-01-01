.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

効率
====

Boost.Regex の再帰モードと非再帰モード両方の効率については、他の幅広い正規表現ライブラリと比較されてしかるべきである。再帰モードは少しばかり高速（主にメモリ割り当てにスレッドの同期が必要な場合）だが、あまり大きな差は無い。以下のページで 2 種類のコンパイラを用いて数種類の正規表現ライブラリと比較を行っている。

* `単純な最左最長マッチのテスト（platform = linux, compiler = GNU C++ version 6.3.0） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id1378460593.html>`_
* `Perl 検索のテスト（platform = linux, compiler = GNU C++ version 6.3.0） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id1675827111.html>`_
* `単純な最左最長マッチのテスト（platform = Windows x64, compiler = Microsoft Visual C++ version 14.1） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id3141719723.html>`_
* `最左最長検索のテスト（platform = Windows x64, compiler = Microsoft Visual C++ version 14.1） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id3258595385.html>`_
* `単純な Perl マッチのテスト（platform = linux, compiler = GNU C++ version 6.3.0） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id3261825021.html>`_
* `Perl 検索のテスト（platform = Windows x64, compiler = Microsoft Visual C++ version 14.1） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id3752650613.html>`_
* `単純な Perl マッチのテスト（platform = Windows x64, compiler = Microsoft Visual C++ version 14.1） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id4128344975.html>`_
* `最左最長検索のテスト（platform = linux, compiler = GNU C++ version 6.3.0） <http://www.boost.org/libs/regex/doc/html/boost_regex/background/performance/section_id4148872883.html>`_

.. xpressive の翻訳版ではリンク先も翻訳して含めた。
