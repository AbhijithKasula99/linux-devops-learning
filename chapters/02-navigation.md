# Chapter 2 — Navigation

## Goal

Learn how Linux organizes its filesystem and how to move through it from the command line.

---

## 1. Understanding the Filesystem Tree

Linux organizes files in a **hierarchical directory structure** — a tree of directories that can contain files and other directories.

The first directory is the **root directory**:

```text
/
```

Linux uses a single filesystem tree. Storage devices are mounted at points within that tree.

Mental model:

```text
/
├── home
│   └── abhijith
│       └── linux-devops-learning
├── etc
├── var
└── tmp
```

You are always "standing" inside one directory.

---

## 2. Current Working Directory

The directory you are currently in is your **current working directory**.

Use:

```bash
pwd
```

`pwd` = **print working directory**

Example:

```bash
pwd
```

Output:

```text
/home/abhijith/linux-devops-learning
```

When a terminal session starts, the current working directory is normally the user's home directory.

---

## 3. Listing a Directory

Use:

```bash
ls
```

`ls` lists the contents of a directory.

Example:

```bash
ls
```

You can also specify a directory:

```bash
ls chapters
```

**Chapter 2 scope:** basic directory listing only. More advanced `ls` functionality belongs to Chapter 3.

---

## 4. Changing Directories

Use:

```bash
cd <pathname>
```

`cd` = **change directory**

Example:

```bash
cd chapters
```

Check where you are:

```bash
pwd
```

Go to the parent directory:

```bash
cd ..
```

---

## 5. Absolute Pathnames

An **absolute pathname** starts from the root directory `/`.

Example:

```bash
cd /usr/bin
```

The path is interpreted from `/`:

```text
/
└── usr
    └── bin
```

Absolute paths do not depend on your current working directory.

---

## 6. Relative Pathnames

A **relative pathname** starts from your current working directory.

Suppose you are here:

```text
/home/abhijith/linux-devops-learning
```

Then:

```bash
cd chapters
```

means:

```text
current directory
└── chapters
```

You can also explicitly use:

```bash
cd ./chapters
```

In normal cases, `./` can be omitted.

---

## 7. `.` — Current Directory

A single dot:

```text
.
```

means **the current working directory**.

For example:

```bash
cd ./chapters
```

means: from the current directory, enter `chapters`.

---

## 8. `..` — Parent Directory

Two dots:

```text
..
```

mean **the parent directory**.

If you are here:

```text
/home/abhijith/linux-devops-learning/chapters
```

then:

```bash
cd ..
```

takes you to:

```text
/home/abhijith/linux-devops-learning
```

You can use `..` multiple times:

```bash
cd ../..
```

Each `..` moves one level upward.

---

## 9. `~` — Home Directory

The tilde:

```text
~
```

represents the current user's **home directory**.

For example:

```bash
cd ~
```

takes you to your home directory.

You can also use it as part of a path:

```bash
cd ~/linux-devops-learning
```

---

## 10. Useful `cd` Shortcuts

### `cd`

With no pathname:

```bash
cd
```

takes you to your home directory.

### `cd -`

```bash
cd -
```

takes you to the **previous working directory**.

It is useful for moving back and forth between two directories.

Example:

```text
chapters
   ↓ cd -
linux-devops-learning
   ↓ cd -
chapters
```

### `cd ~user_name`

The book also documents:

```bash
cd ~user_name
```

as a way to change to another user's home directory.

For example:

```bash
cd ~bob
```

would refer to Bob's home directory.

---

## 11. Important Filename Rules

### Hidden files

Filenames beginning with `.` are hidden from a normal:

```bash
ls
```

listing.

Examples:

```text
.git
.bashrc
```

To include hidden files:

```bash
ls -a
```

Chapter 2 introduces `-a` specifically for showing hidden files. More extensive `ls` options belong to Chapter 3.

### Case sensitivity

Linux filenames and commands are **case-sensitive**.

These are different names:

```text
file1
File1
FILE1
```

### File extensions

Linux does not use file extensions to determine what a file is.

For example, `.txt`, `.md`, and `.jpg` are part of a filename; the extension itself does not determine the file's contents.

Applications may nevertheless use extensions for their own purposes.

### Filename recommendations

