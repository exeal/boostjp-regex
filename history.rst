.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


履歴
====

新規項目は `svn.boost.org <http://svn.boost.org/>`_ に提出してほしい。チケットにはあなたのメールアドレスを入れておく必要がある。

現在オープンな項目は\ `ここ <https://svn.boost.org/trac/boost/query?status=assigned&status=new&status=reopened&component=regex&order=priority&col=id&col=summary&col=status&col=type&col=milestone&col=component>`_\から見られる。

クローズなものを含めた全項目は\ `ここ <https://svn.boost.org/trac/boost/query?status=assigned&status=closed&status=new&status=reopened&component=regex&order=priority&col=id&col=summary&col=status&col=type&col=milestone&col=component>`_\から見られる。


.. _background.history.boost_regex_5_1_3_boost_1_64_0:

Boost.Regex-5.1.3（Boost-1.64.0）
---------------------------------

* Oracle C++ でのコンパイルが静的リンクに限定されていたのを修正した。
* libFuzzer を使ったファジング対応と見つかった不具合の修正（`#12818 <https://svn.boost.org/trac/boost/ticket/12818>`_）。


.. _background.history.boost_regex_5_1_2_boost_1_62_0:

Boost.Regex-5.1.2（Boost-1.62.0）
---------------------------------

* 特定の正規表現を解析したときに発生するバッファオーバーランを修正した（`#12222 <https://svn.boost.org/trac/boost/ticket/12222>`_）。
* ライブラリビルドにおける ICU の検出方法を修正した（`#12152 <https://svn.boost.org/trac/boost/ticket/12152>`_）。
* 大文字小文字の区別切り替えにおけるバグを修正した（`#11940 <https://svn.boost.org/trac/boost/ticket/11940>`_）。
* :regexp:`\\x{}` 式内で（:cpp:type:`!char32_t` 等の）\ :cpp:type:`!int` より広い型を使えるようにした（`#11988 <https://svn.boost.org/trac/boost/ticket/11988>`_）。


.. _background.history.boost_regex_5_1_1_boost_1_61_0:

Boost.Regex-5.1.1（Boost-1.61.0）
---------------------------------

* メモリキャッシュをロックフリーな実装に切り替えた。`PR#23 <https://github.com/boostorg/regex/pull/23>`_ を見よ。


.. _background.history.boost_regex_5_1_0_boost_1_60_0:

Boost.Regex-5.1.0（Boost-1.60.0）
---------------------------------

* Perl のバックトラッキング動詞をサポートした（`#11205 <https://svn.boost.org/trac/boost/ticket/11205>`_）。ただし現時点では :regexp:`(*MARK)` とマークに対する操作はサポートしていないことに注意していただきたい。
* :regexp:`[[:unicode:]]` にマッチする範囲の末尾が間違っていたのを修正した（`#11524 <https://svn.boost.org/trac/boost/ticket/11524>`_）。
* :cpp:func:`!reg_comp` POSIX API を未初期化のメモリをチェックしないように変更した。これまで（:cpp:func:`!reg_free` を呼び出さず）メモリリークしなかったコード（結局は不正なのだが）でリークが発生することに注意していただきたい（`#11472 <https://svn.boost.org/trac/boost/ticket/11472>`_）。
* :cpp:class:`!sub_match` を合法な C++ 範囲型にした（`#11036 <https://svn.boost.org/trac/boost/ticket/11036>`_）。


.. _background.history.boost_regex_5_0_1__boost_1_58_0_:

Boost.Regex-5.0.1（Boost-1.58.0）
---------------------------------

* 誤字を修正した（`#10682 <https://svn.boost.org/trac/boost/ticket/10682>`_）。
* Coverity の警告について\ `プルリクエスト #6 <https://github.com/boostorg/regex/pull/6>`_ をマージした。
* Coverity の警告について\ `プルリクエスト #7 <https://github.com/boostorg/regex/pull/7>`_ をマージした。
* Coverity の警告について\ `プルリクエスト #8 <https://github.com/boostorg/regex/pull/8>`_ をマージした。
* ICU にリンクする場合により多くのビルドバリアントを可能にするため\ `プルリクエスト #10 <https://github.com/boostorg/regex/pull/10>`_ をマージした。
* ICU と部分マッチを組み合わせたときに発生するバグを修正した（`#10114 <https://svn.boost.org/trac/boost/ticket/10114>`_）。
* ICU ライブラリの遅延ロードサポートを削除した。遅延ロードは最新の ICU リリースでは（リンカエラーにより）動作しない。


