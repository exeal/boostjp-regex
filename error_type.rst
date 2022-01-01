.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


error_type
==========

.. cpp:type:: implementation_specific_type error_type

   型 :cpp:type:`!error_type` は、正規表現解析時にライブラリが発生させる可能性のある様々な種類のエラーを表す。


.. _ref.error_type.synopsis:

概要
----

::

   namespace boost{ namespace regex_constants{

   typedef implementation-specific-type error_type;

   static const error_type error_collate;
   static const error_type error_ctype;
   static const error_type error_escape;
   static const error_type error_backref;
   static const error_type error_brack;
   static const error_type error_paren;
   static const error_type error_brace;
   static const error_type error_badbrace;
   static const error_type error_range;
   static const error_type error_space;
   static const error_type error_badrepeat;
   static const error_type error_complexity;
   static const error_type error_stack;
   static const error_type error_bad_pattern;

   } // namespace regex_constants
   } // namespace boost


.. _ref.error_type.description:

説明
----

型 :cpp:type:`!error_type` は以下のいずれかの値をとる実装固有の列挙型である。

.. list-table::
   :header-rows: 1

   * - 定数
     - 意味
   * - :cpp:var:`!error_collate`
     - :regexp:`[[.name.]]` ブロックで指定した照合要素が不正。
   * - :cpp:var:`!error_ctype`
     - :regexp:`[[:name:]]` ブロックで指定した文字クラス名が不正。
   * - :cpp:var:`!error_escape`
     - 不正なエスケープか本体のないエスケープが見つかった。
   * - :cpp:var:`!error_backref`
     - 存在しないマーク済み部分式への後方参照が見つかった。
   * - :cpp:var:`!error_brack`
     - 不正な文字集合 :regexp:`[...]` が見つかった。
   * - :cpp:var:`!error_paren`
     - :regexp:`(` と :regexp:`)` が正しく対応していない。
   * - :cpp:var:`!error_brace`
     - :regexp:`{` と :regexp:`}` が正しく対応していない。
   * - :cpp:var:`!error_badbrace`
     - :regexp:`{...}` ブロックの内容が不正。
   * - :cpp:var:`!error_range`
     - 文字範囲が不正（例 :regexp:`[d-a]`）。
   * - :cpp:var:`!error_space`
     - メモリ不足。
   * - :cpp:var:`!error_badrepeat`
     - 繰り返し不能なものを繰り返そうとした（例 :regexp:`a*+`）。
   * - :cpp:var:`error_complexity`
     - 式が複雑で処理できなかった。
   * - :cpp:var:`!error_stack`
     - プログラムのスタック空間不足。
   * - :cpp:var:`!error_bad_pattern`
     - その他のエラー。
