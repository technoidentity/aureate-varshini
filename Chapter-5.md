# Chapter 5 — Getting to Grips with grep

## 1. What is grep?

- **`grep` (Global Regular Expression Print)** → Searches text for matching patterns.
- Prints lines that match a given pattern.
- Supports regular expressions (Regex).
- Commonly used for searching logs, files, and pipeline output.

**Syntax:**

```bash
grep [options] 'pattern' filename
```

```bash
grep 'error' app.log
```

Displays lines containing `error`.

## 2. Essential grep Options

| Option | Purpose |
|---|---|
| `-i` | Case-insensitive search |
| `-n` | Show line numbers |
| `-v` | Invert match (non-matching lines) |
| `-c` | Count matching lines |
| `-l` | Show filenames containing matches |
| `-L` | Show filenames without matches |
| `-r` | Search directories recursively |
| `-R` | Recursive search, following symlinks |
| `-w` | Match whole words |
| `-x` | Match entire lines |
| `-o` | Print only matched text |
| `-E` | Extended regular expressions |
| `-F` | Fixed string search (no regex interpretation) |
| `-q` | Quiet mode (check match through exit status) |

```bash
grep -in 'error' app.log
# Ignore case + show line numbers
```

## 3. Basic Searching

**Search for a word**

```bash
grep 'error' app.log
```

**Ignore case**

```bash
grep -i 'error' app.log
```

Matches `error`, `ERROR`, `Error`, etc.

**Search multiple files**

```bash
grep 'error' *.log
```

**Search recursively**

```bash
grep -rn 'TODO' .
```

Searches all files under the current directory and displays line numbers.

**Show only matching filenames**

```bash
grep -rl 'TODO' .
```

## 4. Matching with Regex

| Pattern | Purpose |
|---|---|
| `^ERROR` | Lines starting with ERROR |
| `done$` | Lines ending with done |
| `^$` | Empty lines |
| `[0-9]` | Contains a digit |
| `[0-9]+` | One or more digits (ERE) |
| `error\|warning` | Either error or warning (ERE) |
| `\.` | Literal dot |

```bash
grep '^ERROR' app.log
grep -E 'error|warning' app.log
grep -E '[0-9]{3}' file.txt
```

**Remember:** Use `grep -E` for extended regex operators like `+`, `?`, `|`, and `{n}`.

## 5. Matching Words vs Substrings

```bash
grep 'cat' file.txt
```

Matches `cat`, `catalog`, `educate`, etc.

```bash
grep -w 'cat' file.txt
```

Matches `cat` as a whole word.

```bash
grep -x 'cat' file.txt
```

Matches only lines whose entire content is `cat`.

**Remember:**
- Normal `grep` → Substring match.
- `-w` → Whole word.
- `-x` → Entire line.

## 6. Inverting & Counting Matches

**Display non-matching lines**

```bash
grep -v 'DEBUG' app.log
```

**Count matching lines**

```bash
grep -c 'ERROR' app.log
```

**Count occurrences (not lines)**

```bash
grep -o 'ERROR' app.log | wc -l
```

**Count non-empty lines**

```bash
grep -cve '^$' file.txt
```

**Important:** `grep -c` counts matching lines, not the total number of occurrences within those lines.

## 7. Extracting Only Matching Text

By default, `grep` displays entire matching lines.

Use `-o` to extract only the matched portion.

```bash
echo 'ID: 12345' | grep -Eo '[0-9]+'
```

Output:

```text
12345
```

**Extract IP-like patterns**

```bash
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' app.log
```

This matches IPv4-shaped text but does not validate that each number is 0–255.

## 8. Searching with Context

Useful when debugging logs.

| Option | Meaning |
|---|---|
| `-A 3` | Show 3 lines after match |
| `-B 3` | Show 3 lines before match |
| `-C 3` | Show 3 lines before and after |

```bash
grep -A 3 'ERROR' app.log
grep -B 3 'ERROR' app.log
grep -C 3 'ERROR' app.log
```

**Remember:**
- `A` → After.
- `B` → Before.
- `C` → Context.

## 9. grep with Pipelines

`grep` is commonly combined with other commands.

**Find errors and count them**

```bash
grep -i 'error' app.log | wc -l
```

**Find unique error lines**

```bash
grep -i 'error' app.log | sort -u
```

**Find most frequent error lines**

```bash
grep -i 'error' app.log | sort | uniq -c | sort -nr
```

**Show first five matching lines**

```bash
grep 'ERROR' app.log | head -n 5
```

**Search process list**

```bash
ps aux | grep '[n]ginx'
```

For searching running processes by name, `pgrep nginx` is usually simpler.

## 10. Practical Workplace Examples

**Find TODO comments in a project**

```bash
grep -rn 'TODO' --include='*.py' .
```

**Find errors in all log files**

```bash
grep -in 'error' *.log
```

**Exclude DEBUG messages**

```bash
grep -v 'DEBUG' app.log
```

**Find error and warning messages**

```bash
grep -Ei 'error|warning' app.log
```

**Find failed login attempts**

```bash
grep -i 'failed' auth.log
```

**Search all `.log` files recursively**

```bash
grep -rn --include='*.log' 'ERROR' .
```

**Exclude specific directories**

```bash
grep -rn --exclude-dir='.git' 'TODO' .
```

## 11. grep Exit Status

`grep` returns an exit status that scripts can use.

| Exit Code | Meaning |
|---|---|
| `0` | Match found |
| `1` | No match found |
| `2` | Error occurred (GNU grep) |

```bash
if grep -q 'ERROR' app.log; then
    echo 'Errors found'
fi
```

`-q` avoids printing matching lines.

## 12. grep vs find vs rg

| Command | Purpose |
|---|---|
| `grep` | Search text inside files |
| `find` | Search filesystem entries |
| `rg` (ripgrep) | Fast recursive text search |

```bash
find . -name '*.log'    # Find log files
grep 'error' app.log    # Search file content
rg 'error'              # Search project text recursively
```

**Remember:** `rg` respects ignore files (such as `.gitignore`) by default, unlike ordinary recursive `grep`.

## 13. Common Mistakes

- `grep` searches file contents, while `find` searches file paths.
- `grep` is case-sensitive by default.
- `grep -c` counts matching lines, not occurrences.
- `grep 'cat'` also matches words containing `cat`.
- Use quotes around regex patterns to prevent shell expansion.
- `grep -E` supports extended regex syntax.
- `grep -r` searches recursively and may include unwanted directories.
- `grep -F` treats regex symbols as literal characters.

## 14. Quick Recall

- `grep` → Search matching lines.
- `-i` → Ignore case.
- `-n` → Line numbers.
- `-v` → Exclude matches.
- `-c` → Count matching lines.
- `-l` → Matching filenames.
- `-r` → Recursive search.
- `-w` → Whole word.
- `-x` → Whole line.
- `-o` → Only matched text.
- `-E` → Extended regex.
- `-F` → Fixed strings.
- `-A` → After.
- `-B` → Before.
- `-C` → Context.
- `-q` → Quiet.
- `rg` → Modern, fast text searching.

**Core takeaway:** `grep` filters text by patterns. Master `-i`, `-n`, `-v`, `-r`, `-o`, `-E`, and `-c`, then combine `grep` with pipelines for practical log analysis and troubleshooting.