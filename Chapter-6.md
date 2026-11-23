# Chapter 6 — sed and Stream Editing

## 1. What is sed?

- **`sed` (Stream Editor)** → Processes and transforms text line by line.
- Used for searching, replacing, deleting, and printing text.
- Works with files and pipelines.
- By default, prints the processed text without modifying the original file.

**Syntax:**

```bash
sed [options] 'command' filename
```

```bash
sed 's/hello/hi/' file.txt
```

Replaces the first occurrence of `hello` with `hi` on each line.

## 2. Essential sed Commands & Options

| Command / Option | Purpose |
|---|---|
| `s/old/new/` | Substitute first match per line |
| `s/old/new/g` | Replace all matches per line |
| `s/old/new/i` | Case-insensitive replacement (GNU sed) |
| `-n` | Suppress automatic printing |
| `p` | Print selected lines |
| `d` | Delete matching lines |
| `-i` | Edit file in place |
| `-E` | Use extended regular expressions |
| `q` | Quit processing |
| `a\` | Append text after a line |
| `i\` | Insert text before a line |

## 3. Substitution — `s`

**Syntax:**

```bash
sed 's/pattern/replacement/flags' file.txt
```

**Replace first occurrence on every line**

```bash
sed 's/error/warning/' app.log
```

**Replace all occurrences**

```bash
sed 's/error/warning/g' app.log
```

**Ignore case (GNU sed)**

```bash
sed 's/error/warning/gi' app.log
```

**Replace only the second occurrence per line**

```bash
sed 's/error/warning/2' app.log
```

**Remember:** `g` means global replacement within each processed line, not across the entire file as one string.

## 4. Printing Specific Lines

By default, `sed` prints every processed line.

Use `-n` with `p` to print only selected lines.

| Command | Purpose |
|---|---|
| `sed -n '5p' file` | Print line 5 |
| `sed -n '1,5p' file` | Print lines 1–5 |
| `sed -n '$p' file` | Print last line |
| `sed -n '/ERROR/p' file` | Print matching lines |
| `sed -n '2,10p' file` | Print lines 2–10 |

```bash
sed -n '1,5p' app.log
```

**Remember:** Without `-n`, using `p` also prints the line automatically, causing duplicate output for selected lines.

## 5. Deleting Lines — `d`

`d` removes lines from the output stream.

```bash
sed '3d' file.txt         # Delete line 3
sed '2,5d' file.txt       # Delete lines 2–5
sed '/DEBUG/d' app.log    # Remove DEBUG lines
sed '/^$/d' file.txt      # Remove empty lines
```

**Important:** These commands do not modify the original file unless you use `-i`.

## 6. Editing Files In Place — `-i`

By default, `sed` only displays modified output.

Use `-i` to save changes to the original file.

```bash
sed -i 's/old/new/g' file.txt
```

**Create a backup before modifying**

```bash
sed -i.bak 's/old/new/g' file.txt
```

This creates:
- `file.txt` → Modified file.
- `file.txt.bak` → Original backup.

**Remember:** `-i` changes the file. Always verify your pattern before using it.

## 7. Using Regex with sed

`sed` supports regular expressions.

| Pattern | Meaning |
|---|---|
| `^` | Start of line |
| `$` | End of line |
| `.` | Any single character |
| `*` | Zero or more |
| `[0-9]` | Digit |
| `[^0-9]` | Non-digit |
| `[[:space:]]` | Whitespace |
| `()` | Grouping with `-E` |
| `+` | One or more with `-E` |

**Replace numbers**

```bash
sed -E 's/[0-9]+/NUM/g' file.txt
```

**Remove leading whitespace**

```bash
sed 's/^[[:space:]]*//' file.txt
```

**Remove trailing whitespace**

```bash
sed 's/[[:space:]]*$//' file.txt
```

**Replace lines starting with ERROR**

```bash
sed 's/^ERROR/WARNING/' app.log
```

## 8. Addressing — Selecting Lines

You can specify which lines `sed` should process.

**Replace only on line 3**

```bash
sed '3s/old/new/' file.txt
```

**Replace within lines 2–5**

```bash
sed '2,5s/old/new/g' file.txt
```

**Replace only on lines containing ERROR**

```bash
sed '/ERROR/s/failed/retry/' app.log
```

**Remember:** An address before a command limits where that command applies.

## 9. Using Different Delimiters

The `/` character is the default substitution delimiter.

For paths containing `/`, using another delimiter is easier.

```bash
sed 's|/home/old|/home/new|g' file.txt
```

Instead of:

```bash
sed 's/\/home\/old/\/home\/new/g' file.txt
```

**Tip:** `|`, `#`, or `@` can be convenient alternative delimiters.

