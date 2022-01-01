.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


POSIX 互換 C API
================

.. note::

   本リファレンスは POSIX API 関数の要約である。これらは（C++ 以外の言語によるアクセスが必要でない限り）新しいコードで使用する API ではなく、他のライブラリとの互換性のために提供されている。これらの関数が使用している名前は実際の関数名に展開されるマクロであるため、他のバージョンとの共存が可能である。

::

   #include <boost/cregex.hpp>

あるいは ::

   #include <boost/regex.h>

以下の関数は POSIX 互換の C ライブラリが必要なユーザ向けである。Unicode 版とナロー文字版の両方が利用可能であり、標準 POSIX API 名は :c:macro:`!UNICODE` が定義されているかどうかでいずれかの版に展開されるマクロである。

.. important::

   ここで定義するシンボルは、C++ プログラムではすべて名前空間 :cpp:member:`!boost` 内にあることに注意していただきたい。:code:`#include <boost/regex.h>` を使用した場合はシンボルが名前空間 :cpp:member:`!boost` 内で定義されるのは変わらないが、大域名前空間でも利用できるようになっている。

関数の定義は以下のとおりである。 ::

   extern "C" {

   struct regex_tA;
   struct regex_tW;

   int regcompA(regex_tA*, const char*, int);
   unsigned int regerrorA(int, const regex_tA*, char*, unsigned int);
   int regexecA(const regex_tA*, const char*, unsigned int, regmatch_t*, int);
   void regfreeA(regex_tA*);

   int regcompW(regex_tW*, const wchar_t*, int);
   unsigned int regerrorW(int, const regex_tW*, wchar_t*, unsigned int);
   int regexecW(const regex_tW*, const wchar_t*, unsigned int, regmatch_t*, int);
   void regfreeW(regex_tW*);

   #ifdef UNICODE
   #define regcomp regcompW
   #define regerror regerrorW
   #define regexec regexecW
   #define regfree regfreeW
   #define regex_t regex_tW
   #else
   #define regcomp regcompA
   #define regerror regerrorA
   #define regexec regexecA
   #define regfree regfreeA
   #define regex_t regex_tA
   #endif
   }

これらの関数はすべて構造 :cpp:class:`!regex_t` に対して処理を行う。この構造体は次の 2 つの公開メンバを持つ。

.. list-table::
   :header-rows: 1

   * - メンバ
     - 意味
   * - :cpp:type:`!unsigned int` :cpp:member:`~regex_t::re_nsub`
     - :cpp:func:`!regcomp` により値が設定され、正規表現中の部分式の総数を表す。
   * - :cpp:type:`!const TCHAR*` :cpp:member:`~regex_t::re_endp`
     - フラグ :cpp:var:`!REG_PEND` が設定されている場合、コンパイルする正規表現の終端を指す。

.. note::

   :cpp:class:`!regex_t` は実際は :code:`#define` であり、:c:macro:`!UNICODE` が定義されているかどうかにより :cpp:class:`!regex_tA` か :cpp:class:`!regex_tW` のいずれかとなる。:cpp:type:`!TCHAR` はマクロ :c:macro:`!UNICODE` により :cpp:type:`!char` か :cpp:type:`!wchar_t` のいずれかとなる。


.. _ref.posix.regcomp:

regcomp
-------

:cpp:func:`!regcomp` は :cpp:class:`!regex_t` へのポインタ、コンパイルする式へのポインタおよび以下の組み合わせとなるフラグ引数をとる。

