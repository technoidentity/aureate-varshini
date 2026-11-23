# Chapter 3 — Filesystem

## 1. Linux Filesystem Basics

- **Filesystem** → Organizes files and directories in a hierarchical tree.
- **Root (`/`)** → Starting point of the entire filesystem.
- **Directory** → Contains files and other directories.
- **Path** → Location of a file or directory.
- **Inode** → Stores file metadata and pointers to data (not its filename).
- **Everything is a file** → Linux exposes many devices and system interfaces through file-like paths.

## 2. Important Linux Directories

| Directory | Purpose |
|---|---|
| `/` | Root of filesystem |
| `/home` | Regular users' home directories |
| `/root` | Root user's home |
| `/etc` | System configuration |
| `/bin`, `/usr/bin` | Executable commands |
| `/sbin`, `/usr/sbin` | System administration commands |
| `/var` | Variable data (logs, caches) |
| `/tmp` | Temporary files |
| `/dev` | Device files |
| `/proc` | Process and kernel information |
| `/sys` | Kernel/device information |
| `/mnt` | Temporary mount points |
| `/media` | Removable media |
| `/usr` | Applications, libraries, shared resources |

**Remember:** `/` (filesystem root) and `/root` (root user's home) are different.

## 3. Creating Files & Directories

| Command | Purpose |
|---|---|
| `touch file.txt` | Create empty file/update timestamp |
| `mkdir dir` | Create directory |
| `mkdir -p a/b/c` | Create nested directories |
| `rmdir dir` | Remove empty directory |
| `rm file.txt` | Delete file |
| `rm -r dir` | Delete directory recursively |
| `rm -i file.txt` | Ask before deleting |
| `rm -rf dir` | Force recursive deletion (dangerous) |

```bash
mkdir projects
mkdir -p projects/linux/notes
touch notes.txt
rmdir empty-dir
```

**Important:** `rm` normally deletes files without moving them to a trash folder.

## 4. Copying & Moving Files

| Command | Purpose |
|---|---|
| `cp a.txt b.txt` | Copy file |
| `cp file.txt dir/` | Copy into directory |
| `cp -r src/ dest/` | Copy directory recursively |
| `cp -i a.txt b.txt` | Confirm before overwrite |
| `cp -a src/ dest/` | Preserve metadata while copying |
| `mv old.txt new.txt` | Rename file |
| `mv file.txt dir/` | Move file |
| `mv -i a.txt b.txt` | Confirm before overwrite |

```bash
cp config.txt backup.txt
mv backup.txt backups/
mv oldname.txt newname.txt
```

**Remember:**
- `cp` → Original remains.
- `mv` → Original path changes.

## 5. Inspecting Files

| Command | Purpose |
|---|---|
| `cat file.txt` | Display contents |
| `file file.txt` | Identify file type |
| `stat file.txt` | Show file metadata |
| `ls -l` | Detailed listing |
| `ls -la` | Include hidden files |
| `ls -lh` | Human-readable sizes |
| `du -sh dir/` | Directory disk usage |
| `df -h` | Filesystem disk space |

```bash
file image.png
stat script.sh
ls -lah
du -sh ~/Downloads
```

`stat` commonly shows size, permissions, inode number, and timestamps.

## 6. Finding Files

| Command | Purpose |
|---|---|
| `find . -name "*.txt"` | Find matching names |
| `find . -type f` | Find regular files |
| `find . -type d` | Find directories |
| `find . -iname "*.LOG"` | Case-insensitive name search |
| `find . -maxdepth 1` | Current directory only |
| `find . -size +10M` | Files larger than 10 MiB |
| `find . -mtime -7` | Files modified within last 7 days |

```bash
find . -type f -name ".env"
find . -type f -name "*.bak"
find ~/projects -type d -name "backend"
```

**Remember:** Always quote patterns such as `"*.txt"` so the shell doesn't expand them before `find` receives them.

## 7. Links — `ln`

Linux supports two common types of links.

| Hard Link | Symbolic Link |
|---|---|
| Another name for same inode | Stores path to target |
| Usually cannot cross filesystems | Can cross filesystems |
| Typically used for files | Can reference files/directories |
| Remains valid if original filename removed | Breaks if target disappears |

```bash
ln original.txt hardlink.txt
ln -s original.txt shortcut.txt
```

Inspect links:

```bash
ls -l
readlink shortcut.txt
```

**Remember:** `ln` → Hard link; `ln -s` → Symbolic link.

## 8. File Permissions — Introduction

```text
-rwxr-xr--
```

| Position | Meaning |
|---|---|
| `-` | Regular file (`d` = directory, `l` = symlink) |
| `rwx` | Owner permissions |
| `r-x` | Group permissions |
| `r--` | Others' permissions |

| Permission | Value |
|---|---|
| `r` | 4 — Read |
| `w` | 2 — Write |
| `x` | 1 — Execute |

```bash
chmod 755 script.sh
chmod u+x script.sh
```

- `755` → `rwxr-xr-x`
- `644` → `rw-r--r--`

For directories, `x` allows traversal, while `r` allows listing names.

*Permissions are covered in greater detail in Chapter 9.*

## 9. Wildcards & Globbing

Wildcards help match filenames.

| Pattern | Meaning |
|---|---|
| `*` | Any sequence of characters |
| `?` | Exactly one character |
| `[abc]` | Any one listed character |
| `[0-9]` | Any one digit |
| `*.txt` | Names ending with `.txt` |
| `file?.log` | `file1.log`, `fileA.log`, etc. |

```bash
ls *.txt
ls file?.log
cp *.md notes/
```

**Remember:** The shell expands unquoted wildcards before executing most commands. Hidden names beginning with `.` are usually excluded by `*` in Bash's default configuration.

## 10. Practical Workplace Examples

**Create a project structure**

```bash
mkdir -p project/{src,docs,tests}
touch project/README.md
```

**Find hidden `.env` files**

```bash
find . -type f -name ".env"
```

**Find all backup files**

```bash
find . -type f -name "*.bak"
```

**Count backup files**

```bash
find . -type f -name "*.bak" | wc -l
```

**List five sorted backup paths**

```bash
find . -type f -name "*.bak" | sort | head -n 5
```

**Create a symbolic link**

```bash
ln -s ~/projects/myapp ~/myapp
```

## 11. Common Mistakes

- `rm -r` can permanently delete entire directory trees.
- `rmdir` only removes empty directories.
- `cp` and `mv` may overwrite existing destinations.
- Relative paths depend on the current working directory.
- A symbolic link may break if its target moves or disappears.
- Hidden files are not displayed by ordinary `ls`.
- `du` measures disk usage; `df` shows filesystem capacity/free space.

## 12. Quick Recall

- `/` → Filesystem root.
- `~` → Home directory.
- `touch` → Create empty file/update timestamp.
- `mkdir` → Create directory.
- `mkdir -p` → Create nested directories.
- `cp` → Copy.
- `mv` → Move/rename.
- `rm` → Delete.
- `rmdir` → Delete empty directory.
- `file` → Identify type.
- `stat` → Metadata.
- `find` → Search files/directories.
- `ln` → Hard link.
- `ln -s` → Symbolic link.
- `chmod` → Change permissions.
- `du` → Disk usage.
- `df` → Filesystem free space.
- `*` → Filename wildcard.

**Core takeaway:** Understand how Linux organizes files, how paths work, and how to safely create, inspect, search, copy, move, link, and delete files.