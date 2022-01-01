.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


regex_replace
=============

::

   #include <boost/regex.hpp>

アルゴリズム :cpp:func:`regex_replace` は文字列を検索して正規表現に対するマッチをすべて発見する。さらに各マッチについて :cpp:func:`!match_results<>::format` を呼び出して文字列を書式化し、結果を出力イテレータに送る。マッチしなかったテキスト部分は :cpp:var:`!flags` 引数にフラグ :cpp:var:`!format_no_copy` が設定されていない場合に限り、変更を加えず出力にコピーする。フラグ :cpp:var:`!format_first_only` が設定されている場合は、すべてのマッチではなく最初のマッチのみ置換する。 ::

   template <class OutputIterator, class BidirectionalIterator, class traits, class Formatter>
   OutputIterator regex_replace(OutputIterator out,
                                BidirectionalIterator first,
                                BidirectionalIterator last,
                                const basic_regex<charT, traits>& e,
                                Formatter fmt,
                                match_flag_type flags = match_default);

   template <class traits, class Formatter>
   basic_string<charT> regex_replace(const basic_string<charT>& s,
                                     const basic_regex<charT, traits>& e,
                                     Formatter fmt,
                                     match_flag_type flags = match_default);


.. _ref.regex_replace.description:

説明
----

.. cpp:function:: template <class OutputIterator, class BidirectionalIterator, class traits, class Formatter> \
		  OutputIterator regex_replace(OutputIterator out, BidirectionalIterator first, BidirectionalIterator last, const basic_regex<charT, traits>& e, Formatter fmt, match_flag_type flags = match_default)

   シーケンス [first, last) 中の正規表現 :cpp:var:`!e` に対するすべてのマッチを列挙し、各マッチと書式化文字列 :cpp:var:`!fmt` をマージして得られる文字列で置換し、結果の文字列を :cpp:var:`!out` にコピーする。:cpp:var:`!fmt` が単項・二項・三項関数オブジェクトである場合、関数オブジェクトが生成した文字シーケンスは変更を加えられることなく出力にコピーされる。

   :cpp:var:`!flags` に :cpp:var:`!format_no_copy` が設定されている場合、マッチしなかったテキスト部分は出力にコピーされない。

   :cpp:var:`!flags` に :cpp:var:`!format_first_only` が設定されている場合、:cpp:var:`!e` の最初のマッチのみが置換される。

   書式化文字列 :cpp:var:`!fmt` およびマッチ検索に使用する規則は :cpp:var:`!flags` に設定されているフラグにより決定する。:cpp:type:`match_flag_type` を見よ。

   :要件:
      型 :cpp:type:`!Formatter` は :cpp:type:`!char_type[]` 型の null 終端文字列へのポインタ、:cpp:type:`!char_type` 型のコンテナ（例えば :cpp:class:`!std::basic_string<char_type>`）、あるいは関数呼び出しにより置換文字列を生成する単項・二項・三項関数子のいずれかでなければならない。関数子の場合、:cpp:expr:`fmt(what)` は置換テキストと使用する :cpp:type:`char_type` のコンテナを返さなければならず、:cpp:expr:`fmt(what, out)` および :cpp:expr:`fmt(what, out, flags)` はいずれも置換テキストを :cpp:expr:`*out` に出力し :cpp:type:`OutputIterator` の新しい位置を返さなければならない。以上において :cpp:var:`!what` は見つかったマッチを表す :cpp:class:`match_results` オブジェクトである。書式化オブジェクトが関数子の場合、\ **値渡しされる**\ことに注意していただきたい。関数オブジェクトを内部状態とともに渡す場合は、`Boost.Ref <http://www.boost.org/libs/ref.html>`_ を使ってオブジェクトが参照渡しされるようラップするとよい。

   :効果:
      :cpp:class:`regex_iterator` オブジェクトを構築し、 ::

         regex_iterator<BidirectionalIterator, charT, traits, Allocator>
                                                   i(first, last, e, flags),

      :cpp:var:`!i` を使ってシーケンス [first, last) 中のすべてのマッチ :cpp:var:`!m`\（:cpp:class:`match_results`\<BidirectionalIterator> 型）を列挙する。

      マッチが見つからず、かつ ::

         !(flags & format_no_copy)

      であれば、次を呼び出す。 ::

         std::copy(first, last, out)

      それ以外で ::

         !(flags & format_no_copy)

      であれば、 ::

         std::copy(m.prefix().first, m.prefix().last, out)

      を呼び出し、次を呼び出す。 ::

         m.format(out, fmt, flags)

      以上のいずれにも該当せず、 ::

         !(flags & format_no_copy)

      であれば、次を呼び出す。 ::

         std::copy(last_m.suffix().first, last_m,suffix().last, out)

      ただし :cpp:var:`!last_m` は最後に見つかったマッチのコピーである。

      :cpp:expr:`flags & format_first_only` が非 0 であれば、最初に見つかったマッチのみを置換する。

   :throws std\:\:runtime_error:
      長さ :samp:`{N}` の文字列に対して式のマッチの計算量が O(:samp:`{N}`\ :superscript:`2`) を超え始めた場合、正規表現のマッチ中にプログラムのスタック空間が枯渇した場合（Boost.Regex が再帰モードを使うように構成されているとき）、あるいはマッチオブジェクトが許可されているメモリ割り当てを消耗しきった場合（Boost.Regex が非再帰モードを使うように構成されているとき）。
   :returns: :cpp:var:`!out`。


