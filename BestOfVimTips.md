% copied from http://zzapper.co.uk/vimtips.html 

Searching
---------
|                                            |                                                      |
|--------------------------------------------|------------------------------------------------------|
| `/joe/e`                                   | Cursor set to End of match                           |
| `3/joe/e+1`                                | Find 3rd joe cursor set to End of match plus 1       |
| `/joe/s-2`                                 | Cursor set to Start of match minus 2                 |
| `/joe/+3`                                  | Find joe move cursor 3 lines down                    |
| `/^joe.\*fred.\*bill/`                     | Find joe AND fred AND Bill (Joe at start of line)    |
| `/^[A-J]/`                                 | Search for lines beginning with one or more A-J      |
| `/begin\_.*end`                            | Search over possible multiple lines                  |
| `/fred\_s*joe/`                            | Any whitespace including newline                     |
| `/fred\|joe`                               | Search for FRED OR JOE                               |
| `/.*fred\&.*joe`                           | Search for FRED AND JOE in any ORDER!                |
| `/\<fred\>/`                               | Search for fred but not alfred or frederick          |
| `/\<\d\d\d\d\>`                            | Search for exactly 4 digit numbers                   |
| `/\D\d\d\d\d\D`                            | Search for exactly 4 digit numbers                   |
| `/\<\d\{4}\>`                              | Search for exactly 4 digit numbers                   |
| `/\([^0-9]\|^\)%.*%`                       | Search for absence of a digit or beginning of line   |
| `/^\n\{3}`                                 | Find 3 empty lines                                   |
| `/^str.*\nstr`                             | Find 2 successive lines starting with str            |
| `/\(^str.*\n)\{2}`                         | Find 2 successive lines starting with str            |
| `:/fred/;/joe/-2,/sid/+3s/sally/alley/gIC` | Find fred before beginning search for joe            |

Regex
-----
|                                  |                                                                    |
|----------------------------------|--------------------------------------------------------------------|
| `/\(fred\).*\(joe\).*\2.*\1`     | using rexexp memory in a search find fred.\*joe.\*joe.\*fred \*C\* |
| `/^\([^,]*,\)\{8}`               | Repeating the Regexp (rather than what the Regexp finds)           |


Visual searching
----------------
|                                                                |                                      |
|----------------------------------------------------------------|--------------------------------------|
| `vmap // y/<C-R>"<CR>`                                         | search for visually highlighted text |
| `vmap <silent> //    y/<C-R>=escape(@", '\\/.\*$^~[]')<CR><CR> | with spec chars                      |

Searching over multiple lines \_ means including newline
--------------------------------------------------------
|                         |                                         |
|-------------------------|-----------------------------------------|
| `/<!--\_p\{-}-->`       | search for multiple line comments       |
| `/fred\_s\*joe/`        | any whitespace including newline        |
| `/bugs\(\_.\)\*bunny`   | bugs followed by bunny anywhere in file |

Search for declaration of subroutine/function under cursor
----------------------------------------------------------
|                                                      |   |
|------------------------------------------------------|---|
| `:nmap gx yiw/^\(sub\<bar>function\)\s\+<C-R>"<CR>`  |   |


Multiple file search better but cheating
----------------------------------------
|                                     |                                  |
|-------------------------------------|----------------------------------|
| `:bufdo %s/searchstr/&/gic`         | say n and then a to stop         |
| `:bufdo execute "normal! @a" \| w`  | execute macro a over all buffers |
| `:bufdo exe ":normal Gp" \| update` | Paste to the end of each buffer  |
| `:argdo exe '%!sort' |w`            |                                  |

Specify what you are NOT searching for (vowels)
-----------------------------------------------

|                               |                                                        |
|-------------------------------|--------------------------------------------------------|
| `/\c\v([^aeiou]&\a){4}`       | search for 4 consecutive consonants                    |
| `/\%>20l\%<30lgoat`           | Search for goat between lines 20 and 30                |
| `/^.\{-}home.\{-}\zshome/e`   | match only the 2nd occurence in a line of "home"       |
| `:%s/home.\{-}\zshome/alone`  | Substitute only the 2nd occurrence of home in any line |
| `:%s/.*\zsone/two/`          | Substitute only the LAST occurrence of one |

Find str but not on lines containing tongue
-------------------------------------------
|                                          |                                                      |
|------------------------------------------|------------------------------------------------------|
| `^\(.*tongue.*\)\@!.\*nose.\*$`        |                                                      |
| `\v^((tongue)@!.)*nose((tongue)@!.)*$` |                                                      |
| `.*nose.*\&^\%(\%(tongue\)\@!.\)*$`   |                                                      |
| `:v/tongue/s/nose/&/gic`                 |                                                      |
| `'a,'bs/extrascost//gc`                  | trick: restrict search to between markers (answer n) |
| `/integ<C-L>`                            | Control-L to complete search term                    |

