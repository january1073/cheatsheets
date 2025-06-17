# Vim Cheatsheet

## Installation
* Debian-based: `sudo apt install vim`
* Fedora-based: `sudo dnf install vim`
* Arch-based: `sudo pacman -S vim`

## Vim modes (displayed at the bottom left)
* `[Esc]` → **Normal Mode** (copy, paste, cut, find/replce, etc.); `:` → **Command Mode** (update configs, save, quit, open new files, tabs, buffers, etc.)
* **Insert Mode** (select text)
  
  | Key | Switch to Insert Mode and ... |
  |-----|-------------|
  | `i` | edit text **right before** the current cursor position |
  | `I` | place the cursor at the **beginning of the line** |
  | `a` | edit text **right after** the current cursor position |
  | `A` | place the cursor at the **end of the line** |
  | `o` | **add a new line below** the current one |
  | `O` | **add a new line above** the current one |
  
* **Visual Mode** (edit text)
  
  | Key | Switch to Visual Mode and ... |
  |-----|-------------|
  | `v` | start from the **character under the cursor** |
  | `V` | select **entire lines**, starting with the current line |
  | `[Ctrl]` + `[v]` | select **vertical blocks** |

## Help
* `h`: basic help
* `h <topic>`: help on a topic
* `h help`: help on help (~25-30 min.)
* `vimtutor`: interactive intro for the most common commands
* [Official website](http://www.vim.org/)
* [Other useful links](https://vimschool.netlify.app/links/)