.. cpp:function:: template <class traits, class Formatter> \
		  basic_string<charT> regex_replace(const basic_string<charT>& str, const basic_regex<charT, traits>& e, Formatter fmt, match_flag_type flags = match_default)

   :要件:
      型 :cpp:type:`!Formatter` は :cpp:type:`!char_type[]` 型の null 終端文字列へのポインタ、:cpp:type:`!char_type` 型のコンテナ（例えば :cpp:class:`!std::basic_string<char_type>`）、あるいは関数呼び出しにより置換文字列を生成する単項・二項・三項関数子のいずれかでなければならない。関数子の場合、:cpp:expr:`fmt(what)` は置換テキストと使用する :cpp:type:`!char_type` のコンテナを返さなければならず、:cpp:expr:`fmt(what, out)` および :cpp:expr:`fmt(what, out, flags)` はいずれも置換テキストを :cpp:expr:`*out` に出力し :cpp:type:`!OutputIterator` の新しい位置を返さなければならない。以上において :cpp:var:`!what` は見つかったマッチを表す :cpp:class:`match_results` オブジェクトである。
   :効果:
      オブジェクト :cpp:type:`!basic_string<charT>` :cpp:var:`!result` を構築し、:cpp:expr:`regex_replace(back_inserter(result), s.begin(), s.end(), e, fmt, flags)` を呼び出し、:cpp:var:`!result` を返す。


.. _ref.regex_replace.examples:

使用例
------

