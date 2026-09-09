# Chapter 5 — Getting Help

## What Exactly Are Commands?

A command can be one of four kinds:

- **Executable program** — a program stored on the filesystem, such as `cp`.
- **Shell builtin** — a command provided by the shell itself, such as `cd`, `echo`, and `pwd`.
- **Shell function** — a command defined as a shell function. In this chapter, we only need awareness of this type.
- **Alias** — a command shortcut defined from another command, such as `ls`.

### `type`

Use `type` to identify what kind of command Bash will execute.

```bash
type cd
type ls
type cp
type echo
```

Examples:

```text
cd is a shell builtin
ls is aliased to `ls --color=auto'
cp is /usr/bin/cp
echo is a shell builtin
```

If the command is not known:

```bash
type banana
```

Bash reports that it is not found.

## Identifying Commands

### `which`

`which` shows the location of an executable program.

```bash
which cp
```

Example:

```text
/usr/bin/cp
```

`which` does not identify shell builtins such as `cd`.

```bash
which cd
```

This produces no output.

A useful distinction:

- `type` → tells you **what kind of command** Bash will execute.
- `which` → tells you **where an executable program is located**.

## Getting Command Documentation

### `help`

`help` provides help for Bash shell builtins.

```bash
help cd
```

It provides information such as synopsis, description, options, and exit status.

It does not provide help for external programs:

```bash
help cp
```

### `man`

`man` displays a command's manual page.

```bash
man cp
```

Manual pages are primarily **reference documentation**, rather than tutorials.

Common sections:

| Section | Meaning |
|---|---|
| 1 | User commands |
| 2 | Kernel system calls |
| 3 | C library functions |
| 4 | Special files |
| 5 | File formats |
| 6 | Games |
| 7 | Miscellaneous |
| 8 | System administration |

You can specify a section:

```bash
man 5 passwd
```

Here, `5` selects the **file formats** section.

### `apropos`

`apropos` searches manual-page descriptions for a keyword.

```bash
apropos copy
```

### `info`

`info` is an alternative documentation system to `man`.

```bash
info cp
```

Its documentation is organized in a more structured, navigable form.

Think:

- `man` → reference page
- `info` → structured, navigable documentation

### `whatis`

`whatis` gives a one-line description of a manual-page topic.

```bash
whatis cp
```

Example:

```text
cp (1) - copy files and directories
```

## Creating Your Own Commands

### `alias`

An alias creates a shortcut from one command to another command.

```bash
alias ll='ls -l'
```

Now:

```bash
ll
```

runs:

```bash
ls -l
```

See current aliases:

```bash
alias
```

Inspect a particular alias:

```bash
type ll
```

Example:

```text
ll is aliased to `ls -l'
```

## Quick Reference

| Tool | Purpose |
|---|---|
| `type` | Identify the type of command Bash will execute |
| `which` | Locate an executable program |
| `help` | Get help for Bash builtins |
| `man` | Read manual/reference documentation |
| `apropos` | Search manual-page descriptions |
| `info` | Read structured GNU documentation |
| `whatis` | Get a one-line manual-page description |
| `alias` | Create command shortcuts |

## Practical Examples

```bash
type cd
type ls
type cp
type echo
type banana

which cp
which cd

help cd
man cp
man 5 passwd
apropos copy
info cp
whatis cp

alias
alias ll='ls -l'
type ll
ll
```

## Interview Takeaways

- Know the difference between an **executable**, **builtin**, **function**, and **alias**.
- `type` identifies the command type Bash will execute.
- `which` locates executable programs.
- `help` is for Bash builtins.
- `man` is the standard manual/reference system.
- `apropos` searches manual-page descriptions.
- `info` provides structured GNU documentation.
- `whatis` gives a one-line description.
- `man 5 passwd` demonstrates selecting a specific manual section.
- An alias provides a convenient shortcut for another command.
