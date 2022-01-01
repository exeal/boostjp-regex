.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


マーク済み部分式と捕捉の理解
============================

捕捉とは、正規表現にマッチしたマーク済み部分式に「捕捉された」イテレータ範囲である。マーク済み部分式が複数回マッチした場合は、1 つのマーク済み部分式が複数の捕捉について対応する可能性がある。本文書では捕捉とマーク済み部分式の Boost.Regex での表現とアクセス方法について述べる。


.. _captures.marked_sub_expressions:

マーク済み部分式
----------------

Perl は括弧グループ :regexp:`()` を含む正規表現について、マーク済み部分式という追加のフィールドを吐き出す。例えば次の正規表現にはマーク済み部分式が 2 つある（それぞれ $1 、$2 という）。

.. code-block:: none

   (\w+)\W+(\w+)

また、マッチ全体を $& 、最初のマッチより前すべてを $`、マッチより後ろすべてを $' であらわす。よって :regex-input:`@abc def--` に対して上の式で検索をかけると次の結果を得る。

.. list-table::
   :header-rows: 1

   * - 部分式
     - 検索されるテキスト
   * - :regexp:`$\``
     - :regex-input:`@`
   * - :regexp:`$&`
     - :regex-input:`abc def`
   * - :regexp:`$1`
     - :regex-input:`abc`
   * - :regexp:`$2`
     - :regex-input:`def`
   * - :regexp:`$'`
     - :regex-input:`--`

Boost.Regex ではこれらはすべて、正規表現マッチアルゴリズム（:cpp:func:`regex_search` 、:cpp:func:`regex_match` 、:cpp:class:`regex_iterator`）のいずれかを呼び出したときに値が埋められる :cpp:class:`match_results` クラスによりアクセスできる。以下が与えられていたとすると、 ::

   boost::match_results<IteratorType> m;

Perl と Boost.Regex の対応は次のようになる。

.. list-table::
   :header-rows: 1

   * - Perl
     - Boost.Regex
   * - :regexp:`$\``
     - :cpp:expr:`m.prefix()`
   * - :regexp:`$&`
     - :cpp:expr:`m[0]`
   * - :regexp:`$n`
     - :cpp:expr:`m[n]`
   * - :regexp:`$'`
     - :cpp:expr:`m.suffix()`

Boost.Regex では各部分式マッチは :cpp:class:`sub_match` オブジェクトで表現される。これは基本的には部分式がマッチした位置の先頭と終端を指すイテレータの組に過ぎないが、:cpp:class:`sub_match` オブジェクトが :cpp:class:`!std::basic_string` に類似した振る舞いをするように、演算子がいくつか追加されている。例えば :cpp:class:`!basic_string` への暗黙の型変換により、文字列との比較、文字列への追加、および出力ストリームへの出力が可能になっている。


.. _captures.unmatched_sub_expressions:

マッチしなかった部分式
----------------------

マッチが見つかったとして、すべてのマーク済み部分式が関与する必要のない場合がある。例えば、

.. code-block:: none

   (abc)|(def)

この式は $1 か $2 のいずれかがマッチする可能性があるが、両方とも同時にマッチすることはない。Boost.Regex では :cpp:member:`sub_match::matched` データメンバにアクセスすることでマッチしたかどうか調べることができる。


.. _captures.repeated_captures:

捕捉の繰り返し
--------------

マーク済み部分式が繰り返されている場合、その部分式は複数回「捕捉される」。しかし通常利用可能なのは最後の捕捉のみである。例えば、

.. code-block:: none

   (?:(\w+)\W+)+

この式は以下にマッチした場合、

.. code-block:: none

   one fine day

$1 には文字列 “day” が格納され、それ以前の捕捉はすべて捨てられる。