Substitution
------------
|||
|---|---|
| `:%s/fred/joe/igc`           | general substitute command                 |
| `:%s//joe/igc`               | Substitute what you last searched for      |
| `:%s/~/sue/igc`              | Substitute your last replacement string    |
| `:%s/\r//g`                  | Delete DOS returns ^M                      |
| `:%s/\r/\r/g`                | Turn DOS returns ^M into real returns      |
| `:%s=  \*$==`                | delete end of line blanks                  |
| `:%s= \+$==`                 | delete end of line blanks                  |
| `:%s#\s\*\r\?$##`            | Clean both trailing spaces AND DOS returns |
| `:%s#\s\*\r\*$##`            | Clean both trailing spaces AND DOS returns |

Deleting empty lines
--------------------
|                                 |                                                   |
|---------------------------------|---------------------------------------------------|
| `:%s/^\n\{3}//`                 | Delete blocks of 3 empty lines                    |
| `:%s/^\n\+/\r/`                 | Compressing empty lines                           |
| `:%s#<[^>]\+>##g`               | Delete html tags, leave text (non-greedy)         |
| `:%s#<\_.\{-1,}>##g`            | Delete html tags possibly multi-line (non-greedy) |
| `:%s#.*\(\d\+hours\).*#\1#`     | Delete all but memorised string (\1)              |

Parse xml/soap 
----------------
|                                 |                                                   |
|---------------------------------|---------------------------------------------------|
| `%s#><\(\[^/\\]\)#>\r<\1#g`     | split jumbled up XML file into one tag per line   |
| `%s/\</\r&/g`                   | simple split of html/xml/soap                     |
| `:%s#\<\[^\/\]#\r&#gic`         | simple split of html/xml/soap but not closing tag |
| `:%s#\<\[^\/\]#\r&#gi`          | parse on open xml tag                             |
| `:%s#\[\d\+\]#\r&#g`            | parse on numbered array elements [1]              |
| `ggVGgJ`                        | rejoin XML without extra spaces (gJ)              |
| `:%s#^\[^\t\]\\+\\t##`          | Delete up to and including first tab              |

VIM Power Substitute
----------------
|                               |             |
|-------------------------------|-------------|
| `:'a,'bg/fred/s/dick/joe/igc` | VERY USEFUL |

Duplicating columns
-------------------
|                         |                      |
|-------------------------|----------------------|
| `:%s= [^ ]\+$=&&=`      | duplicate end column |
| `:%s= \f\+$=&&=`        | Dupicate filename    |
| `:%s= \S\+$=&&`         | usually the same     |

Memory
------
|                              |                                               |
|------------------------------|-----------------------------------------------|
| `:%s#example#& = &#gic`      | duplicate entire matched string               |
| `:%s#.*\(tbl_\w\+\).*#\1#`   | extract list of all strings tbl\_\* from text |
| `:s/\(.*\):\(.*\)/\2 : \1/`  | reverse fields separated by :                 |
| `:%s/^\(.*\)\n\1$/\1/`       | delete duplicate lines                        |
| `:%s/^\(.*\)\(\n\1\)\+$/\1/` | delete multiple duplicate lines               |

Non-greedy matching \{-}
------------------------
|                                          |                                                  |
|------------------------------------------|--------------------------------------------------|
| `:%s/^.\{-}pdf/new.pdf/`                 | delete to 1st occurence of pdf only (non-greedy) |
| `%s#^.\{-}\([0-9]\{3,4\}serial\)#\1#gic` | delete up to 123serial or 1234serial             |

Use of optional atom \\?
------------------------
|                                       |                                            |
|---------------------------------------|--------------------------------------------|
| `:%s#\<[zy]\?tbl\_[a-z\_]\+\>#\L&#gc` | lowercase with optional leading characters |

Over possibly many lines
------------------------
|                          |                                     |
|--------------------------|-------------------------------------|
| `:%s/<!--\_.\{-}-->//`   | delete possibly multi-line comments |
| `:help /\{-}`            | help non-greedy                     |

Substitute using a register
---------------------------
|                                       |                                                       |
|---------------------------------------|-------------------------------------------------------|
| `:s/fred/\<c-r\>a/g`                  | sub "fred" with contents of register "a"              |
| `:s/fred/\<c-r\>asome\_text<c-r>s/g`  |                                                       |
| `:s/fred/\=@a/g`                      | better alternative as register not displayed (not \*) |
| `:s/fred/\=@*/g`                      | replace string with contents of paste register        |


ORing
-------
|                            |                                              |
|----------------------------|----------------------------------------------|
| `:%s/goat\|cow/sheep/gc`   | ORing (must break pipe)                      |
| `:%s/\v(.\*\n){5}/&\r`     | insert a blank line every 5 lines            |

Calling a VIM function
----------------------
|                                              |                        |
|----------------------------------------------|------------------------|
| `:s/__date__/\=strftime("%c")/`              | insert datestring      |
| `:inoremap \zd <C-R>=strftime("%d%b%y")<CR>` | insert date eg 31Jan11 |


Format a mysql query 
--------------------
|                                                                 |   |
|-----------------------------------------------------------------|---|
| `:%s#\<from\>\|\<where\>\|\<left join\>\|\<\inner join\>#\r&#g` |   |

