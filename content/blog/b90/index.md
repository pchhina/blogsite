---
title: "Just Enough... Vim"
author: "Paramjot Singh"
date: 2018-03-06T05:05:20-06:00
draft: FALSE
tags: ["Vim"]
---
Note that any of these commands can be executed multiple times by simply appending a number in front. Also dot(.) will repeat the last change(anything you do in insert mode before coming back to normal mode).

### Movement Commands

*Jump up and down the lines*

> k j

*Jump to beginning of file, end of file*

> gg G

*Jump to top, bottom and middle of current screen*

> H L M

*Bring current line to the middle of screen*

> zz

*Jump half page up/down*

> Ctrl-d Ctrl-u

*Search forward/backward for partial/exact match*

> /\<search> ?\<search>

*Jump to beginning/end of the line*

> ^ $

*Jump forwards/backwards by words and paragraphs*

> w b { }

*Jump forwards/backwards to/till a specific character in a line and repeat the search*

> f\<chr>; f\<chr>,

### Editing Commands

*Insert before/after a character*

> i a

*Insert at the beginning/end of a line*

> I A

*Insert before/after the current line*

> O o

*Replace a character with a single/bunch of characters*

> r\<chr> s\<chrs>

### Editing Text Objects

*Replace a word/line/paragraph with new word/line/paragraph*

> ciw S cip 

*Delete a word/line/paragraph and repeat this change*

> daw dd dap

*Yank a word/line/paragraph to the end of file*

> yaw yy yap followed by G p

*Change from the current location to the end of line*

> C

*Undo/Redo*

> u Ctrl-r

*Remove line break from current line(join the next line with the current line)*

> J 

*Replace a word with a new one in the whole file with/without confirming each change*

> :%s/old/new/gc :%s/old/new/g

*Change inside ( ' "*

> ci( ci' ci"

*Delete contents of ( ' "*

> di( di' di"

### Other Commands

*Change case of a character*

> ~

*Delete a character*

> x

*Set a mark and jump to it*

> m\<chr>

> `\<chr>

*Record a macro and run it*

> q\<chr> \<commands> q 

> @\<chr>

### Learning More

* This [Vim Meetup Talk](https://www.youtube.com/watch?v=wlR5gYd6um0) by Chris Toomey is an awesome resource. He talks about the structure of the language which makes it easy to understand why the commands look cryptic but are not.
* [Your problem with vim is that you don't grok vi](https://stackoverflow.com/questions/1218390/what-is-your-most-productive-shortcut-with-vim/1220118#1220118) - an interesting stackoverflow answer.
* Mike Saunders from Linux voice magazine has put a nice [video](https://www.youtube.com/watch?v=rfl9KQb_HVk) for new users to get you off the ground.