しかしながら Boost.Regex の実験的な機能を使用すれば捕捉情報をすべて記憶しておくことが可能である。これにアクセスするには :cpp:func:`!match_results::captures` メンバ関数か :cpp:func:`!sub_match::captures` メンバ関数を使う。これらの関数は、正規表現マッチの間に記憶した捕捉をすべて含んだコンテナを返す。以下のサンプルプログラムでこの情報の使用方法を説明する。 ::

   #include <boost/regex.hpp>
   #include <iostream>

   void print_captures(const std::string& regx, const std::string& text)
   {
      boost::regex e(regx);
      boost::smatch what;
      std::cout << "正規表現：\"" << regx << "\"\n";
      std::cout << "テキスト：\"" << text << "\"\n";
      if(boost::regex_match(text, what, e, boost::match_extra))
      {
         unsigned i, j;
         std::cout << "** マッチが見つかりました **\n 部分式：\n";
         for(i = 0; i < what.size(); ++i)
            std::cout << " $" << i << " = \"" << what[i] << "\"\n";
         std::cout << " 捕捉：\n";
         for(i = 0; i < what.size(); ++i)
         {
            std::cout << " $" << i << " = {";
            for(j = 0; j < what.captures(i).size(); ++j)
            {
               if(j)
                  std::cout << ", ";
               else
                  std::cout << " ";
               std::cout << "\"" << what.captures(i)[j] << "\"";
            }
            std::cout << " }\n";
         }
      }
      else
      {
         std::cout << "** マッチは見つかりません **\n";
      }
   }

   int main(int , char* [])
   {
      print_captures("(([[:lower:]]+)|([[:upper:]]+))+", "aBBcccDDDDDeeeeeeee");
      print_captures("(.*)bar|(.*)bah", "abcbar");
      print_captures("(.*)bar|(.*)bah", "abcbah");
      print_captures("^(?:(\\w+)|(?>\\W+))*$",
         "now is the time for all good men to come to the aid of the party");
      return 0;
   }

このプログラムの出力は次のようになる。

.. code-block:: console

   正規表現："(([[:lower:]]+)|([[:upper:]]+))+"
   テキスト："aBBcccDDDDDeeeeeeee"
   ** マッチが見つかりました **
      部分式：
         $0 = "aBBcccDDDDDeeeeeeee"
         $1 = "eeeeeeee"
         $2 = "eeeeeeee"
         $3 = "DDDDD"
      捕捉：
         $0 = { "aBBcccDDDDDeeeeeeee" }
         $1 = { "a", "BB", "ccc", "DDDDD", "eeeeeeee" }
         $2 = { "a", "ccc", "eeeeeeee" }
         $3 = { "BB", "DDDDD" }
   正規表現："(.*)bar|(.*)bah"
   テキスト："abcbar"
   ** マッチが見つかりました **
      部分式：
         $0 = "abcbar"
         $1 = "abc"
         $2 = ""
      捕捉：
         $0 = { "abcbar" }
         $1 = { "abc" }
         $2 = { }
   正規表現："(.*)bar|(.*)bah"
   テキスト："abcbah"
   ** マッチが見つかりました **
      部分式：
         $0 = "abcbah"
         $1 = ""
         $2 = "abc"
      捕捉：
         $0 = { "abcbah" }
         $1 = { }
         $2 = { "abc" }
   正規表現："^(?:(\w+)|(?>\W+))*$"
   テキスト："now is the time for all good men to come to the aid of the party"
   ** マッチが見つかりました **
      部分式：
         $0 = "now is the time for all good men to come to the aid of the party"
         $1 = "party"
      捕捉：
         $0 = { "now is the time for all good men to come to the aid of the party" }
         $1 = { "now", "is", "the", "time", "for", "all", "good", "men", "to",
            "come", "to", "the", "aid", "of", "the", "party" }

残念ながらこの機能を有効にすると（実際に使用しない場合でも）効率に影響が出る上、使用した場合はさらに効率が悪化するため、以下の 2 つを行わないと使用できないようになっている。

* ライブラリのソースコードをインクルードするすべての翻訳単位で :c:macro:`BOOST_REGEX_MATCH_EXTRA` を定義する（:file:`boost/regex/user.hpp` にシンボルの定義部分があるので、このコメントを解除するのが最もよい）。
* 実際に捕捉情報が必要な個々のアルゴリズムで :cpp:var:`!match_extra` フラグを渡す（:cpp:func:`!regex_search` 、:cpp:func:`!regex_match` 、:cpp:func:`!regex_iterator`）。