Global command 
--------------
|                                |                                                   |
|--------------------------------|---------------------------------------------------|
| `:g/gladiolli/#`               | display with line numbers                         |
| `:g/fred.\*joe.\*dick/`        | display all lines fred,joe & dick                 |
| `:g/\<fred\>/`                 | display all lines fred but not freddy             |
| `:g/^\s\*$/d`                  | delete all blank lines                            |
| `:g!/^dd/d`                    | delete lines not containing string                |
| `:v/^dd/d`                     | delete lines not containing string                |
| `:g/fred/,/joe/j`              | Join Lines                                        |
| `:g/-------/.-10,.d`           | Delete string & 10 previous lines                 |
| `:v/./,/./-j`                  | compress empty lines                              |
| `:g/^$/,/./-j`                 | compress empty lines                              |
| `:'a,'bg/^/m'b`                | Reverse a section a to b                          |
| `:g/^/t.`                      | duplicate every line                              |
| `:g/fred/t$`                   | copy (transfer) lines matching fred to EOF        |
| `:g/stage/t'a`                 | copy (transfer) lines matching stage to marker a  |
| `:g/^Chapter/t.\|s/./-/g`      | Automatically underline selecting headings        |
| `:g/\\(^I\[^^I\]\*\\)\\{80}/d` | delete all lines containing at least 80 tabs      |

Save results to a register/paste buffer
----------------
|                                           |                                     |
|-------------------------------------------|-------------------------------------|
| `:g/fred/y A`                             | append all lines fred to register a |
| `:g/fred/y A \| :let @\*=@a`              | put into paste buffer               |
| `:g//y A \| :let @\*=@a`                  | put last glob into paste buffer     |
| `:let @a=''\|g/Barratt/y A \|:let @\*=@a` |                                     |

Filter lines to a file (file must already exist)
------------------------------------------------
`:'a,'bg/^Error/ . w >> errors.txt`

Duplicate every line in a file wrap a print '' around each duplicate
----------------
`:g/./yank|put|-1s/'/"/g|s/.\*/Print '&'/`

Replace string with contents of a file, -d deletes the "mark"
------------
|                                       |                      |
|---------------------------------------|----------------------|
| `:g/^MARK$/r tmp.txt | -d`            |                      |
| `:g/<pattern>/z#.5`                   | display with context |
| `:g/<pattern>/z#.5|echo "=========="` | display beautifully  |

Send output of previous global command to a new window
----
`:nmap <F3>  :redir @a<CR>:g//<CR>:redir END<CR>:new<CR>:put! a<CR><CR>`

Create a new file for each line of file eg 1.txt,2.txt,3,txt etc
----------------------------------------
`:g/^/exe ".w ".line(".").".txt"`

Chain an external command
----------------------------------------
`:.g/^/ exe ".!sed 's/N/X/'" | s/I/Q/`

Operate until string found
--------------------------
|            |                                      |
|------------|--------------------------------------|
| `d/fred/`  | delete until fred                    |
| `y/fred/`  | yank until fred                      |
| `c/fred/e` | change until fred end                |
| `v12|`     | visualise/change/delete to column 12 |

Summary of editing repeats
--------------------------
|          |                                    |
|----------|------------------------------------|
| `.`      | last edit (magic dot)              |
| `:&`     | last substitute                    |
| `:%&`    | last substitute every line         |
| `:%&gic` | last substitute every line confirm |
| `g%`     | normal mode repeat last substitute |
| `g&`     | last substitute on all lines       |
| `@@`     | last recording                     |
| `@:`     | last command-mode command          |
| `:!!`    | last :! command                    |
| `:~`     | last substitute                    |

Summary of repeated searches
----------------------------------------
|     |                                          |
|-----|------------------------------------------|
| `;` | last f, t, F or T                        |
| `,` | last f, t, F or T in opposite direction  |
| `n` | last / or ? search                       |
| `N` | last / or ? search in opposite direction |

Absolutely-essential
----------------------------------------
| `* # g* g#`           | find word under cursor (<cword>) (forwards/backwards) |
| `%`                   | match brackets {}[]() |
| `.`                   | repeat last modification  |
| `@:`                  | repeat last : command (then @@) |
| `matchit.vim`         | % now matches tags <tr><td><script> <?php etc |
| `<C-N><C-P>`          | word completion in insert mode |
| `<C-X><C-L>`          | Line complete SUPER USEFUL |
| `/<C-R><C-W>`         | Pull <cword> onto search/command line |
| `/<C-R><C-A>`         | Pull <CWORD> onto search/command line |
| `:set ignorecase`     | you nearly always want this                                    |
| `:set smartcase`      | overrides ignorecase if uppercase used in search string (cool) |
| `:syntax on`          | colour syntax in Perl,HTML,PHP etc                             |
| `:h regexp<C-D>`      | type control-D and get a list all help topics containing       |

MAKE IT EASY TO UPDATE/RELOAD _vimrc
----------------------------------------
:nmap ,s :source $VIM/_vimrc
:nmap ,v :e $VIM/_vimrc
:e $MYVIMRC         : edits your _vimrc whereever it might be  [N]
" How to have a variant in your .vimrc for different PCs [N]
if $COMPUTERNAME == "NEWPC"
ab mypc vista
else
ab mypc dell25
endif

