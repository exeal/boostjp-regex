.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

テストとサンプルプログラム
==========================

.. _background.examples.test_programs:

テストプログラム
----------------

regress：
   退行テストアプリケーションはマッチ・検索アルゴリズムを完全にテストする。このプログラムが存在することにより、ライブラリが要求どおりに動作する（少なくともテストにある項目はテストされている）ことが保証される。未テストの項目を発見された方がおられたら、よろこんで拝聴するしだいである。

   ファイル：

      * `main.cpp <http://www.boost.org/libs/regex/test/regress/main.cpp>`_
      * `basic_tests.cpp <http://www.boost.org/libs/regex/test/regress/basic_tests.cpp>`_
      * `test_alt.cpp <http://www.boost.org/libs/regex/test/regress/test_alt.cpp>`_
      * `test_anchors.cpp <http://www.boost.org/libs/regex/test/regress/test_anchors.cpp>`_
      * `test_asserts.cpp <http://www.boost.org/libs/regex/test/regress/test_asserts.cpp>`_
      * `test_backrefs.cpp <http://www.boost.org/libs/regex/test/regress/test_backrefs.cpp>`_
      * `test_deprecated.cpp <http://www.boost.org/libs/regex/test/regress/test_deprecated.cpp>`_
      * `test_emacs.cpp <http://www.boost.org/libs/regex/test/regress/test_emacs.cpp>`_
      * `test_escapes.cpp <http://www.boost.org/libs/regex/test/regress/test_escapes.cpp>`_
      * `test_grep.cpp <http://www.boost.org/libs/regex/test/regress/test_grep.cpp>`_
      * `test_icu.cpp <http://www.boost.org/libs/regex/test/regress/test_icu.cpp>`_
      * `test_locale.cpp <http://www.boost.org/libs/regex/test/regress/test_locale.cpp>`_
      * `test_mfc.cpp <http://www.boost.org/libs/regex/test/regress/test_mfc.cpp>`_
      * `test_non_greedy_repeats.cpp <http://www.boost.org/libs/regex/test/regress/test_non_greedy_repeats.cpp>`_
      * `test_operators.cpp <http://www.boost.org/libs/regex/test/regress/test_operators.cpp>`_
      * `test_overloads.cpp <http://www.boost.org/libs/regex/test/regress/test_overloads.cpp>`_
      * `test_perl_ex.cpp <http://www.boost.org/libs/regex/test/regress/test_perl_ex.cpp>`_
      * `test_replace.cpp <http://www.boost.org/libs/regex/test/regress/test_replace.cpp>`_
      * `test_sets.cpp <http://www.boost.org/libs/regex/test/regress/test_sets.cpp>`_
      * `test_simple_repeats.cpp <http://www.boost.org/libs/regex/test/regress/test_simple_repeats.cpp>`_
      * `test_tricky_case.cpp <http://www.boost.org/libs/regex/test/regress/test_tricky_cases.cpp>`_
      * `test_unicode.cpp <http://www.boost.org/libs/regex/test/regress/test_unicode.cpp>`_

bad_expression_test：
   「不正な」正規表現により無限ループが発生せず、例外が投げられることを検証する。

   ファイル：`bad_expression_test.cpp <http://www.boost.org/libs/regex/test/pathology/bad_expression_test.cpp>`_

recursion_test：
   （正規表現が何であるかに関わらず）スタックオーバーランを起こさないことを検証する。

   ファイル：`recursion_test.cpp <http://www.boost.org/libs/regex/test/pathology/recursion_test.cpp>`_

concepts：
   ライブラリがドキュメントにあるコンセプトをすべて満たしているか検証する（コンパイルのみのテスト）。

   ファイル：`concept_check.cpp <http://www.boost.org/libs/regex/test/pathology/concept_check.cpp>`_

capture_test：
   捕捉をテストするコード。

   ファイル：`capture_test.cpp <http://www.boost.org/libs/regex/test/pathology/capture_test.cpp>`_


.. _background.examples.example_programs:

サンプルプログラム
------------------

