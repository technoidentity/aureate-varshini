# Chapter 9 — Linux Permissions

## 1. Users, Groups & Permissions

Linux is a **multiuser operating system**. Permissions control who can access, modify, or execute files.

Three permission categories:

- **u (User)** → File owner
- **g (Group)** → Users belonging to the file's group
- **o (Others)** → Everyone else

```bash
id          # Display user ID, group ID, groups
whoami      # Current username
ls -l       # View file permissions
```

Important files:
- `/etc/passwd` → User account information
- `/etc/group` → Group information
- `/etc/shadow` → Protected password information

## 2. Understanding File Permissions

Example:

```text
-rwxr-xr--
```

<box border radius="lg" padding={3} gap={2}>
  <grid columns={4} gap={1}>
    {#each [{p:"-",l:"File type",d:"Regular file"},{p:"rwx",l:"Owner",d:"Read, write, execute"},{p:"r-x",l:"Group",d:"Read, execute"},{p:"r--",l:"Others",d:"Read only"}] as item}
      <grid-item>
        <box background="surface-secondary" radius="md" padding={2} align="center" gap={1}>
          <title size="lg" color="default">{item.p}</title>
          <text weight="medium" size="xs">{item.l}</text>
          <text color="secondary" size="3xs" textAlign="center">{item.d}</text>
        </box>
      </grid-item>
    {/each}
  </grid>
</box>

| Permission | File | Directory |
|---|---|---|
| `r` (read) | Read contents | List filenames |
| `w` (write) | Modify contents | Create, delete, rename entries (with `x`) |
| `x` (execute) | Execute file | Enter/traverse directory |

**Important:** Deleting a file depends on the parent directory's permissions, not the file's write permission.

## 3. chmod — Change Permissions

Two ways to change permissions:

### Numeric / Octal Mode

| Value | Permission |
|---|---|
| `4` | Read (`r`) |
| `2` | Write (`w`) |
| `1` | Execute (`x`) |
| `7` | `rwx` (4+2+1) |
| `6` | `rw-` (4+2) |
| `5` | `r-x` (4+1) |
| `0` | `---` |

```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 600 secrets.txt  # rw-------
chmod 700 private/     # rwx------
```

**Remember:** Three digits represent Owner → Group → Others.

### Symbolic Mode

| Symbol | Meaning |
|---|---|
| `u`, `g`, `o`, `a` | User, group, others, all |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permissions |

```bash
chmod u+x script.sh    # Add execute for owner
chmod g-w file.txt     # Remove group write
chmod o-r file.txt     # Remove others read
chmod a+r file.txt     # Allow everyone to read
```

## 4. umask — Default Permissions

`umask` controls which permissions are removed when new files and directories are created.

```bash
umask       # Check current mask
umask 022   # Set mask
```

Typical starting permissions:

| Type | Base | With umask 022 |
|---|---|---|
| File | `666` | `644` (`rw-r--r--`) |
| Directory | `777` | `755` (`rwxr-xr-x`) |

**Remember:** `umask` masks permission bits; it isn't ordinary arithmetic subtraction.

## 5. Ownership — chown & chgrp

| Command | Purpose |
|---|---|
| `chown` | Change file owner |
| `chgrp` | Change group ownership |
| `ls -l` | Check current owner and group |

```bash
sudo chown alice file.txt
sudo chown alice:developers file.txt
sudo chgrp developers file.txt
sudo chown -R alice:developers project/
```

`-R` → Apply recursively (use carefully).

## 6. sudo vs su

| Command | Purpose |
|---|---|
| `sudo command` | Execute a command with another user's privileges (usually root) |
| `su - username` | Switch to another user's login environment |
| `sudo -i` | Open a root login shell |

```bash
sudo systemctl restart nginx
sudo -i
exit
```

- **Root** → Superuser with UID `0`.
- `sudo` → Temporary elevated privileges.
- `su` → Switch user identity.

**Important:** Avoid using `sudo` unnecessarily. Incorrect root commands can modify or damage system files.

## 7. User & Group Management

| Command | Purpose |
|---|---|
| `passwd` | Change password |
| `usermod` | Modify user account |
| `addgroup` | Add group (distribution-dependent) |
| `id` | Check user and group identity |

```bash
passwd
id
sudo usermod -aG developers alice
```

`-aG` → Append user to supplementary group without removing existing memberships.

On NixOS, users and groups may be managed declaratively; persistent changes should normally be made through the NixOS configuration.

## 8. Special Permissions

| Permission | Purpose |
|---|---|
| SUID | Execute program with file owner's effective privileges |
| SGID | Execute with group privileges; on directories, inherit group ownership |
| Sticky bit | Restrict deletion/renaming in shared directories |

Example:

```bash
ls -ld /tmp
# drwxrwxrwt
```

`t` → Sticky bit, commonly used on `/tmp`.

**Remember:** Sticky bit helps prevent users from deleting one another's files in shared writable directories.

## 9. Common Mistakes

- `chmod 777` gives everyone full access — avoid unless genuinely required.
- `chmod 755` is common for executable scripts/directories, not ordinary text files.
- `chmod 644` is common for regular non-executable files.
- File write permission does not necessarily allow deleting the file.
- `sudo` does not permanently change a file's owner.
- `umask` affects default permissions for newly created files/directories, not existing ones.

## 10. Quick Recall

- `id` → User/group identity.
- `ls -l` → View permissions and ownership.
- `r=4`, `w=2`, `x=1`.
- `755` → `rwxr-xr-x`.
- `644` → `rw-r--r--`.
- `600` → Owner read/write only.
- `chmod` → Change permissions.
- `chown` → Change owner.
- `chgrp` → Change group.
- `umask` → Mask default permissions.
- `sudo` → Run with elevated privileges.
- `su` → Switch user.
- `passwd` → Change password.
- `SUID`, `SGID`, `Sticky bit` → Special permissions.

**Core takeaway:** Linux permissions control access through **owner, group, and others**. Master `rwx`, numeric permissions (`755`, `644`, `600`), `chmod`, `chown`, `umask`, and `sudo`.