Splitting windows
----------------------------------------
:vsplit other.php       # vertically split current file with other.php [N]

VISUAL MODE (easy to add other HTML Tags)
----------------------------------------

:vmap sb "zdi<b><C-R>z</b><ESC>  : wrap <b></b> around VISUALLY selected Text
:vmap st "zdi<?= <C-R>z ?><ESC>  : wrap <?=   ?> around VISUALLY selected Text

vim 7 tabs
----------
vim -p fred.php joe.php             : open files in tabs
:tabe fred.php                      : open fred.php in a new tab
:tab ball                           : tab open files
:close                              : close a tab but leave the buffer *N*
" vim 7 forcing use of tabs from .vimrc
:nnoremap gf <C-W>gf
:cab      e  tabe
:tab sball                           : retab all files in buffer (repair) [N]

Exploring
---------
:e .                            : file explorer
:Exp(lore)                      : file explorer note capital Ex
:Sex(plore)                     : file explorer in split window
:browse e                       : windows style browser
:ls                             : list of buffers
:cd ..                          : move to parent directory
:args                           : list of files
:pwd                            : Print Working Directory (current directory) [N]
:args *.php                     : open list of files (you need this!)
:lcd %:p:h                      : change to directory of current file
:cd  %:p:h                      : change to directory of current file [N]
:autocmd BufEnter * lcd %:p:h   : change to directory of current file automatically (put in _vimrc)
Changing Case
-------------
guu                             : lowercase line
gUU                             : uppercase line
Vu                              : lowercase line
VU                              : uppercase line
g~~                             : flip case line
vEU                             : Upper Case Word
vE~                             : Flip Case Word
ggguG                           : lowercase entire file

Titlise Visually Selected Text (map for .vimrc)
------
vmap ,c :s/\<\(.\)\(\k*\)\>/\u\1\L\2/g<CR>
Title Case A Line Or Selection (better)
------
vnoremap <F6> :s/\%V\<\(\w\)\(\w*\)\>/\u\1\L\2/ge<cr> [N]

Titlise a line
--------------
nmap ,t :s/.*/\L&/<bar>:s/\<./\u&/g<cr>  [N]
Uppercase first letter of sentences
--------------
:%s/[.!?]\_s\+\a/\U&\E/g
gf                              : open file name under cursor (SUPER)
:nnoremap gF :view <cfile><cr>  : open file under cursor, create if necessary
ga                              : display hex,ascii value of char under cursor
ggVGg?                          : rot13 whole file
ggg?G                           : rot13 whole file (quicker for large file)
:8 | normal VGg?                : rot13 from line 8
:normal 10GVGg?                 : rot13 from line 8
<C-A>,<C-X>                     : increment,decrement number under cursor
                                  win32 users must remap CNTRL-A
<C-R>=5*5                       : insert 25 into text (mini-calculator)

Disguise text (watch out)
----------------------------------------
ggVGg?                          : rot13 whole file (toggles)
:set rl!                        : reverse lines right to left (toggles)
:g/^/m0                         : reverse lines top to bottom (toggles)
:%s/\(\<.\{-}\>\)/\=join(reverse(split(submatch(1), '.\zs')), '')/g   : reverse all text *N*

