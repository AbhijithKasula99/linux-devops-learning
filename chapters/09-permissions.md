# Chapter 9 — Permissions

## Topics
1. Users, Group Members, and Everybody Else
2. Reading, Writing, and Executing
3. `chmod` — Change File Mode
4. Setting File Mode with the GUI
5. `umask` — Set Default Permissions
6. Some Special Permissions
7. Changing Identities
8. `su` — Substitute User and Group IDs
9. `sudo` — Execute a Command as Another User
10. `chown` — Change File Owner and Group
11. `chgrp` — Change Group Ownership
12. Exercising Our Privileges
13. Changing Your Password
14. Summing Up

## Permission model

Linux permissions divide access into:

- **Owner (`u`)**
- **Group (`g`)**
- **Others (`o`)**

Inspect them with:

```bash
ls -l filename
```

Example:

```text
-rw-r--r-- 1 abhijith abhijith 0 permissions-test.txt
```

The first `abhijith` is the owner; the second is the group.

### Basic permissions

```text
r = read
w = write
x = execute
- = not granted
```

Example:

```text
-rwxr-xr--
```

means:

- Owner: `rwx`
- Group: `r-x`
- Others: `r--`

## chmod

Symbolic form:

```bash
chmod u+x file
chmod g+w file
chmod g-w file
chmod o+w file
chmod o-w file
```

- `u` = owner
- `g` = group
- `o` = others
- `+` = add
- `-` = remove

Multiple changes can be combined:

```bash
chmod u+x,g+x,o-r app.sh
```

### Numeric mode

```text
r = 4
w = 2
x = 1
```

Examples:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
0 = ---
```

Therefore:

```bash
chmod 754 file
```

means:

```text
Owner  rwx
Group  r-x
Others r--
```

And:

```bash
chmod 755 script.sh
```

means `rwxr-xr-x`.

## umask

`umask` removes permissions from the defaults used when creating new files/directories.

Check it:

```bash
umask
```

With:

```text
0022
```

a newly created regular file commonly gets:

```text
-rw-r--r--
```

or `644`.

With:

```bash
umask 0077
touch private.txt
```

a new regular file becomes:

```text
-rw-------
```

or `600`.

**Important:** changing `umask` affects newly created objects; it does not change existing files.

Restore:

```bash
umask 0022
```

## Special permissions

### setuid

`setuid` appears as `s` in the owner's execute position:

```text
-rwsr-xr-x
   ^
```

Example:

```bash
ls -l /usr/bin/passwd
```

A setuid program can run with the file owner's privileges.

### setgid

`setgid` appears as `s` in the group's execute position:

```text
drwxr-sr-x
     ^
```

For a directory, setgid causes newly created files to inherit the directory's group ownership.

Enable with:

```bash
chmod g+s directory
```

### Sticky bit

The sticky bit appears as `t` in the others' execute position:

```text
drwxrwxrwt
        ^
```

A shared directory such as `/tmp` uses it so users generally cannot delete or rename files owned by other users unless appropriately privileged.

## Changing identities

### sudo

```bash
sudo whoami
```

can return:

```text
root
```

`sudo` elevates the specified command; it does not change the current shell's identity.

### su

```bash
su -
```

starts a login shell as the target user and requires that user's authentication.

In our WSL environment, `su -` failed because root authentication was not available for that attempt. We demonstrated a root shell with:

```bash
sudo su -
```

Then:

```bash
whoami
```

returned `root`.

Exit with:

```bash
exit
```

## Ownership

### chown

Change owner:

```bash
sudo chown root permissions-test.txt
```

Change owner and group:

```bash
sudo chown abhijith:users permissions-test.txt
```

General form:

```bash
chown user:group file
```

### chgrp

Change only the group:

```bash
sudo chgrp abhijith permissions-test.txt
```

So:

```text
chown → owner, and optionally group
chgrp → group only
```

## Exercising privileges

A root-owned file:

```text
-rw-r--r-- 1 root root root-owned.txt
```

could be read by the normal user but not written directly:

```bash
echo "hello" >> root-owned.txt
```

returned `Permission denied`.

This worked:

```bash
sudo sh -c 'echo "hello" >> root-owned.txt'
```

because the **shell performing `>>`** was running as root.

This distinction is important:

```bash
sudo echo "hello" >> root-owned.txt
```

does not make the redirection root-owned; the current shell performs the redirection.

## Passwords

Change your own password:

```bash
passwd
```

Inspect password status:

```bash
passwd -S
```

Example:

```text
abhijith P 2026-08-04 0 99999 7 -1
```

`P` indicates that a password is set.

## Practical commands practiced

```bash
id
ls -l file
chmod u+x file
chmod g+w file
chmod g-w file
chmod o+w file
chmod o-w file
chmod 754 file
umask
umask 0077
umask 0022
ls -ld /tmp
chmod g+s directory
sudo whoami
sudo su -
su -
chown user:group file
chgrp group file
sudo sh -c 'echo "hello" >> file'
passwd -S
```

## Interview checkpoint

Covered:

- Owner/group/others
- Symbolic and numeric `chmod`
- `umask`
- setuid, setgid, sticky bit
- `sudo` vs `su`
- `chown` vs `chgrp`
- Privilege boundaries
- Shell redirection with `sudo`
- Password status

### Corrections worth remembering

- `umask 0022` + a new regular file commonly results in `644` (`rw-r--r--`).
- `chmod 755` means `rwxr-xr-x`.
- If a `755` script still cannot execute, possible causes include a `noexec` filesystem or a bad/missing interpreter in its shebang.
