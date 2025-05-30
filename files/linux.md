# Linux Terminal Cheatsheet

## Auto-completion

| Shortcut | Action                                  |
| -------- | --------------------------------------- |
| `TAB`    | Auto-complete files, commands, or paths |

## Cursor Movement

| Shortcut       | Action                     |
| -------------- | -------------------------- |
| `Ctrl + A`     | Move to start of line      |
| `Ctrl + E`     | Move to end of line        |
| `Ctrl + ← / →` | Move to previous/next word |
| `Alt + B`      | Move backward one word     |
| `Alt + F`      | Move forward one word      |

## Text Editing

| Shortcut   | Action                              |
| ---------- | ----------------------------------- |
| `Ctrl + U` | Delete from cursor to start of line |
| `Ctrl + K` | Delete from cursor to end of line   |
| `Ctrl + W` | Delete word before cursor           |
| `Ctrl + Y` | Paste last deleted text             |

## Process Control

| Shortcut   | Action                                                 |
| ---------- | ------------------------------------------------------ |
| `Ctrl + C` | Kill the running process                               |
| `Ctrl + D` | End-of-file (EOF), closes terminal if shell is idle    |
| `Ctrl + Z` | Suspend the running process (can be resumed with `fg`) |

## Terminal Control

| Shortcut       | Action                           |
| -------------- | -------------------------------- |
| `Ctrl + L`     | Clear the terminal screen        |
| `Ctrl + + / -` | Zoom in / Zoom out terminal font |

## Command History

| Shortcut   | Action                          |
| ---------- | ------------------------------- |
| `Ctrl + R` | Search through command history  |
| `↑ / ↓`    | Navigate previous/next commands |

## Application Switching

| Shortcut    | Action                           |
| ----------- | -------------------------------- |
| `Alt + Tab` | Switch between open applications |

## Troubleshooting

| Action                            | Shortcut / Command                                        |
| --------------------------------- | --------------------------------------------------------- |
| Switch to terminal mode           | `Ctrl + Alt + F3`                                         |
| Run command window                | `Alt + F2`                                                |
| Kill frozen process (interactive) | `htop` (navigate and kill via UI)                         |
| Restart Gnome display manager     | `sudo systemctl restart gdm`                              |
| Magic SysRq Key (REISUB reboot)   | `Alt + SysRq` then `R` `E` `I` `S` `U` `B` (sequentially) |

> SysRq key is often labeled as "Print Screen"

Reach out: https://linktr.ee/january1073
