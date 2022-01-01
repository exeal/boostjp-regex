.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


POSIX 拡張正規表現構文
======================

.. _syntax.basic_extended.synopsis:

概要
----

POSIX 拡張正規表現構文は POSIX C 正規表現 API によりサポートされ、:program:`egrep` および :program:`awk` ユーティリティがその変種を使用している。Boost.Regex で POSIX 拡張正規表現を使用するには、コンストラクタにフラグ :cpp:var:`!extended` を渡す。例えば、 ::

   // e1 は大文字小文字を区別する POSIX 拡張正規表現：
   boost::regex e1(my_expression, boost::regex::extended);
   // e2 は大文字小文字を区別しない POSIX 拡張正規表現：
   boost::regex e2(my_expression, boost::regex::extended|boost::regex::icase);


.. _syntax.basic_extended.posix_extended_syntax:

POSIX 拡張構文
--------------

POSIX 拡張正規表現では、以下の特別なものを除くあらゆる文字が文字そのものにマッチする。

.. code-block:: none

   .[{()\*+?|^$


.. _syntax.basic_extended.wildcard_:

ワイルドカード
^^^^^^^^^^^^^^

文字集合外部の :regexp:`.` 1 文字は、以下以外のあらゆる文字 1 文字にマッチする。

* NULL 文字（マッチアルゴリズムにフラグ :cpp:var:`!match_not_dot_null` を渡した場合）。
* 改行文字（マッチアルゴリズムにフラグ :cpp:var:`!match_not_dot_newline` を渡した場合）。


.. _syntax.basic_extended.anchors_:

アンカー
^^^^^^^^

:regexp:`^` は、正規表現の先頭、あるいは部分式の先頭で使用した場合に行頭にマッチする。

:regexp:`$` は、正規表現の終端、あるいは部分式の終端で使用した場合に行末にマッチする。


.. _syntax.basic_extended.marked_sub_expressions_:

マーク済み部分式
^^^^^^^^^^^^^^^^

開始が :regexp:`(` で終了が :regexp:`)` の節は部分式として機能する。マッチした部分式はすべてマッチアルゴリズムにより個別のフィールドに分けられる。マーク済み部分式は繰り返しと後方参照により参照が可能である。


.. _syntax.basic_extended.repeats_:

繰り返し
^^^^^^^^

あらゆるアトム（文字、部分式、文字クラス）は :regexp:`*` 、:regexp:`+` 、:regexp:`?` および :regexp:`{}` 演算子による繰り返しが可能である。

:regexp:`*` 演算子は直前のアトムの **0 回以上の繰り返し**\にマッチする。例えば正規表現 :regexp:`a*b` は以下のいずれにもマッチする。

.. code-block:: none

   b
   ab
   aaaaaaaab

:regexp:`+` 演算子は直前のアトムの **1 回以上の繰り返し**\にマッチする。例えば正規表現 :regexp:`a+b` は以下のいずれにもマッチする。

.. code-block:: none

   ab
   aaaaaaaab

しかし次にはマッチしない。

.. code-block:: none

   b

:regexp:`?` 演算子は直前のアトムの **0 回あるいは 1 回の出現**\にマッチする。例えば正規表現 :regexp:`ca?b` は以下のいずれにもマッチする。

.. code-block:: none

   cb
   cab

しかし次にはマッチしない。

.. code-block:: none

   caab

アトムの繰り返しは回数境界指定の繰り返しによっても可能である。

:regexp:`a{n}` は :regex-input:`a` の\ **ちょうど n 回の繰り返し**\にマッチする。

:regexp:`a{n,}` は :regex-input:`a` の **n 回以上の繰り返し**\にマッチする。

:regexp:`a{n,m}` は :regex-input:`a` の **n 回以上 m 回以下の繰り返し**\にマッチする。

例えば

.. code-block:: none

   ^a{2,3}$

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

   a(*)

:regexp:`*` 演算子を適用可能なものがないためエラーとなる。


.. _syntax.basic_extended.back_references_:

後方参照
^^^^^^^^

エスケープ文字の直後に数字 :samp:`{n}` があると、部分式 :samp:`{n}` にマッチしたものと同じ文字列にマッチする。:samp:`{n}` は 0 から 9 の範囲である。例えば次の正規表現は、

.. code-block:: none

   ^(a*)[^a]*\1$

次の文字列にマッチする。

.. code-block:: none

   aaabbaaa

しかし、次の文字列にはマッチしない。

.. code-block:: none

   aaabba

.. caution:: POSIX 標準は「拡張」正規表現の後方参照をサポートしない。これは標準に対する互換拡張である。


.. _syntax.basic_extended.alternation:

選択
^^^^

:regexp:`|` 演算子は引数のいずれかにマッチする。よって、例えば :regexp:`abc|def` は :regex-input:`abc` か :regex-input:`def` のいずれかにマッチする。

括弧を使用すると選択をグループ化できる。例えば :regexp:`ab(d|ef)` は :regex-input:`abd` か :regex-input:`abef` のいずれかにマッチする。


.. _syntax.basic_extended.character_sets_:

文字集合
^^^^^^^^

文字集合は :regexp:`[` で始まり :regexp:`]` で終わる括弧式であり、文字の集合を定義する。集合に含まれるいずれかの 1 文字にマッチする。

文字集合に含められる要素は以下の組み合わせである。


.. _syntax.basic_extended.single_characters_:

単一の文字
~~~~~~~~~~

例えば :regexp:`[abc]` は “a”、“b”、“c” のいずれか 1 文字にマッチする。


.. _syntax.basic_extended.character_ranges_:

文字範囲
~~~~~~~~

例えば :regexp:`[a-c]` は‘a’から‘c’までの範囲の 1 文字にマッチする。POSIX 拡張正規表現の既定では、文字 :samp:`{x}` が :samp:`{y}` から :samp:`{z}` の範囲であるとは、文字の照合順がその範囲内にある場合をいう。結果はロカールの影響を受ける。この動作は :cpp:var:`!collate` :doc:`オプションフラグ <syntax_option_type>`\を設定しないことで抑止でき、文字が特定の範囲内にあるかどうかは文字のコードポイントのみで決定する。


.. _syntax.basic_extended.negation_:

否定
~~~~

括弧式が文字 :regexp:`^` で始まっている場合は、正規表現に含まれる文字の補集合となる。例えば :regexp:`[^a-c]` は範囲 :regexp:`a-c` を除くあらゆる文字にマッチする。


.. _syntax.basic_extended.character_classes_:

文字クラス
~~~~~~~~~~

:regexp:`[[:name:]]` のような形式の正規表現は名前付き文字クラス「name」にマッチする。例えば :regexp:`[[:lower:]]` はあらゆる小文字にマッチする。:doc:`character_class_names`\を見よ。


.. _syntax.basic_extended.collating_elements_:

照合要素
~~~~~~~~

:regexp:`[[.col.]]` のような形式の式は照合要素 :samp:`{col}` にマッチする。照合要素とは、単一の照合単位として扱われる文字か文字シーケンスである。照合要素は範囲の端点としても使用できる。例えば :regexp:`[[.ae.]-c]` は文字シーケンス “ae” に加えて、現在の範囲 “ae”-c の文字のいずれかにマッチする。 後者において “ae” は現在のロカールにおける単一の照合要素として扱われる。

照合要素は（通常、文字集合内で使用できない）エスケープの代わりとして使用できる。例えば :regexp:`[[.^.]abc]` は ‘abc^’ のいずれかの 1 文字にマッチする。

この拡張として、照合要素を\ :doc:`シンボル名 <collating_names>`\で指定する方法もある。例えば、

.. code-block:: none

   [[.NUL.]]

は NUL 文字にマッチする。


.. _syntax.basic_extended.equivalence_classes_:

等価クラス
~~~~~~~~~~

:regexp:`[[=col=]]` のような形式の正規表現は、第 1 位のソートキーが照合要素 :samp:`{col}` と同じ文字および照合要素にマッチする。照合要素名 :samp:`{col}` は :doc:`シンボル名 <collating_names>`\でもよい。第 1 位のソートキーでは大文字小文字の違い、アクセント記号の有無、ロカール固有のテーラリング（tailoring）は無視される。よって :regexp:`[[=a=]]` は a 、À 、Á 、Â 、Ã 、Ä 、Å 、A 、à 、á 、â 、ã 、ä および å のいずれにもマッチする。残念ながらこの機能の実装はプラットフォームの照合と地域化のサポートに依存し、すべてのプラットフォームで移植性の高い動作は期待できず、単一のプラットフォームにおいてもすべてのロカールで動作するとは限らない。


.. _syntax.basic_extended.combinations_:

結合
~~~~

以上の要素はすべて 1 つの文字集合宣言内で結合可能である。例：:regexp:`[[:digit:]a-c[.NUL.]]`


.. _syntax.basic_extended.escapes:

エスケープ
^^^^^^^^^^

POSIX 標準は、POSIX 拡張正規表現についてエスケープシーケンスを定義していない。ただし、以下の例外がある。

* 特殊文字の前にエスケープが付いている場合は、文字そのものにマッチする。
* 通常の文字の前にエスケープを付けた場合の効果は未定義である。
* 文字クラス宣言内のエスケープは、エスケープ文字自身にマッチする。言い換えると、エスケープ文字は文字クラス宣言内では特殊文字ではない。よって :regexp:`[\\^]` は直値の :regex-input:`\`\\` か :regex-input:`^` にマッチする。

しかしながら、これではいささか制限が強すぎるため、Boost.Regex は以下の標準互換の拡張をサポートする。


.. _syntax.basic_extended.escapes_matching_a_specific_character:

特定の文字にマッチするエスケープ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

以下のエスケープシーケンスは、すべて 1 文字の別名である。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 文字
   * - :regexp:`\\a`
     - ‘\\a’
   * - :regexp:`\\e`
     - 0x1B
   * - :regexp:`\\f`
     - \\f
   * - :regexp:`\\n`
     - \\n
   * - :regexp:`\\r`
     - \\r
   * - :regexp:`\\t`
     - \\t
   * - :regexp:`\\v`
     - \\v
   * - :regexp:`\\b`\（文字クラス宣言内のみ）。
     - \\b
   * - :regexp:`\\cX`
     - ASCII エスケープシーケンス。コードポイントが X % 32 の文字
   * - :regexp:`\\xdd`
     - 16 進エスケープシーケンス。コードポイントが 0xdd の文字にマッチする。
   * - :regexp:`\\x{dddd}`
     - 16 進エスケープシーケンス。コードポイントが 0xdddd の文字にマッチする。
   * - :regexp:`\\0ddd`
     - 8 進エスケープシーケンス。コードポイントが 0ddd の文字にマッチする。
   * - :regexp:`\\N{name}`
     - シンボル名 :samp:`{name}` の文字にマッチする。例えば :regexp:`\\N{newline}` は文字 :regex-input:`\\n` にマッチする。


.. _syntax.basic_extended._quot_single_character_quot__character_classes_:

「単一文字」文字クラス
~~~~~~~~~~~~~~~~~~~~~~

:samp:`{x}` が文字クラス名である場合、エスケープ文字 :samp:`{x}` はその文字クラスに属するあらゆる文字にマッチし、エスケープ文字 :samp:`{X}` はその文字クラスに属さないあらゆる文字にマッチする。

既定でサポートされているものは以下のとおりである。

.. list-table::
   :header-rows: 1

   * - エスケープシーケンス
     - 等価な文字クラス
   * - :regexp:`\\d`
     - :regexp:`[[:digit:]]`
   * - :regexp:`\\l`
     - :regexp:`[[:lower:]]`
   * - :regexp:`\\s`
     - :regexp:`[[:space:]]`
   * - :regexp:`\\u`
     - :regexp:`[[:upper:]]`
   * - :regexp:`\\w`
     - :regexp:`[[:word:]]`
   * - :regexp:`\\D`
     - :regexp:`[^[:digit:]]`
   * - :regexp:`\\L`
     - :regexp:`[^[:lower:]]`
   * - :regexp:`\\S`
     - :regexp:`[^[:space:]]`
   * - :regexp:`\\U`
     - :regexp:`[^[:upper:]]`
   * - :regexp:`\\W`
     - :regexp:`[^[:word:]]`

.. _syntax.basic_extended.character_properties:

文字プロパティ
~~~~~~~~~~~~~~

次の表の文字プロパティ名はすべて文字クラスで使用する名前と等価である。

.. list-table::
   :header-rows: 1

   * - 形式
     - 説明
     - 等価な文字集合の形式
   * - :regexp:`\\pX`
     - プロパティ :samp:`{X}` をもつあらゆる文字にマッチする。
     - :regexp:`[[:X:]]`
   * - :regexp:`\\p{Name}`
     - プロパティ :samp:`{Name}` をもつあらゆる文字にマッチする。
     - :regexp:`[[:Name:]]`
   * - :regexp:`\\PX`
     - プロパティ :samp:`{X}` をもたないあらゆる文字にマッチする。
     - :regexp:`[^[:X:]]`
   * - :regexp:`\\P{Name}`
     - プロパティ :samp:`{Name}` をもたないあらゆる文字にマッチする。
     - :regexp:`[^[:Name:]]`

例えば :regexp:`\\pd` は :regexp:`\\p{digit}` と同様、あらゆる「数字」（digit）にマッチする。


.. _syntax.basic_extended.word_boundaries:

単語境界
~~~~~~~~

次のエスケープシーケンスは単語の境界にマッチする。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 意味
   * - :regexp:`\\<`
     - 単語の先頭にマッチする。
   * - :regexp:`\\>`
     - 単語の終端にマッチする。
   * - :regexp:`\\b`
     - 単語境界（単語の先頭か終端）にマッチする。
   * - :regexp:`\\B`
     - 単語境界以外にマッチする。


.. _syntax.basic_extended.buffer_boundaries:

バッファ境界
~~~~~~~~~~~~

以下はバッファ境界にのみマッチする。この場合の「バッファ」とは、マッチ対象の入力テキスト全体である（:regexp:`^` および :regexp:`$` はテキスト中の改行にもマッチすることに注意していただきたい）。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 意味
   * - :regexp:`\``
     - バッファの先頭にのみマッチする。
   * - :regexp:`\\'`
     - バッファの終端にのみマッチする。
   * - :regexp:`\\A`
     - バッファの先頭にのみマッチする（:regexp:`\\\`` と同じ）。
   * - :regexp:`\\z`
     - バッファの終端にのみマッチする（:regexp:`\\'` と同じ）。
   * - :regexp:`\\Z`
     - バッファ終端の長さ 0 以上の改行シーケンスにマッチする。正規表現 :regexp:`\\n*\\z` と等価である。


.. _syntax.basic_extended.continuation_escape:

継続エスケープ（Continuation Escape）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

シーケンス :regexp:`\\G` は最後にマッチが見つかった位置、あるいは前回のマッチが存在しない場合はマッチ対象テキストの先頭にのみマッチする。各マッチが 1 つ前のマッチの終端から始まっているようなマッチをテキスト中から列挙する場合に、このシーケンスは有効である。


.. _syntax.basic_extended.quoting_escape:

クォーティングエスケープ（Quoting Escape）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

エスケープシーケンス :regexp:`\\Q` は「クォートされたシーケンス」の開始を表す。以降、正規表現の終端か :regexp:`\\E` までの文字はすべて直値として扱われる。例えば、正規表現 :regexp:`\\Q\\*+\\Ea+` は以下のいずれかにマッチする。

.. code-block:: none

   \*+a
   \*+aaa


.. _syntax.basic_extended.unicode_escapes:

Unicode エスケープ
~~~~~~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 意味
   * - :regexp:`\\C`
     - 単一のコードポイントにマッチする。Boost.Regex では :regexp:`.` 演算子とまったく同じ意味である。
   * - :regexp:`\\X`
     - 結合文字シーケンス（非結合文字に 0 以上の結合文字シーケンスが続く）にマッチする。


.. _syntax.basic_extended.any_other_escape:

その他のエスケープ
~~~~~~~~~~~~~~~~~~

その他のエスケープシーケンスは、エスケープ対象の文字そのものにマッチする。例えば :regexp:`\\@` は直値 :regex-input:`@` にマッチする。


.. _syntax.basic_extended.operator_precedence:

演算子の優先順位
^^^^^^^^^^^^^^^^

演算子の優先順位は以下のとおりである。

# 照合関係の括弧記号 :regexp:`[==]` :regexp:`[::]` :regexp:`[..]`
# エスケープ :regexp:`\\`
# 文字集合（括弧式） :regexp:`[]`
# グループ :regexp:`()`
# 単一文字の繰り返し :regexp:`*` :regexp:`+` :regexp:`?` :regexp:`{m,n}`
# 結合
# アンカー :regexp:`^$`
# 選択 :regexp:`|`


.. _syntax.basic_extended.what_gets_matched:

マッチするもの
^^^^^^^^^^^^^^

正規表現のマッチに複数の、可能な「最良」マッチは最左最長の規則で得られるものである。


.. _syntax.basic_extended.variations:

バリエーション
--------------

.. _syntax.basic_extended.egrep:

egrep
^^^^^

:doc:`egrep <syntax_option_type>` フラグを設定して正規表現をコンパイルすると、改行区切りの :doc:`POSIX 拡張正規表現 <syntax_extended>`\のリストとして扱われ、リスト内にマッチする正規表現があればマッチとなる。例えば次のコードは、 ::

   boost::regex e("abc\ndef", boost::regex::egrep);

POSIX 基本正規表現の :regexp:`abc` か :regexp:`def` のいずれかにマッチする。

名前が示すように、この動作は Unix ユーティリティの :program:`egrep` および :program:`grep` に :option:`!-E` オプションを付けて使用したものに合致する。


.. _syntax.basic_extended.awk:

awk
^^^

:ref:`POSIX 拡張機能 <syntax.basic_extended.posix_extended_syntax>`\に加えて、エスケープ文字が文字クラス宣言内で特殊となる。

さらに、POSIX 拡張仕様が定義しないいくつかのエスケープシーケンスをサポートすることが要求される。Boost.Regex はこれらのエスケープシーケンスを既定でサポートする。


.. _syntax.basic_extended.options:

オプション
----------

正規表現構築時に :ref:`!extended` および :cpp:var:`!egrep` オプションとともに指定可能な\ :ref:`フラグが多数ある <ref.syntax_option_type.syntax_option_type_extended>`\。特に :cpp:var:`!collate` 、:cpp:var:`!no_subs` 、:cpp:var:`!icase` :ref:`オプション <ref.syntax_option_type.syntax_option_type_extended>`\が大文字小文字の区別やロカール依存の動作を変更するのに対し、\ :ref:`newline_alt <ref.syntax_option_type.syntax_option_type_extended>` オプションは構文を変更するという点に注意していただきたい。


.. _syntax.basic_extended.references:

参考
----

`IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Base Definitions and Headers, Section 9, Regular Expressions <http://www.opengroup.org/onlinepubs/000095399/basedefs/xbd_chap09.html>`_\。

`IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, Utilities, egrep <http://www.opengroup.org/onlinepubs/000095399/utilities/grep.html>`_\。

`IEEE Std 1003.1-2001, Portable Operating System Interface (POSIX), Shells and Utilities, Section 4, Utilities, awk <http://www.opengroup.org/onlinepubs/000095399/utilities/awk.html>`_\。
