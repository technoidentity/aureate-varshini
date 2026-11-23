# Chapter 11 — The Environment

## 1. What is the Environment?

- **Environment** → Collection of variables used by the shell and programs to determine their behavior.
- Used to configure programs, command locations, language, and user settings.
- The shell also maintains aliases and functions.

**Two types of variables:**

| Shell Variables | Environment Variables |
|---|---|
| Available in current shell | Passed to child processes |
| `name="Varshini"` | `export name="Varshini"` |

```bash
name="Varshini"         # Shell variable
export ENV="development" # Environment variable
```

**Remember:** `export` makes a shell variable available to subsequently launched programs.

## 2. Examining the Environment

| Command | Purpose |
|---|---|
| `printenv` | Display environment variables |
| `printenv PATH` | Display specific environment variable |
| `env` | Display environment / run command with modified environment |
| `set` | Show shell variables, functions and environment (Bash) |
| `echo "$HOME"` | Display a variable's value |
| `alias` | Display defined aliases |

```bash
printenv | less
printenv PATH
echo "$SHELL"
```

**Remember:** `printenv` displays exported environment variables, while Bash's `set` shows more shell information.

## 3. Important Environment Variables

| Variable | Purpose |
|---|---|
| `PATH` | Directories searched for commands |
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | User's configured login shell |
| `PWD` | Current working directory |
| `OLDPWD` | Previous working directory |
| `LANG` | Default language/locale |
| `EDITOR` | Preferred text editor |
| `PAGER` | Program used to view paginated output |
| `PS1` | Bash primary prompt setting (shell variable) |

```bash
echo "$PATH"
echo "$HOME"
echo "$PWD"
```

## 4. PATH — Most Important Variable

`PATH` tells the shell where to search for executable commands.

```bash
echo "$PATH"
```

Example:

```text
/usr/local/bin:/usr/bin:/bin
```

- `:` separates directories.
- Directories are searched in order.
- If a command cannot be found, its executable directory may not be in `PATH`.

**Check where a command is found:**

```bash
command -v git
which python3
```

**Temporarily add a directory to PATH:**

```bash
export PATH="$HOME/bin:$PATH"
```

This applies to the current shell and its child processes.

**Remember:** `PATH` is for executable commands, not searching for ordinary files.

## 5. Modifying Variables

```bash
name="Varshini"       # Create shell variable
echo "$name"          # Read variable
export name           # Export to child processes
unset name            # Remove variable
```

**Temporary environment variable for one command:**

```bash
ENV=development ./script.sh
```

The variable is supplied to that command without permanently changing the current shell's environment.

## 6. Aliases

An **alias** creates a shortcut for a command.

```bash
alias ll='ls -lah'
alias gs='git status'
```

Usage:

```bash
ll
gs
```

**View and remove aliases:**

```bash
alias
unalias gs
```

**Remember:** Aliases are usually shell-specific and primarily useful in interactive shells.

## 7. Shell Configuration Files

Shell startup files customize the environment.

| File | Purpose |
|---|---|
| `~/.bashrc` | Interactive Bash configuration |
| `~/.profile` | Login shell configuration |
| `~/.bash_profile` | Bash login configuration, if present |
| `/etc/profile` | System-wide login configuration |

**Reload Bash configuration:**

```bash
source ~/.bashrc
```

`source` executes a file in the current shell, allowing it to modify your environment.

**NixOS note:** In your setup, Home Manager manages many configuration files declaratively. Make persistent configuration changes in the appropriate `.nix` file rather than editing generated read-only files.

## 8. Common Mistakes

- Shell variables are not automatically inherited by child processes.
- `export` does not make variables permanent across new login sessions.
- Adding a directory to `PATH` does not install a program.
- Shell configuration changes may require a new session or `source`.
- `~/.bashrc` applies to Bash, not necessarily Fish or other shells.
- Don't overwrite `PATH` accidentally; preserve existing entries using `:$PATH`.

## 9. Quick Recall

- `printenv` → Show environment variables.
- `set` → Show shell variables and functions (Bash).
- `env` → Inspect/modify command environment.
- `export` → Pass variables to child processes.
- `unset` → Remove variable.
- `PATH` → Executable search directories.
- `HOME` → Home directory.
- `PWD` → Current directory.
- `SHELL` → Login shell.
- `alias` → Command shortcut.
- `unalias` → Remove alias.
- `source` → Execute file in current shell.
- `~/.bashrc` → Bash interactive shell configuration.

**Core takeaway:** Understand the difference between shell and environment variables, how `export` works, and how `PATH` controls command execution. Learn to inspect and customize your environment using `printenv`, `set`, `alias`, and `source`.