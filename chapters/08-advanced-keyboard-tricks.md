# Chapter 8 — Advanced Keyboard Tricks

> **The Linux Command Line, 3rd Edition — William Shotts**
>
> Practical notes and exercises from Chapter 8.

## What This Chapter Is About

Bash provides a set of keyboard features that make command-line work faster and more efficient. Instead of repeatedly retyping commands, you can edit the current command, complete names, navigate history, search history, reuse previous commands, and record a shell session.

Bash uses **Readline** for command-line editing.

---

## 1. Command Line Editing

### Cursor Movement

| Shortcut | Action |
|---|---|
| `Ctrl+A` | Move to the beginning of the line |
| `Ctrl+E` | Move to the end of the line |
| `Ctrl+F` | Move forward one character |
| `Ctrl+B` | Move backward one character |
| `Alt+F` | Move forward one word |
| `Alt+B` | Move backward one word |
| `Ctrl+L` | Clear the screen and move the cursor to the top-left |

**Mental model:**

```text
Ctrl+A                         Ctrl+E
   ↓                              ↓
[ beginning → command text → end ]
```

---

## 2. Modifying Text

| Shortcut | Action |
|---|---|
| `Ctrl+D` | Delete the character at the cursor |
| `Ctrl+T` | Transpose the current character with the preceding character |
| `Alt+T` | Transpose the current word with the preceding word |
| `Alt+L` | Lowercase characters from the cursor to the end of the word |
| `Alt+U` | Uppercase characters from the cursor to the end of the word |

### Example

```bash
echo ab
```

Place the cursor after `a` and use `Ctrl+T`:

```bash
echo ba
```

---

## 3. Killing and Yanking Text

In Readline terminology:

- **Kill** = cut text from the command line.
- **Yank** = paste killed text back.
- Killed text is kept temporarily in the **kill-ring**.

| Shortcut | Action |
|---|---|
| `Ctrl+K` | Kill from cursor to the end of the line |
| `Ctrl+U` | Kill from cursor to the beginning of the line |
| `Alt+D` | Kill from cursor to the end of the current word |
| `Alt+Backspace` | Kill from cursor to the beginning of the current word |
| `Ctrl+Y` | Yank the killed text back |

### Example

```bash
echo hello world
```

Move the cursor before `world` and press `Ctrl+K`. The text after the cursor is killed. Press `Ctrl+Y` to restore it.

---

## 4. The Meta Key

In Bash/Readline, **Alt** generally acts as the **Meta** key.

Some terminals may handle `Alt` differently. In those environments, `Esc` can sometimes be used to produce the same Meta function.

---

## 5. Command Completion

Press **Tab** to ask Bash to complete what you have typed.

The most common use is pathname completion:

```bash
cd /u<Tab>
```

which can become:

```bash
cd /usr/
```

Tab can also complete:

- Pathnames
- Commands
- Variables
- Usernames
- Hostnames

When multiple completions are possible, pressing **Tab twice** displays the available possibilities.

### Useful completion behavior

```text
One Tab      → complete when possible
Two Tabs     → show possible completions
```

Bash also supports programmable completion, but its implementation is outside the scope of this chapter's practical exercises.

---

## 6. Using History

Bash maintains a history of commands.

The history is normally stored in:

```bash
~/.bash_history
```

View history:

```bash
history
```

Browse it with:

```bash
history | less
```

You can also filter it, for example:

```bash
history | grep /usr/bin
```

### Navigate History

| Shortcut | Action |
|---|---|
| `↑` / `Ctrl+P` | Previous history entry |
| `↓` / `Ctrl+N` | Next history entry |
| `Alt-<` | Beginning of history |
| `Alt->` | End of history / current line |
| `Ctrl+O` | Execute current history item and advance |

---

## 7. Searching History

### Reverse Incremental Search

Press:

```text
Ctrl+R
```

Then type part of a previous command.

Example:

```text
(reverse-i-search)`docker': ...
```

Press **Ctrl+R again** to search for the next older match.

Useful controls:

- `Enter` → execute the found command
- `Ctrl+J` → copy the found command to the current line
- `Ctrl+G` / `Ctrl+C` → quit the search

This is especially useful when the history contains many commands.

---

## 8. History Expansion

History expansion uses `!` notation.

| Syntax | Meaning |
|---|---|
| `!!` | Previous command |
| `!number` | Command with that history number |
| `!string` | Most recent command beginning with `string` |
| `!?string` | Most recent command containing `string` |

### Examples

```bash
echo first
!!
```

`!!` expands to:

```bash
echo first
```

Using a history number:

```bash
!1975
```

Using a command prefix:

```bash
!echo
```

Using text contained in a command:

```bash
!?banana
```

### Preview Before Executing

History expansion can execute the expanded command immediately.

Use:

```bash
!echo:p
```

to print the expansion without executing it.

**Practical safety rule:** when you are unsure what `!` expansion will produce, preview it with `:p`.

---

## 9. Recording a Shell Session

The `script` program records a shell session, including what is displayed.

Start a recording:

```bash
script session.txt
```

Work normally, then end the recording with:

```bash
exit
```

The output is saved in `session.txt`.

### Important Practical Lesson

The destination must be writable by your user.

For example, attempting:

```bash
cd /usr
script session.txt
```

may fail with:

```text
Permission denied
```

because a normal user generally cannot create arbitrary files directly in `/usr`.

A writable location such as the home directory is appropriate:

```bash
cd ~
script session.txt
```

---

## 10. Practical Exercises Completed

During this chapter we practiced:

- Moving to the beginning/end of a command with `Ctrl+A` and `Ctrl+E`
- Moving by characters with `Ctrl+F` and `Ctrl+B`
- Moving by words with `Alt+F` and `Alt+B`
- Deleting characters with `Ctrl+D`
- Transposing characters and words
- Killing and yanking text
- Tab completion
- Viewing and navigating command history
- Reverse history search with `Ctrl+R`
- History expansion with `!!`, `!number`, `!string`, and `!?string`
- Safely previewing history expansion with `:p`
- Recording a shell session with `script`

---

## 11. Interview Takeaways

### Cursor editing

**Q:** Move to the beginning of a long command?

```text
Ctrl+A
```

**Q:** Kill everything from the cursor to the end?

```text
Ctrl+K
```

**Q:** Restore killed text?

```text
Ctrl+Y
```

### History

**Q:** Interactively search backward?

```text
Ctrl+R
```

**Q:** Execute the previous command?

```bash
!!
```

**Q:** Execute history entry 1975?

```bash
!1975
```

**Q:** Execute the most recent command containing `docker`?

```bash
!?docker
```

**Q:** Preview a history expansion without executing it?

```bash
!?docker:p
```

### Completion

**Q:** Ask Bash to complete a command or pathname?

```text
Tab
```

---

## Chapter 8 Summary

The command line becomes much more powerful when you stop treating it as a place where commands are merely typed.

Think of Readline as a **command-line editing toolkit**:

```text
EDIT
  ↓
COMPLETE
  ↓
REUSE HISTORY
  ↓
SEARCH
  ↓
PREVIEW
  ↓
EXECUTE
```

These skills reduce typing, make experimentation safer, and become especially valuable when working with long DevOps commands.

---

## Boundary

**Chapter 8 ends here.**

Covered:

- Command Line Editing
- Cursor Movement
- Modifying Text
- Cutting and Pasting (Killing and Yanking) Text
- Command Completion
- Using History
- Searching History
- History Expansion
- `script`
- Summing Up

**Next chapter: Chapter 9 — Permissions.**
