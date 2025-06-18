# Vim Cheatsheet

## Installation
* Debian-based: `sudo apt install vim`
* Fedora-based: `sudo dnf install vim`
* Arch-based: `sudo pacman -S vim`

## Basics

### Vim modes (displayed at the bottom left)
* `[Esc]` → **Normal Mode** (copy, paste, cut, find/replce, etc.); `:` → **Command Mode** (update configs, save, quit, open new files, tabs, buffers, etc.)
  
* **Insert Mode** (select text)
  
  | Key | Switch to Insert Mode and ... |
  |-----|-------------|
  | `i` | edit text right before the current cursor position |
  | `I` | place the cursor at the beginning of the line |
  | `a` | edit text right after the current cursor position |
  | `A` | place the cursor at the end of the line |
  | `o` | add a new line below the current one |
  | `O` | add a new line above the current one |
  
* **Visual Mode** (edit text)
  
  | Key | Switch to Visual Mode and ... |
  |-----|-------------|
  | `v` | start from the character under the cursor |
  | `V` | select entire lines, starting with the current line |
  | `[Ctrl]` + `[v]` | select vertical blocks |

### Navigation
| `h` | `j` | `k` | `l` |
|---|---|---|---|
| ← | ↓ | ↑ | → |

#### Screen or File Navigation
| Key   | Description                                  |
|-------|----------------------------------------------|
| `zt`  | Move the screen to the top of the window     |
| `zz`  | Move the screen to the middle of the window  |
| `zb`  | Move the screen to the bottom of the window  |
| `H`   | Move the cursor to the highest line on screen|
| `M`   | Move the cursor to the middle line on screen |
| `L`   | Move the cursor to the lowest line on screen |
| `gg`  | Move the cursor to the beginning of the file |
| `G`   | Move the cursor to the end of the file       |
| ``  | Return to the previous place                   |
| `"`   | Return to previous line                      |

#### Words or Content Navigation
| Key   | Description                                         |
|-------|-----------------------------------------------------|
| `w`   | Move to beginning of next word                      |
| `e`   | Move to end of current word                         |
| `b`   | Move to beginning of previous word                  |
| `B`   | [...]                                               |
| `^`   | Move to first non-blank character of the line       |
| `0`   | Move to beginning of the line                       |
| `$`   | Move to end of the line                             |
| `ge`  | Move to end of previous word                        |
| `fc`  | Jump to next occurrence of character `c`            |
| `Fc`  | Jump to previous occurrence of character `c`        |
| `tc`  | Jump just before next occurrence of character `c`   |
| `Tc`  | Jump just after previous occurrence of character `c`|
| `;`   | Repeat last character search forward                |
| `,`   | Repeat last character search backward               |
| `*`   | Jump to next occurrence of word under cursor        |
| `#`   | Jump to previous occurrence of word under cursor    |

### Write (Save) & Quit
| Command | Description                                                      |
|---------|------------------------------------------------------------------|
| `:wq`   | Write and quit                                            |
| `:x`    | Write if changed, then quit                                      |
| `ZZ`    | Write if changed, then quit; shortcut in Normal mode (`Shift+ZZ`)|
| `:q!`   | Quit without saving                                              |

#### Yank (Copy)
| Key     | Description                          |
|---------|--------------------------------------|
| `y␣`    | Yank char under the cursor           |
| `yy`    | Yank the line under the cursor       |
| `2yy`   | Yank the 2 lines under the cursor    |
| `yw`    | Yank word                            |
| `y$`    | Yank to end of line                  |

#### Paste
| Key   | Description              |
|-------|--------------------------|
| `p`   | Paste after the cursor   |
| `P`   | Paste before the cursor  |

#### Delete (Cut)
| Key   | Description                        |
|-------|------------------------------------|
| `dd`  | Delete the line under the cursor   |
| `dw`  | Delete the word                    |
| `x`   | Delete current char                |
| `X`   | Delete previous char               |
| `xp`  | Transpose two letters              |
| `D`   | Delete to end of line              |

#### Change (Cut & Enter Insert Mode)
| Key   | Description                          |
|-------|--------------------------------------|
| `s`   | Delete char under the cursor         |
| `S`   | Delete line under the cursor         |

## Save & Exit
| Command | Description                     | When to Use                                |
|---------|---------------------------------|--------------------------------------------|
| `:wq`   | Write (save) and quit           | Save changes and exit Vim                  |
| `:x`    | Write if changed, then quit     | Same as `:wq`, but only saves if modified  |
| `ZZ`    | Write if changed, then quit     | Shortcut in Normal mode (`Shift+ZZ`)       |
| `:q!`   | Quit without saving             | Exit and discard changes                   |

## Customize (tbd.)
`.vimrc`: configuration details

## Advanced (tbd.)
* Fold
* Marks
* Search
* Regex Review
* Substitute Text
* Buffers
* Macros
* Run External Commands
* Check Spelling

## Extend Vim
* [vim-go-ide](https://github.com/rgerardi/vim-go-ide)
* [VimAwesome](https://vimawesome.com/)

## Help
* `h`: basic help
* `h <topic>`: help on a topic
* `h help`: help on help (~25-30 min.)
* [Official website](http://www.vim.org/)

## Learn
* `vimtutor`: interactive intro for the most common commands
* [vim-tutor-sequel](https://github.com/micahkepe/vimtutor-sequel)

Reach out: https://linktr.ee/january1073
