.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


スレッド安全性
==============

Boost がスレッド安全であれば、Boost.Regex はスレッド安全である。Boost がスレッド安全モードであるかどうか確認するには、:c:macro:`BOOST_HAS_THREADS` が定義されているか調べるとよい。コンパイラがスレッドのサポートを有効にしていれば、設定システムがこのマクロを自動的に定義する。

:cpp:class:`basic_regex` クラスとその typedef である :cpp:type:`!regex` 、:cpp:type:`wregex` は、コンパイル済み正規表現がスレッド間で安全に共有可能という意味でスレッド安全である。マッチアルゴリズム :cpp:func:`regex_match` 、:cpp:func:`regex_search` および :cpp:func:`regex_replace` はすべて再入可能かつスレッド安全である。:cpp:class:`match_results` クラスは、マッチ結果をあるスレッドから別のスレッドへ安全にコピー（例えばあるスレッドがマッチを検索して :cpp:class:`match_results` インスタンスをキューに追加し、同時に別のスレッドが同じキューをポップすることが）可能という意味では、スレッド安全である。それ以外の場合はスレッドごとに個別の :cpp:class:`match_results` インスタンスを使用しなければならない。

:doc:`POSIX API 関数 <posix_api>`\はすべて再入可能かつスレッド安全であり、:cpp:func:`!regcomp` でコンパイルした正規表現もスレッド間で共有可能である。

:cpp:class:`RegEx` クラスは、各スレッドが :cpp:class:`!RegEx` インスタンスを保持する場合のみスレッド安全である（アパートメントスレッディング）。これは :cpp:class:`!RegEx` が正規表現のコンパイルとマッチの両方を処理するためである。

最後に、大域ロカールを変更するとあらゆるコンパイル済み正規表現が無効になるため、あるスレッドで正規表現を使用しているときに別のスレッドが :cpp:func:`!setlocale` を呼び出すと予期しない結果となることに注意していただきたい。

また :cpp:func:`!main()` の開始前は、実行中のスレッドは 1 つだけでなければならないという要件がある。
