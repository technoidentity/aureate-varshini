# Chapter 1 — Navigating the Command Line

## 1. Shell & Terminal

- **Terminal** → Interface used to interact with the shell.
- **Shell** → Interprets and executes commands (e.g., `bash`, `zsh`, `fish`).
- **Prompt** → Indicates the shell is ready to accept commands.
- **Command structure** → `command [options] [arguments]`

```bash
ls -la /home
# ls       → command
# -la      → options
# /home    → argument
```

## 2. Essential Navigation Commands

| Command | Purpose |
|---|---|
| `pwd` | Print current working directory |
| `ls` | List files and directories |
| `ls -a` | Include hidden files |
| `ls -l` | Long listing with metadata |
| `ls -h` | Human-readable sizes (with `-l`) |
| `ls -la` | Detailed listing including hidden files |
| `ls -ltr` | Sort by modification time, oldest first |
| `ls -R` | List directories recursively |
| `cd dir` | Enter a directory |
| `cd ..` | Move to parent directory |
| `cd ~` | Go to home directory |
| `cd -` | Return to previous directory |
| `cd /` | Go to filesystem root |
| `clear` | Clear terminal display |
| `history` | Show command history |

**Remember:** Linux commands and filenames are case-sensitive.

## 3. Paths

**Absolute path** → Full path starting from `/`.

```bash
cd /home/user/projects
```

**Relative path** → Path relative to the current directory.

```bash
cd projects
cd ../documents
```

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `~` | Home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous directory (with `cd`) |

**Remember:** Hidden files usually begin with `.` (e.g., `.env`, `.bashrc`).

## 4. Command-Line Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + A` | Move cursor to beginning |
| `Ctrl + E` | Move cursor to end |
| `Ctrl + U` | Delete from cursor to beginning |
| `Ctrl + K` | Delete from cursor to end |
| `Ctrl + W` | Delete previous word |
| `Ctrl + L` | Clear terminal display |
| `Ctrl + C` | Interrupt running command |
| `Ctrl + D` | Send EOF / exit shell at empty prompt |
| `Ctrl + R` | Search command history |
| `↑ / ↓` | Navigate command history |
| `Tab` | Autocomplete commands and paths |

## 5. Inspecting Files & Directories

```bash
ls -la                    # All files including hidden
ls -lh                    # Detailed listing with readable sizes
ls /home/user/projects    # Inspect without changing directory
cat file.txt              # Display file contents
file script.sh            # Identify file type
stat file.txt             # Display file metadata
find . -name "*.log"      # Find matching files recursively
find . -maxdepth 1 -name "*.log"  # Search current directory only
```

**Remember:** `find .` searches from the current directory, including subdirectories by default.

## 6. Getting Help

| Command | Purpose |
|---|---|
| `man ls` | Open command manual |
| `ls --help` | Quick help (for supported commands) |
| `help cd` | Help for Bash built-in commands |
| `tldr ls` | Common usage examples (if installed) |
| `type cd` | Identify how a command is resolved |

- `man` → Detailed documentation.
- `tldr` → Practical examples.
- `--help` → Quick reference.

## 7. Quick Practical Examples

**Inspect your current environment**

```bash
pwd
ls -la
```

**Find a hidden configuration file**

```bash
find . -type f -name ".env"
```

**Inspect another directory without entering it**

```bash
ls -la backend/
```

**Find backup files**

```bash
find . -type f -name "*.bak"
```

**Navigate between directories**

```bash
cd ~/projects
cd ..
cd -
```

## 8. Quick Recall

- `pwd` → Where am I?
- `ls` → What's here?
- `cd` → Move around.
- `.` → Here.
- `..` → One level up.
- `~` → Home.
- `/` → Root.
- `Tab` → Autocomplete.
- `Ctrl + R` → Search history.
- `man` → Understand commands.

**Core takeaway:** Get comfortable navigating directories, understanding paths, inspecting files, and using keyboard shortcuts without relying on a graphical file manager.