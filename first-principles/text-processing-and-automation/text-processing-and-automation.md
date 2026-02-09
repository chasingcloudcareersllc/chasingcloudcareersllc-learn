---
title: "Text Processing and Automation"
description: "Vim, shell scripting, regular expressions, text processing pipelines, and task scheduling — the tools that turn manual operations into repeatable, reliable automation."
position: 4
icon: "pen-tool"
---

# Text Processing and Automation

**Prerequisites:** [Operating Systems and Linux](/learn/first-principles/operating-systems-and-linux/)

This domain teaches you to manipulate text, automate workflows, and build pipelines that process data at the speed of thought. Vim is your editor. Bash is your glue language. grep, sed, and awk are your transformation engines. Regular expressions are the pattern language that connects them all. Cron and systemd timers make automation run on a schedule without human intervention.

These are not legacy tools you learn for historical reasons. They are the daily instruments of every infrastructure professional. Log analysis, configuration management, deployment scripting, incident response automation, data extraction from APIs -- all of it flows through text processing and shell scripting. Master this domain and you will spend minutes on tasks that take others hours.

## Learning Objectives

By the end of this domain, you will be able to:

1. Edit files in Vim fluently using modal editing, motions, operators, text objects, registers, and macros
2. Write Bash scripts with argument parsing, error handling, control flow, and signal trapping
3. Build multi-stage text processing pipelines with grep, sed, awk, sort, and pipes
4. Use regular expressions for pattern matching and explain their connection to finite automata (formalized in [Domain 13](/learn/first-principles/theory-of-computation/))
5. Schedule tasks with cron and systemd timers, and prevent concurrent execution with flock
6. Debug scripts systematically and know when to switch to Python

---

## A. Vim

### Theory

Vim's modes are a finite state machine. This is not a metaphor. Normal mode, insert mode, visual mode, and command-line mode are states. Keypresses are transitions between those states. Pressing `i` in normal mode transitions to insert mode. Pressing `Esc` in insert mode transitions back to normal mode. Pressing `:` in normal mode transitions to command-line mode.

```mermaid
stateDiagram-v2
    [*] --> Normal
    Normal --> Insert: i, a, o, c, s, R
    Insert --> Normal: Esc / Ctrl-[
    Normal --> Visual: v, V, Ctrl-v
    Visual --> Normal: Esc / Ctrl-[
    Normal --> CommandLine: : / /
    CommandLine --> Normal: Enter / Esc
    Visual --> CommandLine: :
```

The modal model is efficient because of a fundamental observation about editing: most editing time is spent navigating and thinking, not typing. When you are reading code, searching for a function, moving between files, or deciding what to change, you are in normal mode. Typing new characters is a brief interruption in an otherwise navigation-heavy workflow. By separating the editing state from the typing state, Vim dedicates the entire keyboard to navigation and manipulation commands. Every letter becomes a command instead of a character.

This is the opposite of how modeless editors work. In VS Code or Sublime Text, every keypress inserts a character unless you hold a modifier key. In Vim, every keypress executes a command unless you are explicitly in insert mode. The result is that complex editing operations -- delete a word, change a sentence, yank a paragraph, indent a block -- are one or two keystrokes instead of reaching for the mouse or memorizing multi-key chord sequences.

### Practice

#### Modal Model

| Mode | Purpose | Enter With | Leave With |
|------|---------|------------|------------|
| Normal | Navigate, manipulate, compose commands | `Esc`, `Ctrl-[` | (transition keys) |
| Insert | Type text | `i`, `a`, `o`, `O`, `I`, `A`, `s`, `S`, `c`, `C`, `R` | `Esc` |
| Visual | Select regions | `v` (char), `V` (line), `Ctrl-v` (block) | `Esc` |
| Command-line | Ex commands, search | `:`, `/`, `?` | `Enter`, `Esc` |
| Replace | Overwrite characters | `R` from normal | `Esc` |

The first habit to build: when you finish typing, press `Esc` immediately. You should spend 80% or more of your time in normal mode. If you find yourself staying in insert mode and using arrow keys, you are using Vim like a modeless editor and losing the efficiency gains.

#### Navigation

Horizontal movement:

| Key | Motion |
|-----|--------|
| `h` | One character left |
| `l` | One character right |
| `w` | Forward to start of next word |
| `b` | Backward to start of previous word |
| `e` | Forward to end of current/next word |
| `W`, `B`, `E` | Same as w/b/e but WORD (whitespace-delimited) |
| `0` | Start of line |
| `^` | First non-blank character of line |
| `$` | End of line |
| `f{char}` | Forward to next occurrence of {char} on line |
| `t{char}` | Forward to just before next {char} on line |
| `F{char}`, `T{char}` | Same but backward |
| `;` | Repeat last f/t motion forward |
| `,` | Repeat last f/t motion backward |

Vertical movement:

| Key | Motion |
|-----|--------|
| `j` | One line down |
| `k` | One line up |
| `gg` | First line of file |
| `G` | Last line of file |
| `{number}G` or `:{number}` | Go to line number |
| `{` | Previous blank line (paragraph up) |
| `}` | Next blank line (paragraph down) |
| `Ctrl-d` | Half page down |
| `Ctrl-u` | Half page up |
| `Ctrl-f` | Full page down |
| `Ctrl-b` | Full page up |
| `H` | Top of screen |
| `M` | Middle of screen |
| `L` | Bottom of screen |
| `%` | Matching bracket/paren/brace |

Search movement:

| Key | Motion |
|-----|--------|
| `/pattern` | Search forward for pattern |
| `?pattern` | Search backward for pattern |
| `n` | Next search match |
| `N` | Previous search match |
| `*` | Search forward for word under cursor |
| `#` | Search backward for word under cursor |

Marks:

```vim
ma          " Set mark 'a' at current position
'a          " Jump to line of mark 'a'
`a          " Jump to exact position of mark 'a'
''          " Jump to position before last jump
`.          " Jump to position of last change
```

Marks `a-z` are local to a buffer. Marks `A-Z` are global across files. The backtick form jumps to the exact column; the apostrophe form jumps to the first non-blank character of the marked line.