.. _background.history.boost_regex_5_0_0__boost_1_56_0_:

Boost.Regex-5.0.0（Boost-1.56.0）
---------------------------------

* Git へ移行後、ライブラリ固有のバージョン番号を使用することにした。また、破壊的変更が1つあったためバージョン 4 からバージョン 5 となった。
* **破壊的変更：**:cpp:func:`!basic_regex<>::mark_count()` の挙動を既存のドキュメントと一致するよう修正した。同時に :cpp:func:`!basic_regex<>::subexpression(n)` がマッチするよう変更した。`#9227 <https://svn.boost.org/trac/boost/ticket/9227>`_ を見よ。
* チケット `#8903 <https://svn.boost.org/trac/boost/ticket/8903>`_ を修正した。
* ドキュメントの誤字を修正した（`#9283 <https://svn.boost.org/trac/boost/ticket/9283>`_）。
* 照合コードについて、ロカールが NUL を含む照合文字列を生成した場合に失敗するバグを修正した。`#9451 <https://svn.boost.org/trac/boost/ticket/9451>`_ を見よ。
* まれなスレッドの使用方法（静的に初期化されていないミューテックス）に対するパッチを適用した。`#9461 <https://svn.boost.org/trac/boost/ticket/9461>`_ を見よ。
* 不正な UTF-8 シーケンスに対するチェック機能を改善した。`#9473 <https://svn.boost.org/trac/boost/ticket/9473>`_ を見よ。


.. _background.history.boost_1_54:

Boost-1.54
----------

* 以下のチケットの修正：`#8569 <https://svn.boost.org/trac/boost/ticket/8569>`_。


.. _background.history.boost_1_53:

Boost-1.53
----------

* 以下のチケットの修正：`#7744 <https://svn.boost.org/trac/boost/ticket/7744>`_ 、`#7644 <https://svn.boost.org/trac/boost/ticket/7644>`_。


.. _background.history.boost_1_51:

Boost-1.51
----------

* 以下のチケットの修正：`#589 <https://svn.boost.org/trac/boost/ticket/589>`_ 、`#7084 <https://svn.boost.org/trac/boost/ticket/7084>`_ 、`#7032 <https://svn.boost.org/trac/boost/ticket/7032>`_ 、`#6346 <https://svn.boost.org/trac/boost/ticket/6346>`_。


.. _background.history.boost_1_50:

Boost-1.50
----------

* :regexp:`(?!)` が正しい式とならない問題を修正し、正しい条件式の構成要素についてドキュメントを更新した。


.. _background.history.boost_1_48:

Boost-1.48
----------

* 以下のチケットの修正：`#698 <https://svn.boost.org/trac/boost/ticket/698>`_ 、`#5835 <https://svn.boost.org/trac/boost/ticket/5835>`_ 、`#5958 <https://svn.boost.org/trac/boost/ticket/5958>`_ 、`#5736 <https://svn.boost.org/trac/boost/ticket/5736>`_。


.. _background.history.boost_1_47:

Boost 1.47
----------

* 以下のチケットの修正：`#5223 <https://svn.boost.org/trac/boost/ticket/5223>`_ 、`#5353 <https://svn.boost.org/trac/boost/ticket/5353>`_ 、`#5363 <https://svn.boost.org/trac/boost/ticket/5363>`_ 、`#5462 <https://svn.boost.org/trac/boost/ticket/5462>`_ 、`#5472 <https://svn.boost.org/trac/boost/ticket/5472>`_ 、`#5504 <https://svn.boost.org/trac/boost/ticket/5504>`_。


.. _background.history.boost_1_44:

Boost 1.44
----------

