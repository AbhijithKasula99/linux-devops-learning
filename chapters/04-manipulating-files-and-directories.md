# Chapter 4 — Manipulating Files and Directories

## What this chapter covers

- Wildcards
- Dot files
- `mkdir` — create directories
- `cp` — copy files and directories
- `mv` — move and rename
- `rm` — remove files and directories
- `ln` — create hard and symbolic links
- A combined file-manipulation playground

---

## 1. Wildcards

Wildcards let the shell match filenames using patterns.

### `*`

Matches any number of characters.

```bash
ls report*.txt
```

Can match:

```text
report1.txt
report2.txt
report10.txt
```

### `?`

Matches exactly one character.

```bash
ls report?.txt
```

Matches `report1.txt` and `report2.txt`, but not `report10.txt`.

### Character sets

```bash
ls report[12].txt
```

Matches `report1.txt` and `report2.txt`.

A negated set:

```bash
ls report[!12].txt
```

matches a single character that is not `1` or `2`.

### Important mental model

The shell expands wildcards before the command runs.

```bash
echo report*.txt
```

The shell first expands the pattern, then `echo` receives the resulting filenames.

---

## 2. Dot Files

Files beginning with `.` are normally hidden from a normal `ls`.

```bash
ls -a
```

shows hidden entries.

Wildcard patterns normally do not match hidden files unless the pattern itself begins with `.`.

Useful patterns include:

```bash
.[!.]*
.??*
```

These help avoid accidentally matching the special `.` and `..` entries.

---

## 3. `mkdir` — Create Directories

Create one directory:

```bash
mkdir playground
```

Create several:

```bash
mkdir logs backups configs
```

`mkdir` creates directories but does not change the current working directory.

---

## 4. `cp` — Copy Files and Directories

Copy a file to a new filename:

```bash
cp source.txt copy.txt
```

Copy a file into a directory:

```bash
cp source.txt destination/
```

Copy multiple files:

```bash
cp app.log error.log destination/
```

Copy a directory recursively:

```bash
cp -r destination destination-copy
```

Without `-r`, `cp` will not copy a directory.

### Useful options

```bash
cp -i source.txt copy.txt
```

Prompts before overwriting.

```bash
cp -a source.txt archive-copy.txt
```

Uses archive mode and preserves file attributes where applicable.

---

## 5. `mv` — Move and Rename

Rename:

```bash
mv source.txt renamed.txt
```

Move:

```bash
mv renamed.txt destination/
```

Move and rename:

```bash
mv destination/renamed.txt destination/final.txt
```

Interactive mode:

```bash
mv -i source.txt destination/
```

prompts before overwriting an existing destination.

---

## 6. `rm` — Remove Files and Directories

Remove a file:

```bash
rm file.txt
```

By default, `rm` does not remove directories.

### Recursive

```bash
rm -r directory
```

removes the directory and its contents recursively.

### Interactive

```bash
rm -i file.txt
```

asks for confirmation.

- `y` → remove
- `n` → keep

### Force

```bash
rm -f file.txt
```

removes without prompting and ignores nonexistent files.

Remember:

```text
rm -r directory   → recursive
rm -f file        → force
```

`-f` does not replace `-r` for directory removal.

---

## 7. `ln` — Create Links

### Hard links

Create one:

```bash
ln original.txt hardlink.txt
```

Inspect inode numbers:

```bash
ls -li original.txt hardlink.txt
```

A hard link and the original have the **same inode** and therefore refer to the same underlying file.

Example:

```text
34583 ... 2 ... hardlink.txt
34583 ... 2 ... original.txt
```

Changing data through one name is visible through the other.

Removing one filename does not remove the underlying data while another hard link still exists.

### Symbolic links

Create one:

```bash
ln -s target.txt symlink.txt
```

Inspect it:

```bash
ls -li target.txt symlink.txt
```

A symbolic link has its **own inode** and points to the target by path:

```text
symlink.txt -> target.txt
```

If the target is removed, the symlink can remain but becomes a **dangling/broken symlink**.

### Hard link vs symbolic link

| | Hard link | Symbolic link |
|---|---|---|
| Refers to | Same inode | Target path |
| Inode | Same as target | Separate inode |
| Target removed | Other link still works | Link becomes broken |
| `ls -l` | Normal file entry | Shows `-> target` |

---

## 8. Let's Build a Playground

Create the structure:

```bash
mkdir playground
cd playground
mkdir dir1 dir2
```

Create files:

```bash
touch file1.txt file2.txt
```

Copy a file:

```bash
cp file1.txt dir1/
```

Copy a directory:

```bash
cp -r dir1 dir2/
```

Move and rename:

```bash
mv file2.txt dir1/renamed.txt
```

Create a hard link:

```bash
ln dir1/file1.txt file1-hardlink.txt
```

Verify the shared inode:

```bash
ls -li dir1/file1.txt file1-hardlink.txt
```

Create a symbolic link:

```bash
ln -s dir1/file1.txt file1-symlink.txt
```

Inspect it:

```bash
ls -li dir1/file1.txt file1-symlink.txt
```

Remove the symbolic link:

```bash
rm file1-symlink.txt
```

Remove the hard link:

```bash
rm file1-hardlink.txt
```

Clean up:

```bash
rm dir1/file1.txt
rm -r dir1 dir2
rm file1.txt
```

---

## Key takeaways

- `*` matches any number of characters.
- `?` matches exactly one character.
- Dot files are hidden from normal `ls` output.
- `mkdir` creates directories.
- `cp` copies; `cp -r` copies directories recursively.
- `mv` moves or renames.
- `rm` removes; `rm -r` removes directories recursively.
- `rm -i` asks before removal.
- `rm -f` forces removal without prompting.
- A hard link refers to the same inode.
- A symbolic link has its own inode and stores a path to its target.
- Removing one hard-link name does not remove the data while another hard link remains.
- Removing a symbolic-link target leaves a dangling/broken link.

## Chapter 4 interview checklist

Be able to explain:

1. Why `report?.txt` matches `report1.txt` but not `report10.txt`.
2. The difference between `cp -r` and `mv`.
3. What happens when the original filename of a hard-linked file is removed.
4. What happens when the target of a symbolic link is removed.
5. The purpose of `rm -i`, `rm -r`, and `rm -f`.
6. The fundamental difference between a hard link and a symbolic link.
