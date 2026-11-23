# Chapter 13 — How Linux Works: User Environments

## 1. What is a User Environment?

- **User environment** → Settings that control how the shell and programs behave.
- **Startup files** → Configure the shell when a user logs in or opens a terminal.
- **Dotfiles** → Hidden configuration files beginning with `.`.
- **Environment variables** → Store settings used by programs.

Examples:

```text
~/.bashrc
~/.profile
~/.config/
```

**Remember:** Startup files determine how your shell is configured.

## 2. Shell Startup Files

| File | Purpose |
|---|---|
| `/etc/profile` | System-wide login shell settings |
| `~/.profile` | User login environment |
| `~/.bash_profile` | Bash login shell configuration |
| `~/.bashrc` | Interactive Bash settings |

- **Login shell** → Started during login; reads login startup files.
- **Interactive shell** → Accepts user commands; Bash commonly reads `.bashrc`.

```bash
source ~/.bashrc    # Reload Bash configuration
```

**Remember:** Different shell types load different startup files.

## 3. Guidelines for Startup Files

Two important principles:

- **Simplicity** → Keep startup files short and avoid unnecessary configurations.
- **Readability** → Use meaningful comments and clear organization.

Modify startup files only when needed, such as changing the prompt, configuring essential software, or fixing broken settings.

**Important:** Incorrect configurations can affect every new shell session.

## 4. Important Environment Variables

| Variable | Purpose |
|---|---|
| `PATH` | Directories searched for executable commands |
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | Configured login shell |
| `EDITOR` | Preferred text editor |
| `PAGER` | Program for viewing paginated output |
| `PS1` | Bash prompt configuration |

```bash
echo "$PATH"
echo "$HOME"
printenv
```

**Remember:** `PATH` is particularly important because it determines where the shell searches for commands.

## 5. Customizing the Environment

**Set and export a variable:**

```bash
name="Varshini"
export name
```

**Add a directory to PATH:**

```bash
export PATH="$HOME/bin:$PATH"
```

**Create an alias:**

```bash
alias ll='ls -lah'
```

**Reload configuration:**

```bash
source ~/.bashrc
```

| Command | Purpose |
|---|---|
| `export` | Pass variables to child processes |
| `alias` | Create command shortcuts |
| `source` | Execute configuration in the current shell |
| `printenv` | Display environment variables |

## 6. NixOS / Home Manager

In your NixOS setup, many user settings are managed declaratively through Home Manager.

- Modify the appropriate `.nix` configuration file.
- Avoid editing generated, read-only configuration files.
- Apply changes through your usual NixOS rebuild process.

Example from your Git configuration:

```nix
programs.git.settings.user = {
  name = "Varshini";
  email = "your-email@example.com";
};
```

This is a practical example of managing user configuration declaratively.

## 7. Quick Recall

- **User environment** → Shell and program settings.
- **Dotfiles** → Hidden configuration files.
- `~/.bashrc` → Interactive Bash settings.
- `~/.profile` → Login environment.
- `PATH` → Executable search directories.
- `export` → Pass variables to child processes.
- `alias` → Command shortcut.
- `source` → Execute file in current shell.
- `printenv` → Inspect environment variables.
- **Simplicity + readability** → Essential startup-file principles.

**Core takeaway:** Understand how startup files configure user environments, how `PATH` and environment variables affect programs, and why configuration files should remain simple and readable.