grep
   簡単な :program:`grep` の実装。:option:`!-h` コマンドラインオプションを付けて走らせると使用法が表示される。

   ファイル：`grep.cpp <http://www.boost.org/libs/regex/example/grep/grep.cpp>`_

timer.exe
   簡単な対話式の正規表現マッチアプリケーション。結果はすべて計時される。効率が問題となる場合に、プログラマはこのプログラムを使って正規表現の最適化を行うことができる。

   ファイル：`regex_timer.cpp <http://www.boost.org/libs/regex/example/grep/regex_timer.cpp>`_


.. _background.examples.code_snippets:

コード片
--------

コード片の例は本ドキュメントで使用したコード例である。

* `captures_example.cpp <http://www.boost.org/libs/regex/example/snippets/captures_example.cpp>`_\：捕捉のデモンストレーション。
* `credit_card_example.cpp <http://www.boost.org/libs/regex/example/snippets/credit_card_example.cpp>`_\：クレジットカード番号の書式化コード。
* `partial_regex_grep.cpp <http://www.boost.org/libs/regex/example/snippets/partial_regex_grep.cpp>`_\：部分マッチを使った検索の例。
* `partial_regex_match.cpp <http://www.boost.org/libs/regex/example/snippets/partial_regex_match.cpp>`_\：:cpp:func:`!regex_match` で部分マッチを使った例。
* `regex_iterator_example.cpp <http://www.boost.org/libs/regex/example/snippets/regex_iterator_example.cpp>`_\：マッチの一連を反復する。
* `regex_match_example.cpp <http://www.boost.org/libs/regex/example/snippets/regex_match_example.cpp>`_\：FTP を題材にした :cpp:func:`!regex_match` の例。
* `regex_merge_example.cpp <http://www.boost.org/libs/regex/example/snippets/regex_merge_example.cpp>`_\：:cpp:func:`!regex_merge` の例。C++ ファイルを、構文強調した HTML に変換する。 [#]_
* `regex_replace_example.cpp <http://www.boost.org/libs/regex/example/snippets/regex_replace_example.cpp>`_\：:cpp:func:`!regex_replace` の例。C++ ファイルを、構文強調した HTML に変換する。
* `regex_search_example.cpp <http://www.boost.org/libs/regex/example/snippets/regex_search_example.cpp>`_\：:cpp:func:`!regex_search` の例。cpp ファイルからクラス定義を検索する。
* `regex_token_iterator_eg_1.cpp <http://www.boost.org/libs/regex/example/snippets/regex_token_iterator_eg_1.cpp>`_\：文字列をトークン列に分割する。
* `regex_token_iterator_eg_2.cpp <http://www.boost.org/libs/regex/example/snippets/regex_token_iterator_eg_2.cpp>`_\：HTML ファイル内の URL リンクを列挙する。

以下は非推奨である。

* `regex_grep_example_1.cpp <http://www.boost.org/libs/regex/example/snippets/regex_grep_example_1.cpp>`_\：cpp ファイルからクラス定義を検索する。
* `regex_grep_example_2.cpp <http://www.boost.org/libs/regex/example/snippets/regex_grep_example_2.cpp>`_\：cpp ファイルからクラス定義を検索する。グローバルなコールバック関数を使用している。
* `regex_grep_example_3.cpp <http://www.boost.org/libs/regex/example/snippets/regex_grep_example_3.cpp>`_\：cpp ファイルからクラス定義を検索する。束縛したメンバ関数のコールバックを使用している。
* `regex_grep_example_4.cpp <http://www.boost.org/libs/regex/example/snippets/regex_grep_example_4.cpp>`_\：cpp ファイルからクラス定義を検索する。C++ Builder のクロージャをコールバックに使用している。
* `regex_split_example_1.cpp <http://www.boost.org/libs/regex/example/snippets/regex_split_example_1.cpp>`_\：文字列をトークンに分割する。
* `regex_split_example_2.cpp <http://www.boost.org/libs/regex/example/snippets/regex_split_example_2.cpp>`_\：リンクした URL を分割する。


.. [#] 訳注　:cpp:func:`!regex_merge` は非推奨機能の 1 つです。本文書（日本語訳）には記述はありません。