.. list-table::
   :header-rows: 1

   * - フラグ
     - 意味
   * - :cpp:var:`!REG_EXTENDED`
     - 現代的な正規表現をコンパイルする。:cpp:expr:`regbase::char_classes | regbase::intervals | regbase::bk_refs` と等価である。
   * - :cpp:var:`!REG_BASIC`
     - 基本的な（旧式の）正規表現構文をコンパイルする。:cpp:expr:`regbase::char_classes | regbase::intervals | regbase::limited_ops | regbase::bk_braces | regbase::bk_parens | regbase::bk_refs` と等価である。
   * - :cpp:var:`!REG_NOSPEC`
     - 文字をすべて通常の文字として扱う。正規表現は直値文字列である。
   * - :cpp:var:`!REG_ICASE`
     - 大文字小文字を区別しないマッチを行う。
   * - :cpp:var:`!REG_NOSUB`
     - このライブラリでは効果なし。
   * - :cpp:var:`!REG_NEWLINE`
     - このフラグを設定した場合、ドットが改行文字にマッチしない。
   * - :cpp:var:`!REG_PEND`
     - このフラグを設定した場合、:cpp:class:`!regex_t` 構造体の :cpp:member:`~regex_t::re_endp` 引数はコンパイルする正規表現の終端を指していなければならない。
   * - :cpp:var:`!REG_NOCOLLATE`
     - このフラグを設定した場合、文字範囲においてロカール依存の照合が無効になる。
   * - :cpp:var:`!REG_ESCAPE_IN_LISTS`
     - このフラグを設定した場合、括弧式（文字集合）内でエスケープシーケンスが使用できる。
   * - :cpp:var:`!REG_NEWLINE_ALT`
     - このフラグを設定した場合、改行文字は選択演算子 :regexp:`|` と等価である。
   * - :cpp:var:`!REG_PERL`
     - Perl 似の正規表現をコンパイルする。
   * - :cpp:var:`!REG_AWK`
     - awk 似動作のショートカット：:cpp:expr:`REG_EXTENDED | REG_ESCAPE_IN_LISTS`
   * - :cpp:var:`!REG_GREP`
     - grep 似動作のショートカット：:cpp:expr:`REG_BASIC | REG_NEWLINE_ALT`
   * - :cpp:var:`!REG_EGREP`
     - egrep 似動作のショートカット：:cpp:expr:`REG_EXTENDED | REG_NEWLINE_ALT`


.. _ref.posix.regerror:

regerror
--------

:cpp:func:`!regerror` は以下の引数をとり、エラーコードを可読性の高い文字列に変換する。

.. list-table::
   :header-rows: 1

   * - 引数
     - 意味
   * - :cpp:type:`!int` :cpp:var:`!code`
     - エラーコード。
   * - :cpp:type:`!const regex_t*` :cpp:var:`!e`
     - 正規表現（null でもよい）。
   * - :cpp:type:`!char*` :cpp:var:`!buf`
     - エラーメッセージを書き込む文字列。
   * - :cpp:type:`!unsigned int` :cpp:var:`!buf_size`
     - :cpp:var:`!buf` の長さ

エラーコードが :cpp:var:`!REG_ITOA` との論理和になっている場合は、結果はメッセージではなく、例えば “REG_BADPAT” のようなコードの印字可能な名前となる。:cpp:var:`!code` が :cpp:var:`!REG_ATOI` の場合は、:cpp:var:`!e` は null であってはならず :cpp:expr:`e->re_endp` は印字可能名の終端を指していなければならない。またこの場合の戻り値はエラーコードの値である。:cpp:var:`!code` の値がこれら以外の場合は、戻り値はエラーメッセージの文字数であり、戻り値が :cpp:var:`!buf_size` 以上であればより大きなバッファを用いて :cpp:func:`!regerror` を再度呼び出す必要がある。


.. _ref.posix.regexec:

regexec
-------

:cpp:func:`!regexec` は文字列 :cpp:var:`!buf` 内から式 :cpp:var:`!e` の最初のマッチを検索する。:cpp:var:`!len` が 0 以外の場合は、:cpp:expr:`*m` には正規表現にマッチした内容が書き込まれる。:cpp:expr:`m[0]` はマッチした文字列全体、:cpp:expr:`m[1]` は 1 番目の部分式、:cpp:expr:`m[2]` は 2 番目などとなる。詳細はヘッダファイルの :cpp:class:`!regmatch_t` の宣言を見よ。:cpp:var:`!eflags` 引数は以下の組み合わせである。


.. list-table::
   :header-rows: 1

   * - フラグ
     - 意味
   * - :cpp:var:`!REG_NOTBOL`
     - 引数 :cpp:var:`!buf` が行の先頭ではない。
   * - :cpp:var:`!REG_NOTEOL`
     - 引数 :cpp:var:`!buf` が行末で終了していない。
   * - :cpp:var:`!REG_STARTEND`
     - 検索する文字列は :cpp:expr:`buf + pmatch[0].rm_so` が先頭で、:cpp:expr:`buf + pmatch[0].rm_eo` が終端である。


.. _ref.posix.regfree:

regfree
-------

:cpp:func:`!regfree` は :cpp:func:`!regcomp` が割り当てたメモリをすべて解放する。
