.. Copyright 2006-2007 John Maddock.
.. Distributed under the Boost Software License, Version 1.0.
.. (See accompanying file LICENSE_1_0.txt or copy at
.. http://www.boost.org/LICENSE_1_0.txt).

文字クラス名
============

.. _syntax.character_classes.std_char_classes:

常にサポートされている文字クラス
--------------------------------

以下の文字クラスが Boost.Regex において常にサポートされている。

.. table::

   =======  ==============  ======================================================
   名前     POSIX 標準名か  説明
   =======  ==============  ======================================================
   alnum    ○               アルファベットか数字。
   alpha    ○               アルファベット。
   blank    ○               行区切り以外の空白類文字。
   cntrl    ○               制御文字。
   d        ×               10 進数字。
   digit    ○               10 進数字。
   graph    ○               グラフィカルな文字。
   l        ×               小文字。
   lower    ○               小文字。
   print    ○               印字可能な文字。
   punct    ○               区切り文字。
   s        ×               空白類文字。
   space    ○               空白類文字。
   unicode  ×               コードポイントが 256 以上の文字。
   u        ×               大文字。
   upper    ○               大文字。
   w        ×               単語構成文字（アルファベット、数字、アンダースコア）。
   word     ×               単語構成文字（アルファベット、数字、アンダースコア）。
   xdigit   ○               16 進数字。
   =======  ==============  ======================================================


.. _syntax.character_classes.optional_char_class_names:

Unicode 正規表現によりサポートされる文字クラス
----------------------------------------------

以下の文字クラスは Unicode 正規表現（:cpp:type:`!u32regex` 型）でのみサポートされている。使用する名前は Unicode 標準 4 章と同じである。

.. table::

   ========  ======================
   短い名前  長い名前
   ========  ======================
   （なし）  ASCII
   （なし）  Any
   （なし）  Assigned
   C*        Other
   Cc        Control
   Cf        Format
   Cn        Not Assigned
   Co        Private Use
   Cs        Surrogate
   L*        Letter
   Ll        Lowercase Letter
   Lm        Modifier Letter
   Lo        Other Letter
   Lt        Titlecase
   Lu        Uppercase Letter
   M*        Mark
   Mc        Spacing Combining Mark
   Me        Enclosing Mark
   Mn        Non-Spacing Mark
   N*        Number
   Nd        Decimal Digit Number
   Nl        Letter Number
   No        Other Number
   P*        Punctuation
   Pc        Connector Punctuation
   Pd        Dash Punctuation
   Pe        Close Punctuation
   Pf        Final Punctuation
   Pi        Initial Punctuation
   Po        Other Punctuation
   Ps        Open Punctuation
   S*        Symbol
   Sc        Currency Symbol
   Sk        Modifier Symbol
   Sm        Math Symbol
   So        Other Symbol
   Z*        Separator
   Zl        Line Separator
   Zp        Paragraph Separator
   Zs        Space Separator
   ========  ======================