History, Markers & moving about (what Vim Remembers) [C]
----------------------------------------
'.               : jump to last modification line (SUPER)
`.               : jump to exact spot in last modification line
g;               : cycle thru recent changes (oldest first)
g,               : reverse direction 
:changes
:h changelist    : help for above
<C-O>            : retrace your movements in file (starting from most recent)
<C-I>            : retrace your movements in file (reverse direction)
:ju(mps)         : list of your movements
:help jump-motions
:history         : list of all your commands
:his c           : commandline history
:his s           : search history
q/               : Search history Window (puts you in full edit mode) (exit CTRL-C)
q:               : commandline history Window (puts you in full edit mode) (exit CTRL-C)
:<C-F>           : history Window (exit CTRL-C)

Abbreviations and Maps
-------------
:map   <f7>   :'a,'bw! c:/aaa/x       : save text to file x
:map   <f8>   :r c:/aaa/x             : retrieve text 
:map   <f11>  :.w! c:/aaa/xr<CR>      : store current line
:map   <f12>  :r c:/aaa/xr<CR>        : retrieve current line
:ab php          : list of abbreviations beginning php
:map ,           : list of maps beginning ,

allow use of F10 for mapping (win32)
----------------
set wak=no       : :h winaltkeys

For use in Maps
---------------
<CR>             : carriage Return for maps
<ESC>            : Escape
<LEADER>         : normally \
<BAR>            : | pipe
<BACKSPACE>      : backspace
<SILENT>         : No hanging shell window

Display RGB colour under the cursor eg #445588
----------
:nmap <leader>c :hi Normal guibg=#<c-r>=expand("<cword>")<cr><cr>
map <f2> /price only\\|versus/ :in a map need to backslash the \

Type table,,, to get <table></table>       ### Cool ###
---------
imap ,,, <esc>bdwa<<esc>pa><cr></<esc>pa><esc>kA

List current mappings of all your function keys
---------
:for i in range(1, 12) | execute("map <F".i.">") | endfor   [N]

For your .vimrc
-----------------
:cab ,f :for i in range(1, 12) \| execute("map <F".i.">") \| endfor

Chain commands in abbreviation
-----------------
cabbrev vrep tabe class.inc \| tabe report.php   ## chain commands [N]

Using a register as a map (preload registers in .vimrc)
----------------------------------------
:let @m=":'a,'bs/"
:let @s=":%!sort -u"

Useful tricks
-------------
"ayy@a           : execute "Vim command" in a text file
yy@"             : same thing using unnamed register
u@.              : execute command JUST typed in
"ddw             : store what you delete in register d [N]
"ccaw            : store what you change in register c [N]

Get output from other commands (requires external programs)
----------------------------------------
:r!ls -R         : reads in output of ls
:put=glob('**')  : same as above                 [N]
:r !grep "^ebay" file.txt  : grepping in content   [N]
:20,25 !rot13    : rot13 lines 20 to 25   [N]
!!date           : same thing (but replaces/filters current line)

Sorting with external sort
--------------------------
:%!sort -u       : use an external program to filter content
:'a,'b!sort -u   : use an external program to filter content
!1} sort -u      : sorts paragraph (note normal mode!!)
:g/^$/;/^$/-1!sort : Sort each block (note the crucial ;)

Sorting with internal sort
--------------------------
:sort /.*\%2v/   : sort all lines on second column [N]
" number lines  (linux or cygwin only)
:new | r!nl #                  [N]

Multiple Files Management 
-------------------------
:bn              : goto next buffer
:bp              : goto previous buffer
:wn              : save file and move to next (super)
:wp              : save file and move to previous
:bd              : remove file from buffer list (super)
:bun             : Buffer unload (remove window but not from list)
:badd file.c     : file from buffer list
:b3              : go to buffer 3 [C]
:b main          : go to buffer with main in name eg main.c (ultra)
:sav php.html    : Save current file as php.html and "move" to php.html
:sav! %<.bak     : Save Current file to alternative extension (old way)
:sav! %:r.cfm    : Save Current file to alternative extension
:sav %:s/fred/joe/           : do a substitute on file name
:sav %:s/fred/joe/:r.bak2    : do a substitute on file name & ext.
:!mv % %:r.bak   : rename current file (DOS use Rename or DEL)
:help filename-modifiers
:e!              : return to unmodified file
:w c:/aaa/%      : save file elsewhere
:e #             : edit alternative file (also cntrl-^)
:rew             : return to beginning of edited files list (:args)
:brew            : buffer rewind
:sp fred.txt     : open fred.txt into a split
:sball,:sb       : Split all buffers (super)
:scrollbind      : in each split window
:map   <F5> :ls<CR>:e # : Pressing F5 lists all buffer, just type number
:set hidden      : Allows to change buffer w/o saving current buffer
:w !sudo tee %   : Write to read only file on linux (trick) [N]
Quick jumping between splits
----------------------------
map <C-J> <C-W>j<C-W>_
map <C-K> <C-W>k<C-W>_

Recording 
---------
qq  # record to q
q   # end recording
@q to execute
@@ to Repeat
5@@ to Repeat 5 times
qQ@qq                             : Make an existing recording q recursive [N]

Editing a register/recording
----------------------------
"qp                               :display contents of register q (normal mode)
<ctrl-R>q                         :display contents of register q (insert mode)

You can now see recording contents, edit as required
-----------------
"qdd                              :put changed contacts back into q
@q                                :execute recording/register q

Operating a Recording on a Visual BLOCK (blockwise)
-------------------
1) define recording/register
qq:s/ to/ from/g^Mq
2) Define Visual BLOCK
V}
3) hit : and the following appears
:'<,'>
4)Complete as follows
:'<,'>norm @q

Visual basics
-------------
v                               : enter visual mode
V                               : visual mode whole line
<C-V>                           : enter VISUAL BLOCKWISE mode (remap on Windows to say C-Q *C*
gv                              : reselect last visual area (ultra)
o                               : navigate visual area
"*y or "+y                      : yank visual area into paste buffer
V%                              : visualise what you match
V}J                             : Join Visual block (great)
V}gJ                            : Join Visual block w/o adding spaces
`[v`]                           : Highlight last insert
:%s/\%Vold/new/g                : Do a substitute on last visual area

Text objects :h text-objects
---
daW                                   : delete contiguous non whitespace
di<   yi<  ci<                        : Delete/Yank/Change HTML tag contents
da<   ya<  ca<                        : Delete/Yank/Change whole HTML tag
dat   dit                             : Delete HTML tag pair
diB   daB                             : Empty a function {}
das                                   : delete a sentence

vimrc essentials
----------------
:imap <TAB> <C-N>                     : set tab to complete
:set incsearch : jumps to search word as you type (annoying but excellent)
:set wildignore=*.o,*.obj,*.bak,*.exe : tab complete now ignores these
:set shiftwidth=3                     : for shift/tabbing
:set vb t_vb=".                       : set silent (no beep)
:set browsedir=buffer                 : Maki GUI File Open use current directory
set iminsert=0  (or toggle CTRL-shift-^)    : reset "stuck" caps lock/capslock key

Launching Win IE
----------------------------------------
:nmap ,f :update<CR>:silent !start c:\progra~1\intern~1\iexplore.exe file://%:p<CR>
:nmap ,i :update<CR>: !start c:\progra~1\intern~1\iexplore.exe <cWORD><CR>

FTPing from VIM
---------------
cmap ,r  :Nread ftp://209.51.134.122/public_html/index.html
cmap ,w  :Nwrite ftp://209.51.134.122/public_html/index.html
gvim ftp://www.somedomain.com/index.html # uses netrw.vim

Appending to registers (use CAPITAL)
------------------------------------
"a5yy
10j
"A5yy
[I     : show lines matching word under cursor <cword> (super)

Conventional Shifting/Indenting
-------------------------------
:'a,'b>>
" visual shifting (builtin-repeat)
:vnoremap < <gv
:vnoremap > >gv
" Block shifting (magic)
>i{
>a{
" also
>% and <%
==                            : index current line same as line above [N]

Redirection & Paste register
----------------------------------------
:redir @*                    : redirect commands to paste buffer
:redir END                   : end redirect
:redir >> out.txt            : redirect to a file
" Working with Paste buffer
"*yy                         : yank current line to paste
"*p                          : insert from paste buffer

Yank to paste buffer (ex mode)
-------------
:'a,'by*                     : Yank range into paste
:%y*                         : Yank whole buffer into paste
:.y*                         : Yank Current line to paster

:set paste                    : prevent vim from formatting pasted in text *N*

Reformatting text
------------------
gq}                          : Format a paragraph
gqap                         : Format a paragraph
ggVGgq                       : Reformat entire file
Vgq                          : current line

Operate command over multiple files
----------------------------------------
:argdo %s/foo/bar/gec        : operate on all files in :args
:bufdo %s/foo/bar/gec
:windo %s/foo/bar/gec
:argdo exe '%!sort'|w!       : include an external command
:bufdo exe "normal @q" | w   : perform a recording on open files
:silent bufdo !zip proj.zip %:p   : zip all current files

Automate editing of a file (Ex commands in convert.vim)
------
vim -s "convert.vim" file.c

" use :cn(ext) :cp(rev) to navigate list
:h grep

Using vimgrep with copen                              [N]
-------
:vimgrep /keywords/ *.php
:copen

Complex diff parts of same file [N]
--------
:1,2yank a | 7,8yank b
:tabedit | put a | vnew | put b
:windo diffthis 

Vim traps
---------
In regular expressions you must backslash + (match 1 or more)
In regular expressions you must backslash | (or)
In regular expressions you must backslash ( (group)
In regular expressions you must backslash { (count)
/fred\+/                   : matches fred/freddy but not free
/\(fred\)\{2,3}/           : note what you have to break

\\v or very magic (usually) reduces backslashing
----------------------------------------
/codes\(\n\|\s\)*where  : normal regexp
/\vcodes(\n|\s)*where   : very magic

Pulling objects onto command/search line
----------------------------------------
<C-R><C-W> : pull word under the cursor into a command line or search
<C-R><C-A> : pull WORD under the cursor into a command line or search
<C-R>-                  : pull small register (also insert mode)
<C-R>[0-9a-z]           : pull named registers (also insert mode)
<C-R>%                  : pull file name (also #) (also insert mode)
<C-R>=somevar           : pull contents of a variable (eg :let sray="ray[0-9]")

List your Registers
-------------------
:reg             : display contents of all registers
:reg a           : display content of register a
:reg 12a         : display content of registers 1,2 & a [N]
"5p              : retrieve 5th "ring" 
"1p....          : retrieve numeric registers one by one
:let @y='yy@"'   : pre-loading registers (put in .vimrc)
qqq              : empty register "q"
qaq              : empty register "a"
:reg .-/%:*"     : the seven special registers
:reg 0           : what you last yanked, not affected by a delete
"_dd             : Delete to blackhole register "_ , don't affect any register

Manipulating registers
----------------------
:let @a=@_              : clear register a
:let @a=""              : clear register a
:let @a=@"              : Save unnamed register
:let @*=@a              : copy register a to paste buffer
:let @*=@:              : copy last command to paste buffer
:let @*=@/              : copy last search to paste buffer
:let @*=@%              : copy current filename to paste buffer

Where was an option set
-----------------------
:scriptnames            : list all plugins, _vimrcs loaded (super)
:verbose set history?   : reveals value of history and where set
:function               : list functions
:func SearchCompl       : List particular function

Save this page as a VIM Help File
---------------------------------
:sav! $VIMRUNTIME/doc/vimtips.txt|:1,/^__BEGIN__/d|:/^__END__/,$d|:w!|:helptags $VIMRUNTIME/doc

Capturing output of current script in a separate buffer
----------------------------------------
:new | r!perl #                   : opens new buffer,read other buffer
:new! x.out | r!perl #            : same with named file
:new+read!ls

Create a new buffer, paste a register "q" into it, then sort new buffer
----------------------------------------
:new +put q|%!sort

Inserting DOS Carriage Returns
----------------------------------------
:%s/$/\<C-V><C-M>&/g          :  that's what you type
:%s/$/\<C-Q><C-M>&/g          :  for Win32
:%s/$/\^M&/g                  :  what you'll see where ^M is ONE character

Automatically delete trailing Dos-returns,whitespace
----------------------------------------
autocmd BufRead * silent! %s/[\r \t]\+$//
autocmd BufEnter *.php :%s/[ \t\r]\+$//e

Perform an action on a particular file or file type
----------------------------------------
autocmd VimEnter c:/intranet/note011.txt normal! ggVGg?
autocmd FileType *.pl exec('set fileformats=unix')

Retrieving last command line command for copy & pasting into text
----------------------------------------
i<c-r>:
" Retrieving last Search Command for copy & pasting into text
i<c-r>/

More completions
----------------
<C-X><C-F>                        :insert name of a file in current directory

Substituting a Visual area
--------------------------
:'<,'>s/Emacs/Vim/g               : REMEMBER you dont type the '<.'>
gv                                : Re-select the previous visual area (ULTRA)

Inserting line number into file
-------------------------------
:g/^/exec "s/^/".strpart(line(".")."    ", 0, 4)
:%s/^/\=strpart(line(".")."     ", 0, 5)
:%s/^/\=line('.'). ' '
Numbering lines VIM way
-----------------------
:set number                       : show line numbers
:map <F12> :set number!<CR>       : Show linenumbers flip-flop
:%s/^/\=strpart(line('.')."        ",0,&ts)

Advanced incrementing (vimrc)
----------------------------------------
let g:I=0
function! INC(increment)
let g:I =g:I + a:increment
return g:I
endfunction
" eg create list starting from 223 incrementing by 5 between markers a,b
:let I=223
:'a,'bs/^/\=INC(5)/
" create a map for INC
cab viminc :let I=223 \| 'a,'bs/$/\=INC(5)/

Generate a list of numbers  23-64
----------------------------------------

o23<ESC>qqYp<C-A>q40@q
unmap =                           : get the original vim command on a mapped key [N]

Editing/moving within current insert 
----------------------------------------
<C-U>                             : delete all entered
<C-W>                             : delete last word
<HOME><END>                       : beginning/end of line
<C-LEFTARROW><C-RIGHTARROW>       : jump one word backwards/forwards
<C-X><C-E>,<C-X><C-Y>             : scroll while staying put in insert

Encryption (use with care: DON'T FORGET your KEY)
----------------------------------------
:X                                : you will be prompted for a key
:h :X


Delete duplicate lines
----------------------------------------
function! Del()
 if getline(".") == getline(line(".") - 1)
   norm dd
 endif
endfunction

:g/^/ call Del()

Digraphs (non alpha-numerics)
-----------------------------
:digraphs                         : display table
i<C-K>e'                          : enters é
i<C-V>233                         : enters é (Unix)
i<C-Q>233                         : enters é (Win32)
ga                                : View hex value of any character

Deleting non-ascii characters (some invisible)
----------------------------------------------
:%s/[\x00-\x1f\x80-\xff]/ /g      : type this as you see it
:%s/[<C-V>128-<C-V>255]//gi       : where you have to type the Control-V
:%s/[€-ÿ]//gi                     : Should see a black square & a dotted y
:%s/[<C-V>128-<C-V>255<C-V>01-<C-V>31]//gi : All pesky non-asciis
:exec "norm /[\x00-\x1f\x80-\xff]/"        : same thing

Pull a non-ascii character onto search bar
------------------------------------------
yl/<C-R>"                         :
/[^a-zA-Z0-9_[:space:][:punct:]]  : search for all non-ascii

Complex Vim
-----------
Swap two words
--------------

:%s/\<\(on\|off\)\>/\=strpart("offon", 3 * ("off" == submatch(0)), 3)/g

Swap two words
--------------
:vnoremap <C-X> <Esc>`.``gvP``P

Swap word with next word
------------------------
nmap <silent> gw    "_yiw:s/\(\%#\w\+\)\(\_W\+\)\(\w\+\)/\3\2\1/<cr><c-o><c-l> [N]


Force syntax automatically (for a file with non-standard extension)
---------------------------------------------------------------------
au BufRead,BufNewFile \\*/Content.IE?/\* setfiletype html
" make cursor standout against highlighted text for vim terminal (linux) [N]
:highlight search term=reverse ctermbg=5  
:highlight Normal guibg=white     : restore white background [N]
:set noma (non modifiable)        : Prevents modifications
:set ro (Read Only)               : Protect a file from unintentional writes

Sessions (Open a set of files)
----------------------------------------

gvim file1.c file2.c lib/lib.h lib/lib2.h : load files for "session"
:mksession                        : Make a Session file (default Session.vim)
:mksession MySession.vim          : Make a Session file named file [C]
:q
gvim -S                           : Reload all files (loads Session.vim) [C]
gvim -S MySession.vim             : Reload all files from named session [C]

tags (jumping to subroutines/functions)
----------------------------------------

taglist.vim                       : popular plugin
:Tlist                            : display Tags (list of functions)
<C-]>                             : jump to function under cursor

