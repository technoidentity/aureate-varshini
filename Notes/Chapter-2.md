# Chapter 2 — Thinking in Pipelines

## 1. What is a Pipeline?

- **Pipeline (`|`)** → Connects multiple commands.
- Output of one command becomes the input of the next.
- Helps solve complex tasks by combining simple commands.

```bash
command1 | command2 | command3
```

**Remember:** Each command does one job, and the pipeline combines them.

## 2. Standard Streams

Linux commands commonly use three streams:

| Stream | Number | Purpose |
|---|---|---|
| `stdin` | `0` | Standard input |
| `stdout` | `1` | Standard output |
| `stderr` | `2` | Standard error |

```bash
cat file.txt | grep "error"
```

- `cat` → Sends file contents to stdout.
- `|` → Passes stdout to the next command's stdin.
- `grep` → Filters matching lines.

**Important:** `|` passes stdout, not stderr, by default.

## 3. Redirection Operators

| Operator | Purpose |
|---|---|
| `>` | Write output to file (overwrite) |
| `>>` | Append output to file |
| `<` | Read input from file |
| `2>` | Redirect errors to file |
| `2>>` | Append errors to file |
| `2>&1` | Redirect stderr to current stdout destination |
| `|` | Pipe stdout to another command |

```bash
echo "hello" > file.txt      # Overwrite
echo "world" >> file.txt     # Append
sort < file.txt              # Read input
ls /invalid 2> errors.txt    # Save errors
```

**Remember:** `>` overwrites existing content. Use it carefully.

## 4. Common Pipeline Commands

| Command | Purpose |
|---|---|
| `cat` | Display/concatenate file contents |
| `grep` | Filter matching lines |
| `sort` | Sort lines |
| `uniq` | Remove adjacent duplicate lines |
| `wc -l` | Count lines |
| `head -n 5` | Display first 5 lines |
| `tail -n 5` | Display last 5 lines |
| `cut` | Extract fields |
| `tee` | Display output and save to a file |
| `find` | Search for files/directories |

## 5. Essential Pipeline Examples

**Find lines containing "error"**

```bash
grep "error" app.log
```

**Count matching lines**

```bash
grep "error" app.log | wc -l
```

**Sort and remove duplicates**

```bash
sort names.txt | uniq
```

`uniq` only removes **adjacent duplicates**, so sorting first is useful.

**Count occurrences of each error**

```bash
grep -i "error" app.log | sort | uniq -c | sort -nr
```

- `grep -i` → Case-insensitive filtering.
- `sort` → Group identical lines together.
- `uniq -c` → Count repetitions.
- `sort -nr` → Sort counts numerically, highest first.

**Find the five largest counts**

```bash
sort names.txt | uniq -c | sort -nr | head -n 5
```

## 6. Sorting Essentials

| Command | Purpose |
|---|---|
| `sort` | Alphabetical/lexicographic order |
| `sort -n` | Numeric sorting |
| `sort -r` | Reverse sorting |
| `sort -nr` | Numeric descending |
| `sort -h` | Human-readable sizes |
| `sort -f` | Ignore case |
| `sort -u` | Sort and remove duplicates |
| `sort -t ',' -k3,3` | Sort CSV-like lines by third field |

```bash
sort -nr scores.txt
sort -t ',' -k3,3 data.csv
```

## 7. Using `tee`

`tee` sends output to both the terminal and a file.

```bash
ls | tee files.txt
```

- Displays filenames.
- Saves filenames to `files.txt`.

Use `tee -a` to append instead of overwriting.

```bash
grep "error" app.log | tee errors.txt | wc -l
```

Saves matching lines while displaying their count.

## 8. Practical Workplace Examples

**Find all `.bak` files recursively**

```bash
find . -type f -name "*.bak"
```

**Sort backup filenames**

```bash
find . -type f -name "*.bak" | sort
```

**Count backup files**

```bash
find . -type f -name "*.bak" | wc -l
```

**Show first five sorted filenames**

```bash
find . -type f -name "*.bak" | sort | head -n 5
```

**Save the sorted list, display only the first five**

```bash
find . -type f -name "*.bak" | sort | tee backup-list.txt | head -n 5
```

**Count files, save their sorted paths, and display only the count and first five**

```bash
find . -type f -name "*.bak" |
  tee >(wc -l) |
  sort |
  tee backup-list.txt |
  head -n 5
```

`>(...)` is Bash/Zsh process substitution. It runs the count separately without stopping the main pipeline.

## 9. Pipeline vs Redirection

| Pipeline | Redirection |
|---|---|
| `command1 \| command2` | `command > file` |
| Connects commands | Connects command to file |
| Passes output for processing | Saves or reads data |

```bash
cat file.txt | sort
sort < file.txt
sort file.txt > sorted.txt
```

## 10. Common Mistakes

- `uniq` without sorting may leave non-adjacent duplicates.
- `>` overwrites; `>>` appends.
- Pipelines don't automatically pass error messages.
- `grep` filters lines; `find` searches filesystem entries.
- `head` may stop a pipeline early after receiving enough lines.
- `wc -l` counts newline characters, not necessarily logical records.

## 11. Quick Recall

- `|` → Connect commands.
- `stdin` → Input.
- `stdout` → Normal output.
- `stderr` → Error output.
- `>` → Overwrite file.
- `>>` → Append to file.
- `grep` → Filter.
- `sort` → Arrange.
- `uniq` → Remove adjacent duplicates.
- `uniq -c` → Count duplicates.
- `wc -l` → Count lines.
- `head` → First lines.
- `tail` → Last lines.
- `tee` → Save and forward output.
- `find` → Locate files.

**Core takeaway:** Think of Linux commands as small building blocks. Use pipelines to combine filtering, sorting, counting, and saving into a single workflow.