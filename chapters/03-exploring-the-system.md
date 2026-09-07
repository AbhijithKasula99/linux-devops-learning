# Chapter 3 — Exploring the System

## Goal

Learn to inspect files and directories using `ls`, `file`, and `less`; understand long-format output; explore the Linux filesystem hierarchy; and understand symbolic and hard links.

## 1. More Fun with `ls`

`ls` can list the contents of a directory without changing your current working directory.

```bash
ls ~
ls /usr
ls ~ /usr
```

The last command lists both directories.

### Options and arguments

```bash
ls -l chapters
```

- `ls` → command
- `-l` → option
- `chapters` → argument

Short options can be combined:

```bash
ls -lt
```

Long options use `--`:

```bash
ls -lt --reverse
```

Options are case-sensitive.

### Useful `ls` options

| Option | Meaning |
|---|---|
| `-a` | Show all entries, including `.` and `..` |
| `-A` | Show almost all entries, excluding `.` and `..` |
| `-d` | List a directory itself rather than its contents |
| `-F` | Append indicators showing file types |
| `-h` | Show file sizes in human-readable form |
| `-l` | Long format |
| `-r` | Reverse the sorting order |
| `-S` | Sort by file size, largest first |
| `-t` | Sort by modification time, newest first |

Examples:

```bash
ls -l
ls -lh
ls -lt
ls -lS
ls -lrS
ls -a
ls -A
ls -F
ls -ld chapters
```

## 2. A Longer Look at Long Format

Example:

```text
-rw-r--r-- 1 user user 735 Sep 7 06:43 README.md
```

The fields are:

```text
-rw-r--r--  1  user  user  735  Sep 7 06:43  README.md
│           │    │     │    │         │             │
│           │    │     │    │         │             └─ name
│           │    │     │    │         └─ modification time
│           │    │     │    └─ size
│           │    │     └─ group
│           │    └─ owner
│           └─ hard-link count
└─ type + permissions
```

### File type

The first character identifies the type:

```text
-   regular file
d   directory
l   symbolic link
```

### Permissions

The next nine characters are three groups of three:

```text
-rw-r--r--
 ^^^ ^^^ ^^^
 owner group others
```

- `r` → read
- `w` → write
- `-` → that permission is not granted

For example, `rw- r-- r--` means owner has read/write, while group and others have read.

### Owner and group

After the link count, the listing shows the file owner and group owner.

### Size

The size is shown in bytes by default.

`ls -h` makes sizes easier to read:

```bash
ls -lh
```

### Modification time

The date and time show when the entry was last modified. This is what `ls -t` uses for time-based sorting.

### Hard-link count

The number after the permissions is the hard-link count. A file with two hard-link names has a count of `2`.

## 3. Determining a File's Type with `file`

`file` examines an object and reports what type of data it detects.

```bash
file README.md
```

Example:

```text
README.md: Unicode text, UTF-8 text
```

It can also inspect directories:

```bash
file README.md chapters
```

The filename extension is not the same thing as the detected file type; `file` examines the data rather than simply assuming the type from the extension.

## 4. Viewing File Contents with `less`

`less` lets you view text files interactively, one screen at a time.

```bash
less README.md
```

Useful controls:

| Key | Action |
|---|---|
| `Space` | Next page |
| `b` | Previous page |
| `q` | Quit |

`(END)` means you have reached the end of the file.

## 5. Guided Tour of the Filesystem

Linux presents a single filesystem tree beginning at `/`, the root directory.

Some useful locations:

```text
/
├── home/
├── usr/
│   └── bin/
├── etc/
├── var/
└── tmp/
```

- `/` → root of the filesystem hierarchy
- `/home` → users' home directories
- `/usr/bin` → many executable programs
- `/etc` → system configuration files and related data
- `/var` → variable/changing system data
- `/tmp` → temporary files

The exact contents vary between Linux systems.

## 6. Symbolic Links

A symbolic link is a filesystem object that points to another pathname.

Example:

```text
lrwxrwxrwx 1 user user 9 Sep 7 10:51 readme-link -> README.md
```

- `l` → symbolic link
- `readme-link` → link name
- `-> README.md` → target pathname

Create one:

```bash
ln -s README.md readme-link
```

A symbolic link does not contain the target's data; it refers to the target by pathname.

If the target is deleted, the symbolic link can remain but becomes broken:

```text
readme-link -> README.md
                   X
```

Restoring the target makes the link usable again.

## 7. Hard Links

A hard link is another directory entry referring to the same inode.

Display an inode with:

```bash
ls -i README.md
```

Create a hard link:

```bash
ln README.md readme-hardlink
```

Compare them:

```bash
ls -li README.md readme-hardlink
```

Both names should show the same inode number.

Conceptually:

```text
README.md       ─┐
                 ├──→ inode → file data
readme-hardlink ─┘
```

The hard-link count increases because another name now refers to the same inode.

If one hard-link name is deleted, another hard-link name can remain usable:

```text
readme-hardlink ───→ inode → file data
```

The link count decreases accordingly.

## 8. Symbolic Link vs Hard Link

| | Symbolic link | Hard link |
|---|---|---|
| Refers to | A pathname | The same inode |
| Own filesystem object | Yes | No separate inode for the linked file data |
| Can become broken when target name is deleted | Yes | No, if another hard-link name remains |
| `ls -l` appearance | Starts with `l` and shows `->` | Looks like the regular file |
| Inode relationship | Different from target | Same inode |

Mental model:

```text
Symbolic:
link ─────→ pathname ─────→ file

Hard:
name A ───┐
          ├────→ same inode ───→ data
name B ───┘
```

## 9. Practical Microtasks

Inspect directories:

```bash
ls ~
ls /usr
ls ~ /usr
```

Explore `ls`:

```bash
ls -l
ls -lt
ls -lt --reverse
ls -a
ls -A
ls -lh
ls -lS
ls -lrS
ls -F
ls -ld chapters
```

Inspect long format:

```bash
ls -l README.md
ls -li README.md
```

Determine type:

```bash
file README.md
file README.md chapters
```

View text:

```bash
less README.md
```

Practice navigation with `Space`, `b`, and `q`.

Practice symbolic links:

```bash
ln -s README.md readme-link
ls -l readme-link
```

Practice hard links:

```bash
ln README.md readme-hardlink
ls -li README.md readme-hardlink
```

Clean up practice links:

```bash
rm readme-link readme-hardlink
```

## 10. Interview Takeaways

You should be able to explain:

1. Command, option, and argument.
2. `-l`, `-a`, `-A`, `-d`, `-F`, `-h`, `-r`, `-S`, and `-t`.
3. Why `ls -lrS` gives smallest-to-largest ordering.
4. The major fields in `ls -l`.
5. The difference between the file-type `-` and `-` inside permission groups.
6. The conceptual role of an inode.
7. What `file` does.
8. Why `less` is useful.
9. The broad purpose of `/`, `/home`, `/usr/bin`, `/etc`, `/var`, and `/tmp`.
10. The difference between symbolic and hard links.
11. Why a symbolic link can become broken.
12. Why a hard link can remain usable after another hard-link name is deleted.
13. Why two hard links can have the same inode number.

## Chapter 3 Completion Checklist

- [x] More Fun with `ls`
- [x] Options and arguments
- [x] Long-format output
- [x] `file`
- [x] `less`
- [x] Filesystem guided tour
- [x] Symbolic links
- [x] Hard links
- [x] Practical exercises
- [x] Interview round

### Chapter boundary

This chapter ends with **Exploring the System**. Topics such as wildcards and creating, copying, moving, and removing files and directories belong to later chapters and are intentionally not included here.