* 以下のチケットの修正：`#4309 <https://svn.boost.org/trac/boost/ticket/4309>`_ 、`#4215 <https://svn.boost.org/trac/boost/ticket/4215>`_ 、`#4212 <https://svn.boost.org/trac/boost/ticket/4212>`_ 、`#4191 <https://svn.boost.org/trac/boost/ticket/4191>`_ 、`#4132 <https://svn.boost.org/trac/boost/ticket/4132>`_ 、`#4123 <https://svn.boost.org/trac/boost/ticket/4123>`_ 、`#4114 <https://svn.boost.org/trac/boost/ticket/4114>`_ 、`#4036 <https://svn.boost.org/trac/boost/ticket/4036>`_ 、`#4020 <https://svn.boost.org/trac/boost/ticket/4020>`_ 、`#3941 <https://svn.boost.org/trac/boost/ticket/3941>`_ 、`#3902 <https://svn.boost.org/trac/boost/ticket/3902>`_ 、`#3890 <https://svn.boost.org/trac/boost/ticket/3890>`_。


.. _background.history.boost_1_42:

Boost 1.42
----------

* 書式化式として文字列だけでなく関数子も受け付けるようにした。
* 例外を投げたときに、より適切な情報を含めてエラー報告を強化した。
* 再帰式を使用した場合の効率が上がり、スタックの使用量が減少した。
* 以下のチケットの修正：`#2802 <https://svn.boost.org/trac/boost/ticket/2802>`_ 、`#3425 <https://svn.boost.org/trac/boost/ticket/3425>`_ 、`#3507 <https://svn.boost.org/trac/boost/ticket/3507>`_ 、`#3546 <https://svn.boost.org/trac/boost/ticket/3546>`_ 、`#3631 <https://svn.boost.org/trac/boost/ticket/3631>`_ 、`#3632 <https://svn.boost.org/trac/boost/ticket/3632>`_ 、`#3715 <https://svn.boost.org/trac/boost/ticket/3715>`_ 、`#3718 <https://svn.boost.org/trac/boost/ticket/3718>`_ 、`#3763 <https://svn.boost.org/trac/boost/ticket/3763>`_ 、`#3764 <https://svn.boost.org/trac/boost/ticket/3764>`_。


.. _background.history.boost_1_40:

Boost 1.40
----------

* 名前付き部分式、選択分岐による部分式番号のリセット、再帰正規表現といった Perl 5.10 の構文要素の多くを追加した。


.. _background.history.boost_1_38:

Boost 1.38
----------

* **破壊的な変更：**\Perl の正規表現構文で空の正規表現および空の選択を許容するようにした。この変更は Perl との互換性のためのものである。新しい :cpp:type:`syntax_option_type` である :cpp:var:`!no_empty_expressions` が設定されていれば以前の挙動となり、空の式は許可されない。チケット `#1081 <https://svn.boost.org/trac/boost/ticket/1081>`_ にもとづいている。
* 書式化文字列において Perl 形式の :regexp:`${n}` 式をサポートした（チケット `#2556 <https://svn.boost.org/trac/boost/ticket/2556>`_）。
* 正規表現文字列内の部分式の位置へのアクセスをサポートした（チケット `#2269 <https://svn.boost.org/trac/boost/ticket/2269>`_）。
* コンパイラ互換性について修正を行った（チケット `#2244 <https://svn.boost.org/trac/boost/ticket/2244>`_ 、`#2514 <https://svn.boost.org/trac/boost/ticket/2514>`_ および `#2458 <https://svn.boost.org/trac/boost/ticket/2458>`_）。


.. _background.history.boost_1_34:

Boost 1.34
----------

* 貪欲でない繰り返しと部分マッチが場合によっては正常に動作しないのを修正した。
* 貪欲でない繰り返しが VC++ で場合によっては正常に動作しないのを修正した（バグレポート 1515830）。
* :cpp:expr:`*this` が部分マッチを表しているときに :cpp:func:`!match_results::position()` が意味のある結果を返すように変更した。
* 改行文字が :regexp:`|` と同様に扱われるように :cpp:var:`!grep` および :cpp:var:`!egrep` オプションを修正した。


.. _background.history.boost_1_33_1:

Boost 1.33.1
------------

