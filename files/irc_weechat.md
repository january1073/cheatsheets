/set irc.server.libera.autojoin "#cybersecurity,#security,#linux,#python,#c,#ubuntu,#debian,#archlinux,#libera,#weechat"


# IRC & Weechat Cheatsheet

This cheatsheet covers basic to intermediate IRC and Weechat usage, ideal for new users focused on security channels.

---

## Setup Commands

| Command | Description |
|--------|-------------|
| `/server add <name> <host>/<port> [-ssl] [-autoconnect]` | Add a new IRC server |
| `/connect <server>` | Connect to a saved server |
| `/disconnect` | Disconnect from current server |
| `/quit [message]` | Quit Weechat or current IRC session |
| `/save` | Save current Weechat config |
| `/buffer` | Show or switch to a specific buffer |
| `/buffer <# or name>` | Jump to buffer by number or name |
| `/nick <newnick>` | Change your nickname |
| `/msg NickServ REGISTER <pass> <email>` | Register your nickname |
| `/msg NickServ IDENTIFY <nick> <pass>` | Authenticate registered nick |
| `/join <#channel>` | Join a channel |
| `/leave` or `/part` | Leave the current channel |
| `/me <action>` | Send an action message (`/me waves`) |

---

## Buffer Navigation

| Key/Command | Description |
|------------|-------------|
| `Alt + ← / →` | Move between buffers |
| `Ctrl + n / Ctrl + p` | Next / previous buffer |
| `/buffers` | List all open buffers |
| `/buffer <number>` | Switch to buffer by number |
| `/close` | Close the current buffer |

---

## Common IRC Commands

| Command | Description |
|---------|-------------|
| `/whois <nick>` | Info on user |
| `/names <#channel>` | List users in channel |
| `/topic` | Show current topic |
| `/topic <new topic>` | Set a new topic (op only) |
| `/kick <nick> [reason]` | Kick a user (op only) |
| `/ban <nick>` | Ban a user (op only) |
| `/msg <nick> <message>` | Send a private message |

---

## Logging & History

| Command | Description |
|---------|-------------|
| `/set logger.file.auto_log 1` | Enable logging for all buffers |
| `/set logger.file.mask "%Y-%m-%d_%H-%M-${info:buffer}"` | Customize log filenames |
| `~/.weechat/logs/` | Location of log files |
| `less ~/.weechat/logs/<file>` | View logs in terminal |
| `/set irc.server.<name>.sasl_username "<nick>"` | Enable SASL login (username) |
| `/set irc.server.<name>.sasl_password "<pass>"` | SASL password |
| `/set irc.server.<name>.sasl_mechanism plain` | SASL mechanism (PLAIN) |

---

## Persistent Settings

| Setting | Example |
|---------|---------|
| Auto-join channels | `/set irc.server.libera.autojoin "##security,#nmap"` |
| Nick list | `/set irc.server.libera.nicks "YourNick,YourNick_,YourNick__"` |
| Real name | `/set irc.server.libera.realname "John Doe"` |
| Auto-connect | `/set irc.server.libera.autoconnect on` |

---

## Useful Scripts

| Command | Description |
|---------|-------------|
| `/script list` | List installed scripts |
| `/script search <term>` | Search for scripts |
| `/script install buffers.pl` | Install buffer list sidebar |
| `/buffers` | Show/hide sidebar (after installing `buffers.pl`) |

---

## Tips

- Don’t ask “can I ask a question?” — just ask it.
- Lurk and observe before posting.
- Use `/help <command>` for inline help.

---

## Helpful Channels on Libera.Chat

| Channel | Topic |
|---------|-------|
| `##security` | General infosec discussion |
| `#nmap` | Network scanning with Nmap |
| `##linux` | Linux users & help |
| `#weechat` | Help with Weechat itself |
| `#libera` | Network support |

---