次の例は C/C++ ソースコードを入力として受け取り、構文強調した HTML コードを出力する。 ::

   #include <fstream>
   #include <sstream>
   #include <string>
   #include <iterator>
   #include <boost/regex.hpp>
   #include <fstream>
   #include <iostream>

   // 目的：
   // ファイルの内容を受け取り、構文強調した
   // HTML 形式に変換する

   boost::regex e1, e2;
   extern const char* expression_text;
   extern const char* format_string;
   extern const char* pre_expression;
   extern const char* pre_format;
   extern const char* header_text;
   extern const char* footer_text;

   void load_file(std::string& s, std::istream& is)
   {
      s.erase();
      s.reserve(is.rdbuf()->in_avail());
      char c;
      while(is.get(c))
      {
         if(s.capacity() == s.size())
            s.reserve(s.capacity() * 3);
         s.append(1, c);
      }
   }

   int main(int argc, const char** argv)
   {
      try{
         e1.assign(expression_text);
         e2.assign(pre_expression);
         for(int i = 1; i < argc; ++i)
         {
            std::cout << "次のファイルを処理中 " << argv[i] << std::endl;
            std::ifstream fs(argv[i]);
            std::string in;
            load_file(in, fs);
            std::string out_name(std::string(argv[i]) + std::string(".htm"));
            std::ofstream os(out_name.c_str());
            os << header_text;
            // 最初に一時文字列ストリームに出力して
            // '<' と '>' を取り去る
            std::ostringstream t(std::ios::out | std::ios::binary);
            std::ostream_iterator<char, char> oi(t);
            boost::regex_replace(oi, in.begin(), in.end(),
               e2, pre_format, boost::match_default | boost::format_all);
            // 次に最終的な出力ストリームに出力し
            // 構文強調を追加する：
            std::string s(t.str());
            std::ostream_iterator<char, char> out(os);
            boost::regex_replace(out, s.begin(), s.end(),
               e1, format_string, boost::match_default | boost::format_all);
            os << footer_text;
         }
      }
      catch(...)
      { return -1; }
      return 0;
   }

   extern const char* pre_expression = "(<)|(>)|(&)|\\r";
   extern const char* pre_format = "(?1<)(?2>)(?3&)";


   const char* expression_text =
      // プリプロセッサディレクティブ：添字 1
      "(^[[:blank:]]*#(?:[^\\\\\\n]|\\\\[^\\n[:punct:][:word:]]*[\\n[:punct:][:word:]])*)|"
      // 注釈：添字 2
      "(//[^\\n]*|/\\*.*?\\*/)|"
      // 直値：添字 3
      "\\<([+-]?(?:(?:0x[[:xdigit:]]+)|(?:(?:[[:digit:]]*\\.)?[[:digit:]]+"
      "(?:[eE][+-]?[[:digit:]]+)?))u?(?:(?:int(?:8|16|32|64))|L)?)\\>|"
      // 文字列直値：添字 4
      "('(?:[^\\\\']|\\\\.)*'|\"(?:[^\\\\\"]|\\\\.)*\")|"
      // キーワード：添字 5
      "\\<(__asm|__cdecl|__declspec|__export|__far16|__fastcall|__fortran|__import"
      "|__pascal|__rtti|__stdcall|_asm|_cdecl|__except|_export|_far16|_fastcall"
      "|__finally|_fortran|_import|_pascal|_stdcall|__thread|__try|asm|auto|bool"
      "|break|case|catch|cdecl|char|class|const|const_cast|continue|default|delete"
      "|do|double|dynamic_cast|else|enum|explicit|extern|false|float|for|friend|goto"
      "|if|inline|int|long|mutable|namespace|new|operator|pascal|private|protected"
      "|public|register|reinterpret_cast|return|short|signed|sizeof|static|static_cast"
      "|struct|switch|template|this|throw|true|try|typedef|typeid|typename|union|unsigned"
      "|using|virtual|void|volatile|wchar_t|while)\\>"
      ;

   const char* format_string = "(?1<font color=\"#008040\">$&</font>)"
                               "(?2<I><font color=\"#000080\">$&</font></I>)"
                               "(?3<font color=\"#0000A0\">$&</font>)"
                               "(?4<font color=\"#0000FF\">$&</font>)"
                               "(?5<B>$&</B>)";

   const char* header_text =
      "<HTML>\n<HEAD>\n"
      "<TITLE>Auto-generated html formated source</TITLE>\n"
      "<META HTTP-EQUIV=\"Content-Type\Type\" CONTENT=\"text/html; charset=windows-1252\">\n"
      "</HEAD>\n"
      "<BODY LINK=\"#0000ff\" VLINK=\"#800080\" BGCOLOR=\"#ffffff\">\n"
      "<P> </P>\n<PRE>";

   const char* footer_text = "</PRE>\n</BODY>\n\n";
