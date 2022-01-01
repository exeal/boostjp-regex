.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

POSIX 基本正規表現構文
======================

.. _syntax.basic_syntax.synopsis:

概要
----

POSIX 基本正規表現構文は Unix ユーティリティ :program:`sed` が使用しており、:program:`grep` および :program:`emacs` がその変種を使用している。Boost.Regex で POSIX 基本正規表現を使用するには、コンストラクタにフラグ :cpp:var:`!basic` を渡す（:cpp:type:`syntax_option_type` を見よ）。例えば、 ::

   // e1 は大文字小文字を区別する POSIX 基本正規表現：
   boost::regex e1(my_expression, boost::regex::basic);
   // e2 は大文字小文字を区別しない POSIX 基本正規表現：
   boost::regex e2(my_expression, boost::regex::basic|boost::regex::icase);


.. _syntax.basic_syntax.posix_basic:

POSIX 基本構文
--------------

POSIX 基本正規表現では、以下の特別なものを除くあらゆる文字が文字そのものにマッチする。

.. code-block:: none

   .[\*^$


.. _syntax.basic_syntax.wildcard_:

ワイルドカード
--------------

文字集合外部の :regexp:`.` 1 文字は、以下以外のあらゆる文字 1 文字にマッチする。

* NULL 文字（マッチアルゴリズムにフラグ :cpp:var:`!match_not_dot_null` を渡した場合）。
* 改行文字（マッチアルゴリズムにフラグ :cpp:var:`!match_not_dot_newline` を渡した場合）。


.. _syntax.basic_syntax.anchors_:

アンカー
--------

:regexp:`^` は、正規表現の先頭、あるいは部分式の先頭で使用した場合に行頭にマッチする。

:regexp:`$` は、正規表現の終端、あるいは部分式の終端で使用した場合に行末にマッチする。


.. _syntax.basic_syntax.marked_sub_expressions_:

マーク済み部分式
----------------

開始が :regexp:`\\(` で終了が :regexp:`\\)` の節は部分式として機能する。マッチした部分式はすべてマッチアルゴリズムにより個別のフィールドに分けられる。マーク済み部分式は繰り返しと後方参照により参照が可能である。


.. _syntax.basic_syntax.repeats_:

繰り返し
--------

あらゆるアトム（文字、部分式、文字クラス）は :regexp:`*` 演算子による繰り返しが可能である。

例えば :regexp:`a*` は文字 a の 0 回以上の繰り返しにマッチする（アトムの 0 回の繰り返しは空文字列にマッチする）ため、正規表現 :regexp:`a*b` は以下のいずれにもマッチする。

.. code-block:: none

   b
   ab
   aaaaaaaab

アトムの繰り返しは回数境界指定の繰り返しによっても可能である。

:regexp:`a\\{n\\}` は :regex-input:`a` のちょうど n 回の繰り返しにマッチする。

:regexp:`a\\{n,\\}` は :regex-input:`a` の n 回以上の繰り返しにマッチする。

:regexp:`a\\{n,m\\}` は :regex-input:`a` の n 回以上 m 回以下の繰り返しにマッチする。

例えば

.. code-block:: none

   ^a\{2,3\}$

は、次のいずれにもマッチするが、

.. code-block:: none

   aa
   aaa

次のいずれにもマッチしない。

.. code-block:: none

   a
   aaaa

直前の構造が繰り返し不能な場合に繰り返し演算子を使うとエラーになる。例えば次は

.. code-block:: none

   a\\(*\\)

:regexp:`*` 演算子を適用可能なものがないためエラーとなる。


.. _syntax.basic_syntax.back_references_:

後方参照
--------

エスケープ文字の直後に数字 :samp:`{n}` があると、部分式 :samp:`{n}` にマッチしたものと同じ文字列にマッチする。:samp:`{n}` は 0 から 9 の範囲である。例えば次の正規表現は、

.. code-block:: none

   ^\\(a*\\)[^a]*\\1$

次の文字列にマッチする。

.. code-block:: none

   aaabbaaa

しかし、次の文字列にはマッチしない。

.. code-block:: none

   aaabba


.. _syntax.basic_syntax.character_sets_:

文字集合
--------

文字集合は :regexp:`[` で始まり :regexp:`]` で終わる括弧式であり、文字の集合を定義する。集合に含まれるいずれかの 1 文字にマッチする。

文字集合に含められる要素は以下の組み合わせである。


.. _syntax.basic_syntax.single_characters_:

単一の文字
^^^^^^^^^^

例えば :regexp:`[abc]` は :regex-input:`a` 、:regex-input:`b` 、:regex-input:`c` のいずれか 1 文字にマッチする。


.. _syntax.basic_syntax.character_ranges_:

文字範囲
^^^^^^^^

例えば :regexp:`[a-c]` は ‘a’ から ‘c’ までの範囲の 1 文字にマッチする。POSIX 基本正規表現の既定では、文字 :samp:`{x}` が :samp:`{y}` から :samp:`{z}` の範囲であるとは、文字の照合順がその範囲内にある場合をいう。結果はロカールの影響を受ける。この動作は正規表現構築時に :cpp:var:`!collate` オプションフラグを設定しないことで抑止でき、文字が特定の範囲内にあるかどうかは文字のコードポイントのみで決定する。


.. _syntax.basic_syntax.negation_:

否定
^^^^

括弧式が文字 :regexp:`^` で始まっている場合は、正規表現に含まれる文字の補集合となる。例えば :regexp:`[^a-c]` は範囲 :regexp:`a-c` を除くあらゆる文字にマッチする。


.. _syntax.basic_syntax.character_classes_:

文字クラス
^^^^^^^^^^

:regexp:`[[:name:]]` のような形式の正規表現は名前付き文字クラス「name」にマッチする。例えば :regexp:`[[:lower:]]` はあらゆる小文字にマッチする。:doc:`character_class_names`\を見よ。


.. _syntax.basic_syntax.collating_elements_:

照合要素
--------

:regexp:`[[.col.]]` のような形式の式は照合要素 :samp:`{col}` にマッチする。照合要素とは、単一の照合単位として扱われる文字か文字シーケンスである。照合要素は範囲の端点としても使用できる。例えば :regexp:`[[.ae.]-c]` は文字シーケンス “ae” に加えて、現在の範囲 “ae”-c の文字のいずれかにマッチする。 後者において “ae” は現在のロカールにおける単一の照合要素として扱われる。

照合要素は（通常、文字集合内で使用できない）エスケープの代わりとして使用できる。例えば :regexp:`[[.^.]abc]` は ‘abc^’ のいずれかの 1 文字にマッチする。

この拡張として、照合要素をシンボル名で指定する方法もある。例えば、

.. code-block:: none

   [[.NUL.]]

は NUL 文字にマッチする。\ :doc:`照合要素名 <collating_names>`\を見よ。


.. _syntax.basic_syntax.equivalence_classes_:

等価クラス
^^^^^^^^^^

:regexp:`[[=col=]]` のような形式の正規表現は、第 1 位のソートキーが照合要素 :samp:`{col}` と同じ文字および照合要素にマッチする。照合要素名 :samp:`{col}` は\ :doc:`照合シンボル名 <collating_names>`\でもよい。第 1 位のソートキーでは大文字小文字の違い、アクセント記号の有無、ロカール固有のテーラリング（tailoring）は無視される。よって :regexp:`[[=a=]]` は a 、À 、Á 、Â 、Ã 、Ä 、Å 、A 、à 、á 、â 、ã 、ä および å のいずれにもマッチする。残念ながらこの機能の実装はプラットフォームの照合と地域化のサポートに依存し、すべてのプラットフォームで移植性の高い動作は期待できず、単一のプラットフォームにおいてもすべてのロカールで動作するとは限らない。


.. _syntax.basic_syntax.combinations_:

結合
^^^^

以上の要素はすべて 1 つの文字集合宣言内で結合可能である。例：:regexp:`[[:digit:]a-c[.NUL.]]`


.. _syntax.basic_syntax.escapes:

エスケープ
----------

上で述べた :regexp:`\\{` 、:regexp:`\\}` 、:regexp:`\\(` および :regexp:`\\)` を例外として、エスケープの直後に文字が現れる場合はその文字にマッチする。これにより特殊文字

.. code-block:: none

   .[\*^$

を「通常の」文字にすることができる。エスケープ文字は文字集合内ではその特殊な意味を失うことに注意していただきたい。したがって :regexp:`[\^]` は直値の :regex-input:`\\` か :regex-input:`^` にマッチする。


.. _syntax.basic_syntax.what_gets_matched:

マッチするもの
--------------

正規表現のマッチに複数の、可能な「最良」マッチは最左最長の規則で得られるものである。


.. _syntax.basic_syntax.variations:

バリエーション
--------------

.. _syntax.basic_syntax.grep:

grep
^^^^

grep フラグを設定して正規表現をコンパイルすると、改行区切りの :ref:`POSIX 基本正規表現 <syntax.basic_syntax.posix_basic>`\のリストとして扱われ、リスト内にマッチする正規表現があればマッチとなる。例えば次のコードは、 ::

   boost::regex e("abc\ndef", boost::regex::grep);

:ref:`POSIX 基本正規表現 <syntax.basic_syntax.posix_basic>`\の :regexp:`abc` か :regexp:`def` のいずれかにマッチする。

名前が示すように、この動作は Unix ユーティリティの :program:`grep` に合致する。


.. _syntax.basic_syntax.emacs:

emacs
^^^^^

POSIX 基本機能に加えて以下の文字が特殊である。

.. list-table::
   :header-rows: 1

   * - 文字
     - 説明
   * - :regexp:`+`
     - 直前のアトムの 1 回以上の繰り返し。
   * - :regexp:`?`
     - 直前のアトムの 0 回か 1 回の繰り返し。
   * - :regexp:`*?`
     - :regexp:`*` の貪欲でない版。
   * - :regexp:`+?`
     - :regexp:`+` の貪欲でない版。
   * - :regexp:`??`
     - :regexp:`?` の貪欲でない版。

また、以下のエスケープシーケンスが考慮される。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 説明
   * - :regexp:`\\|`
     - 選択を表す。
   * - :regexp:`\\(?: ... )`
     - マーク付けを行わないグループ構造。余計な部分式を生成することなく、字句的なグループ化が可能である。
   * - :regexp:`\\w`
     - 単語構成文字にマッチする。
   * - :regexp:`\\W`
     - 非単語構成文字にマッチする。
   * - :regexp:`\\sx`
     - 構文グループ :samp:`{x}` に属する文字にマッチする。次の emacs グルーピングをサポートする：‘s’ 、‘ ’ 、‘_’ 、‘w’ 、‘.’ 、‘)’ 、‘(’ 、‘"’ 、‘\’ 、‘>’、‘<’。詳細は emacs のドキュメントを見よ。
   * - :regexp:`\\Sx`
     - 構文グループ :samp:`{x}` に属さない文字にマッチする。
   * - :regexp:`\\c` および :regexp:`\\C`
     - これらはサポートしない。
   * - :regexp:`\\\``
     - バッファ（あるいはマッチ対象テキスト）の先頭 0 文字にのみマッチする。
   * - :regexp:`\\'`
     - バッファ（あるいはマッチ対象テキスト）の終端 0 文字にのみマッチする。
   * - :regexp:`\\b`
     - 単語境界の先頭 0 文字にのみマッチする。
   * - :regexp:`\\B`
     - 非単語境界の先頭 0 文字にのみマッチする。
   * - :regexp:`\\<`
     - 単語の先頭 0 文字にのみマッチする。
   * - :regexp:`\\>`
     - 単語の終端 0 文字にのみマッチする。</td>

最後に emacs スタイルの正規表現マッチは、\ :ref:`Perl の「深さ優先探索」規則 <syntax.perl_syntax.what_gets_matched>`\にしたがうことに注意していただきたい。emacs の正規表現は、\ :doc:`POSIX スタイルの最左最長規則 <leftmost_longest>`\と調和しない Perl ライクの拡張を含むためこのような動作をする。


.. _syntax.basic_syntax.options:

オプション
----------

正規表現構築時に basic および grep オプションとともに指定可能な\ :ref:`フラグが多数ある <ref.syntax_option_type.syntax_option_type_basic>`\。特に :cpp:var:`!collate` 、:cpp:var:`!icase` :ref:`オプション <ref.syntax_option_type.syntax_option_type_basic>`\が大文字小文字の区別やロカール依存の動作を変更するのに対し、:cpp:var:`!newline_alt` 、:cpp:var:`!no_char_classes` 、:cpp:var:`!no_intervals` 、:cpp:var:`!bk_plus_qm` 、:cpp:var:`!bk_plus_vbar` :ref:`オプション <ref.syntax_option_type.syntax_option_type_basic>`\は構文を変更するという点に注意していただきたい。


.. _syntax.basic_syntax.references:

参考
----

`IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Base Definitions and Headers, Section 9, Regular Expressions (FWD.1) <http://www.opengroup.org/onlinepubs/000095399/basedefs/xbd_chap09.html>`_\。

`IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, Utilities, grep (FWD.1) <http://www.opengroup.org/onlinepubs/000095399/utilities/grep.html>`_\。

`Emacs 21.3 <http://www.gnu.org/software/emacs/>`_\。
