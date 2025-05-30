# Regex Cheatsheet

## Character Classes

| Pattern       | Description                           |
| ------------- | ------------------------------------- |
| `.`           | Any character except newline          |
| `[abc]`       | Any one of `a`, `b`, or `c`           |
| `[^abc]`      | Any character except `a`, `b`, or `c` |
| `[a-z]`       | Any lowercase letter                  |
| `[A-Z]`       | Any uppercase letter                  |
| `[0-9]`       | Any digit                             |
| `[a-zA-Z0-9]` | Any alphanumeric character            |
| `\d`          | Digit, same as `[0-9]`                |
| `\D`          | Non-digit                             |
| `\w`          | Word character: `[a-zA-Z0-9_]`        |
| `\W`          | Non-word character                    |
| `\s`          | Whitespace: space, tab, newline, etc. |
| `\S`          | Non-whitespace character              |

## Anchors

| Pattern | Description             |
| ------- | ----------------------- |
| `^`     | Start of string or line |
| `$`     | End of string or line   |
| `\b`    | Word boundary           |
| `\B`    | Not a word boundary     |

## Quantifiers

| Pattern        | Description                     |
| -------------- | ------------------------------- |
| `a*`           | 0 or more `a`                   |
| `a+`           | 1 or more `a`                   |
| `a?`           | 0 or 1 `a`                      |
| `a{n}`         | Exactly `n` occurrences of `a`  |
| `a{n,}`        | At least `n` occurrences        |
| `a{n,m}`       | Between `n` and `m` occurrences |
| `*?` `+?` etc. | Lazy (non-greedy) versions      |

## Groups and Alternation

| Pattern         | Description                          |                                      |
| --------------- | ------------------------------------ | ------------------------------------ |
| `(abc)`         | Group expression                     |                                      |
| `(?:abc)`       | Non-capturing group                  |                                      |
| `(?P<name>...)` | Named capturing group (Python-style) |                                      |
| \`a             | b\`                                  | Matches `a` or `b`                   |
| \`(?            | ...)\`                               | Branch reset group (in some engines) |

## Lookaround

| Pattern    | Description         |
| ---------- | ------------------- |
| `(?=...)`  | Positive lookahead  |
| `(?!...)`  | Negative lookahead  |
| `(?<=...)` | Positive lookbehind |
| `(?<!...)` | Negative lookbehind |

## Escaped Characters

| Pattern | Description               |
| ------- | ------------------------- |
| `\.`    | Matches literal `.`       |
| `\\`    | Matches literal backslash |
| `\*`    | Matches literal `*`, etc. |
| `\t`    | Tab                       |
| `\n`    | Newline                   |
| `\r`    | Carriage return           |

## Flags (Modifiers)

| Flag | Description                                |
| ---- | ------------------------------------------ |
| `i`  | Ignore case                                |
| `m`  | Multiline mode (`^` and `$` match lines)   |
| `s`  | Dot matches newline                        |
| `x`  | Verbose mode (allows comments and spacing) |
| `g`  | Global search (varies by language/tool)    |

* In Python: `re.compile(r"pattern", re.IGNORECASE | re.MULTILINE)`
* In JavaScript: `/pattern/gim`

## Common Use Cases

| Task                    | Regex Example                 | Notes                          |
| ----------------------- | ----------------------------- | ------------------------------ |
| Match email             | `\b[\w.-]+@[\w.-]+\.\w{2,}\b` | Simplified version             |
| Match IPv4              | `\b\d{1,3}(\.\d{1,3}){3}\b`   | No range check                 |
| Match date (YYYY-MM-DD) | `\b\d{4}-\d{2}-\d{2}\b`       | Assumes valid formatting       |
| Match hex color         | `#?[0-9A-Fa-f]{6}`            | Optional `#`, case-insensitive |
| Match URL               | `https?://[^\s/$.?#].[^\s]*`  | Basic version                  |
| Match HTML tags         | `<[^>]+>`                     | Does not handle nested tags    |

## Tips

* Always test your regex on actual sample data.
* Use tools like [regex101.com](https://regex101.com) or `grep -P`, `sed`, `awk`, or language-native regex engines.
* Be careful with greedy quantifiers (`*`, `+`) when parsing nested or complex data.

Reach out: https://linktr.ee/january1073
