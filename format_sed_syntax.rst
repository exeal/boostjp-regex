.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

.. _format.sed_format:

sed 書式化文字列の構文
======================

sed スタイルの書式化文字列は、以下以外のすべての文字を直値として扱う。

.. list-table::
   :header-rows: 1

   * - 文字
     - 説明
   * - :regex-format:`&`
     - アンパーサンド文字は出力ストリーム中でマッチした正規表現全体に置換される。直値の ‘&’ を出力するには :regex-format:`\\&` を使用する。
   * - :regex-format:`\\`
     - エスケープシーケンスを指定する。

エスケープ文字の直後に文字 :samp:`{x}` が続いている場合、:samp:`{x}` が以下のエスケープシーケンス以外であればその文字を出力する。

.. list-table::
   :header-rows: 1

   * - 文字
     - 説明
   * - :regex-format:`\\a`
     - ベル文字 ‘\\a’ を出力する。
   * - :regex-format:`\\e`
     - ANSI エスケープ文字（コードポイント 27）を出力する。
   * - :regex-format:`\\f`
     - フォームフィード文字 ‘\\f’ を出力する。
   * - :regex-format:`\\n`
     - 改行文字 ‘\\n’ を出力する。
   * - :regex-format:`\\r`
     - 復改文字 ‘\\r’ を出力する。
   * - :regex-format:`\\t`
     - タブ文字 ‘\\t’ を出力する。
   * - :regex-format:`\\v`
     - 垂直タブ文字 ‘\\v’ を出力する。
   * - :regex-format:`\\xDD`
     - 16 進数コードポイントが 0xDD である文字を出力する。
   * - :regex-format:`\\x{DDDD}`
     - 16 進数コードポイントが 0xDDDD である文字を出力する。
   * - :regex-format:`\\cX`
     - ANSI エスケープシーケンス “escape-X” を出力する。
   * - :regex-format:`\\D`
     - :samp:`{D}` が範囲 1-9 の 10 進数字であれば、部分式 :samp:`{D}` にマッチしたテキストを出力する。
