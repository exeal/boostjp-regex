.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


部分マッチ
==========

アルゴリズム :cpp:func:`regex_match` 、:cpp:func:`regex_search` 、:cpp:func:`regex_grep` およびイテレータ :cpp:class:`regex_iterator` で使用可能な :cpp:type:`match_flag_type` に :cpp:var:`~match_flag_type::match_partial` がある。このフラグを使うと完全マッチだけでなく部分マッチも検索される。部分マッチは入力テキストの終端 1 文字以上にマッチしたが、正規表現全体にはマッチしなかった（が、さらに入力が追加されれば全体にマッチする可能性のある）場合である。部分マッチを使用する典型的な場合として、入力データの検証（キーボードから文字が入力されるたびにチェックする場合）や、テキスト検索においてテキストがメモリ（あるいはメモリマップドファイル）に読み込めないほど非常に長いか、（ソケットなどから読み込むため）長さが不確定な場合がある。部分マッチと完全マッチの違いを以下の表に示す（変数 :cpp:var:`!M` は :cpp:class:`match_results` のインスタンスであり、:cpp:func:`regex_match` 、:cpp:func:`regex_search` 、:cpp:func:`regex_grep` のいずれかの結果が入っているとする）。

.. list-table::
   :header-rows: 1

   * - 
     - 結果
     - M[0].matched
     - M[0].first
     - M[0].second
   * - マッチしなかった場合
     - 偽
     - 未定義
     - 未定義
     - 未定義
   * - 部分マッチ
     - 真
     - 偽
     - 部分マッチの先頭
     - 部分マッチの終端（テキストの終端）
   * - 完全マッチ
     - 真
     - 真
     - 完全マッチの先頭
     - 完全マッチの終端

部分マッチはいささか不完全な振る舞いをする場合があることに注意していただきたい。

* :regexp:`.*abc` のようなパターンは常に部分マッチを生成する。この問題を軽減して使用するには、正規表現を注意深く構築するか、:cpp:var:`!match_not_dot_newline` のようなフラグを設定して :regexp:`.*` といったパターンが前の行境界にマッチしないようにする。
* 現時点では Boost.Regex は完全マッチに対して最左マッチを採用しているため、:regex-input:`ab` に対して :regexp:`abc|b` でマッチをかけると :regex-input:`b` に対する完全マッチではなく :regex-input:`ab` に対する部分マッチが得られる。
* 部分マッチが成立するにも関わらず完全マッチが見つかる場合がある。例えば部分文字列の末尾が :regex-input:`abc` で正規表現が :regexp:`\w+` であれば、後続にアルファベット文字が現れる可能性があるにも関わらず完全マッチが見つかる。マッチが現在の入力文字列の終端で見つかるか調べれば、今回のケースを検出可能である。ただし不可能な場合もあり、例えば :regexp:`abc.*123` のような正規表現は（それがどれだけ長くなるかに関わらず）入力文字列全体にマッチするだろうから、常により長いマッチとなる。

次の例は、テキストが正しいクレジットカード番号となり得るかを、調べる。ユーザが打鍵して入力された文字が文字列に追加されるたびに、文字列を :cpp:func:`!is_possible_card_number` に渡すという使い方を想定している。この手続きが真を返す場合、テキストは正しいクレジットカード番号であり、ユーザインターフェイスの OK ボタンを有効にする。偽を返す場合、テキストはまだ正しいカード番号になっていないが、さらに入力があれば正しい番号となるため、ユーザインターフェイスの OK ボタンを無効にする。最後に手続きが例外を投げる場合は、入力が正しい番号となる可能性が無いため、入力テキストを破棄して適切なエラーをユーザに表示しなければならない。 ::

   #include <string>
   #include <iostream>
   #include <boost/regex.hpp>

   boost::regex e("(\\d{3,4})[- ]?(\\d{4})[- ]?(\\d{4})[- ]?(\\d{4})");

   bool is_possible_card_number(const std::string& input)
   {
      //
      // 部分マッチに対しては偽、完全マッチに対しては真を返す。
      // マッチの可能性がない場合は例外を投げる…
      boost::match_results<std::string::const_iterator> what;
      if(0 == boost::regex_match(input, what, e, boost::match_default | boost::match_partial))
      {
         // 入力が正しい形式となる可能性はなくなったので拒絶する：
         throw std::runtime_error(
            "不正なデータが入力されました - 追加の入力があっても正しい番号となる可能性はありません");
      }
      // OK 、今のところはよろしい。だが、入力はこれで終わりだろうか？
      if(what[0].matched)
      {
         // 素晴らしい。正しい結果が得られた：
         return true;
      }
      // この時点では部分的にマッチしただけ…
      return false;
   }

次の例では、入力テキストは長さが未知であるストリームから取得する。この例は単純にストリーム中で見つかった HTML タグの数を数える。テキストはバッファに読み込まれ、1 度に一部分だけを検索する。部分マッチが見つかった場合、さらにその部分マッチを次のテキスト群の先頭として検索を行う。 ::

   #include <iostream>
   #include <fstream>
   #include <sstream>
   #include <string>
   #include <boost/regex.hpp>

   // HTML タグにマッチする：
   boost::regex e("<[^>]*>");
   // タグの数：
   unsigned int tags = 0;

   void search(std::istream& is)
   {
      // 検索するバッファ：
      char buf[4096];
      // 部分マッチの先頭位置を保存：
      const char* next_pos = buf + sizeof(buf);
      // 入力がまだあるかを示すフラグ：
      bool have_more = true;

      while(have_more)
      {
         // 前回の試行から何文字コピーするか：
         unsigned leftover = (buf + sizeof(buf)) - next_pos;
         // および、ストリームから何文字読み込むか：
         unsigned size = next_pos - buf;
         // 前回残っていた部分をバッファの先頭にコピー：
         std::memmove(buf, next_pos, leftover);
         // 残りをストリームからの入力で埋める：
         is.read(buf + leftover, size);
         unsigned read = is.gcount();
         // テキストをすべて走査したかチェック：
         have_more = read == size;
         // next_pos をリセット：
         next_pos = buf + sizeof(buf);
         // 走査を行う：
         boost::cregex_iterator a(
            buf,
            buf + read + leftover,
            e,
            boost::match_default | boost::match_partial);
         boost::cregex_iterator b;

         while(a != b)
         {
            if((*a)[0].matched == false)
            {
               // 部分マッチ。位置を保存しループを脱出：
               next_pos = (*a)[0].first;
               break;
            }
            else
            {
               // 完全マッチ：
               ++tags;
            }

            // 次のマッチへ移動：
            ++a;
         }
      }
   }