Columnise a csv file for display only as may crop wide columns
--------------------------------------------------------------
```
	:let width = 20
	:let fill=' ' | while strlen(fill) < width | let fill=fill.fill | endwhile
	:%s/\([^;]*\);\=/\=strpart(submatch(1).fill, 0, width)/ge
	:%s/\s\+$//ge
```

Highlight a particular csv column (put in .vimrc)
-------------------------------------------------
```
function! CSVH(x)
	execute 'match Keyword /^\([^,]*,\)\{'.a:x.'}\zs[^,]*/'
	execute 'normal ^'.a:x.'f,'
endfunction
command! -nargs=1 Csv :call CSVH(<args>)
```
|||
|---|---|
| :Csv 5                             | highlight fifth column |

Folding : hide sections to allow easier comparisons
----------------------------------------
zf1G                              : fold everything before this line [N]
zf}                               : fold paragraph using motion
v}zf                              : fold paragraph using visual
zf'a                              : fold to mark
zo                                : open fold
zc                                : re-close fold

Also visualise a section of code then type zf
---------------------------------------------
:help folding
zfG      : fold everything after this line

Displaying "non-asciis"
----------------------------------------
:set list
:h listchars

How to paste "normal vim commands" w/o entering insert mode
----------------------------------------
:norm qqy$jq