* メイクファイルが壊れていたのを修正した。
* VC7.1 + STLport-4.6.2 で :option:`!/Zc:wchar_t` を使用してビルドできるように設定セットアップを修正した。
* SGI Irix コンパイラが対処できるように、:file:`static_mutex.hpp` のクラスインラインの宣言を移動した。
* 必要な標準ライブラリの :code:`#include` を :file:`fileiter.hpp` 、:file:`regex_workaround.hpp` および :file:`cpp_regex_traits.hpp` に追加した。
* 貪欲でない繰り返しが奇妙な事情により最大値よりも多く繰り返す場合があったのを修正した。
* デフォルトコンストラクタで構築したオブジェクトが :cpp:func:`!basic_regex<>::empty()` で返す値を修正した。
* :cpp:class:`!regex_error` の定義を Boost-1.32.0 と後方互換になるように変更した。
* Intel C++ 8.0 未満で外部テンプレートを無効にした。未解決の参照が発生していた。
* gcc で特定のメンバ関数だけがエクスポートされるように extern なテンプレートコードを書き直した。リンク時にデバッグ用コードと非デバッグコードを混ぜたときに奇妙な未解決の参照が発生していた。
* Unicode イテレータのメンバを初期化するようにした。gcc で不要な警告が出なくなった。
* ICU 関連のコードを VC6 と VC7 に移植した。
* STLport のデバッグモードをクリーン化した（？）。
* 後読み表明を固定長さの繰り返しが使えるように、また反復時に後読みが現在の検索範囲の前に（前回のマッチに）遡れるように修正した。
* 前方先読み内の貪欲でない繰り返しに関する奇妙なバグを修正した。
* 文字集合内で文字クラスの否定が使えるようにした。
* :regexp:`[a-z-]` を再び正しい正規表現として退行テストを修正した。
* いくつか不正な式を受け付けていたバグを修正した。


.. _background.history.boost_1_33_0:

Boost 1.33.0
------------

* 式の解析コードを完全に書き直し、特性クラスのサポートを追加した。これにより標準草案に適合した。
* 破壊的な変更：:cpp:class:`!basic_regex` コンストラクタに渡す構文オプションを合理化した。既定のオプション（:cpp:var:`!perl`）が値 0 となり、どの正規表現構文スタイル（Perl 、POSIX 拡張、POSIX 基本など）にどのオプションを適用できるか明確に文書化した。
* 破壊的な変更：POSIX 拡張正規表現および POSIX 基本正規表現が以前よりも厳密に POSIX 標準に従うようになった。
* :regexp:`(?imsx-imsx)` 構造のサポートを追加した。
* 先読みの式 :regexp:`(?<=positive-lookbehind)` および :regexp:`(?<!negative-lookbehind)` のサポートを追加した。
* :regexp:`(?(assertion)true-expression|false-expression)` のサポートを追加した。
* MFC/ATL 文字列のラッパを追加した。
* Unicode サポートを追加した。ICU を使用している。
* 改行のサポートについて、:regexp:`\\f` を行区切り（あらゆる文字型で）、:regexp:`\\x85` をワイド文字の行区切り（Unicode のみ）として処理するように変更した。
* 置換文字列を Perl や Sed スタイルの書式化文字列ではなく直値として扱う、新しい書式化フラグ :cpp:var:`!format_literal` を追加した。
* エラーの通知を :cpp:class:`!regex_error` 型の例外で表現するようになった。以前使用していた型 :cpp:class:`!bad_expression` および :cpp:class:`!bad_pattern` は :cpp:class:`!regex_error` に対する typedef でしかなくなった。:cpp:class:`!regex_error` 型は新しい 2 、3 のメンバを持つ。:cpp:func:`!code()` は文字列ではなくエラーコードを返し、:cpp:func:`!position()` は式中のエラーの発生位置を返す。


.. _background.history.boost_1_32_1:

Boost 1.32.1
------------

* :regexp:`.` の境界付き繰り返しの部分マッチに関するバグを修正した。


.. _background.history.boost_1_31_0:

Boost 1.31.0
------------

* パターンマッチのコードを完全に書き直した。以前よりも 10 倍速くなった。
* ドキュメントを再編成した。
* 正規表現標準草案にないインターフェイスをすべて非推奨とした。
* :cpp:class:`!regex_iterator` と :cpp:class:`regex_token_iterator` を追加した。
* Perl スタイルの独立部分式のサポートを追加。
* :cpp:class:`!sub_match` クラスに非メンバ演算子を追加した。これにより :cpp:class:`!sub_match` の文字列との比較、および文字列への追加による新文字列の生成が可能になった。
* 拡張的な捕捉情報に対する実験的なサポートを追加した。
* マッチフラグの型を（整数でない別の型に）変更した。マッチフラグを :cpp:type:`!match_flag_type` ではなく整数としてアルゴリズムに渡そうとするとコンパイルエラーとなるようになった。
