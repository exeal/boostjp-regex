.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

.. _format.perl_format:

Perl 書式化文字列の構文
=======================

Perl スタイルの書式化文字列は、プレースホルダーおよびエスケープシーケンスを開始する ‘$’ および ‘\\’ 以外のすべての文字を直値として扱う。

プレースホルダーシーケンスは、正規表現に対するマッチのどの部分を出力に送るかを指定する。

.. list-table::
   :header-rows: 1

   * - プレースホルダー
     - 意味
   * - :regex-format:`$&`
     - 正規表現全体にマッチした部分を出力する。
   * - :regex-format:`$MATCH`
     - :regex-format:`$&` と同じ。
   * - :regex-format:`${^MATCH}`
     - :regex-format:`$&` と同じ。
   * - :regex-format:`$``
     - 最後に見つかったマッチの終端（前回のマッチが存在しない場合はテキストの先頭）から現在のマッチの先頭までのテキストを出力する。
   * - :regex-format:`$PREMATCH`
     - :regex-format:`$`` と同じ。
   * - :regex-format:`${^PREMATCH}`
     - :regex-format:`$`` と同じ。
   * - :regex-format:`$'`
     - 現在のマッチの終端より後方のすべてのテキストを出力する。
   * - :regex-format:`$POSTMATCH`
     - :regex-format:`$'` と同じ。
   * - :regex-format:`${^POSTMATCH}`
     - :regex-format:`$'` と同じ。
   * - :regex-format:`$+`
     - 正規表現中の最後のマーク済み部分式にマッチした部分を出力する。
   * - :regex-format:`$LAST_PAREN_MATCH`
     - :regex-format:`$+` と同じ。
   * - :regex-format:`$LAST_SUBMATCH_RESULT`
     - 最後の部分式に実際にマッチした部分を出力する。
   * - :regex-format:`$^N`
     - :regex-format:`$LAST_SUBMATCH_RESULT` と同じ。
   * - :regex-format:`$$`
     - 直値の ‘$’ を出力する。
   * - :regex-format:`$n`
     - :samp:`{n}` 番目の部分式にマッチした部分を出力する。
   * - :regex-format:`${n}`
     - :samp:`{n}` 番目の部分式にマッチした部分を出力する。
   * - :regex-format:`$+{NAME}`
     - “NAME” という名前の部分式にマッチした部分を出力する。

上に挙げなかった $ プレースホルダーはすべて直値の ‘$’ として扱われる。

エスケープ文字の直後に文字 :samp:`{x}` が続いている場合、:samp:`{x}` が以下のエスケープシーケンス以外であればその文字を出力する。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 意味
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
   * - :regex-format:`\\l`
     - 次に出力する 1 文字を小文字で出力する。
   * - :regex-format:`\\u`
     - 次に出力する 1 文字を大文字で出力する。
   * - :regex-format:`\\L`
     - 以降 :regex-format:`\\E` が現れるまで、出力する文字をすべて小文字にする。
   * - :regex-format:`\\U`
     - 以降 :regex-format:`\\E` が現れるまで、出力する文字をすべて大文字にする。
   * - :regex-format:`\\E`
     - :regex-format:`\\L` および :regex-format:`\\U` シーケンスを終了する。
