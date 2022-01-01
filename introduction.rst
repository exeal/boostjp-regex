.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


導入と概要
==========

正規表現はテキスト処理でよく使われるパターンマッチの形式である。Unix ユーティリティの grep 、sed 、awk やプログラミング言語 Perl といった正規表現を広範にわたって利用しているツールに馴染みのあるユーザは多い。古くから C++ ユーザが正規表現処理を行う方法は POSIX C API に限られていた。Boost.Regex もこれらの API を提供するが、本ライブラリの最適な利用法ではない。例えば Boost.Regex はワイド文字列に対応している。また従来の C ライブラリでは不可能だった（sed や Perl で使用されているような）検索や置換も可能である。

:cpp:class:`basic_regex` は本ライブラリのキーとなるクラスであり、「マシン可読の」正規表現を表す。正規表現パターンは文字列であると同時に、正規表現アルゴリズムに要求される状態マシンでもあるという観点から、:cpp:class:`!std::basic_string` に非常に近いモデリングになっている。:cpp:class:`!std::basic_string` と同様に、このクラスを参照するのにほとんど常に使われる :code:`typedef` が 2 つある。 ::

   namespace boost {

   template <class charT,
                class traits = regex_traits<charT> >
   class basic_regex;

   typedef basic_regex<char> regex;
   typedef basic_regex<wchar_t> wregex;

   }

このライブラリの使い方を見るために、クレジットカードを処理するアプリケーションを考える。クレジットカードの番号は、通常 16 桁の数字が空白かハイフンで 4 桁ずつのグループに分けられた文字列になっている。クレジットカードの番号をデータベースに格納する前に（顧客にとってはどうでもいいことだろうが）番号が正しい形式になっているか確認したいと思う。あらゆる（10 進）数字にマッチする正規表現として :regexp:`[0-9]` が使えるが、このような文字の範囲は実際にはロカール依存である。代わりに使用すべきなのは POSIX 標準形式の :regexp:`[[:digit:]]` か、Boost.Regex および Perl での短縮形である :regexp:`\d` となる（古いライブラリは C ロカールについてハードコードされていることが多く、結果的に問題となっていなかったことに注意していただきたい）。以上のことから、次の正規表現を使えばクレジットカードの番号形式を検証できる。

.. code-block:: none

   (\d{4}[- ]){3}\d{4}

式中の括弧は部分式のグループ化（および後で参照するためのマーク付け）を行い、:regexp:`{4}` は「4 回ちょうどの繰り返し」を意味する。これは Perl 、awk および egrep で使われている拡張正規表現構文の例である。Boost.Regex は sed や grep で使われている古い「基本的な」構文もサポートしているが、基本的な正規表現がすでにあって再利用しようと考えているのでなければ、通常はあまり役に立たない。

では、この式を使ってクレジットカード番号を検証する C++ コードを書いてみる。 ::

   bool validate_card_format(const std::string& s)
   {
      static const boost::regex e("(\\d{4}[- ]){3}\\d{4}");
      return regex_match(s, e);
   }

式にエスケープが追加されていることに注意していただきたい。エスケープは正規表現エンジンに処理される前に C++ コンパイラによって処理されるため、結局、正規表現中のエスケープは C/C++ コードでは二重にしなければならない。また、本文書の例はすべてコンパイラが引数依存の名前探索をサポートしているものとしているという点に注意していただきたい。未サポートのコンパイラ（VC6 など）では関数呼び出しの前に :code:`boost::` を付けなければならない場合がある。