> **Try It**: Open any file in Vim. Navigate using only `h/j/k/l` for one minute. Then switch to word motions (`w/b/e`) for one minute. Notice how much faster word motions are. Now try `/` to search for a specific string and `n/N` to jump between matches. Finally, set a mark with `ma`, move elsewhere in the file, and jump back with `` `a ``.

#### Operators and Text Objects

This is where Vim becomes a language instead of a collection of shortcuts. Vim commands follow a composable grammar:

```
[count] operator [count] motion/text-object
```

The core operators:

| Operator | Action |
|----------|--------|
| `d` | Delete (cut) |
| `c` | Change (delete and enter insert mode) |
| `y` | Yank (copy) |
| `>` | Indent right |
| `<` | Indent left |
| `=` | Auto-indent |
| `gU` | Uppercase |
| `gu` | Lowercase |
| `g~` | Toggle case |

Text objects define regions of text. They come in two flavors: `i` (inner -- excludes delimiters) and `a` (around -- includes delimiters):

| Text Object | What It Selects |
|-------------|----------------|
| `iw` / `aw` | Inner/around word |
| `iW` / `aW` | Inner/around WORD |
| `is` / `as` | Inner/around sentence |
| `ip` / `ap` | Inner/around paragraph |
| `i"` / `a"` | Inner/around double quotes |
| `i'` / `a'` | Inner/around single quotes |
| `` i` `` / `` a` `` | Inner/around backticks |
| `i(` / `a(` | Inner/around parentheses |
| `i[` / `a[` | Inner/around brackets |
| `i{` / `a{` | Inner/around braces |
| `i<` / `a<` | Inner/around angle brackets |
| `it` / `at` | Inner/around HTML/XML tag |

Now combine operators with text objects:

```vim
dw          " Delete from cursor to start of next word
diw         " Delete inner word (the whole word, wherever cursor is)
daw         " Delete around word (word + surrounding space)
ci"         " Change inner double quotes (delete contents, enter insert)
da(         " Delete around parentheses (contents + parens)
yip         " Yank inner paragraph
>i{         " Indent inner braces
gUiw        " Uppercase inner word
```

The power is in composability. You do not memorize `ci"` as a single command meaning "change inside quotes." You learn `c` (change), `i` (inner), `"` (double quotes) as three components. Once you know these components, you can construct commands you have never seen before:

- `da[` -- delete around brackets. You have never typed this before, but you know exactly what it does.
- `gUis` -- uppercase inner sentence. You constructed it on the fly.
- `>ip` -- indent inner paragraph. The grammar produces it.

This composability is what makes Vim's learning curve worth the investment. A modeless editor with 200 keyboard shortcuts requires you to memorize 200 things. Vim with 10 operators and 15 text objects gives you 150 combinations, and you only memorized 25 things.

#### The Dot Command

The `.` command repeats the last change. This is one of Vim's most powerful features because it turns any editing operation into a repeatable action.

```vim
ciw replacement_text<Esc>    " Change inner word to "replacement_text"
n                             " Jump to next search match
.                             " Repeat the change -- same replacement
n.                            " Jump and repeat again
```

The dot command works with any change: deletions, insertions, replacements, indentation. Plan your edits so they are dot-repeatable. If you need to do the same thing in five places, do it once with a repeatable command, then navigate and press `.` four times.

#### Registers

Vim has multiple registers for storing text. Every delete and yank goes into a register.

| Register | Description |
|----------|-------------|
| `""` | Unnamed register (default for d/y) |
| `"a` to `"z` | Named registers (you choose) |
| `"A` to `"Z` | Append to named register |
| `"0` | Yank register (last yank only, not affected by deletes) |
| `"1` to `"9` | Numbered registers (history of deletes) |
| `"+` | System clipboard |
| `"*` | Primary selection (X11) / system clipboard (macOS) |
| `"_` | Black hole register (discard) |
| `".` | Last inserted text |
| `"%` | Current filename |
| `"=` | Expression register |

Using registers:

```vim
"ayiw        " Yank inner word into register a
"ap          " Paste from register a
"+y          " Yank to system clipboard
"+p          " Paste from system clipboard
"_dd         " Delete line without affecting any register
:reg         " View all register contents
```

The yank register `"0` solves a common frustration: you yank text, delete something else (which overwrites the unnamed register), then try to paste your yanked text. Use `"0p` to paste what you last yanked, regardless of subsequent deletes.

The expression register `"=` lets you insert calculated values. In insert mode, press `Ctrl-r =` and type an expression like `2+2` followed by Enter. Vim inserts `4`.

#### Search and Replace

The substitute command is Vim's search-and-replace engine. It uses regular expressions by default.

```vim
:s/old/new/           " Replace first 'old' with 'new' on current line
:s/old/new/g          " Replace all on current line
:%s/old/new/g         " Replace all in entire file
:%s/old/new/gc        " Replace all, asking for confirmation each time
:5,20s/old/new/g      " Replace all on lines 5-20
:'<,'>s/old/new/g     " Replace all in visual selection
```

The `:g` (global) command executes an Ex command on every line matching a pattern:

```vim
:g/pattern/d          " Delete every line matching pattern
:g/^$/d               " Delete all blank lines
:g/TODO/normal A ;    " Append " ;" to every line containing TODO
:g/DEBUG/move $       " Move all DEBUG lines to end of file
```

The `:v` command is the inverse -- it operates on lines that do NOT match:

```vim
:v/pattern/d          " Delete every line NOT matching pattern
                      " (keep only lines that match)
```

> **Try It**: Create a file with 20 lines of mixed text. Use `:%s/foo/bar/gc` to selectively replace words. Use `:g/^#/d` to delete all comment lines. Use `:v/error/d` to keep only lines containing "error."

#### Macros

A macro records a sequence of keystrokes and replays them. This is batch editing at its most powerful.

```vim
qa          " Start recording into register 'a'
...         " Perform editing operations
q           " Stop recording

@a          " Play macro from register 'a'
@@          " Repeat last played macro
5@a         " Play macro 'a' five times
```

Macro best practices:

1. Start with a consistent position. Begin the macro at the start of a line (`0`) or on a search match.
2. Use motions, not arrow keys. Motions like `w`, `f{char}`, and `/pattern` are more robust.
3. End in position for the next iteration. If processing lines, end with `j0` to move to the start of the next line.
4. Recursive macros: `qaqqa...@aq` -- clear register `a`, start recording, do your edits, call `@a` recursively, stop. The macro runs until it hits an error (like end of file).

Example: add semicolons to the end of every line that does not already have one:

```vim
qa          " Start recording to 'a'
0           " Go to start of line
$           " Go to end of line
a;<Esc>     " Append semicolon
j           " Move to next line
q           " Stop recording
100@a       " Replay 100 times (stops at end of file)
```

#### Buffers, Windows, and Tabs

Vim separates the concepts of buffers (open files), windows (viewports), and tabs (collections of windows).

```vim
:e filename         " Open file in current window
:ls                 " List all buffers
:bn / :bp           " Next/previous buffer
:bd                 " Close buffer
:b name             " Switch to buffer by partial name

:split / :sp        " Horizontal split
:vsplit / :vsp      " Vertical split
Ctrl-w h/j/k/l     " Move between windows
Ctrl-w =            " Equalize window sizes
Ctrl-w _            " Maximize current window height
Ctrl-w |            " Maximize current window width
:close              " Close current window

:tabnew             " New tab
:tabn / :tabp       " Next/previous tab
gt / gT             " Next/previous tab
```

Think of it this way: a buffer is a file loaded into memory. A window is a viewport into a buffer. Multiple windows can show the same buffer. A tab is a layout of windows. You almost always want multiple buffers and windows. Tabs are less commonly needed.

#### Configuration

Vim reads `~/.vimrc` at startup. Essential settings:

```vim
" ~/.vimrc
set nocompatible          " Disable Vi compatibility
syntax on                 " Syntax highlighting
filetype plugin indent on " Filetype detection and indentation

set number                " Line numbers
set relativenumber        " Relative line numbers (powerful with motions)
set cursorline            " Highlight current line
set showmatch             " Highlight matching brackets
set hlsearch              " Highlight search results
set incsearch             " Incremental search (show matches as you type)
set ignorecase            " Case-insensitive search
set smartcase             " Case-sensitive if search has uppercase

set tabstop=4             " Tab width
set shiftwidth=4          " Indent width
set expandtab             " Spaces instead of tabs
set autoindent            " Copy indent from current line to new line
set smartindent           " Smart autoindenting for C-like languages

set scrolloff=8           " Keep 8 lines visible above/below cursor
set sidescrolloff=8       " Keep 8 columns visible left/right
set nowrap                " Do not wrap long lines
set signcolumn=yes        " Always show sign column

set undofile              " Persistent undo across sessions
set undodir=~/.vim/undodir

" Key mappings
let mapleader = " "       " Space as leader key
nnoremap <leader>w :w<CR> " Space+w to save
nnoremap <leader>q :q<CR> " Space+q to quit
nnoremap <leader>/ :nohlsearch<CR>  " Clear search highlight

" Autocommands
autocmd BufWritePre * %s/\s\+$//e   " Strip trailing whitespace on save
autocmd FileType python setlocal tabstop=4 shiftwidth=4
autocmd FileType yaml setlocal tabstop=2 shiftwidth=2
```

#### Neovim

Neovim is a fork of Vim with modern defaults, a Lua scripting engine, and built-in LSP (Language Server Protocol) support. Configuration lives in `~/.config/nvim/init.lua`:

```lua
-- ~/.config/nvim/init.lua
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.tabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = true
vim.opt.smartindent = true
vim.opt.undofile = true
vim.opt.scrolloff = 8
vim.opt.signcolumn = "yes"

vim.g.mapleader = " "
vim.keymap.set("n", "<leader>w", ":w<CR>")
vim.keymap.set("n", "<leader>q", ":q<CR>")
```

Neovim's LSP integration provides code completion, go-to-definition, hover documentation, and diagnostics directly in the editor. This makes Neovim competitive with full IDEs while retaining Vim's modal editing model.

### Connection

The operator + text object composability (`ciw` = change inner word, `da(` = delete around parens) is a language, not a set of shortcuts. Once you internalize the grammar, you can construct commands you have never seen before. This is the same principle behind composable Unix commands (small tools connected by pipes) and will appear again in functional programming (composable functions in [Domain 5](/learn/first-principles/programming-fundamentals/)).

The finite state machine model of Vim's modes is a concrete instance of the automata theory formalized in [Domain 13](/learn/first-principles/theory-of-computation/). You are interacting with a finite state machine every time you press a key. States are modes. Transitions are keypresses. The next time someone says "automata are theoretical," you can point out that you have been using one for hours every day.

> **Try It**: Open a source code file in Vim. Without looking up any commands, try to construct these operations from the grammar you just learned: (1) delete everything inside curly braces, (2) change the text inside double quotes, (3) yank an entire paragraph, (4) uppercase a word, (5) auto-indent an entire file. You should be able to derive all five from the operator + text object grammar.

---

## B. Text Processing Pipeline

### Theory

Unix text processing follows a pipeline model: data flows from one program to the next through pipes. Each program does one thing well. The shell connects them. This is the Unix philosophy in action -- small, composable tools combined to solve problems no single tool was designed for.

A typical pipeline has four stages:

```mermaid
flowchart LR
    A["Extract"] --> B["Filter"]
    B --> C["Transform"]
    C --> D["Aggregate"]
```

- **Extract**: Pull data from files, commands, or APIs (`cat`, `curl`, process substitution)
- **Filter**: Keep or discard lines based on patterns (`grep`, `sed` deletion, `awk` conditions)
- **Transform**: Reshape data (`sed` substitution, `awk` field manipulation, `cut`, `tr`)
- **Aggregate**: Summarize results (`sort`, `uniq -c`, `wc`, `awk` accumulators)

Each stage receives text on stdin, processes it, and writes text to stdout. The pipe operator `|` connects stdout of one process to stdin of the next. The kernel manages the pipe buffer. If the downstream process is slower, the upstream process blocks. This is automatic backpressure.

**Regular expressions** are the pattern language used by grep, sed, awk, and most programming languages. A regex defines a pattern that describes a set of strings. At a deep level, a regex defines a regular language -- a set of strings that can be recognized by a finite automaton. This connection between the practical tool (regex syntax) and the theoretical framework (automata theory) is formalized in [Domain 13](/learn/first-principles/theory-of-computation/), but the practical intuition starts here.

When `grep -E '^\d{1,3}(\.\d{1,3}){3}$'` matches an IP address, it is because the regex describes a regular language that a DFA (deterministic finite automaton) can accept. You do not need to know the formal theory yet -- but you will, and when you do, this will click.

### Practice

#### grep

`grep` searches for patterns in text. It is the most-used text processing tool.

```bash
# Basic search
grep "error" /var/log/syslog          # Lines containing "error"
grep -i "error" /var/log/syslog       # Case-insensitive
grep -v "debug" /var/log/syslog       # Lines NOT containing "debug"
grep -c "error" /var/log/syslog       # Count matching lines
grep -n "error" /var/log/syslog       # Show line numbers
grep -l "TODO" src/*.py               # List files containing "TODO"
grep -r "import os" ./src/            # Recursive search in directory
grep -rn "def main" ./src/            # Recursive with line numbers

# Extended regex (-E or egrep)
grep -E "error|warning|critical" log  # Alternation (OR)
grep -E "^[0-9]{4}-" log             # Lines starting with 4 digits and dash
grep -E "\b[A-Z]{2,5}\b" log         # Words of 2-5 uppercase letters

# Context lines
grep -A 3 "error" log                # 3 lines After each match
grep -B 2 "error" log                # 2 lines Before each match
grep -C 2 "error" log                # 2 lines Context (before and after)

# Fixed strings (no regex interpretation)
grep -F "192.168.1.1" log            # Literal string, faster than regex

# Inverse match with counting
grep -vc "^#" config.conf            # Count non-comment lines

# Only matching part
grep -o '[0-9]\+\.[0-9]\+\.[0-9]\+\.[0-9]\+' log  # Extract IP addresses only
```

Basic vs. extended regex: `grep` uses Basic Regular Expressions (BRE) by default, where `+`, `?`, `{`, `|`, and `(` are literal unless escaped with `\`. `grep -E` (Extended Regular Expressions, ERE) treats them as metacharacters without escaping. Always use `grep -E` unless you have a reason not to.

#### sed

`sed` (stream editor) transforms text line by line. It reads input, applies editing commands, and writes the result.

```bash
# Substitution: s/pattern/replacement/flags
sed 's/old/new/' file               # Replace first occurrence per line
sed 's/old/new/g' file              # Replace all occurrences per line
sed 's/old/new/gi' file             # Replace all, case-insensitive
sed 's/old/new/3' file              # Replace third occurrence per line

# In-place editing
sed -i 's/old/new/g' file           # Modify file directly (GNU sed)
sed -i '' 's/old/new/g' file        # macOS sed requires '' after -i
sed -i.bak 's/old/new/g' file       # Create backup before modifying

# Address ranges
sed '5s/old/new/' file              # Only on line 5
sed '5,10s/old/new/g' file          # Lines 5-10
sed '/pattern/s/old/new/g' file     # Lines matching /pattern/
sed '/start/,/end/s/old/new/g' file # From /start/ to /end/

# Deletion
sed '/^#/d' file                    # Delete comment lines
sed '/^$/d' file                    # Delete blank lines
sed '1,5d' file                     # Delete lines 1-5

# Insertion and appending
sed '3i\New line above line 3' file  # Insert before line 3
sed '3a\New line below line 3' file  # Append after line 3

# Multiple commands
sed -e 's/foo/bar/g' -e '/^$/d' file
sed 's/foo/bar/g; /^$/d' file        # Same thing with semicolons

# Backreferences
sed 's/\(.*\):\(.*\)/\2:\1/' file    # Swap fields around colon
sed -E 's/(.*):(.*)/\2:\1/' file     # Same with extended regex (-E)

# Print specific lines
sed -n '5p' file                     # Print only line 5
sed -n '5,10p' file                  # Print lines 5-10
sed -n '/error/p' file               # Print lines matching "error"

# Transliteration (like tr)
sed 'y/abc/ABC/' file                # Replace a->A, b->B, c->C
```

The `-n` flag suppresses automatic output. Combine with `p` to print only matched lines -- this makes sed behave like grep, but with more transformation power.

#### awk

`awk` is a field-processing language. It splits each line into fields (default delimiter: whitespace) and lets you operate on them by position.

```bash
# Basic field extraction
awk '{print $1}' file               # Print first field
awk '{print $NF}' file              # Print last field ($NF = number of fields)
awk '{print $1, $3}' file           # Print first and third fields
awk '{print $2 "\t" $4}' file       # Print with explicit tab separator

# Custom field separator
awk -F: '{print $1, $7}' /etc/passwd   # Colon-separated (username, shell)
awk -F, '{print $2}' data.csv          # Comma-separated

# Pattern matching (only process matching lines)
awk '/error/ {print $0}' log           # Lines containing "error"
awk '$3 > 100 {print $1, $3}' data     # Lines where field 3 > 100
awk '$NF == "FAIL" {print}' results    # Lines where last field is "FAIL"
awk 'NR >= 10 && NR <= 20' file        # Lines 10-20 (NR = line number)

# BEGIN and END blocks
awk 'BEGIN {print "Name\tScore"} {print $1 "\t" $2} END {print "---done---"}' data

# Arithmetic and accumulators
awk '{sum += $2} END {print "Total:", sum}' data
awk '{sum += $2; count++} END {print "Average:", sum/count}' data
awk '{if ($3 > max) max = $3} END {print "Max:", max}' data

# Built-in variables
awk '{print NR, NF, $0}' file       # Line number, field count, full line
awk 'END {print NR}' file           # Total number of lines (like wc -l)
awk -F: 'BEGIN {OFS="\t"} {print $1, $7}' /etc/passwd  # Output separator

# String functions
awk '{print length($0)}' file        # Length of each line
awk '{print toupper($1)}' file       # Uppercase first field
awk '{gsub(/old/, "new"); print}' file  # Global substitution

# Associative arrays (hash maps)
awk '{count[$1]++} END {for (k in count) print k, count[k]}' log
# Counts occurrences of each unique value in field 1

# Formatted output
awk '{printf "%-20s %10.2f\n", $1, $2}' data
```

awk is a complete programming language. It has variables, arrays, functions, loops, and conditionals. For simple field extraction, the one-liner form is ideal. For complex logic, consider a standalone awk script or switching to Python.

#### Supporting Tools

```bash
# sort - sort lines
sort file                            # Alphabetical sort
sort -n file                         # Numeric sort
sort -r file                         # Reverse sort
sort -k2 file                        # Sort by second field
sort -k2,2n file                     # Sort numerically by second field only
sort -t: -k3 -n /etc/passwd          # Sort by UID (field 3, colon-separated)
sort -u file                         # Sort and remove duplicates

# uniq - deduplicate adjacent lines (requires sorted input)
sort file | uniq                     # Remove duplicates
sort file | uniq -c                  # Count occurrences
sort file | uniq -d                  # Show only duplicates
sort file | uniq -c | sort -rn       # Frequency count, most common first

# cut - extract columns
cut -d: -f1,7 /etc/passwd           # Fields 1 and 7, colon-delimited
cut -c1-10 file                     # Characters 1-10 of each line
cut -d, -f2-4 data.csv              # Fields 2-4, comma-delimited

# tr - translate or delete characters
tr 'a-z' 'A-Z' < file               # Lowercase to uppercase
tr -d '\r' < file                    # Remove carriage returns (DOS -> Unix)
tr -s ' ' < file                     # Squeeze repeated spaces to single space
tr ':' '\t' < file                   # Replace colons with tabs

# wc - word, line, character count
wc -l file                           # Line count
wc -w file                           # Word count
wc -c file                           # Byte count
wc -m file                           # Character count (multi-byte aware)

# diff - compare files
diff file1 file2                     # Show differences
diff -u file1 file2                  # Unified diff format (most readable)
diff -y file1 file2                  # Side-by-side comparison
diff -r dir1/ dir2/                  # Recursive directory comparison

# tee - write to file AND stdout
command | tee output.log             # Save output and display it
command | tee -a output.log          # Append instead of overwrite
command | tee >(grep error > err.log) | process_further
```

#### Multi-Stage Pipeline Examples

**Example 1**: Find the top 10 most frequent IP addresses in a web server log.

```bash
awk '{print $1}' /var/log/nginx/access.log |
  sort |
  uniq -c |
  sort -rn |
  head -10
```

Stage by stage: (1) extract the first field (IP address), (2) sort so identical IPs are adjacent, (3) count unique occurrences, (4) sort by count descending, (5) take the top 10.

**Example 2**: Find all unique HTTP status codes and their counts.

```bash
awk '{print $9}' /var/log/nginx/access.log |
  grep -E '^[0-9]{3}$' |
  sort |
  uniq -c |
  sort -rn
```

**Example 3**: Extract email addresses from a file.

```bash
grep -oE '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' file |
  sort -u
```

**Example 4**: Process a CSV -- compute average of column 3 for rows where column 2 is "active".

```bash
awk -F, '$2 == "active" {sum += $3; count++} END {print sum/count}' data.csv
```

**Example 5**: Find all processes using more than 1GB of memory.

```bash
ps aux |
  awk 'NR > 1 && $6 > 1048576 {printf "%-10s %8.1f MB  %s\n", $1, $6/1024, $11}'
```

**Example 6**: Monitor a log file in real time, filtering for errors.

```bash
tail -f /var/log/syslog | grep --line-buffered -i "error" | tee errors.log
```

The `--line-buffered` flag ensures grep outputs each match immediately rather than buffering.

> **Try It**: Create a sample CSV file with columns: name, department, salary. Write a pipeline that: (1) filters to a specific department, (2) extracts the salary column, (3) computes the total. Then write another pipeline that finds the top 3 highest-paid people across all departments.

#### Regular Expression Syntax

Regular expressions are patterns that describe sets of strings. This table covers the syntax used across grep, sed, awk, and most programming languages.

| Syntax | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `.` | Any single character | `h.t` | hat, hit, hot |
| `*` | Zero or more of preceding | `ab*c` | ac, abc, abbc |
| `+` | One or more of preceding | `ab+c` | abc, abbc (not ac) |
| `?` | Zero or one of preceding | `colou?r` | color, colour |
| `{n}` | Exactly n of preceding | `a{3}` | aaa |
| `{n,}` | n or more of preceding | `a{2,}` | aa, aaa, aaaa... |
| `{n,m}` | Between n and m | `a{2,4}` | aa, aaa, aaaa |
| `^` | Start of line | `^Error` | Lines starting with Error |
| `$` | End of line | `\.conf$` | Lines ending with .conf |
| `[abc]` | Character class (any of a, b, c) | `[aeiou]` | Any vowel |
| `[^abc]` | Negated class (not a, b, c) | `[^0-9]` | Any non-digit |
| `[a-z]` | Range | `[A-Za-z]` | Any letter |
| `\b` | Word boundary | `\bword\b` | "word" not "password" |
| `\d` | Digit (Perl-compatible) | `\d+` | One or more digits |
| `\w` | Word character (a-z, 0-9, _) | `\w+` | One or more word chars |
| `\s` | Whitespace | `\s+` | One or more spaces/tabs |
| `(...)` | Grouping | `(ab)+` | ab, abab, ababab |
| `\1` | Backreference to group 1 | `(\w+) \1` | "the the" (repeated words) |
| `\|` or `|` | Alternation (OR) | `cat\|dog` | cat or dog |

**Character class shortcuts** (POSIX, used in grep and sed):

| Class | Equivalent | Meaning |
|-------|-----------|---------|
| `[:alpha:]` | `[A-Za-z]` | Letters |
| `[:digit:]` | `[0-9]` | Digits |
| `[:alnum:]` | `[A-Za-z0-9]` | Letters and digits |
| `[:space:]` | `[ \t\n\r\f\v]` | Whitespace |
| `[:upper:]` | `[A-Z]` | Uppercase letters |
| `[:lower:]` | `[a-z]` | Lowercase letters |

Use these inside bracket expressions: `[[:digit:]]` matches a digit. Note the double brackets.

**BRE vs. ERE differences:**

| Feature | BRE (grep) | ERE (grep -E) |
|---------|-----------|---------------|
| Alternation | `\|` | `|` |
| Grouping | `\(...\)` | `(...)` |
| One or more | `\+` | `+` |
| Zero or one | `\?` | `?` |
| Repetition | `\{n,m\}` | `{n,m}` |

ERE is almost always what you want. Use `grep -E`, `sed -E`, and `awk` (which uses ERE natively).

### Connection

Text processing pipelines embody the Unix philosophy: each tool does one thing, tools communicate through text streams, and the shell composes them into solutions. This same composability principle appears in functional programming (function composition in [Domain 5](/learn/first-principles/programming-fundamentals/)), in microservice architectures (service composition in [Domain 12](/learn/first-principles/infrastructure-at-scale/)), and in CI/CD pipelines (stage composition in [Domain 11](/learn/first-principles/software-engineering-and-collaboration/)).

Regular expressions are not just a practical tool -- they are the syntax for describing regular languages, one of the foundational classes of formal languages in computer science. Every regex you write specifies a pattern that can be compiled into a finite automaton. The `grep` command literally does this: it compiles your regex into a DFA and runs it against each line of input. When you understand this in [Domain 13](/learn/first-principles/theory-of-computation/), you will also understand why some patterns (like matching balanced parentheses) cannot be expressed as regex -- they require a more powerful computational model.

> **Try It**: Write a regex that matches valid IPv4 addresses (four groups of 1-3 digits separated by dots). Then think about whether your regex also matches invalid addresses like `999.999.999.999`. Tightening the regex to reject invalid addresses is surprisingly difficult -- this hints at the limits of what regular languages can express compactly.

---

## C. Shell Scripting

### Theory

A shell script is a program written in the shell's scripting language. Bash (Bourne Again Shell) is the most widely used shell on Linux systems. Shell scripts are the glue of system administration: they connect commands, automate workflows, handle errors, and run on schedules.

Shell scripting occupies a specific niche in the programming landscape. It excels at:

- Orchestrating other programs (running commands, piping output, checking exit codes)
- File and directory manipulation
- Text processing (invoking grep, sed, awk)
- Environment configuration
- Quick automation of repetitive tasks

It is weaker at:

- Complex data structures (no native objects, limited arrays)
- Error handling (global traps, not exceptions)
- Large-scale software engineering (no modules, no type system)
- Performance-critical computation
- Cross-platform portability (subtle differences between systems)

Know when to stay in Bash and when to switch to Python. If your script exceeds 200 lines, needs data structures beyond arrays, or requires complex error handling, it is time for Python. The heuristic: shell scripts orchestrate; Python programs compute.

### Practice

#### The Basics

Every script starts with a shebang line that tells the kernel which interpreter to use:

```bash
#!/usr/bin/env bash
```

Use `#!/usr/bin/env bash` instead of `#!/bin/bash` because `env` finds bash in the user's PATH, which is more portable across systems where bash may be installed in different locations.

Make a script executable:

```bash
chmod +x script.sh
./script.sh           # Run it
bash script.sh        # Or invoke bash explicitly
```

Exit codes communicate success or failure:

```bash
exit 0    # Success
exit 1    # General error
exit 2    # Misuse of shell command
# Convention: 0 = success, 1-255 = various errors

# Check the last command's exit code
some_command
echo $?   # Prints 0 if successful, non-zero if failed
```

Every command returns an exit code. `$?` contains the exit code of the last executed command. `0` means success. Anything else means failure. This convention is used by `if`, `&&`, `||`, and `set -e` to make decisions.

#### Variables and Quoting

```bash
# Assignment: no spaces around =
name="world"
count=42
readonly PI=3.14159    # Cannot be reassigned

# Usage: dollar sign
echo "Hello, $name"
echo "Count is ${count}"   # Braces for clarity/disambiguation

# Quoting: the most critical Bash concept for correctness
echo "Hello $name"        # Double quotes: variable expansion, glob protection
echo 'Hello $name'        # Single quotes: literal, no expansion
echo Hello $name          # No quotes: word splitting + globbing (DANGEROUS)
```

**Quoting rules**: Double-quote every variable expansion unless you specifically need word splitting or globbing. Unquoted variables are the single largest source of shell scripting bugs.

```bash
file="my document.txt"
cat $file              # WRONG: word splits into "my" and "document.txt"
cat "$file"            # RIGHT: passes "my document.txt" as one argument

files="*.txt"
echo $files            # Expands glob: file1.txt file2.txt file3.txt
echo "$files"          # Literal: *.txt
```

**Parameter expansion** provides powerful string manipulation without external commands:

```bash
# Default values
echo "${VAR:-default}"        # Use "default" if VAR is unset or empty
echo "${VAR:=default}"        # Assign "default" if VAR is unset or empty
echo "${VAR:?error message}"  # Exit with error if VAR is unset or empty
echo "${VAR:+alternate}"      # Use "alternate" if VAR IS set

# String operations
file="/home/user/documents/report.tar.gz"
echo "${file#*/}"             # home/user/documents/report.tar.gz  (remove shortest prefix match)
echo "${file##*/}"            # report.tar.gz  (remove longest prefix match -- basename)
echo "${file%.*}"             # /home/user/documents/report.tar  (remove shortest suffix)
echo "${file%%.*}"            # /home/user/documents/report  (remove longest suffix)
echo "${file/report/summary}" # /home/user/documents/summary.tar.gz  (substitution)
echo "${#file}"               # String length

# Substrings
str="Hello, World!"
echo "${str:7}"               # World!  (from position 7)
echo "${str:7:5}"             # World  (from position 7, length 5)

# Case modification (Bash 4+)
echo "${str^^}"               # HELLO, WORLD!  (uppercase)
echo "${str,,}"               # hello, world!  (lowercase)
echo "${str^}"                # Hello, World!  (capitalize first)
```

#### Arrays

```bash
# Indexed arrays
fruits=("apple" "banana" "cherry")
echo "${fruits[0]}"           # apple
echo "${fruits[@]}"           # All elements
echo "${#fruits[@]}"          # Number of elements: 3
fruits+=("date")              # Append

# Iterate safely
for fruit in "${fruits[@]}"; do
    echo "$fruit"
done

# Associative arrays (Bash 4+)
declare -A colors
colors[red]="#FF0000"
colors[green]="#00FF00"
colors[blue]="#0000FF"
echo "${colors[red]}"         # #FF0000
echo "${!colors[@]}"          # All keys: red green blue
echo "${colors[@]}"           # All values

for key in "${!colors[@]}"; do
    echo "$key -> ${colors[$key]}"
done
```

#### Arithmetic

```bash
# Arithmetic expansion
result=$((5 + 3))
echo $result                  # 8

x=10
((x++))                       # Increment
((x += 5))                    # Add 5
echo $((x * 2))               # Multiply

# Integer comparison in arithmetic context
if (( x > 10 )); then
    echo "x is greater than 10"
fi

# Note: Bash arithmetic is INTEGER ONLY. For floating point, use bc or awk:
echo "scale=2; 10 / 3" | bc   # 3.33
awk 'BEGIN {printf "%.2f\n", 10/3}'  # 3.33
```

#### Special Variables

| Variable | Meaning |
|----------|---------|
| `$0` | Script name |
| `$1` to `$9` | Positional arguments |
| `${10}` | Tenth argument (braces required for 10+) |
| `$@` | All arguments as separate words |
| `$*` | All arguments as a single word |
| `$#` | Number of arguments |
| `$$` | PID of current shell |
| `$!` | PID of last background process |
| `$?` | Exit code of last command |
| `$_` | Last argument of previous command |
| `$LINENO` | Current line number in script |
| `$FUNCNAME` | Current function name |
| `$BASH_SOURCE` | Source file of current script |

Always use `"$@"` (quoted) to pass arguments to other commands. `$*` and unquoted `$@` break on arguments containing spaces.

#### Conditionals

```bash
# if/elif/else
if [[ -f "$file" ]]; then
    echo "File exists"
elif [[ -d "$file" ]]; then
    echo "It is a directory"
else
    echo "Not found"
fi

# [[ ]] is the modern Bash conditional. Prefer it over [ ] (test).
# Differences from [ ]:
#   - No word splitting inside [[ ]]
#   - Pattern matching with ==
#   - Regex matching with =~
#   - Logical operators && and || instead of -a and -o
```

**String tests:**

```bash
[[ -z "$str" ]]         # True if string is empty
[[ -n "$str" ]]         # True if string is non-empty
[[ "$a" == "$b" ]]      # String equality
[[ "$a" != "$b" ]]      # String inequality
[[ "$a" < "$b" ]]       # String comparison (lexicographic)
[[ "$a" == pattern* ]]  # Glob pattern matching
[[ "$a" =~ ^[0-9]+$ ]]  # Regex matching
```

**Integer tests:**

```bash
[[ "$a" -eq "$b" ]]    # Equal
[[ "$a" -ne "$b" ]]    # Not equal
[[ "$a" -lt "$b" ]]    # Less than
[[ "$a" -gt "$b" ]]    # Greater than
[[ "$a" -le "$b" ]]    # Less than or equal
[[ "$a" -ge "$b" ]]    # Greater than or equal
# Or use (( )) for arithmetic comparison:
(( a > b ))
```

**File tests:**

```bash
[[ -e "$path" ]]        # Exists
[[ -f "$path" ]]        # Is a regular file
[[ -d "$path" ]]        # Is a directory
[[ -r "$path" ]]        # Is readable
[[ -w "$path" ]]        # Is writable
[[ -x "$path" ]]        # Is executable
[[ -s "$path" ]]        # Exists and is non-empty
[[ -L "$path" ]]        # Is a symbolic link
[[ "$a" -nt "$b" ]]     # a is newer than b
[[ "$a" -ot "$b" ]]     # a is older than b
```

**case statement:**

```bash
case "$1" in
    start)
        echo "Starting..."
        ;;
    stop)
        echo "Stopping..."
        ;;
    restart)
        echo "Restarting..."
        ;;
    status|info)            # Multiple patterns with |
        echo "Running"
        ;;
    *)                      # Default case
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
        ;;
esac
```

#### Loops

```bash
# for-in loop
for item in apple banana cherry; do
    echo "$item"
done

# Iterate over files (safe handling of spaces)
shopt -s nullglob                  # Glob expands to nothing if no matches
for file in /var/log/*.log; do
    echo "Processing: $file"
done

# C-style for loop
for ((i = 0; i < 10; i++)); do
    echo "$i"
done

# while loop
count=0
while [[ $count -lt 5 ]]; do
    echo "$count"
    ((count++))
done

# while read (process input line by line)
while IFS= read -r line; do
    echo "Line: $line"
done < "$input_file"

# Process command output line by line
ps aux | while IFS= read -r line; do
    echo "$line"
done
# Note: pipe creates a subshell -- variables modified inside the loop
# are lost after the loop. Use process substitution to avoid this:
while IFS= read -r line; do
    ((count++))
done < <(ps aux)
echo "$count"   # This works because no subshell was created

# until loop (runs until condition is true)
until ping -c1 google.com &>/dev/null; do
    echo "Waiting for network..."
    sleep 5
done
echo "Network is up"

# select (interactive menu)
select opt in "Option 1" "Option 2" "Quit"; do
    case "$opt" in
        "Option 1") echo "Selected 1" ;;
        "Option 2") echo "Selected 2" ;;
        "Quit") break ;;
        *) echo "Invalid option" ;;
    esac
done
```

**Safe file iteration**: always use `nullglob` when iterating over globs. Without it, if no files match, the literal glob string is passed as an argument. Always quote variables, even inside `for` loops.

#### I/O Redirection

```bash
# Output redirection
command > file          # Redirect stdout to file (overwrite)
command >> file         # Redirect stdout to file (append)
command 2> file         # Redirect stderr to file
command 2>> file        # Redirect stderr to file (append)
command &> file         # Redirect both stdout and stderr (Bash)
command > file 2>&1     # Same thing (POSIX-compatible)
command 2>&1 | less     # Pipe both stdout and stderr

# Input redirection
command < file          # Read stdin from file

# Here documents
cat <<EOF
This is a here document.
Variable expansion works: $HOME
Commands work: $(date)
EOF

cat <<'EOF'             # Single-quoted delimiter: no expansion
$HOME stays literal
$(date) stays literal
EOF

# Here strings
grep "pattern" <<< "$variable"   # Feed variable as stdin

# Discard output
command > /dev/null               # Discard stdout
command 2> /dev/null              # Discard stderr
command &> /dev/null              # Discard everything

# File descriptor management
exec 3> output.log               # Open FD 3 for writing
echo "Log message" >&3           # Write to FD 3
exec 3>&-                        # Close FD 3

exec 4< input.txt                # Open FD 4 for reading
read -r line <&4                 # Read from FD 4
exec 4<&-                        # Close FD 4
```

**Pipes and pipeline control:**

```bash
# Pipe connects stdout of left to stdin of right
command1 | command2 | command3

# PIPESTATUS: array of exit codes from the last pipeline
false | true | false
echo "${PIPESTATUS[@]}"          # 1 0 1

# pipefail: pipeline fails if ANY command fails (not just the last)
set -o pipefail
false | true
echo $?                          # 1 (without pipefail, this would be 0)

# Process substitution
diff <(sort file1) <(sort file2) # Compare sorted versions without temp files
while IFS= read -r line; do
    echo "$line"
done < <(find /var/log -name "*.log")

# Named pipes (FIFOs)
mkfifo /tmp/mypipe
command1 > /tmp/mypipe &         # Write to pipe in background
command2 < /tmp/mypipe           # Read from pipe
rm /tmp/mypipe

# tee: write to file and stdout simultaneously
command | tee output.log | next_command
command | tee >(process1) >(process2) > /dev/null  # Fan out to multiple processes
```

#### Functions

```bash
# Declaration
greet() {
    local name="${1:?Usage: greet <name>}"   # Required argument with error message
    local greeting="${2:-Hello}"              # Optional with default
    echo "$greeting, $name!"
}

# Call
greet "World"                    # Hello, World!
greet "World" "Hi"               # Hi, World!

# Return values: functions use exit codes (0-255) for status
# and stdout capture for data
get_timestamp() {
    date "+%Y-%m-%d %H:%M:%S"
}
ts=$(get_timestamp)              # Capture stdout into variable
echo "$ts"

# Local variables: always use 'local' inside functions
# Without 'local', variables are global and leak into the calling scope
process_file() {
    local filename="$1"
    local line_count
    line_count=$(wc -l < "$filename")
    echo "$line_count"
}

# Sourcing libraries
# utils.sh:
# log() { echo "[$(date '+%H:%M:%S')] $*" >&2; }
# die() { log "FATAL: $*"; exit 1; }
source ./utils.sh                # Import functions from another file
. ./utils.sh                     # POSIX equivalent
```

#### Error Handling: The Defensive Preamble

Every production Bash script should start with this:

```bash
#!/usr/bin/env bash
set -euo pipefail
```

What each flag does:

| Flag | Effect |
|------|--------|
| `set -e` | Exit immediately if a command fails (non-zero exit code) |
| `set -u` | Treat unset variables as an error |
| `set -o pipefail` | Pipeline fails if any command in it fails |

Together, these three flags catch the most common categories of shell scripting bugs: silently ignoring failures, using misspelled variable names, and pipelines that succeed despite upstream errors.

**trap** catches signals and executes cleanup code:

```bash
#!/usr/bin/env bash
set -euo pipefail

# Cleanup function
cleanup() {
    local exit_code=$?
    echo "Cleaning up..." >&2
    rm -f "$tmpfile"
    exit "$exit_code"
}

# Register trap: runs cleanup on EXIT (normal or error), INT (Ctrl-C), TERM (kill)
trap cleanup EXIT INT TERM

# Create temp file safely
tmpfile=$(mktemp)
echo "Working with $tmpfile"

# ERR trap: runs on any command failure (with set -e)
trap 'echo "Error on line $LINENO" >&2' ERR

# Your script logic here
do_work() {
    # If this fails, ERR trap fires, then EXIT trap fires
    some_command "$tmpfile"
}

do_work
```

The `trap ... EXIT` pattern guarantees cleanup runs regardless of how the script terminates -- normal exit, error, Ctrl-C, or TERM signal. Combined with `mktemp`, it prevents temp file leaks.

**Error messages go to stderr:**

```bash
# Good: errors to stderr, data to stdout
log() { echo "[$(date '+%H:%M:%S')] $*" >&2; }
die() { log "FATAL: $*"; exit 1; }

log "Starting process..."
result=$(compute_something) || die "Computation failed"
echo "$result"   # Only data goes to stdout
```

#### Debugging

```bash
# Trace execution: shows every command before it runs
set -x                           # Enable inside script
bash -x script.sh                # Enable from command line

# Custom trace prompt with file, function, and line number
export PS4='+${BASH_SOURCE}:${FUNCNAME[0]:-main}:${LINENO}: '
set -x

# Trace only a section
set -x
problematic_function
set +x                           # Disable tracing

# Verbose mode: prints each line before executing
set -v
```

Example debug output with custom PS4:

```
+script.sh:main:15: tmpfile=/tmp/tmp.abc123
+script.sh:process_data:22: grep -c error /var/log/syslog
+script.sh:process_data:23: echo 'Found 42 errors'
```

#### Scheduling

**cron** runs commands on a schedule:

```bash
# Edit your crontab
crontab -e

# View your crontab
crontab -l

# Cron syntax:
# MIN HOUR DOM MON DOW command
# ┬    ┬    ┬   ┬   ┬
# │    │    │   │   └── Day of week (0-7, 0 and 7 = Sunday)
# │    │    │   └────── Month (1-12)
# │    │    └────────── Day of month (1-31)
# │    └─────────────── Hour (0-23)
# └──────────────────── Minute (0-59)

# Examples:
# Every day at 3:00 AM
0 3 * * * /home/user/backup.sh

# Every 15 minutes
*/15 * * * * /home/user/check_health.sh

# Monday through Friday at 9:00 AM
0 9 * * 1-5 /home/user/morning_report.sh

# First day of every month at midnight
0 0 1 * * /home/user/monthly_cleanup.sh
```

**Cron pitfalls:**

1. **Environment**: Cron runs with a minimal environment. `PATH` is typically just `/usr/bin:/bin`. Always use full paths to commands or set PATH at the top of the crontab.
2. **Output**: Cron emails stdout and stderr to the user. Redirect to a log file: `command >> /var/log/myjob.log 2>&1`.
3. **Timezone**: Cron uses the system timezone. Be explicit. Check with `date`.
4. **No terminal**: Scripts that require a TTY will fail. No interactive prompts.

**systemd timers** are the modern alternative to cron:

```ini
# /etc/systemd/system/backup.timer
[Unit]
Description=Daily backup timer

[Timer]
OnCalendar=*-*-* 03:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

```ini
# /etc/systemd/system/backup.service
[Unit]
Description=Daily backup

[Service]
Type=oneshot
ExecStart=/home/user/backup.sh
User=user
```

```bash
sudo systemctl enable backup.timer
sudo systemctl start backup.timer
systemctl list-timers --all       # View all timers
```

Advantages of systemd timers over cron:

| Feature | cron | systemd timers |
|---------|------|---------------|
| Logging | Must redirect manually | Automatic journald integration |
| Missed runs | Lost | `Persistent=true` catches up |
| Dependencies | None | Can depend on other units |
| Resource control | None | cgroups, memory limits, CPU quotas |
| Calendar syntax | 5-field | Flexible (`OnCalendar`, `OnBootSec`) |
| Monitoring | `crontab -l` | `systemctl`, `journalctl` |

**at** runs a one-time command at a specified time:

```bash
at now + 5 minutes <<< "/home/user/script.sh"
at 3:00 PM <<< "echo 'Meeting reminder' | mail user@example.com"
atq                               # List pending jobs
atrm 5                            # Remove job 5
```

**flock** prevents concurrent execution:

```bash
#!/usr/bin/env bash
# Only one instance of this script can run at a time
exec 200>/var/lock/myscript.lock
flock -n 200 || { echo "Already running" >&2; exit 1; }

# Script logic here
do_expensive_work
```

The `-n` flag makes flock non-blocking: if the lock is held, exit immediately instead of waiting. This is critical for cron jobs that might overlap if a previous run takes longer than expected.

#### Practical Patterns

**getopts for argument parsing:**

```bash
#!/usr/bin/env bash
set -euo pipefail

usage() {
    cat <<EOF
Usage: $(basename "$0") [OPTIONS] <input_file>

Options:
    -o FILE    Output file (default: stdout)
    -v         Verbose mode
    -n NUM     Number of lines to process (default: all)
    -h         Show this help
EOF
    exit "${1:-0}"
}

output="/dev/stdout"
verbose=false
num_lines=0

while getopts ":o:vn:h" opt; do
    case "$opt" in
        o) output="$OPTARG" ;;
        v) verbose=true ;;
        n) num_lines="$OPTARG" ;;
        h) usage 0 ;;
        :) echo "Error: -$OPTARG requires an argument" >&2; usage 1 ;;
        ?) echo "Error: Unknown option -$OPTARG" >&2; usage 1 ;;
    esac
done
shift $((OPTIND - 1))

[[ $# -ge 1 ]] || { echo "Error: input file required" >&2; usage 1; }
input_file="$1"

$verbose && echo "Processing $input_file -> $output" >&2
```

**mktemp + trap cleanup:**

```bash
#!/usr/bin/env bash
set -euo pipefail

tmpdir=$(mktemp -d)
trap 'rm -rf "$tmpdir"' EXIT

# Use $tmpdir freely -- guaranteed cleanup on any exit
curl -sS "https://example.com/data.csv" > "$tmpdir/data.csv"
awk -F, '{print $1}' "$tmpdir/data.csv" > "$tmpdir/names.txt"
sort -u "$tmpdir/names.txt"
```

**jq for JSON processing:**

```bash
# Parse JSON from APIs
curl -s "https://api.github.com/repos/torvalds/linux" | jq '.stargazers_count'

# Extract nested fields
echo '{"user": {"name": "Alice", "age": 30}}' | jq '.user.name'   # "Alice"

# Array access
echo '[1, 2, 3]' | jq '.[0]'           # 1

# Filter and select
echo '[{"name":"a","status":"active"},{"name":"b","status":"inactive"}]' |
    jq '.[] | select(.status == "active") | .name'

# Transform
echo '[1, 2, 3, 4, 5]' | jq 'map(. * 2)'   # [2, 4, 6, 8, 10]

# CSV output
echo '[{"name":"a","val":1},{"name":"b","val":2}]' |
    jq -r '.[] | [.name, .val] | @csv'
```

**POSIX vs. Bash portability**: `[[ ]]`, arrays, `(( ))`, process substitution, and many parameter expansions are Bash-specific. If your script must run on `/bin/sh` (Alpine Linux, some containers), stick to `[ ]`, no arrays, and `expr` for arithmetic. In practice, Bash is available on nearly all Linux systems, so prefer Bash clarity over POSIX portability unless you have a specific constraint.

**shellcheck -- static analysis for shell scripts:**

```bash
# Install
sudo apt install shellcheck         # Debian/Ubuntu
brew install shellcheck             # macOS

# Run
shellcheck script.sh
shellcheck -x script.sh             # Follow 'source' directives

# Common findings:
# SC2086: Double-quote to prevent globbing and word splitting
# SC2046: Quote this to prevent word splitting
# SC2034: Variable appears unused
# SC2155: Declare and assign separately to avoid masking return values
```

Every script passes shellcheck before it is considered done. Integrate shellcheck into your editor and CI pipeline. It catches real bugs, not just style issues.

### Connection

Shell scripting is the bridge between interactive command-line use (learned in [Domain 3](/learn/first-principles/operating-systems-and-linux/)) and software engineering (explored in [Domain 5](/learn/first-principles/programming-fundamentals/) and [Domain 11](/learn/first-principles/software-engineering-and-collaboration/)). The `set -euo pipefail` defensive preamble is a form of defensive programming. Trap handlers are a form of resource management (the same concept appears as context managers in Python and RAII in C++). Signal handling connects back to the process model from [Domain 3](/learn/first-principles/operating-systems-and-linux/) -- when you trap SIGTERM, you are participating in the kernel's process lifecycle.

Cron and systemd timers are the simplest form of orchestration. The same pattern -- scheduled, automated, monitored execution -- scales up to CI/CD pipelines in [Domain 11](/learn/first-principles/software-engineering-and-collaboration/) and container orchestration in [Domain 12](/learn/first-principles/infrastructure-at-scale/). The principle is identical: remove humans from repeatable processes.

> **Try It**: Write a script that takes a directory as an argument, finds all `.log` files older than 7 days, compresses them with gzip, and reports how much space was saved. Use the defensive preamble, argument validation, temp files with trap cleanup, and pass shellcheck with zero warnings.

---

## Exercises

### Vim Exercises

1. Open a source code file in Vim. Without using insert mode, rearrange three functions into alphabetical order using only `dd`, `p`, and navigation commands.

2. Record a macro that takes a line like `firstName lastName email` and reformats it to `lastName, firstName <email>`. Apply the macro to 20 lines.

3. Using `:g` and `:s`, convert all single-quoted strings in a Python file to double-quoted strings, but only on lines that are not comments.

4. Open two files in a vertical split. Yank a function from the left file into a named register and paste it into the right file.

5. Write a `.vimrc` that sets up relative line numbers, 4-space indentation with spaces (not tabs), persistent undo, and a leader key mapping to strip trailing whitespace.

### Text Processing Exercises

6. Given an Apache/Nginx access log, write a pipeline that produces a report of: (a) total requests, (b) unique IP addresses, (c) top 10 requested URLs, (d) count of each HTTP status code.

7. Write a pipeline that finds all TODO and FIXME comments across a codebase, extracts the filename and line number, and formats the output as a markdown checklist.

8. Given a CSV file with headers, write an awk script that computes the mean, min, and max of a numeric column, taking the column name as a parameter.

9. Write a sed script that converts a markdown file's headers (`# Title`, `## Section`) to HTML (`<h1>Title</h1>`, `<h2>Section</h2>`).

10. Write a regex that validates email addresses according to a simplified specification (local part allows alphanumeric, dots, hyphens, underscores; domain allows alphanumeric and hyphens; TLD is 2-10 alphabetical characters). Test it with grep against a file containing valid and invalid addresses.

### Shell Scripting Exercises

11. Write a script called `sysinfo.sh` that outputs: hostname, OS version, CPU model, total/used/free memory, disk usage of `/`, uptime, and current logged-in users. Format the output as a clean table.

12. Write a deployment script that: (a) accepts `--env` (staging/production) and `--version` arguments with getopts, (b) validates arguments, (c) creates a timestamped backup, (d) simulates a deployment, (e) supports `--rollback` to restore the previous version. Use trap for cleanup.

13. Write a log rotation script that: (a) compresses log files older than N days (configurable), (b) deletes compressed logs older than M days, (c) uses flock to prevent concurrent execution, (d) logs its own actions to a separate log file.

14. Write a script that monitors a directory for new files using `inotifywait` (from inotify-tools) or a polling loop. When a new `.csv` file appears, validate its format, process it with awk, and move it to a `processed/` directory.

15. Write a script that queries a JSON API (use a public one like `https://jsonplaceholder.typicode.com/posts`), extracts specific fields with jq, filters by a condition, and outputs the results as a formatted table. Handle network errors gracefully.

---

## Assessment Dimensions

### Explain

You can explain why Vim uses modal editing and how the operator + text object grammar produces composable commands. You can describe the stages of a text processing pipeline and explain why each tool (grep, sed, awk) occupies a distinct niche. You can articulate what `set -euo pipefail` does and why each flag matters. You can explain the relationship between regular expressions and finite automata at an intuitive level. You can describe the tradeoffs between cron and systemd timers.

### Build

You can write a Bash script from scratch with proper error handling, argument parsing, signal trapping, and cleanup. You can construct multi-stage text processing pipelines that solve real data extraction and transformation problems. You can configure Vim with a `.vimrc` that reflects your workflow. You can set up cron jobs and systemd timers for scheduled automation. Every script you write passes shellcheck.

### Debug

Given a Bash script that silently fails, you can enable `set -x` with a custom PS4, trace the execution, identify where it fails, and fix it. Given a text processing pipeline that produces wrong output, you can isolate each stage by running them individually and identify the broken transformation. Given a cron job that does not run, you can check the crontab, verify paths, check the environment, inspect mail output, and diagnose the issue. You know the difference between a script that works on your machine and a script that works in cron's minimal environment.

---

## Key Takeaways

- Vim's modal model is a finite state machine: modes are states, keypresses are transitions, and the operator + text object grammar produces composable editing commands
- The dot command and macros turn single edits into repeatable batch operations
- Text processing pipelines follow extract, filter, transform, aggregate stages using small composable tools connected by pipes
- Regular expressions describe patterns that correspond to regular languages recognizable by finite automata -- the formal connection is in Domain 13
- grep searches, sed transforms line by line, awk processes fields -- know which tool fits which job
- Every Bash script starts with `set -euo pipefail` and uses `trap` for cleanup
- Quote every variable expansion; unquoted variables are the number one source of shell bugs
- mktemp + trap EXIT guarantees temp file cleanup regardless of how the script exits
- shellcheck is non-negotiable: every script passes before it is done
- cron handles simple schedules; systemd timers add logging, persistence, and resource control
- flock prevents concurrent execution of scripts that must not overlap
- When a script exceeds 200 lines or needs complex data structures, switch to Python

---

## Resources & Further Reading

- [Vim documentation](https://vimdoc.sourceforge.net/) -- The authoritative reference
- [Practical Vim by Drew Neil](https://pragprog.com/titles/dnvim2/practical-vim-second-edition/) -- The best book on thinking in Vim
- [Vimtutor](https://vimschool.netlify.app/) -- Built-in tutorial, run `vimtutor` in your terminal
- [Neovim documentation](https://neovim.io/doc/) -- Neovim-specific features and Lua API
- [GNU Bash Manual](https://www.gnu.org/software/bash/manual/) -- Complete Bash reference
- [Advanced Bash-Scripting Guide](https://tldp.org/LDP/abs/html/) -- Comprehensive examples
- [ShellCheck](https://www.shellcheck.net/) -- Online linter and documentation of every rule
- [GNU sed Manual](https://www.gnu.org/software/sed/manual/sed.html)
- [The AWK Programming Language](https://awk.dev/) -- By Aho, Weinberger, and Kernighan (the creators)
- [GNU grep Manual](https://www.gnu.org/software/grep/manual/)
- [Regular-Expressions.info](https://www.regular-expressions.info/) -- Comprehensive regex reference
- [jq Manual](https://jqlang.github.io/jq/manual/) -- Complete jq documentation
- [crontab.guru](https://crontab.guru/) -- Visual cron expression editor
- [systemd Timer documentation](https://www.freedesktop.org/software/systemd/man/systemd.timer.html)
- [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html) -- Industry shell scripting conventions
