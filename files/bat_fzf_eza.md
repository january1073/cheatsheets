# FZF & EZA Cheatsheet

## Bat

### Installation
* `sudo apt install bat`
* `nano .bashrc` → "Terminal Configuration", below
* `source ~/.bashrc`

### Run: `cat <file>`

## FZF

### Installation
* `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf`
* `~/.fzf/install` → `y`, `y`, `y`
* `nano .bashrc` → "Terminal Configuration", below
* `source ~/.bashrc`

### Run: `ff`

## EZA

### Installation
* Install [Nerd Font](https://www.nerdfonts.com/)
* `sudo apt install eza -y`
* `nano .bashrc` → "Terminal Configuration", below`

| Command | Comment |
|---------|---------|
| `ls` | Basic file listing (like plain ls) |
| `l` | Hide .gitignored files (useful inside Git repos) |
| `ll` | Long listing of all files including hidden ones, with headers |
| `llm` | Like ll, but sorted by last modified time |
| `la` | Long listing of all files, human-readable sizes, no header |
| `lx` | Like la, but also show extended attributes (ACLs, xattrs) |
| `lt` | Tree view with icons and group info |
| `tree` | Same as lt; replaces the traditional tree command |

## Terminal Configuration (`~/.bashrc`)
```bash
# Bat
alias cat='batcat'

# FZF
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
alias ff="fzf --style full --preview 'fzf-preview.sh {}' --bind 'focus:transform-header:file --brief {}'"

# EZA
alias ls='eza --group-directories-first $eza_params'
alias l='eza --git-ignore --group-directories-first $eza_params'
alias ll='eza -al --header --group --icons --group-directories-first $eza_params'
alias llm='eza -al --header --group --icons --sort=modified --group-directories-first $eza_params'
alias la='eza -alh --group --icons --group-directories-first $eza_params'
alias lx='eza -alh@ --group --icons --group-directories-first $eza_params'
alias lt='eza --tree --group --icons --group-directories-first $eza_params'
alias tree='eza --tree --group --icons --group-directories-first $eza_params'
```

Reach out: https://linktr.ee/january1073
