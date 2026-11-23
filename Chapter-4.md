# Chapter 4 — Regular Expression Essentials

## 1. What is Regex?

- **Regex (Regular Expression)** → Pattern used to search, match, or manipulate text.
- Used with commands like `grep`, `sed`, and `awk`.
- Useful for searching logs, validating patterns, and filtering text.

```bash
grep -E 'error|warning' app.log
```

Matches lines containing `error` or `warning`.

**Remember:** Regex matches text patterns, not filesystem paths.

## 2. Basic Regex Symbols

| Symbol | Meaning | Example |
|---|---|---|
| `.` | Any single character (except newline normally) | `c.t` → cat, cot |
| `*` | Zero or more of previous element | `ab*c` |
| `+` | One or more of previous element | `ab+c` |
| `?` | Zero or one of previous element | `colou?r` |
| `^` | Start of line | `^ERROR` |
| `$` | End of line | `done$` |
| `\|` | OR | `cat\|dog` |
| `[]` | Character set | `[abc]` |
| `[^]` | Negated character set | `[^0-9]` |
| `()` | Group expressions | `(ab)+` |
| `\` | Escape special characters | `\.` |

Examples use **Extended Regex (`grep -E`)**, unless stated otherwise.

## 3. Character Classes

| Pattern | Matches |
|---|---|
| `[abc]` | a, b, or c |
| `[a-z]` | Lowercase ASCII letters |
| `[A-Z]` | Uppercase ASCII letters |
| `[0-9]` | Digits |
| `[a-zA-Z]` | Letters |
| `[^0-9]` | Any non-digit |
| `[[:digit:]]` | Digits |
| `[[:alpha:]]` | Alphabetic characters |
| `[[:space:]]` | Whitespace characters |
| `[[:alnum:]]` | Letters or digits |

```bash
grep -E '[0-9]' file.txt
grep -E '^[A-Z]' file.txt
```

**Remember:** `^` inside `[^...]` means NOT; outside brackets it means start of line.

## 4. Quantifiers (Repetition)

| Quantifier | Meaning |
|---|---|
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n |
| `{n,}` | At least n |
| `{n,m}` | Between n and m |

```text
a*       → "", a, aa, aaa
a+       → a, aa, aaa
a?       → "" or a
[0-9]{3} → Exactly 3 consecutive digits
```

Example:

```bash
grep -E '[0-9]{3}' file.txt
```

Find lines containing at least three consecutive digits.

## 5. Anchors

Anchors specify where a match must occur.

| Pattern | Meaning |
|---|---|
| `^hello` | Line begins with hello |
| `world$` | Line ends with world |
| `^$` | Empty line |
| `^ERROR$` | Entire line is ERROR |

```bash
grep -E '^ERROR' app.log
grep -E 'failed$' app.log
grep -E '^$' file.txt
```

**Remember:** `^` and `$` match positions, not characters.

## 6. Groups & Alternation

**Alternation (`|`)** → Match either pattern.

```bash
grep -E 'error|warning' app.log
```

**Grouping (`()`)** → Treat multiple characters as one unit.

```bash
grep -E '(ab)+' file.txt
```

Matches `ab`, `abab`, `ababab`, etc.

**Capture groups** → Remember matched portions for reuse in tools that support them.

Example using `sed`:

```bash
echo