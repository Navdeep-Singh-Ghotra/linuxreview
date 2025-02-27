 regex (Regular Expressions)
 Phrase
 sally sells seashells 

by the seashore


Beginning of a line with ^
$ grep ^by phrase.txt 
by the seashore

End of a line with $
grep seashore$ phrase.txt 
by the seashore

Matching any single character with .
$ grep se..s. phrase.txt 
sally sells seashells 

Bracket notation with []
This can be a little tricky, brackets allow us to specify characters found within the bracket.
d[iou]g

would match: dig, dog, dug

The previous anchor tag ^ when used in a bracket means anything except the characters within the bracket.

d[^i]g

would match: dog and dug but not dig

Brackets can also use ranges to increase the amount of characters you want to use.


d[a-c]g

will match patterns like dag, dbg, and dcg

Be careful though as brackets are case sensitive:


d[A-C]g

will match dAg, dBg and dCg but not dag, dbg and dcg



 Vim (Vi Improved)
 Vim stands for vi (Improved) just like its name it stands for an improved version of the vi text editor command.

It's super lightweight, opening and editing a file with vim is quick and easy. It's also almost always available, if you booted up a random Linux distribution, chances are vim is installed by default.

Vim Search Patterns

To search for an expression just type the / key and then your search result while you are in a vim session. Once you hit enter, you can press "n" to go forward or "N" to go backward in your search results.

The ? search command will search the text file backwards, so in the previous example, the last pretty would come up first.

h or the left arrow - will move you left one character
k or the up arrow - will move you up one line
j or the down arrow - will move you down one line
l or the right arrow - will move you right one character

The command mode is used for when you enter commands like h,j,k.l etc. To insert text you'll need to enter insert mode first.

i - insert text before the cursor
O - insert text on the previous line
o - insert text on the next line
a - append text after the cursor
A - append text at the end of the line

x - used to cut the selected text also used for deleting characters
dd - used to delete the current line
y - yank or copy whatever is selected
yy - yank or copy the current line
p - paste the copied text before the cursor

:w - writes or saves the file
:q - quit out of vim
:wq - write and then quit
:q! - quit out of vim without saving the file
ZZ - equivalent of :wq, but one character faster
u - undo your last action
Ctrl-r - redo your last action


Emacs
Emacs is for users who want an extremely powerful text editor, which may be an understatement because you essentially live in emacs. You can do all your code editing, file manipulation, etc all within emacs. It's a bit slower to load up and the learning curve is a bit steeper than vim, but if you want a powerful editor that is extremely extensible, this is the one for you. When I say extensible, I literally mean you can write up scripts for emacs that extend its functionality.

To start emacs just use:

emacs
You should be greeted with the default welcome buffer.

Buffers in emacs is what your text resides in. So if you open up a file, a buffer is used to store that file's content. You can have multiple buffers open at the same time and you can easily switch between buffers.