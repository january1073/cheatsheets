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
  | `i` | Edit text **right before** the current cursor position |
  | `I` | Place the cursor at the **beginning of the line** |
  | `a` | Edit text **right after** the current cursor position |
  | `A` | Place the cursor at the **end of the line** |
  | `o` | **Add a new line below** the current one |
  | `O` | **Add a new line above** the current one |
* **Visual Mode** (edit text)

## Help
* `h`: basic help
* `h <topic>`: help on a topic
* `h help`: help on help (~25-30 min.)
* `vimtutor`: interactive intro for the most common commands
* [Official website](http://www.vim.org/)
* [Other useful links](https://vimschool.netlify.app/links/)