Manipulating file names
----------------------------------------
:h filename-modifiers             : help
:w %                              : write to current file name
:w %:r.cfm                        : change file extention to .cfm
:!echo %:p                        : full path & file name
:!echo %:p:h                      : full path only
:!echo %:t                        : filename only
:reg %                            : display filename
<C-R>%                            : insert filename (insert mode)
"%p                               : insert filename (normal mode)
/<C-R>%                           : Search for file name in text

Delete without destroying default buffer contents
---------------------------------------------------
| "\_d                               | what you've ALWAYS wanted |
| "\_dw                              | eg delete word (use blackhole) |

Pull full path name into paste buffer for attachment to email etc
-----------------------------------------------------------------
nnoremap <F2> :let @\*=expand("%:p")<cr> :unix
nnoremap <F2> :let @\*=substitute(expand("%:p"), "/", "\\", "g")<cr> :win32

Simple Shell script to rename files w/o leaving vim
----------------------------------------
```
	$ vim
	:r! ls \*.c
	:%s/\(.\*\).c/mv & \1.bla
	:w !sh
	:q!
```

Count words/lines in a text file
----------------------------------------
| g<C-G>                                 | counts words |
| :echo line("'b")-line("'a")            | count lines between markers a and b [N] |
| :'a,'bs/^//n                           | count lines between markers a and b |
| :'a,'bs/somestring//gn                 | count occurences of a string |
| :s\/,\/,\/gn                           | count commas in a line (csv/parameters) |

