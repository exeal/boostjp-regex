.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

Perl の正規表現構文
===================

.. _syntax.perl_syntax.synopsis:

概要
----

Perl の正規表現構文は、プログラミング言語 Perl で使われているものに基づいている。Perl の正規表現は Boost.Regex の既定の動作であり、:cpp:class:`basic_regex` のコンストラクタにフラグ :cpp:var:`!perl` を渡すことでも利用できる。例えば以下のとおり。 ::

   // e1 は大文字小文字を区別する Perl の正規表現：
   // Perl は既定のオプションであり、明示的に構文を指定する必要はない：
   boost::regex e1(my_expression);
   // e2 は大文字小文字を区別しない Perl の正規表現：
   boost::regex e2(my_expression, boost::regex::perl|boost::regex::icase);


.. _syntax.perl_syntax.perl_regular_expression_syntax:

Perl の正規表現構文
-------------------

Perl の正規表現では、以下の特別なものを除くあらゆる文字が文字そのものにマッチする。

.. code-block:: none

   .[{()\*+?|^$

他の文字は特定の場合のみ特別扱いである。例えば :regexp:`]` は開き :regexp:`[` の後のみ特別扱いとなる。


.. _syntax.perl_syntax.wildcard:

ワイルドカード
^^^^^^^^^^^^^^

文字集合外部の :regexp:`.` 1 文字は、以下以外のあらゆる文字1文字にマッチする。

* NULL 文字（マッチアルゴリズムにフラグ :cpp:type:`match_not_dot_null <match_flag_type>` を渡した場合）。
* 改行文字（マッチアルゴリズムにフラグ :cpp:type:`match_not_dot_newline <match_flag_type>` を渡した場合）。


.. _syntax.perl_syntax.anchors:

アンカー
^^^^^^^^

:regexp:`^` は行頭にマッチする。

:regexp:`$` は行末にマッチする。


.. _syntax.perl_syntax.marked_sub_expressions:

マーク済み部分式
^^^^^^^^^^^^^^^^

開始が :regexp:`(` で終了が :regexp:`)` の節は部分式として機能する。マッチした部分式はすべてマッチアルゴリズムにより個別のフィールドに分けられる。マーク済み部分式は繰り返しと後方参照により参照が可能である。


.. _syntax.perl_syntax.non_marking_grouping:

マークなしのグループ化
^^^^^^^^^^^^^^^^^^^^^^

マーク済み部分式は正規表現を字句的なグループに分けるのに役立つが、結果的に余分なフィールドを生成するという副作用がある。マーク済み部分式を生成することなく正規表現を字句的なグループに分ける別の手段として、:regexp:`(?:` と :regexp:`)` を使う方法がある。例えば :regexp:`(?:ab)+` は :regexp:`ab` の繰り返しを表し、別個の部分式を生成しない。


.. _syntax.perl_syntax.repeats:

繰り返し
^^^^^^^^

あらゆるアトム（文字、部分式、文字クラス）は :regexp:`*` 、:regexp:`+` 、:regexp:`?` および :regexp:`{}` 演算子による繰り返しが可能である。

:regexp:`*` 演算子は直前のアトムの 0 回以上の繰り返しにマッチする。例えば正規表現 :regexp:`a*b` は以下のいずれにもマッチする。

.. code-block:: none

   b
   ab
   aaaaaaaab

:regexp:`+` 演算子は直前のアトムの 1 回以上の繰り返しにマッチする。例えば正規表現 :regexp:`a+b` は以下のいずれにもマッチする。

.. code-block:: none

   ab
   aaaaaaaab

しかし次にはマッチしない。

.. code-block:: none

   b

:regexp:`?` 演算子は直前のアトムの 0 回あるいは 1 回の出現にマッチする。例えば正規表現 :regexp:`ca?b` は以下のいずれにもマッチする。

.. code-block:: none

   cb
   cab

しかし次にはマッチしない。

.. code-block:: none

   caab

アトムの繰り返しは回数境界指定の繰り返しによっても可能である。

:regexp:`a{n}` は :regex-input:`a` のちょうど n 回の繰り返しにマッチする。

:regexp:`a{n,}` は :regex-input:`a` の n 回以上の繰り返しにマッチする。

:regexp:`a{n,m}` は :regex-input:`a` の n 回以上 m 回以下の繰り返しにマッチする。

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

文字 :regexp:`{` および :regexp:`}` は、繰り返し以外の場面では通常のリテラルとして扱うことに注意していただきたい。これは Perl 5.x と同じ振る舞いである。例えば式 :regexp:`ab{1` 、:regexp:`ab1}` および :regexp:`a{b}c` の波括弧はリテラルとして扱い、\ **エラーは発生しない**\。

直前の構造が繰り返し不能な場合に繰り返し演算子を使うとエラーになる。例えば次は

.. code-block:: none

   a(*)

:regexp:`*` 演算子を適用可能なものがないためエラーとなる。


.. _syntax.perl_syntax.non_greedy_repeats:

貪欲でない繰り返し
^^^^^^^^^^^^^^^^^^

通常の繰り返し演算子は「貪欲」である。貪欲とは、可能な限り長い入力にマッチするという意味である。マッチを生成する中で最も短い入力に一致する貪欲でないバージョンがある。

:regexp:`*?` は直前のアトムの 0 回以上の繰り返しにマッチする最短バージョンである。

:regexp:`+?` は直前のアトムの 1 回以上の繰り返しにマッチする最短バージョンである。

:regexp:`??` は直前のアトムの 0 回か 1 回の出現にマッチする最短バージョンである。

:regexp:`{n,}?` は直前のアトムの n 回以上の繰り返しにマッチする最短バージョンである。

:regexp:`{n,m}?` は直前のアトムの n 回以上 m 回以下の繰り返しにマッチする最短バージョンである。


.. _syntax.perl_syntax.possessive_repeats:

強欲な繰り返し
^^^^^^^^^^^^^^

既定では、繰り返しパターンがマッチに失敗すると正規表現エンジンはマッチが見つかるまでバックトラッキングを行う。しかしながらこの動作が不都合な場合があるため、「強欲な」繰り返しというものがある。可能な限り長い文字列にマッチするが、式の残りの部分がマッチに失敗してもバックトラックを行わない。

:regexp:`*+` は直前のアトムの 0 回以上の繰り返しにマッチし、バックトラックを行わない。

:regexp:`++` は直前のアトムの 1 回以上の繰り返しにマッチし、バックトラックを行わない。

:regexp:`?+` は直前のアトムの 0 回か 1 回の出現にマッチし、バックトラックを行わない。

:regexp:`{n,}+` は直前のアトムの n 回以上の繰り返しにマッチし、バックトラックを行わない。

:regexp:`{n,m}+` は直前のアトムの n 回以上 m 回以下の繰り返しにマッチし、バックトラックを行わない。

.. _syntax.perl_syntax.back_references:

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

:regexp:`\\g` エスケープを使用しても同じ効果が得られる。例えば、

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 意味
   * - :regexp:`\\g1`
     - 1 番目の部分式にマッチ。
   * - :regexp:`\\g{1}`
     - 1 番目の部分式にマッチ。この形式を使うと :regexp:`\\g{1}2` のような式や、:regexp:`\\g{1234}` といった添字が 9 より大きい式を安全に解析できる。
   * - :regexp:`\\g-1`
     - 最後の部分式にマッチ。
   * - :regexp:`\\g{-2}`
     - 最後から 2 番目の部分式にマッチ。
   * - :regexp:`\\g{one}`
     - “one” という名前の部分式にマッチ。

最後に、:regexp:`\\k` エスケープで名前付き部分式を参照できる。例えば :regexp:`\\k<two>` は “two” という名前の部分式にマッチする。


.. _syntax.perl_syntax.alternation:

選択
^^^^

:regexp:`|` 演算子は引数のいずれかにマッチする。よって、例えば :regexp:`abc|def` は :regex-input:`abc` か :regex-input:`def` のいずれかにマッチする。

括弧を使用すると選択をグループ化できる。例えば :regexp:`ab(d|ef)` は :regex-input:`abd` か :regex-input:`abef` のいずれかにマッチする。

空の選択というのは許されないが、本当に必要な場合はプレースホルダーとして :regexp:`(?:)` を使用する。例えば、

* :regexp:`|abc` は有効な式ではない。
* しかし、:regexp:`(?:)|abc` は有効な式であり、実現しようとしていることは同じである。
* :regexp:`(?:abc)??` も全く同じ意味である。


.. _syntax.perl_syntax.character_sets:

文字集合
^^^^^^^^

文字集合は :regexp:`[` で始まり :regexp:`]` で終わる括弧式であり、文字の集合を定義する。集合に含まれるいずれかの 1 文字にマッチする。

文字集合に含められる要素は以下の組み合わせである。


.. _syntax.perl_syntax.single_characters:

単一の文字
~~~~~~~~~~

例えば :regexp:`[abc]` は :regex-input:`a` 、:regex-input:`b` 、:regex-input:`c` のいずれか 1 文字にマッチする。


.. _syntax.perl_syntax.character_ranges:

文字範囲
~~~~~~~~

例えば :regexp:`[a-c]` は ‘a’ から ‘c’ までの範囲の 1 文字にマッチする。Perl の正規表現の既定では、文字 :samp:`{x}` が :samp:`{y}` から :samp:`{z}` の範囲であるとは、文字のコードポイントが範囲の端点を含んだコードポイント内にある場合をいう。ただし、正規表現の構築時に :ref:`collate <ref.syntax_option_type.syntax_option_type_perl>` フラグ設定するとこの範囲はロカール依存となる。


.. _syntax.perl_syntax.negation:

否定
~~~~

括弧式が文字 :regexp:`^` で始まっている場合は、正規表現に含まれる文字の補集合となる。例えば :regexp:`[^a-c]` は範囲 :regexp:`a-c` を除くあらゆる文字にマッチする。


.. _syntax.perl_syntax.character_classes:

文字クラス
~~~~~~~~~~

:regexp:`[[:name:]]` のような形式の正規表現は名前付き文字クラス「name」にマッチする。例えば :regexp:`[[:lower:]]` はあらゆる小文字にマッチする。:doc:`character_class_names`\を見よ。


.. _syntax.perl_syntax.collating_elements:

照合要素
~~~~~~~~

:regexp:`[[.col.]]` のような形式の式は照合要素 :samp:`{col}` にマッチする。照合要素とは、単一の照合単位として扱われる文字か文字シーケンスである。照合要素は範囲の端点としても使用できる。例えば :regexp:`[[.ae.]-c]` は文字シーケンス “ae” のみならず、範囲 “ae”-c のいずれか 1 文字にもマッチする。 後者において “ae” は現在のロカールにおける単一の照合要素として扱われる。

この拡張として、照合要素を\ :doc:`シンボル名 <collating_names>`\で指定する方法もある。例えば、

.. code-block:: none

   [[.NUL.]]

は文字 :regex-input:`\\0` にマッチする。


.. _syntax.perl_syntax.equivalence_classes:

等価クラス
~~~~~~~~~~

:regexp:`[[=col=]]` のような形式の正規表現は、第 1 位のソートキーが照合要素 :samp:`{col}` と同じ文字および照合要素にマッチする。照合要素名 :samp:`{col}` は\ :doc:`シンボル名 <collating_names>`\でもよい。第 1 位のソートキーでは大文字小文字の違い、アクセント記号の有無、ロカール固有のテーラリング [#]_ は無視される。よって :regexp:`[[=a=]]` は a 、À 、Á 、Â 、Ã 、Ä 、Å 、A 、à 、á 、â 、ã 、ä および å のいずれにもマッチする。残念ながらこの機能の実装はプラットフォームの照合と地域化のサポートに依存し、すべてのプラットフォームで移植性の高い動作は期待できず、単一のプラットフォームにおいてもすべてのロカールで動作するとは限らない。


.. _syntax.perl_syntax.escaped_characters:

エスケープ付き文字
~~~~~~~~~~~~~~~~~~

1 文字にマッチするエスケープシーケンスおよび文字クラスが、文字クラスの定義で使用可能である。例えば :regexp:`[\\[\\]]` は :regex-input:`[` 、:regex-input:`]` のいずれかにマッチする。また :regexp:`[\\W\\d]` は「数字」か、「単語」\ **でない** 1 文字にマッチする。


.. _syntax.perl_syntax.combinations:

結合
~~~~

以上の要素はすべて 1 つの文字集合宣言内で結合可能である。例： :regexp:`[[:digit:]a-c[.NUL.]]`


.. _syntax.perl_syntax.escapes:

エスケープ
^^^^^^^^^^

直前にエスケープの付いた特殊文字は、すべてその文字自身にマッチする。

以下のエスケープシーケンスは、すべて 1 文字の別名である。

.. list-table::
   :header-rows: 1

   * - エスケープ
     - 文字
   * - :regexp:`\\a`
     - ``\a``
   * - :regexp:`\\e`
     - ``0x1B``
   * - :regexp:`\\f`
     - ``\f``
   * - :regexp:`\\n`
     - ``\n``
   * - :regexp:`\\r`
     - ``\r``
   * - :regexp:`\\t`
     - ``\t``
   * - :regexp:`\\v`
     - ``\v``
   * - :regexp:`\\b`
     - ``\b``\（文字クラス宣言内のみ）
   * - :regexp:`\\cX`
     - ASCII エスケープシーケンス。コードポイントが X % 32の文字
   * - :regexp:`\\xdd`
     - 16 進エスケープシーケンス。コードポイントが 0xdd の文字にマッチする。
   * - :regexp:`\\x{dddd}`
     - 16 進エスケープシーケンス。コードポイントが 0xdddd の文字にマッチする。
   * - :regexp:`\\0ddd`
     - 8 進エスケープシーケンス。コードポイントが 0ddd の文字にマッチする。
   * - :regexp:`\\N{name}`
     - :doc:`シンボル名 <collating_names>` :samp:`{name}` の文字にマッチする。例えば :regexp:`\\N{newline}` は文字 :regex-input:`\\n` にマッチする。


.. _syntax.perl_syntax._quot_single_character_quot__character_classes_:

「単一文字」文字クラス
~~~~~~~~~~~~~~~~~~~~~~

:samp:`{x}` が文字クラス名である場合、エスケープ文字 :samp:`{x}` はその文字クラスに属するあらゆる文字にマッチし、エスケープ文字 X はその文字クラスに属さないあらゆる文字にマッチする。

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
   * - :regexp:`\\h`
     - 水平空白
   * - :regexp:`\\v`
     - 垂直空白
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
   * - :regexp:`\\H`
     - 水平空白以外
   * - :regexp:`\\V`
     - 垂直空白以外


.. _syntax.perl_syntax.character_properties:

文字プロパティ
~~~~~~~~~~~~~~

次の表の文字プロパティ名はすべて\ :ref:`文字クラスで使用する名前 <syntax.perl_syntax.character_classes>`\と等価である。

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

例えば :regexp:`\\pd` は :regexp:`\\p{digit}` と同様、あらゆる「数字」（“digit”）にマッチする。


.. _syntax.perl_syntax.word_boundaries:

単語境界
~~~~~~~~

次のエスケープシーケンスは単語の境界にマッチする。

:regexp:`\\<` は単語の先頭にマッチする。

:regexp:`\\>` は単語の終端にマッチする。

:regexp:`\\b` は単語境界（単語の先頭か終端）にマッチする。

:regexp:`\\B` は単語境界以外にマッチする。


.. _syntax.perl_syntax.buffer_boundaries:

バッファ境界
~~~~~~~~~~~~

以下はバッファ境界にのみマッチする。この場合の「バッファ」とは、マッチ対象の入力テキスト全体である（:regexp:`^` および :regexp:`$` はテキスト中の改行にもマッチすることに注意していただきたい）。

:regexp:`\\\`` はバッファの先頭にのみマッチする。

:regexp:`\\'` はバッファの終端にのみマッチする。

:regexp:`\\A` はバッファの先頭にのみマッチする（:regexp:`\\`` と同じ）。

:regexp:`\\z` はバッファの終端にのみマッチする（:regexp:`\\'` と同じ）。

:regexp:`\\Z` はバッファ終端における省略可能な改行シーケンスのゼロ幅表明にマッチする。正規表現:regexp:`(?=\\v*\\z)` と等価である。:regexp:`(?=\\n?\\z)` のような動作をする Perl とは微妙に異なることに注意していただきたい。


.. _syntax.perl_syntax.continuation_escape:

継続エスケープ（Continuation Escape）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

シーケンス :regexp:`\\G` は最後にマッチが見つかった位置、あるいは前回のマッチが存在しない場合はマッチ対象テキストの先頭にのみマッチする。各マッチが 1 つ前のマッチの終端から始まっているようなマッチをテキスト中から列挙する場合に、このシーケンスは有効である。


.. _syntax.perl_syntax.quoting_escape:

クォーティングエスケープ（Quoting Escape）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

エスケープシーケンス :regexp:`\\Q` は「クォートされたシーケンス」の開始を表す。以降、正規表現の終端か :regexp:`\\E` までの文字はすべて直値として扱われる。例えば、正規表現 :regexp:`\\Q\\*+\\Ea+` は以下のいずれかにマッチする。

.. code-block:: none

   \*+a
   \*+aaa


.. _syntax.perl_syntax.unicode_escapes:

Unicode エスケープ
~~~~~~~~~~~~~~~~~~

:regexp:`\\C` は単一のコードポイントにマッチする。Boost.Regex では :regexp:`.` 演算子とまったく同じ意味である。:regexp:`\\X` は結合文字シーケンス（非結合文字に 0 以上の結合文字シーケンスが続く）にマッチする。


.. _syntax.perl_syntax.matching_line_endings:

行末へのマッチ
~~~~~~~~~~~~~~

エスケープシーケンス :regexp:`\\R` はあらゆる改行文字シーケンスにマッチする。つまり、式 :regexp:`(?>\\x0D\\x0A?|[\\x0A-\\x0C\\x85\\x{2028}\\x{2029}])` と等価である。


.. _syntax.perl_syntax.keeping_back_some_text:

テキストの除外
~~~~~~~~~~~~~~

:regexp:`\\K` は $0 の開始位置を現在のテキスト位置にリセットする。言い換えると :regexp:`\\K` より前にあるものはすべて差し引かれ、正規表現マッチの一部とならない。$` も同様に更新される。

例えば :regexp:`foo\\Kbar` をテキスト :regex-input:`foobar` にマッチさせると、$0 に対して :regex-input:`bar` 、$` に対して :regex-input:`foo` というマッチ結果が返る。これは可変幅の後方先読みを再現するのに使用する。


.. _syntax.perl_syntax.any_other_escape:

その他のエスケープ
~~~~~~~~~~~~~~~~~~

その他のエスケープシーケンスは、エスケープ対象の文字そのものにマッチする。例えば :regexp:`\\@` は直値 :regex-input:`@` にマッチする。


.. _syntax.perl_syntax.perl_extended_patterns:

Perl の拡張パターン
^^^^^^^^^^^^^^^^^^^

正規表現構文の Perl 固有の拡張はすべて :regexp:`(?` で始まる。


.. _syntax.perl_syntax.named_subexpressions:

名前付き部分式
~~~~~~~~~~~~~~

以下のようにして部分式を作成する。

.. code-block:: none

   (?<NAME>expression)

これで :samp:`{NAME}` という名前で参照可能になる。あるいは以下の :regexp:`'NAME'` のように区切り文字を使う方法もある。

.. code-block:: none

   (?'NAME'expression)

これらの名前付き部分式は後方参照内で :regexp:`\\g{NAME}` か :regexp:`\\k<NAME>` で参照する。検索・置換操作で使用する :doc:`Perl <format_perl_syntax>` 形式の文字列、および :cpp:class:`!match_results` メンバ関数では名前で参照する。


.. _syntax.perl_syntax.comments:

注釈
~~~~

:regexp:`(?# ... )` は注釈（コメント）として扱われ、内容は無視される。


.. _syntax.perl_syntax.modifiers:

修飾子
~~~~~~

:regexp:`(?imsx-imsx ... )` は、パターン中でどの Perl 修飾子を有効にするかを設定する。効果はブロックの先頭から閉じ括弧)までである。:regexp:`-` より前にある文字が Perl 修飾子を有効にし、後にある文字が無効にする。

:regexp:`(?imsx-imsx:pattern)` は、指定した修飾子をパターンのみに適用する。


.. _syntax.perl_syntax.non_marking_groups:

マークなしのグループ
~~~~~~~~~~~~~~~~~~~~

:regexp:`(?:pattern)` は、パターンを字句的にグループ化する。部分式の生成はない。


.. _syntax.perl_syntax.branch_reset:

選択分岐ごとの部分式番号のリセット（Branch reset）
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

:regexp:`(?|pattern)` は、:samp:`{pattern}` 内において :regexp:`|` が現れるごとに部分式の番号をリセットする。

この構造の後ろの部分式番号は、部分式の数が最大である選択分岐により決定する。この構造は、複数の選択マッチから 1 つを単一の部分式添字で捕捉したい場合に有効である。

以下に例を示す。式の下にあるのが各部分式の添字である。

.. code-block:: none

   # before  ---------------branch-reset----------- after
   / ( a )  (?| x ( y ) z | (p (q) r) | (t) u (v) ) ( z ) /x
   # 1            2         2  3        2     3     4


.. _syntax.perl_syntax.lookahead:

先読み
~~~~~~

:regexp:`(?=pattern)` はパターンがマッチした場合に限り、現在位置を進めない。

:regexp:`(?!pattern)` はパターンがマッチしなかった場合に限り、現在位置を進めない。

先読みを使用する典型的な理由は、2 つの正規表現の論理和作成である。例えばパスワードが大文字、小文字、区切り記号を含み、6 文字以上でなければならないとすると、次の正規表現でパスワードを検証できる。

.. code-block:: none

   (?=.*[[:lower:]])(?=.*[[:upper:]])(?=.*[[:punct:]]).{6,}


.. _syntax.perl_syntax.lookbehind:

後読み
~~~~~~

:regexp:`(?<=pattern)` は、現在位置の直前の文字列がパターンにマッチ可能な場合に限り、現在位置を進めない（パターンは固定長でなければならない）。

:regexp:`(?<!pattern)` は、現在位置の直前の文字列がパターンにマッチ不能な場合に限り、現在位置を進めない（パターンは固定長でなければならない）。


.. _syntax.perl_syntax.independent_sub_expressions:

独立部分式
~~~~~~~~~~

:regexp:`(?>pattern)` とすると、:samp:`{pattern}` は周囲のパターンとは独立してマッチし、正規表現は :samp:`{pattern}` にはバックトラックしない。独立部分式を使用する典型的な理由は効率の向上である。可能な限り最良のマッチのみが考慮されるため、独立部分式が正規表現全体のマッチを妨害する場合はマッチは 1 つも見つからない。 [#]_


.. _syntax.perl_syntax.recursive_expressions:

再帰式
~~~~~~

:regexp:`(?N)` :regexp:`(?-N)` :regexp:`(?+N)` :regexp:`(?R)` :regexp:`(?0)`

:regexp:`(?R)` および :regexp:`(?0)` はパターン全体の先頭に再帰する。

:regexp:`(?N)` は :samp:`{N}` 番目の部分式を再帰的に実行する。例えば :regexp:`(?2)` は 2 番目の部分式へ再帰する。

:regexp:`(?-N)` および :regexp:`(?+N)` は相対的な再帰である。例えば :regexp:`(?-1)` は最後の部分式へ、:regexp:`(?+1)` は次の部分式へ再帰する。


.. _syntax.perl_syntax.conditional_expressions:

条件式
~~~~~~

:regexp:`(?(condition)yes-pattern|no-pattern)` は、:samp:`{condition}` が真であれば :samp:`{yes-pattern}` 、それ以外の場合は :samp:`{no-pattern}` のマッチを行う。

:regexp:`(?(condition)yes-pattern)` は、:samp:`{condition}` が真であれば :samp:`{yes-pattern}` のマッチを行い、それ以外の場合は空文字列にマッチする。

:samp:`{condition}` は前方先読み表明、マーク済み部分式の添字（対応する部分式がマッチしていれば条件が真）、あるいは再帰式の添字（指定した再帰式内を直接実行している場合に条件が真）のいずれかである。

考えられる条件式を挙げる。

* :regexp:`(?(?=assert)yes-pattern|no-pattern)` は、前方先読み表明がマッチした場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(?!assert)yes-pattern|no-pattern)` は、前方先読み表明がマッチしなかった場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(N)yes-pattern|no-pattern)` は、:samp:`{N}` 番目の部分式がマッチした場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(<name>)yes-pattern|no-pattern)` は、名前付き部分式 :samp:`{name}` がマッチした場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?('name')yes-pattern|no-pattern)` は、名前付き部分式 :samp:`{name}` がマッチした場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(R)yes-pattern|no-pattern)` は、再帰式内を実行中である場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(RN)yes-pattern|no-pattern)` は、:samp:`{N}` 番目の部分式への再帰内を実行中である場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(R&name)yes-pattern|no-pattern)` は、名前付き部分式 :samp:`{name}` への再帰内を実行中である場合に :samp:`{yes-pattern}` を、それ以外の場合に :samp:`{no-pattern}` を実行する。
* :regexp:`(?(DEFINE)never-executed-pattern)` は絶対に実行されず、どこにもマッチしないコードブロックを定義する。通常、パターン内の別の場所から参照する 1 つ以上の名前付き式を定義するのに使用する。


.. _syntax.perl_syntax.backtracking_control_verbs:

バックトラッキング制御動詞
~~~~~~~~~~~~~~~~~~~~~~~~~~

本ライブラリは Perl のバックトラッキング制御動詞をサポートする。ただし部分的なものであり、特に :regexp:`(*MARK)` はサポートしない。本ライブラリと Perl の間では細かい挙動が異なる可能性がある。少なくとも Perl の挙動はドキュメントが不十分であり、実際にはよく分からない動作をすることがあるからである。サポートしている動詞は以下のとおり。

:regexp:`(*PRUNE)`
   バックトラックしてきた場合に、それ以前のバックトラッキングに関するすべての情報を破棄する。これ以外の場合では作用は無い。
:regexp:`(*SKIP)`
   :regexp:`(*PRUNE)` と同じだが、検索対象文字列における現在位置より前でマッチが発生しないことを示す点が異なる。すでにマッチを構成しないと決定したテキストのチャンクをスキップすることで検索を最適化するのに使用する。
:regexp:`(*THEN)`
   バックトラックしてきた場合に、選択グループ内の残りすべての選択を破棄する。これ以外の場合では作用は無い。
:regexp:`(*COMMIT)`
   バックトラックしてきた場合に、残りすべてのマッチと検索を失敗させる。これ以外の場合では作用は無い。
:regexp:`(*FAIL)`
   この時点で無条件にマッチを失敗させる。正規表現エンジンにバックトラックを強制するのに使用する。
:regexp:`(*ACCEPT)`
   この時点でパターンがマッチしたとする。半開きの部分式はすべてその時点で閉じられる。


.. _syntax.perl_syntax.operator_precedence:

演算子の優先順位
^^^^^^^^^^^^^^^^

演算子の優先順位は以下のとおりである。

#. 照合関係の括弧記号 :regexp:`[==]` :regexp:`[::]` :regexp:`[..]`
#. エスケープ :regexp:`\\`
#. 文字集合（括弧式） :regexp:`[]`
#. グループ :regexp:`()`
#. 単一文字の繰り返し :regexp:`*` :regexp:`+` :regexp:`?` :regexp:`{m,n}`
#. 結合
#. アンカー :regexp:`^$`
#. 選択 :regexp:`|`


.. _syntax.perl_syntax.what_gets_matched:

マッチするもの
--------------

正規表現を有向グラフ（あるいは閉路グラフ）とみなすと、入力テキストに対する最良マッチとは、グラフに対して深さ優先検索を行って最初に見つかるマッチである。

これは言い換えると次のようになる。最良マッチとは各要素が以下のようにマッチする\ :doc:`最左マッチ <leftmost_longest>`\である。

.. list-table::
   :header-rows: 1

   * - 構造
     - マッチするもの
   * - :regexp:`AtomA AtomB`
     - :samp:`{AtomB}` に対するマッチが直後に続く :samp:`{AtomA}` に対する最良マッチを検索する。
   * - :regexp:`ExpressionA | ExpressionB`
     - :samp:`{Expression1}` がマッチ可能であればそのマッチを返す。それ以外の場合は :samp:`{Expression2}` を試行する。
   * - :regexp:`S{N}`
     - :samp:`{S}` のちょうど :samp:`{N}` 回の繰り返しにマッチする。
   * - :regexp:`S{N,M}`
     - :samp:`{S}` の、:samp:`{N}` 回以上 :samp:`{M}` 回以下の可能な限り長い繰り返しにマッチする。
   * - :regexp:`S{N,M}?`
     - :samp:`{S}` の、:samp:`{N}` 回以上 :samp:`{M}` 回以下の可能な限り短い繰り返しにマッチする。
   * - :regexp:`S?` 、:regexp:`S*` 、:regexp:`S+`
     - それぞれ :regexp:`S{0,1}` 、:regexp:`S{0,UINT_MAX}` 、:regexp:`S{1,UINT_MAX}` と同じ。
   * - :regexp:`S??` 、:regexp:`S*?` 、:regexp:`S+?`
     - それぞれ :regexp:`S{0,1}?` 、:regexp:`S{0,UINT_MAX}?` 、:regexp:`S{1,UINT_MAX}?` と同じ。
   * - :regexp:`(?>S)`
     - :samp:`{S}` の最良マッチにマッチするのみ。
   * - :regexp:`(?=S)` 、:regexp:`(?<=S)`
     - :samp:`{S}` の最良マッチにのみマッチする（これが分かるのは、:samp:`{S}` 中に捕捉を行う括弧がある場合のみ）。
   * - :regexp:`(?!S)` 、:regexp:`(?<!S)`
     - :samp:`{S}` に対するマッチが存在するかどうか考慮するのみ。
   * - :regexp:`(?(condition)yes-pattern|no-pattern)`
     - 条件が真であれば :samp:`{yes-pattern}` のみが考慮される。それ以外の場合は :samp:`{no-pattern}` のみが考慮される。


.. _syntax.perl_syntax.variations:

バリエーション
--------------

:cpp:var:`!normal` 、:cpp:var:`!ECMAScript` 、:cpp:var:`!JavaScript` および :cpp:var:`!JScript` の\ :ref:`各オプション <ref.syntax_option_type.syntax_option_type_perl>`\はすべて :cpp:var:`!perl` の別名である。


.. _syntax.perl_syntax.options:

オプション
----------

正規表現構築時に :cpp:var:`!perl` オプションとともに指定可能な\ :ref:`フラグが多数ある <ref.syntax_option_type.syntax_option_type_perl>`\。特に :cpp:var:`!collate` 、:cpp:var:`!no_subs` 、:cpp:var:`!icase` オプションが大文字小文字の区別やロカール依存の動作を変更するのに対し、:cpp:var:`!newline_alt` オプションは構文を変更するという点に注意していただきたい。


.. _syntax.perl_syntax.pattern_modifiers:

パターン修飾子
--------------

:regexp:`(?smix-smix)` を正規表現の前につけるか、\ :ref:`正規表現コンパイル時フラグ <ref.syntax_option_type.syntax_option_type_perl>` :cpp:var:`!no_mod_m` 、:cpp:var:`!mod_x` 、:cpp:var:`!mod_s` 、:cpp:var:`!no_mod_s` を使用することで Perl の :code:`smix` 修飾子を適用することができる。


.. _syntax.perl_syntax.references:

参考
----

`Perl 5.8 <http://perldoc.perl.org/perlre.html>`_\。 [#]_


.. [#] 訳注　独立部分式の知識がある方でなければ意味不明だと思います。Perl のチュートリアル等を参照されることをお勧めします。

.. [#] 訳注　テーラリング（tailoring）は汎用的な処理に対して、特定の事情に即した結果を得るために追加規則を用いて処理をカスタマイズすることを意味します。文字処理の分野では特に各ロカールに対応するためのテーラリングが多数存在します。

.. [#] 訳注　リンク先はバージョン 5 系列の最新版になっています。現在の Boost.Regex には Perl 5.9 以降の機能が追加されているので、確認しておくといいです。
.. 現在っていつ？