## 10. Capture Groups & Backreferences

Capture groups allow matched text to be reused.

With extended regex (`-E`):

```bash
echo "John Smith" | sed -E 's/(John) (Smith)/\2 \1/'
```

Output:

```text
Smith John
```

- `()` → Capture a group.
- `\1` → First captured group.
- `\2` → Second captured group.

**Practical example — Reorder name and ID**

```bash
echo "Alice:123" | sed -E 's/([^:]+):([0-9]+)/\2:\1/'
```

Output:

```text
123:Alice
```

## 11. sed with Pipelines

**Replace text from another command**

```bash
cat app.log | sed 's/ERROR/WARNING/g'
```

**Filter logs and replace text**

```bash
grep 'ERROR' app.log | sed 's/ERROR/ISSUE/g'
```

**Remove blank lines and sort**

```bash
sed '/^[[:space:]]*$/d' file.txt | sort
```

**Extract lines and count them**

```bash
sed -n '/ERROR/p' app.log | wc -l
```

**Remove comments and empty lines**

```bash
sed '/^[[:space:]]*#/d; /^[[:space:]]*$/d' config.txt
```

## 12. Practical Workplace Examples

**Replace an API endpoint in a configuration file**

```bash
sed -i.bak 's|http://localhost:3000|https://api.example.com|g' config.txt
```

**Remove DEBUG logs**

```bash
sed '/DEBUG/d' app.log
```

**Print logs between lines 10 and 20**

```bash
sed -n '10,20p' app.log
```

**Mask numeric IDs**

```bash
echo "user_id=12345" | sed -E 's/[0-9]+/XXXXX/g'
```

**Update a configuration key**

```bash
sed -E 's/^PORT=.*/PORT=8080/' .env
```

Displays the updated text without modifying `.env`.

## 13. grep vs sed vs awk

| Command | Main Purpose |
|---|---|
| `grep` | Search and filter matching lines |
| `sed` | Modify and transform text streams |
| `awk` | Process fields, columns, and structured text |

```bash
grep 'ERROR' app.log
sed 's/ERROR/WARNING/g' app.log
awk '{print $1}' app.log
```

**Remember:**
- `grep` → Find.
- `sed` → Edit.
- `awk` → Extract and process fields.

## 14. Common Mistakes

- `sed` does not modify files by default.
- `-i` modifies files directly; use backups when needed.
- `s/old/new/` changes only the first match per line.
- `s/old/new/g` replaces all matches per line.
- `p` without `-n` can produce duplicate output.
- `d` removes lines from output, not the original file by default.
- Regex metacharacters must be escaped when matching them literally.
- GNU and BSD/macOS `sed` have some syntax differences, especially with `-i` and case-insensitive flags.

## 15. Quick Recall

- `sed` → Stream editor.
- `s` → Substitute.
- `g` → Replace all occurrences per line.
- `i` → Ignore case (GNU sed substitution flag).
- `p` → Print.
- `d` → Delete.
- `-n` → Suppress automatic printing.
- `-i` → Edit file in place.
- `-E` → Extended regex.
- `1,5` → Line range.
- `/pattern/` → Match lines.
- `^` → Start of line.
- `$` → End of line.
- `\1`, `\2` → Captured groups.
- `-i.bak` → Edit with backup.

**Core takeaway:** `sed` is a powerful tool for editing text streams. Master substitution (`s`), global replacement (`g`), printing (`-n ... p`), deleting (`d`), and safe in-place editing (`-i.bak`). These cover most everyday uses of `sed`.