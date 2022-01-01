.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


コンセプト
==========

.. _ref.concepts.charT_concept:

charT の要件
------------

:cpp:class:`basic_regex` テンプレートクラスのテンプレート引数で使用する型 :cpp:type:`!charT` は、自明なデフォルトコンストラクタ、コピーコンストラクタ、代入演算子およびデストラクタを持たなければならない。加えてオブジェクトについては以下の要件を満足しなければならない。以下の表では :cpp:type:`!charT` 型の :cpp:var:`!c` 、:cpp:type:`!charT const` 型の :cpp:var:`!c1` および :cpp:var:`!c2` 、:cpp:type:`!int` 型の :cpp:var:`!i` を用いる。

.. list-table::
   :header-rows: 1

   * - 式
     - 戻り値の型
     - 表明、注釈、事前・事後条件
   * - :code:`charT c`
     - :cpp:type:`!charT`
     - デフォルトコンストラクタ（自明でなければならない）。
   * - :code:`charT c(c1)`
     - :cpp:type:`!charT`
     - コピーコンストラクタ（自明でなければならない）。
   * - :cpp:expr:`c1 = c2`
     - :cpp:type:`!charT`
     - 代入演算子（自明でなければならない）。
   * - :cpp:expr:`c1 == c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` と :cpp:var:`!c2` の値が同じであれば真。
   * - :cpp:expr:`c1 != c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` と :cpp:var:`!c2` が同値でなければ真。
   * - :cpp:expr:`c1 < c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` の値が :cpp:var:`!c2` よりも小さければ真。
   * - :cpp:expr:`c1 > c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` の値が :cpp:var:`!c2` よりも大きければ真。
   * - :cpp:expr:`c1 <= c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` が :cpp:var:`!c2` 以下であれば真。
   * - :cpp:expr:`c1 >= c2`
     - :cpp:type:`!bool`
     - :cpp:var:`!c1` が :cpp:var:`!c2` 以上であれば真。
   * - :code:`intmax_t i = c1`
     - :cpp:type:`!int`
     - :cpp:type:`!charT` は整数型に変換可能でなければならない。

       注意：特性クラスが最小限の標準インターフェイスではなく Boost 固有のフルインターフェイスをサポートする場合は、:cpp:type:`!charT` 型はこの操作をサポートする必要はない（後述の特性クラスの要件を見よ）。
   * - :code:`charT c(i)`
     - :cpp:type:`!charT`
     - :cpp:type:`!charT` は整数型から構築可能でなければならない。


.. _ref.concepts.traits_concept:

特性クラスの要件
----------------

:cpp:class:`basic_regex` の :cpp:type:`!traits` テンプレート引数に対しては要件のセットが 2 つある。最小限のインターフェイス（正規表現標準草案の一部）と、オプションの Boost 固有強化インターフェイスである。


.. _ref.concepts.traits_concept.minimal_requirements_:

最小限の要件
------------

以下の表において :cpp:class:`!X` は :cpp:type:`!charT` 型の文字コンテナについて型と関数を定義する特性クラスを表す。:cpp:var:`!u` は :cpp:class:`X` 型のオブジェクト、:cpp:var:`!v` は :cpp:type:`!const X` 型のオブジェクト、:cpp:var:`!p` は :cpp:type:`!const charT*` 型の値、:cpp:var:`!I1` および :cpp:var:`!I2` は入力イテレータ、:cpp:var:`!c` は :cpp:type:`!const charT` 型の値、:cpp:var:`!s` は型 :cpp:type:`!X::string_type` のオブジェクト、:cpp:var:`!cs` は型 :cpp:type:`!const X::string_type` のオブジェクト、:cpp:var:`!b` は :cpp:type:`!bool` 型の値、:cpp:var:`!I` は :cpp:type:`!int` 型の値、:cpp:var:`!F1` および :cpp:var:`!F2` は :cpp:type:`!const charT*` 型の値、:cpp:var:`!loc` は :cpp:type:`!X::locale_type` 型のオブジェクトである。