Linux supports long filenames and filenames containing spaces, but the book recommends avoiding spaces and limiting punctuation in files you create.

Prefer:

```text
my_notes
my-notes
my.notes
```

instead of:

```text
my notes
```

---

## 12. Absolute vs Relative — Quick Comparison

| Type | Starts from | Example |
|---|---|---|
| Absolute path | Root `/` | `/home/abhijith/linux-devops-learning` |
| Relative path | Current directory | `chapters` |
| Current directory | `.` | `./chapters` |
| Parent directory | `..` | `../etc` |
| Home directory | `~` | `~/linux-devops-learning` |

**Absolute:** start at `/`.

**Relative:** start where you are now.

---

# Hands-On Practice

## Exercise 1 — Identify your location

```bash
pwd
```

## Exercise 2 — Relative navigation

```bash
cd ~/linux-devops-learning
cd chapters
pwd
```

## Exercise 3 — Absolute navigation

```bash
cd /home/abhijith/linux-devops-learning
pwd
```

## Exercise 4 — Parent directory

```bash
cd chapters
cd ..
pwd
```

## Exercise 5 — Current directory

```bash
cd ./chapters
pwd
```

## Exercise 6 — Home directory

```bash
cd ~
pwd
```

## Exercise 7 — Previous directory

```bash
cd ~/linux-devops-learning
cd chapters
cd -
pwd
cd -
pwd
```

Observe how `cd -` switches between the two previous working directories.

## Exercise 8 — Explore the filesystem tree

From `/`:

```bash
cd /
ls
```

Then visit:

```bash
cd /home
pwd
```

```bash
cd /etc
pwd
```

```bash
cd /var
pwd
```

```bash
cd /tmp
pwd
```

---

# Micro-Challenges

### Challenge 1

You are here:

```text
/home/abhijith/linux-devops-learning/chapters
```

Reach:

```text
/home/abhijith/linux-devops-learning
```

without using an absolute path.

```bash
cd ..
```

### Challenge 2

From:

```text
/home/abhijith/linux-devops-learning
```

reach:

```text
/home/abhijith/linux-devops-learning/chapters
```

using a relative path.

```bash
cd chapters
```

### Challenge 3

From:

```text
/home/abhijith/linux-devops-learning/chapters
```

reach your home directory using `~`.

```bash
cd ~
```

### Challenge 4

Move between these two directories using only `cd -`:

```text
/home/abhijith/linux-devops-learning
/home/abhijith/linux-devops-learning/chapters
```

---

# Interview Questions

1. What is the difference between an absolute pathname and a relative pathname?
2. What does `/` represent in the Linux filesystem?
3. What is the current working directory?
4. What does `pwd` do?
5. What does `cd ..` do?
6. What is the difference between `.` and `..`?
7. What does `~` represent?
8. What happens when you run `cd` without an argument?
9. What does `cd -` do?
10. Why can `cd chapters` and `cd ./chapters` produce the same result?
11. Why doesn't normal `ls` show `.git`?
12. Are `file1` and `File1` the same filename in Linux?
13. Does Linux determine a file's type from its filename extension?
14. If you are in `/home/abhijith/linux-devops-learning/chapters`, what directory does `../` refer to?

---

# Chapter 2 Completion Checklist

- [x] Filesystem tree
- [x] Root directory `/`
- [x] Current working directory
- [x] `pwd`
- [x] Basic `ls`
- [x] `cd`
- [x] Absolute paths
- [x] Relative paths
- [x] `.`
- [x] `..`
- [x] `~`
- [x] `cd -`
- [x] Hidden files
- [x] Case sensitivity
- [x] Filename rules
- [x] Chapter 2 / Chapter 3 boundary maintained

---

# Chapter Summary

Chapter 2 teaches how to **navigate the Linux filesystem**.

The essential mental model is:

```text
Filesystem
    ↓
Current working directory
    ↓
Pathname
    ↓
cd
```

Key commands and symbols:

```bash
pwd
ls
cd
cd ..
cd -
cd ~
.
..
~
/
```

The most important distinction:

```text
Absolute path → starts from /
Relative path → starts from where you currently are
```

**Chapter 2 complete.**

Chapter 3 will begin system exploration and introduce advanced `ls` usage, options and arguments, long format, `file`, `less`, and related topics.
