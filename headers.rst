.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).


ヘッダ
======

このライブラリで使用する主なヘッダは 2 つある。:file:`<boost/regex.hpp>` が主要なテンプレートライブラリへの完全なアクセスを提供するのに対し、:file:`<boost/cregex.hpp>` は（非推奨の）高水準クラス :cpp:class:`!RegEx` と POSIX API 関数へのアクセスを提供する。

インターフェイスが :cpp:class:`basic_regex` に依存するものの他に完全な定義が必要ない場合に使用する、前方宣言だけが入ったヘッダ :file:`<boost/regex_fwd.hpp>` もある。