Example of setting your own highlighting
----------------------------------------

:syn match DoubleSpace "  "
:hi def DoubleSpace guibg=#e0e0e0

Reproduce previous line word by word
------------------------------------
imap ]  @@@<ESC>hhkyWjl?@@@<CR>P/@@@<CR>3s
nmap ] i@@@<ESC>hhkyWjl?@@@<CR>P/@@@<CR>3s

Programming keys depending on file type
-------
:autocmd bufenter \*.tex map <F1> :!latex %<CR>
:autocmd bufenter \*.tex map <F2> :!xdvi -hush %<.dvi&<CR>

Allow yanking of php variables with their dollar [N]
------
:autocmd bufenter \*.php :set iskeyword+=\$ 

Reading Ms-Word documents, requires antiword (not docx)
----------------------------------------
:autocmd BufReadPre \*.doc set ro
:autocmd BufReadPre \*.doc set hlsearch!
:autocmd BufReadPost \*.doc %!antiword "%"

A folding method
----------------

vim: filetype=help foldmethod=marker foldmarker=<<<,>>>
A really big section closed with a tag <<< 
--- remember folds can be nested --- 
Closing tag >>> 

Return to last edit position (You want this!)
----------------------------------------
autocmd BufReadPost *
     \ if line("'\"") > 0 && line("'\"") <= line("$") |
     \   exe "normal! g`\"" |
     \ endif

Store text that is to be changed or deleted in register a
--------------------
|||
|---|---|
| "act\<                                 |  Change Till \< |

Just Another Vim Hacker JAVH
---------------------------------

|||
|---|---|
| vim -c ":%s%s\*%Cyrnfr)fcbafbe[Oenz(Zbbyranne%\|:%s)[[()])-)Ig\|norm Vg?" |


updated version at http://www.zzapper.co.uk/vimtips.html

