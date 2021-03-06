kanten
======

[![Build Status](https://travis-ci.org/ivanov/kanten.svg?branch=master)](https://travis-ci.org/ivanov/kanten)

[![Join the chat at https://gitter.im/ivanov/kanten](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/ivanov/kanten?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


the enlightened pager. less paging. more content. read widely.
--------------------------------------------------------------

A multi-column pager for reading in the terminal ...because you have more than
80 columns.

Kanten was inspired by Amar Sagoo's [Tofu](http://amarsagoo.info/tofu/)
for OS X. Tofu is a column-based reader application, where the columns are
arranged horizontally.  Given the aspect ratio of computer monitors, I decided
to create kanten to be a unix pager replacement (more and less) that is not
limited to 80 columns.  Kanten is the Japanese for word for agar, a gelatinous
substance derived from seaweed. The idea being that kanten is similar in
substance to tofu, but has even less nutritional value.

Here's how the above paragraph would be rendered by kanten on a short (7 line)
terminal:

    Kanten was inspired by Amar Sagoo's          replacement (more and less) that is not               
    [Tofu](http://amarsagoo.info/tofu/)          limited to 80 columns.  Kanten is the                 
    for OS X. Tofu is a column-based reader      Japanese for word for agar, a gelatinous              
    application, where the columns are           substance derived from seaweed. The idea              
    arranged horizontally.  Given the aspect     being that kanten is similar in                       
    ratio of computer monitors, I decided        substance to tofu, but has even less                  
    to create kanten to be a unix pager          nutritional value.                                    

TODO
----
[x] horizontal progress bar indicator
[x] figure out how many columns are displayed, and adjust pbar accordingly
[x] max columns should always be shown
[x] progress bar should consider all visible columns as shown 
[x] some vertical empty space at the top
[x] space bar should move whole screen over (displayed_columns)
[x] scroll using the mouse
[ ] middle mouse button pastes contests of X11 paste register into a new buffer
[ ] mouse events don't work after 'v' (entering editor)
[ ] d and u (^D and ^U) go forward and back a column (half-screen in less)
[x] reading from stdin
[x] process keyboard shortcuts after reading from stdin (the way less does)
[ ] read buffering (don't read the whole file before filling in)
    - MUSTFIX for sane STDIN piping (e.g. git log | kanten)
    - urwid's edit.py has an example of lazy loading
    - <*stream*> / <*stream(closed)*> reporting like w3m does
[ ] mouse interactions broken in xterm after sleeping kanten - what gives?
    - can't click to select text in xterm
    - cannot select text even after sleeping kanten
[ ] double spacing option for easier reading?
[x] specifying a filename from commandline
[ ] demo file that explains kanten (tutorial)
[ ] do a full install as a python package with an entry point
[ ] configuration in ~/.config/kanten
[ ] all configurable take defaults from ~/.config, but overwritten by params
[x] configurable number of columns (via -w or --width)
[x] automatically figure out size (for height, and number of columns)
[x] allow setting height from command line
[x] implement :set commands
    (:set height, :set width)
[x] 'v' to edit the file , just like less
    - disabled for stdin? at least if EOF not encountered
    - wouldn't support editing stdin
    [x] respect $EDITOR, use vim if EDITOR not set
        [ ] would be even better if we check if vim exists, use vi otherwise
    [ ] on exit from editing, refresh the file in kanten
[ ] open file to the right line (at least the top-left column's line)
    - set cursor location either via search or mouseclick
    - mouseclick should un-highlight the current match of search (if any) and
      highlight the whole line instead
      - actually - it might make sense to just use widget focus infrastructure
        for this in urwid?
    [ ] mouse click column detection is off by the size of the margins
        - find it from window_width % column_width / num_columns
    [ ] drag click events for paging (flick-click right to go forward, etc)
[ ] run_wrapper to restore previous screen?
    - moar and bpython do this, i think
[ ] (refactor) move all initialization into a function, to allow for re-init
[ ] dynamic resizing of width 
    [ ] (via + and - commands, perhaps)
    [ ] via ":set width=40"
[ ] respond to sigwinch and re-do the number of columns
    - number displayed changed, so the pbar needs an update
[x] param parsing (e.g. add --help)
[x] hide the progress bar (ctrl-n is what zathura uses or something?)
    [x] currently implemented via t
[ ] add a parameter to disable progress bar
[ ] add help dialog on h
    - not ? - that's reverse search, but h and H and F1
    [x] easter egg planted
    [x] :help should work
[x] add : command mode
    [x] support :q
    [x] support :f for file info
    [ ] don't allow deletion of ':' prompt in command mode
    [ ] backspace in command mode should leave it? (like vim
    ( ) history (up and down arrows)
    ( ) q: and :^F command buffer?
[ ] (maybe) support multiple buffers?
    - :n and :p (or :N) for next and previous
    [ ] :e to read a different file?
        - filename tab completion for :e
[x] . (dot) to repeat previous command (page up or down, next, etc)
[ ] support more less/more keys 
    [x] < and >
    [x] z and w
    [x] j and k (page-wise OK)
    [x] = to show file name / info (ctrl-g should also work)
        [x] ctrl-g should show status bar only once
    [ ] others?
    [ ] F - forward forever (for stdin)
[ ] control key combos
    [x] ctrl-v  ctrl-f  for page forward and ctrl-b for back back
    [x] ctrl-g for file info
[ ] (maybe) implement marking system (top lines?)
    - like less, marks will only live for duration of program execution
[ ] support jumping to a particular line
    - would go along with editing a file at that line
    - and allow us to jump back into the same line after editing?
    - jump to the line of the currently highlighted thing
[ ] support ctrl-i and ctrl-o location-list jumping
[x] reflow of text
[ ] (maybe) reading cursor (like dictator?)
[x] 'g' to go to the beginning 
    [ ] gg g0 g$ and other commands?
[ ] implement number prefixes for the commands
[x] 'G' to go to the end
[x] split boxes so that they partially fit
[x] clip off lines that are too long even in one go
[x] last line of text being cut-off
[x] fix formatting (spacing) for cells which were cut
[x] columns currently allowed to expand: don't let them 
    - boxadapter?
    - options('given', width) ?
    - use padding!!
[ ] (alignment) 'the less is' line currently getting screwed up :\
[ ] (refactor) start off with one Padding(text) and keep splitting in
    - don't pre-split
    - such an approach will be a reasonable way to go with searching, anyway
        - i.e. highlight the word in every instance, and reflow?
        - or at least to find the location of the word, and then index into the
          right / approximate column
    - also makes it reasonable to scroll line by line (daisy chain visible cols)
    - stop after all visible columns are filled
[x] search using / (to seek around)
    - use a footer / header of the frame widget to do this and :?
    [x] enter do submit footer content
    [x] esc to refocus the window
    [ ] case insensitive search via -I command line parameter
    [ ] incremental search via incsearch
[x] highlight words in the text
[x] highlight searched word (switch to ANSIText?) 
[x] highlight diff output (via 'd', disable with 'D')
    [ ] detect diff output 
    [x] toggle it via :set ft=diff
    [x] force diff output with -d flag
    [x] use diff output when openining .diff and .patch files
    [ ] handle diff lines that span multiple columns properly 
[ ] parse shell escaped characters properly
    - so we can read man pages
    - look at the piped output of grep --color=always
    - I think the Qt console has some code for this?
[x] replace searched words with highlighted version
[ ] highlight current matched search item differently than some other
    - traverse the contents, and change an 'important' to somehting else
[ ] wrap-around optionally
[x] going forward 4 times and then going back 4 doesn't work :(
    maybe related:
        File "/usr/lib/python2.7/dist-packages/urwid/widget.py", line 142, in cached_render validate_size(self, size, canv)
        File "/usr/lib/python2.7/dist-packages/urwid/widget.py", line 112, in validate_size canv.rows(), size))
        urwid.widget.WidgetError: Widget <Filler box widget <Columns box/flow widget> valign='top'> rendered (193 x 45) canvas when passed size (193, 51
    [x] (maybe) try cache invalidation? nope, _invalidate doesn't hel
    [x] [cols.focus_position=0 seems to have done the trick!
[ ] bash completion for flags (is there one for python  ArgParse code already?)
    - yeap! use https://github.com/kislyuk/argcomplete
[ ] write some tests, with Travis and Coveralls badges in the readme?
    [ ] look at how pudb xo and other urwid apps do testing
    [ ] urwid tests are in e.g. /home/pi/code/urwid/urwid/tests/test_listbox.py
[ ] take a look at what @jlord accomplished with http://jlord.us/blog/horizontal-web.html
[ ] more has a way of listing multiple files
[x] hide progress bar (perhaps on timeout after paging through)
[ ] adding a keyboard shortcut should be sure to remove it from others
[ ] handle really short input gracefully 
    - put the first column on the left-most position
[ ] rest and markdown highlighting (if available)
[ ] use pygments for highlighting, when possible
    [x] detect filetype, at least by filename
    [x] .diff and .patch for ft=diff
    [ ] .py for ft=python
[ ] see  what rifle does in ranger - utilize those plugins / handling
[ ] fix bug for small width and few number of lines (looks ugly, spaces/line
    breaks are missing)
[x] clear edit text on motion key
[ ] kanten.js
[ ] refactor to remove globals with instance variables
[ ] write a kanten man page
    [ ] add section for COMPATIBILITY WITH MORE and COMPATIBILITY WITH LESS
[ ] -F or --quit-if-one-screen
       Causes less to automatically exit if the entire file can be dis‐
       played on the first screen.
[ ] -i or --ignore-case
       Causes searches to ignore case; that is, uppercase and lowercase
       are  considered identical.  This option is ignored if any upper‐
       case letters appear in the search pattern; in other words, if  a
       pattern  contains  uppercase  letters, then that search does not
       ignore case.
[ ] -I or --IGNORE-CASE
       Like -i, but searches ignore case even if the  pattern  contains
       uppercase letters.
[ ] add \c and \C (insensitive and sensitive, respectively) to the
   search parsing
[ ] support set ignorecase? set smartcase?
[ ] add saveas (to save STDIN input)
[ ] center single column?
[ ] version string
    [ ] :version command
[ ] corner cases when -t and -b \lim 0
[ ] for ipython: register .ipynb as a json lexer-able thing
    pygments.lexers.web.JsonLexer
[ ] swallow last empty panel in the banner
[x] make kanten importable (refactor out the cli aspects to make testing easier)
[ ] bug - height is exceeded in my piles (off by one?)
[x] fix command line (ex mode)
[x] fix empty search /<enter>
[x] make reading from stdin work again
[ ] display "Pattern not found" when there are no matching search results
[ ] mouse clicking broken
    Traceback (most recent call last):
      File "/usr/local/bin/kanten", line 9, in <module>
        load_entry_point('kanten==0.5.123', 'console_scripts', 'kanten')()
      File "/Users/pi/code/kanten/kanten.py", line 106, in main
        render_text(text, K)
      File "/Users/pi/code/kanten/kanten.py", line 730, in render_text
        K.loop.run()
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 274, in run
        self.screen.run_wrapper(self._run)
      File "/usr/local/lib/python2.7/site-packages/urwid/raw_display.py", line 268, in run_wrapper
        return fn()
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 339, in _run
        self.event_loop.run()
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 669, in run
        self._loop()
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 706, in _loop
        self._watch_files[fd]()
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 390, in _update
        self.process_input(keys)
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 496, in process_input
        something_handled |= bool(self.unhandled_input(k))
      File "/usr/local/lib/python2.7/site-packages/urwid/main_loop.py", line 542, in unhandled_input
        return self._unhandled_input(input)
      File "/Users/pi/code/kanten/kanten.py", line 470, in show_or_exit
        column = xpos_to_col(key[-2])
    NameError: global name 'xpos_to_col' is not defined
[ ] use collections.deque for the sliding window of visible piles
[ ] look up newspaper column width stuff - reading research lit on it?
[ ] use constant memory - or at least add an option for that (page out columns)
[ ] fix redirection of stdout
    22:25@kanten(master)$ kanten demo  -w 40  -l 7 > hello
    Traceback (most recent call last):
      File "/usr/local/bin/kanten", line 9, in <module>
        load_entry_point('kanten==0.5.123', 'console_scripts', 'kanten')()
      File "/Users/pi/code/kanten/kanten.py", line 107, in main
        render_text(text, K)
      File "/Users/pi/code/kanten/kanten.py", line 710, in render_text
        border = (K.max_width - c_columns[-1]) /  K.displayed_columns
    IndexError: list index out of range
[ ] show more than just the visible columns (new bug)
[x] padding - add more empty columns to fill the visible space
[ ] have kanten respect LESS flags (like `export LESS=-Ri', for example)
[x] BUG: kanten demo does not work to stay on the left most screen when pressing
    spacebar - partially fixed this - but not completely. ok, fixed
[ ] scroll for long contents
[ ] better error message: this is too long:
        Traceback (most recent call last):
        File "/usr/local/bin/kanten", line 9, in <module>
        load_entry_point('kanten==0.5.123', 'console_scripts', '
        File "/Users/pi/code/kanten/kanten.py", line 107, in main
        text, fname = read(fname)
        File "/Users/pi/code/kanten/kanten.py", line 557, in read
        with open(fname) as f:
        IOError: [Errno 2] No such file or directory: 'kanten.'
[ ] line numbering (:set nu)
[ ] horizontal progress bar is again not being updated.
[ ] remove the global lexer - that's just fugly.
[ ] oops - how about we don't explode the memory usage by inserting the contents
    a hole bunch of times.
[ ] it's a struggle for how to think about K.idx
    - on the one hand - K.idx = 0 makes sense,
    - on the other hand - that makes it tricky for dealing with reaching the end
[ ] don't read the whole file on startup - delay it
    - verify via `make big` that the footprint isn't huge on startup
[x] new flag `-c` `--columns` (complement to `--width`)
[ ] last pane is getting extra crap in it
[ ] 'k' kind of almost works - something is not being update
[ ] eliminate all `off_screen` stuff
[x] J and K are just like space and 
[ ] keep track of real length (don't increment index past end - display columns)
[ ] clean up the reading of man pages- take out extra characters
[ ] make line wrapping optional (useful for reviewing code, perhaps)


TEST PLAN
---------

[ ] / to search  (with highlighting)
[ ] g / G to go to the beginning / end of the file
[ ] ? to get help
[x] short file (single column)
[ ] medium file (exactly display_column width)
    - requires getting some stuff right, or specifying width and height
[ ] a long file ( > display_column width)
[ ] remove all 'offscreen' stuff?
[ ] make a page generator, to make manual testing easier
    for example: 
    - runs urwid commands to get dimensions of the terminal
    - generates a text file (or a buffer) that fills precisely one column
    - that fits exactly as many columns as can fit.
    - labels every page and every line on a page?

RELATED PROJECTS
----------------

### actively developed pagers

[less](http://www.greenwoodsoftware.com/less/) - the de-facto pager released in
1985, still actively developed 30 years later  (Mark Nudelman)

[w3m](http://w3m.sourceforge.net/) - a text-based web browser that also works as
a pager, that can even render and browse man pages as w3mman.  (Akinori Ito)

[moar](https://github.com/walles/moar) -  syntax highlighting for code,
incremental search, ANSI color escape support (Johan Walles)

### historical

[more](http://en.wikipedia.org/wiki/More_(command)) - venerable unix tool (Dan
Halbert)

[pg](http://en.wikipedia.org/wiki/Pg_(Unix)) - another old unix pager, you don't
see it in newer systems anymore.

[most](http://www.jedsoft.org/most/) - multiple windows, can scroll left and
right. (John E Davis) (inactive, last release in 2007)


### different application of related ideas

[Tofu](http://amarsagoo.info/tofu/) - a multi-column reader for OS X. The
inspiration for kanten. (Amar Sagoo)

[Sidenote.io](http://sidenote.io) - column format structure for documents.

[wtail](http://www.teaser.fr/~amajorel/wtail/) - tail -f on multiple files

[multitail](http://www.vanheusden.com/multitail/) - tail -f on multiple files

[mpage.vim](http://www.drchip.org/astronaut/vim/index.html#MPAGE) - same idea in
vim. (Charles E Campbell)


