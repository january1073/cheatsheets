# bat, fzf, & eza Cheatsheet

## bat
* [Resources](https://github.com/sharkdp/bat)

### Installation
* `sudo apt install bat`
* `nano .bashrc` → "Terminal Configuration", below
* `source ~/.bashrc`

### Run: `cat <file>`

## fzf
* [Resources](https://github.com/junegunn/fzf)

### Installation
* `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf`
* `~/.fzf/install` → `y`, `y`, `y`
* `nano .bashrc` → "Terminal Configuration", below
* `source ~/.bashrc`

### Run: `ff`

## eza
* [Resources](https://github.com/eza-community/eza)

### Installation
* Install [Nerd Font](https://www.nerdfonts.com/)
* `sudo apt install eza`
* `nano .bashrc` → "Terminal Configuration", below`

### Commands

| Command | Comment |
|---------|---------|
| `ls` | `ls` |
| `l` | `ls`, hiding .gitignored files (useful inside Git repos) |
| `ll` | Long listing of all files including hidden ones, with headers |
| `llm` | ll, sorted by modification time |
| `la` | ll, human-readable sizes, no header |
| `lx` | la plus extended attributes (ACLs, xattrs) |
| `tree` | `tree` with icons and group info |
| `lt` | ´tree´, limited tree depth to 2 levels |

### Themes

* [Hacker theme](https://github.com/january1073/eza_hacker_theme/blob/main/README.md)
* [Official themes repo](https://github.com/eza-community/eza-themes)

## Terminal Configuration (`~/.bashrc`)
```bash
# eza aliases (including color flags)
alias ls='eza --group-directories-first --icons'
alias l='eza --group-directories-first --git-ignore --icons'
alias ll='eza --group-directories-first -al --header --group --icons'
alias llm='eza --group-directories-first -al --header --group --icons --sort=modified'
alias la='eza --group-directories-first -alh --group --icons'
alias lx='eza --group-directories-first -alh@ --group --icons'
alias lt='eza --group-directories-first --tree --level=2 --group --icons'
alias tree='eza --group-directories-first --tree --group --icons'

# bat alias
alias cat='batcat'

# fzf integration
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
alias ff="fzf --style full --preview 'fzf-preview.sh {}' --bind 'focus:transform-header:file --brief {}'"

```

Reach out: https://guns.lol/january1073