クレジットカードの処理に詳しい人であれば、上の形式が人にとって可読性は高いものの、オンラインのクレジットカードシステムに適した形式になっていないと気づくと思う。こういうシステムでは、間に空白の入らない 16 桁（あるいは 15 桁）の文字列を使う。ここで必要なのは 2 つの形式を簡単に交換する方法であり、こういう場合に検索と置換を使う。Perl や sed といったユーティリティに詳しいのであれば、この部分は読み飛ばしてもらって構わない。ここで必要なのは 2 つの文字列である。1 つは正規表現であり、もう1つはマッチしたテキストをどのように置換するか指定する「書式化文字列」である。Boost.Regex でこの検索・置換操作はアルゴリズム :cpp:func:`regex_replace` で行う。今のクレジットカードの例では形式を変換するアルゴリズムを 2 つ記述できる。 ::

   // いずれの形式にもマッチする正規表現：
   const boost::regex e("\\A(\\d{3,4})[- ]?(\\d{4})[- ]?(\\d{4})[- ]?(\\d{4})\\z");
   const std::string machine_format("\\1\\2\\3\\4");
   const std::string human_format("\\1-\\2-\\3-\\4");

   std::string machine_readable_card_number(const std::string s)
   {
      return regex_replace(s, e, machine_format, boost::match_default | boost::format_sed);
   }

   std::string human_readable_card_number(const std::string s)
   {
      return regex_replace(s, e, human_format, boost::match_default | boost::format_sed);
   }

カード番号の 4 つの部分を別々のフィールドに分けるのに、正規表現中でマーク済み部分式を用いた。形式化文字列では、マッチしたテキストを置換するのに sed ライクの構文を使っている。

上の例では正規表現マッチの結果を直接操作することはしなかったが、通常はマッチ結果にはマッチ全体だけでなく部分マッチに関する情報が含まれている。必要な場合はライブラリの :cpp:class:`match_results` クラスのインスタンスを使うとよい。先ほどと同様に、実際に使用する場合は typedef を使うとよい。 ::

   namespace boost{

   typedef match_results<const char*> cmatch;
   typedef match_results<const wchar_t*> wcmatch;
   typedef match_results<std::string::const_iterator> smatch;
   typedef match_results<std::wstring::const_iterator> wsmatch;

   }

アルゴリズム :cpp:func:`regex_search` および :cpp:func:`regex_match` は、:cpp:class:`match_results` を使ってどこがマッチしたかを返す。2 つのアルゴリズムの違いは :cpp:func:`regex_match` が入力テキスト\ **全体**\に対するマッチを検索するのみであるに対し、:cpp:func:`regex_search` は入力テキスト中のあらゆる位置のマッチを検索するということである。

これらのアルゴリズムが、通常の C 文字列の検索に限定されていないことに注意していただきたい。双方向イテレータであれば何でも検索可能であり、ほとんどあらゆる種類のデータにシームレスに対応している。

検索・置換操作については、すでに見た :cpp:func:`regex_replace` に加えて :cpp:class:`match_results` クラスに :cpp:func:`~match_results::format` メンバがある。このメンバ関数はマッチ結果と書式化文字列を取り、この 2 つをマージして新文字列を生成する。

テキスト中のマッチ結果をすべて走査するイテレータ型が 2 つある。:cpp:class:`regex_iterator` は見つかった :cpp:class:`match_results` オブジェクトを列挙する。一方 :cpp:class:`regex_token_iterator` は文字列の列を列挙する（Perl スタイルの分割操作に似ている）。

テンプレートを使うのが嫌な人には、低水準のテンプレートコードをカプセル化した高水準のラッパクラス :cpp:class:`RegEx` がある。ライブラリの全機能は必要ないという人向けの簡単なインターフェイスとなっており、ナロー文字と「拡張」正規表現構文のみをサポートする。正規表現 C++ 標準ライブラリの草案には含まれておらず、現在は非推奨である。

POSIX API 関数 :cpp:func:`regcomp` 、:cpp:func:`regexec` 、:cpp:func:`regfree` および :cpp:func:`regerror` はナロー文字および Unicode 版の両方で利用可能であり、これらの API との互換性が必要な場合のために提供している。

最後に、本ライブラリは GNU および BSD4 の regex パッケージや PCRE 、Perl 5 といった正規表現ライブラリの他に、:doc:`実行時の地域化 <locale>`\と完全な POSIX 正規表現構文もサポートしている。これには複数文字の照合要素や等価クラスのような発展的な機能も含まれている。