.. list-table::
   :header-rows: 1

   * - 式
     - 戻り値の型
     - 表明、注釈、事前・事後条件
   * - :cpp:expr:`X::char_type`
     - :cpp:type:`!charT`
     - :cpp:class:`!basic_regex` クラステンプレートを実装する文字コンテナ型。
   * - :cpp:expr:`X::size_type`
     - \-
     - :cpp:type:`!charT` の null 終端文字列の長さを保持可能な符号なし整数型。
   * - :cpp:expr:`X::string_type`
     - :cpp:type:`!std::basic_string<charT>` か :cpp:type:`!std::vector<charT>`
     - なし。
   * - :cpp:expr:`X::locale_type`
     - （実装定義）
     - 特性クラスが使用するロカールを表現する、コピー構築可能な型。
   * - :cpp:expr:`X::char_class_type`
     - （実装定義）
     - 個々の文字分類（文字クラス）を表現するビットマスク型。この型の複数の値をビット和すると別の有効な値を得る。
   * - :cpp:expr:`X::length(p)`
     - :cpp:type:`!X::size_type`
     - :cpp:expr:`p[i] == 0` である最小の :cpp:var:`!i` を返す。計算量は :cpp:var:`!i` に対して線形である。
   * - :cpp:expr:`v.translate(c)`
     - :cpp:type:`!X::char_type`
     - :cpp:var:`!c` と等価、つまり :cpp:expr:`v.translate(c) == v.translate(d)` となるような文字 :cpp:var:`!d` を返す。
   * - :cpp:expr:`v.translate_nocase(c)`
     - :cpp:type:`!X::char_type`
     - 大文字小文字を区別せずに比較した場合に :cpp:var:`!c` と等価、つまり :cpp:expr:`v.translate_nocase(c) == v.translate_nocase(C)` となるような文字 :cpp:var:`!C` を返す。
   * - :cpp:expr:`v.transform(F1, F2)`
     - :cpp:type:`!X::string_type`
     - イテレータ範囲 [F1, F2) が示す文字シーケンスのソートキーを返す。文字シーケンス [G1, G2) が文字シーケンス [H1, H2) の前にソートされる場合に :cpp:expr:`v.transform(G1, G2) < v.transform(H1, H2)` とならなければならない。
   * - :cpp:expr:`v.transform_primary(F1, F2)`
     - :cpp:type:`!X::string_type`
     - イテレータ範囲 [F1, F2) が示す文字シーケンスのソートキーを返す。大文字小文字を区別せずにソートして文字シーケンス [G1, G2) が文字シーケンス [H1, H2) の前に現れる場合に :cpp:expr:`v.transform_primary(G1, G2) < v.transform_primary(H1, H2)` とならなければならない。
   * - :cpp:expr:`v.lookup_classname(F1, F2)`
     - :cpp:type:`!X::char_class_type`
     - イテレータ範囲 [F1, F2) が示す文字シーケンスを、:cpp:func:`!isctype` に渡せるビットマスク型に変換する。:cpp:func:`lookup_classname` が返した値同士でビット和をとっても安全である。文字シーケンスが :cpp:class:`!X` が解釈できる文字クラス名でなければ 0 を返す。文字シーケンス内の大文字小文字の違いで戻り値が変化することはない。
   * - :cpp:expr:`v.lookup_collatename(F1, F2)`
     - :cpp:type:`!X::string_type`
     - イテレータ範囲 [F1, F2) が示す文字シーケンスが構成する照合要素を表す文字シーケンスを返す。文字シーケンスが正しい照合要素でなければ空文字列を返す。
   * - :cpp:expr:`v.isctype(c, v.lookup_classname(F1, F2))`
     - :cpp:type:`!bool`
     - 文字 :cpp:var:`!c` が、イテレータ範囲 [F1, F2) が示す文字クラスのメンバであれば真を返す。それ以外は偽を返す。
   * - :cpp:expr:`v.value(c, I)`
     - :cpp:type:`!int`
     - 文字 :cpp:var:`!c` が基数 :cpp:var:`!I` で有効な数字であれば、数字cの基数 :cpp:var:`!I` での数値を返す。 [#]_ それ以外の場合は -1 を返す。
   * - :cpp:expr:`u.imbue(loc)`
     - :cpp:type:`!X::locale_type`
     - ロカール :cpp:var:`!loc` を :cpp:var:`!u` に指示する。:cpp:var:`!u` が直前まで使用していたロカールを返す（あれば）。
   * - :cpp:expr:`v.getloc()`
     - :cpp:type:`!X::locale_type`
     - :cpp:var:`!v` が使用中のロカールを返す（あれば）。


.. _ref.concepts.traits_concept.additional_optional_requirements:

オプションの追加要件
--------------------

以下の追加要件は厳密にはオプションである。しかしながら :cpp:class:`basic_regex` でこれらの追加インターフェイスを利用するには、以下の要件をすべて満たさなければならない。:cpp:class:`basic_regex` はメンバ :cpp:type:`!boost_extensions_tag` の有無を検出し、自身を適切に構成する。

.. list-table::
   :header-rows: 1

   * - 式
     - 結果
     - 表明、注釈、事前・事後条件
   * - :cpp:expr:`X::boost_extensions_tag`
     - 型の指定はない。
     - 与えられている場合、この表にある拡張がすべて与えられていなければならない。
   * - :cpp:expr:`v.syntax_type(c)`
     - :cpp:type:`!regex_constants::syntax_type`
     - 正規表現文法における文字 :cpp:var:`!c` の意味を表す :cpp:type:`!regex_constants::syntax_type` 型のシンボル値を返す。
   * - :cpp:expr:`v.escape_syntax_type(c)`
     - :cpp:type:`!regex_constants::syntax_type`
     - 正規表現文法において、:cpp:var:`!c` の前にエスケープ文字がある場合（式中で文字 :cpp:var:`!c` の直前に文字 :cpp:var:`!b` がある場合 :cpp:expr:`v.syntax_type(b) == syntax_escape` の文字 :cpp:var:`!c` の意味を表す :cpp:type:`!regex_constants::escape_syntax_type` 型のシンボル値を返す。
   * - :cpp:expr:`v.translate(c, b)`
     - :cpp:type:`!X::char_type`
     - :cpp:var:`!c` と等価、つまり :cpp:expr:`v.translate(c, false) == v.translate(d, false)` となる文字 :cpp:var:`!d` を返す。あるいは大文字小文字を区別せずに比較した場合に等価、つまり :cpp:expr:`v.translate(c, true) == v.translate(C, true)` となる文字 :cpp:var:`!C` を返す。
   * - :cpp:expr:`v.toi(I1, I2, I)`
     - :cpp:type:`!charT` か :cpp:type:`!int` を保持可能な整数型。
     - :cpp:expr:`I1 == I2` か :cpp:expr:`*I1` が数字でなければ -1 を返す。それ以外の場合はシーケンス [I1, I2) に入力数値書式化処理を行い、結果を :cpp:type:`!int` で返す。事後条件：:cpp:expr:`I1 == I2` か :cpp:expr:`*I1` が数字以外のいずれか。
   * - :cpp:expr:`v.error_string(I)`
     - :cpp:type:`!std::string`
     - エラー状態 :cpp:var:`!I` の可読性の高いエラー文字列を返す。:cpp:var:`!I` は :cpp:type:`!regex_constants::error_type` 型が列挙する値のいずれかである。値 :cpp:var:`!I` が解釈不能な場合は、文字列 “Unknown error” か同じ意味の地域化文字列を返す。
   * - :cpp:expr:`v.tolower(c)`
     - :cpp:type:`!X::char_type`
     - :cpp:var:`!c` を小文字に変換する。Perl スタイルの :regexp:`\\l` および :regexp:`\\L` 書式化処理で使用する。
   * - :cpp:expr:`v.toupper(c)`
     - :cpp:type:`!X::char_type`
     - :cpp:var:`!c` を大文字に変換する。Perl スタイルの :regexp:`\\u` および :regexp:`\\U` 書式化処理で使用する。


.. _ref.concepts.iterator_concept:

イテレータの要件
----------------

正規表現アルゴリズム（およびイテレータ）は、すべて双方向イテレータの要件を満たす。


.. [#] :cpp:var:`!I` の値は 8 、10 、16 のいずれかである。
