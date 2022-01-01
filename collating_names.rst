.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

照合名
======

.. _syntax.collating_names.digraphs:

二重字
------

照合名として使用可能な二重字は以下のとおりである。

“ae” 、“Ae” 、“AE” 、“ch” 、“Ch” 、“CH” 、“ll” 、“Ll” 、“LL” 、“ss” 、“Ss” 、“SS” 、“nj” 、“Nj” 、“NJ” 、“dz” 、“Dz” 、“DZ” 、“lj” 、“Lj” および “LJ”。

例えば次の正規表現は、

.. code-block:: none

   [[.ae.]-c]

照合順が “ae” と “c” の間となるあらゆる文字にマッチする。


.. _syntax.collating_names.posix_symbolic_names:

POSIX シンボル名
----------------

単一文字に加えて以下の表のシンボル名は照合要素名として利用可能である。これにより :regex-input:`[` か :regex-input:`]` にマッチさせたい場合に、例えば次のように書くことができる。

.. code-block:: none

   [[.left-square-bracket.][.right-square-bracket.]]

.. table::

   ====================  ====
   名前                  文字
   ====================  ====
   NUL                   \\x00
   SOH                   \\x01
   STX                   \\x02
   ETX                   \\x03
   EOT                   \\x04
   ENQ                   \\x05
   ACK                   \\x06
   alert                 \\x07
   backspace             \\x08
   tab                   \\t
   newline               \\n
   vertical-tab          \\v
   form-feed             \\f
   carriage-return       \\r
   SO                    \\xE
   SI                    \\xF
   DLE                   \\x10
   DC1                   \\x11
   DC2                   \\x12
   DC3                   \\x13
   DC4                   \\x14
   NAK                   \\x15
   SYN                   \\x16
   ETB                   \\x17
   CAN                   \\x18
   EM                    \\x19
   SUB                   \\x1A
   ESC                   \\x1B
   IS4                   \\x1C
   IS3                   \\x1D
   IS2                   \\x1E
   IS1                   \\x1F
   space                 \\x20
   exclamation-mark      !
   quotation-mark        "
   number-sign           #
   dollar-sign           $
   percent-sign          %
   ampersand             &
   apostrophe            '
   left-parenthesis      (
   right-parenthesis     )
   asterisk              \*
   plus-sign             \+
   comma                 ,
   hyphen                \-
   period                .
   slash                 /
   zero                  0
   one                   1
   two                   2
   three                 3
   four                  4
   five                  5
   six                   6
   seven                 7
   eight                 8
   nine                  9
   colon                 :
   semicolon             ;
   less                  <
   equals-sign           =
   greater-than-sign     >
   question-mark         ?
   commercial-at         @
   left-square-bracket   [
   backslash             \\
   right-square-bracket  ]
   circumflex            ^
   underscore            _
   grave-accent          \`
   left-curly-bracket    {
   vertical-line         \|
   right-curly-bracket   }
   tilde                 ~
   DEL                   \\x7F
   ====================  ====


.. _syntax.collating_names.named_unicode:

名前付き Unicode 文字
---------------------

（:cpp:type:`!u32regex` 型を用いて）Unicode 正規表現を使用すると、Unicode 文字の通常のシンボル名（:file:`Unidata.txt` にデータがある）が考慮される。よって、例えば

.. code-block:: none

   [[.CYRILLIC CAPITAL LETTER I.]]

は Unicode 文字 0x0418 にマッチする。
