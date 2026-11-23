# Chapter 7 — Advanced Text Manipulation with sed

## 1. Recap — What is sed?

- `sed` → Stream editor used to transform text.
- Processes text line by line.
- Supports substitutions, filtering, deleting and extracting text.
- Does not modify original files unless `-i` is used.

```bash
sed 's/old/new/g' file.txt
```

**Remember:** `s` = substitute, `g` = all occurrences per line.

## 2. Structure of a sed Expression

```bash
sed 'address command' file.txt
```

| Component | Purpose |
|---|---|
| Address | Select which lines to process |
| Command | Operation to perform (`s`, `d`, `p`, etc.) |
| Pattern | Regex used to match text |
| Flags | Modify behavior (`g`, `i`, `p`) |

```bash
sed '/ERROR/s/failed/retry/g' app.log
```

Replaces `failed` with `retry` only on lines containing `ERROR`.

## 3. Applying Multiple Commands

Use `-e` or `;` to execute multiple transformations.

```bash
sed -e 's/error/ERROR/g' -e 's/warn/WARNING/g' app.log
```

Equivalent:

```bash
sed 's/error/ERROR/g; s/warn/WARNING/g' app.log
```

**Remember:** `-e` allows multiple editing expressions.

## 4. Selecting Lines with Addresses

| Expression | Meaning |
|---|---|
| `3p` | Print line 3 |
| `2,5p` | Print lines 2–5 |
| `/ERROR/p` | Print matching lines |
| `/START/,/END/p` | Print range between matching lines |
| `$p` | Print last line |

```bash
sed -n '/START/,/END/p' app.log
```

Prints from a line matching `START` through the next line matching `END`, inclusive, repeating for later matches.

## 5. Deleting Comments and Empty Lines

**Remove comment lines beginning with `#`**

```bash
sed '/^[[:space:]]*#/d' config.txt
```

**Remove empty lines**

```bash
sed '/^$/d' file.txt
```

**Remove whitespace-only lines**

```bash
sed '/^[[:space:]]*$/d' file.txt
```

**Remove both comments and blank lines**

```bash
sed -e '/^[[:space:]]*#/d' -e '/^[[:space:]]*$/d' config.txt
```

Useful when cleaning configuration files.

## 6. Capture Groups and Backreferences

Capture groups allow reusing matched text.

| Syntax | Meaning |
|---|---|
| `(...)` | Capture group with `-E` |
| `\1` | First captured group |
| `\2` | Second captured group |
| `&` | Entire matched text in replacement |

**Swap two words**

```bash
echo "John Smith" | sed -E 's/([^ ]+) ([^ ]+)/\2 \1/'
```

Output:

```text
Smith John
```

**Surround numbers with brackets**

```bash
echo "ID 123" | sed -E 's/[0-9]+/[&]/'
```

Output:

```text
ID [123]
```

## 7. Extracting Text with sed

Use `-n` to suppress normal output and `p` to print only what is needed.

**Extract numeric IDs**

```bash
echo "user_id=12345" | sed -nE 's/.*user_id=([0-9]+).*/\1/p'
```

Output:

```text
12345
```

**Extract values after `=`**

```bash
sed -nE 's/^[^=]+=([^=]*)$/\1/p' config.txt
```

**Remember:** `-n` + substitution with `p` is useful for extracting matches.

## 8. Inserting, Appending and Replacing Lines

| Command | Purpose |
|---|---|
| `a\` | Append a line after a match |
| `i\` | Insert a line before a match |
| `c\` | Replace the selected line |

```bash
sed '/ERROR/a\Check logs' app.log
sed '/ERROR/i\Alert:' app.log
sed '/^DEBUG/c\DEBUG REMOVED' app.log
```

These produce transformed output without editing the original file.

## 9. Editing Files Safely

```bash
sed 's/old/new/g' file.txt
```

Preview changes.

```bash
sed -i.bak 's/old/new/g' file.txt
```

Modify the file and keep a `.bak` backup.

**Remember:** Prefer previewing changes before using `-i`.

---

## 10. awk — Basic Introduction (Week 1 Requirement)

`awk` is a text-processing tool especially useful for columns, fields, and calculations.

**Syntax:**

```bash
awk 'pattern { action }' filename
```

| Variable | Meaning |
|---|---|
| `$0` | Entire line |
| `$1` | First field |
| `$2` | Second field |
| `$NF` | Last field |
| `NF` | Number of fields |
| `NR` | Current record/line number |
| `-F` | Field separator |

**Print first column**

```bash
awk '{print $1}' file.txt
```

**Print first and third columns**

```bash
awk '{print $1, $3}' file.txt
```

**Use comma as delimiter**

```bash
awk -F',' '{print $2}' data.csv
```

Note: `-F','` splits on commas but is not a complete parser for quoted CSV data.

**Print lines where the third field is greater than 50**

```bash
awk '$3 > 50' marks.txt
```

**Sum values in the first column**

```bash
awk '{sum += $1} END {print sum}' numbers.txt
```

**Print line numbers**

```bash
awk '{print NR, $0}' file.txt
```

For your course, basic field extraction, filtering, and simple calculations are enough.

## 11. sed vs awk vs grep

| Tool | Best Used For |
|---|---|
| `grep` | Finding matching lines |
| `sed` | Replacing, editing, transforming text |
| `awk` | Field extraction, calculations, structured text |

```bash
grep 'ERROR' app.log
sed 's/ERROR/WARNING/g' app.log
awk '{print $1}' app.log
```

## 12. Practical Pipeline Examples

**Find ERROR messages and replace the label**

```bash
grep 'ERROR' app.log | sed 's/ERROR/ISSUE/g'
```

**Extract first column and count unique values**

```bash
awk '{print $1}' data.txt | sort | uniq -c
```

**Remove comments, then count remaining lines**

```bash
sed '/^[[:space:]]*#/d; /^[[:space:]]*$/d' config.txt | wc -l
```

**Find the most common values in the second column**

```bash
awk '{print $2}' data.txt | sort | uniq -c | sort -nr | head -n 5
```

## 13. Common Mistakes

- `sed -i` modifies the original file.
- Without `-n`, `sed` prints processed lines automatically.
- `s/old/new/` changes only the first match per line.
- `s/old/new/g` replaces all matches per line.
- `awk` fields start at `$1`, not `$0`.
- `$0` in `awk` means the entire record.
- `awk` uses whitespace as its default field separator.
- Capture groups in `sed -E` use `(...)`, while replacements use `\1`, `\2`, etc.
- For very complicated transformations, a short Python script may be easier to maintain.

## 14. Quick Recall

- `sed` → Edit text streams.
- `-e` → Multiple expressions.
- `-n` → Suppress automatic output.
- `p` → Print.
- `d` → Delete.
- `s` → Substitute.
- `g` → Replace all matches per line.
- `a` → Append text.
- `i` → Insert text.
- `c` → Change selected line.
- `\1` → First captured group.
- `&` → Entire matched text.
- `awk` → Field-based processing.
- `$1` → First field.
- `$0` → Entire line.
- `NF` → Number of fields.
- `NR` → Line number.
- `-F` → Field separator.
- `END` → Run after processing input.

**Core takeaway:** Use `sed` for powerful text transformations and `awk` for extracting and processing fields. Combine them with `grep`, `sort`, `uniq`, and pipes to solve practical text-processing tasks.