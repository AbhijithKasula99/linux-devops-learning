# Chapter 1 — What Is the Shell?

> **Goal:** Understand the terminal, shell, prompt, command execution, history, and a few basic system commands.
>
> **Rule:** Spend less time reading and more time experimenting in the terminal.

---

## 1. Terminal vs Shell

These are different things.

```text
You
 │
 │ type commands
 ▼
Terminal
 │
 │ provides input/output
 ▼
Shell
 │
 │ interprets commands
 ▼
Operating System
 │
 ▼
Result
```

### Terminal

A **terminal emulator** provides a way to interact with the shell.

### Shell

A **shell** is a program that accepts commands and passes them to the operating system.

**Bash** is the primary shell used throughout this book.

---

## 2. Understand the Prompt

A typical prompt may look like:

```text
abhijith@AbhisLenovo:~/linux-devops-learning$
```

You don't need to memorize every part yet.

The important part for now is:

```text
$
```

`$` normally indicates that the shell is ready to accept a command from a regular user.

A `#` prompt generally indicates a shell running with superuser privileges.

**Do not type the `$`.**

For example, if you see:

```text
$ date
```

type only:

```bash
date
```

---

# 3. First Commands

Run each command separately.

### Print text

```bash
echo "Hello Linux"
```

Observe the output.

---

### Display the current date and time

```bash
date
```

---

### Display system uptime

```bash
uptime
```

---

### Try a command that does not exist

```bash
this_command_does_not_exist
```

You should see something similar to:

```text
this_command_does_not_exist: command not found
```

This means the shell could not find a command by that name.

### Experiment

Try another made-up command:

```bash
foobar
```

Observe the error.

**Don't just read the error. Learn to recognize what it is telling you.**

---

# 4. Command History

The shell remembers commands you have entered.

Display recent commands:

```bash
history | tail -5
```

You may see something like:

```text
2014 echo "Hello Linux"
2015 date
2016 uptime
2017 this_command_does_not_exist
2018 history | tail -5
```

The numbers are history entries.

---

## Navigate History

Press:

```text
↑
```

The **Up Arrow** recalls the previous command.

Press:

```text
↓
```

to move forward through commands you previously recalled.

### Experiment

1. Press `↑` several times.
2. Observe which commands appear.
3. Press `↓` to move forward again.
4. Press `Enter` when a command is recalled if you want to execute it.

---

# 5. Cancel a Command Line

Press:

```text
Ctrl+C
```

This interrupts/cancels the current command line or running command.

### Experiment

Start typing without pressing Enter:

```bash
echo "this will not run
```

Then press:

```text
Ctrl+C
```

Observe what happens.

---

# 6. Moving Around the Command Line

You can edit a command before executing it.

Useful keys:

| Key | Action |
|---|---|
| `←` | Move cursor left |
| `→` | Move cursor right |
| `Home` | Move to beginning |
| `End` | Move to end |
| `Backspace` | Delete character before cursor |
| `Ctrl+C` | Cancel current input |

### Experiment

Type:

```bash
echo "linux is fun"
```

Before pressing Enter:

1. Move the cursor left and right.
2. Use `Home`.
3. Use `End`.
4. Move into the text.
5. Use Backspace to remove a character.
6. Correct the command.
7. Press Enter.

Expected result:

```text
linux is fun
```

**Don't worry if a particular key, such as Delete, behaves differently on your keyboard. The important skill is learning to edit commands before execution.**

---

# 7. Commands Are Exact

The shell executes what you give it.

Try:

```bash
echo "Linux is fun"
```

Then:

```bash
echo "linux is fun"
```

Notice the difference.

The shell does not assume what you meant.

---

# 8. Basic System Information

These commands give you a quick view of system state.

### Current date/time

```bash
date
```

### Disk/filesystem usage

```bash
df
```

### Memory usage

```bash
free
```

### System uptime/load

```bash
uptime
```

Run them together:

```bash
date
df
free
uptime
```

**For now, focus on what each command is for.**

Do not try to memorize every column in the output.

Detailed filesystem, memory, and process concepts will be covered later.

---

# 9. WSL Observation

If you are learning Linux through WSL, `df` may show Windows-mounted filesystems such as:

```text
/mnt/c
```

This exposes the Windows `C:` drive inside the Linux environment.

You may also see several other mounted filesystems.

**Do not worry about mounts yet.**

Filesystem structure and mounting will be covered later.

---

# 10. Ending a Shell Session

You can end the current shell with:

```bash
exit
```

You can also send an EOF using:

```text
Ctrl+D
```

Closing the terminal emulator also ends the terminal session.

---

# 11. Microtask — Explore Before Moving On

Run the following commands yourself:

```bash
echo "Hello Linux"
date
uptime
df
free
```

Then deliberately run:

```bash
linux_does_not_have_this_command
```

Now inspect your history:

```bash
history | tail -10
```

Use `↑` and `↓` to navigate through the commands you just ran.

Finally, type:

```bash
echo "linux is fun"
```

and execute it.

---

# 12. What You Should Be Able to Explain

Before leaving this chapter, you should be able to explain these without looking them up:

- What is a **terminal emulator**?
- What is a **shell**?
- What is **Bash**?
- What does `$` in the prompt indicate?
- What does `command not found` mean?
- How do you recall a previous command?
- How do you move forward/backward through command history?
- How do you cancel the current command line?
- How do you display the date?
- How do you check system uptime?
- How do you inspect disk/filesystem usage?
- How do you inspect memory usage?
- How do you exit the shell?

---

# 13. Mental Model

Keep this model:

```text
Terminal
   ↓
Shell
   ↓
Operating System
   ↓
Command result
```

And remember:

```text
Terminal ≠ Shell
```

The terminal provides the interface.

The shell interprets and executes commands.

---

# 14. Interview Check

Answer these **without looking above**.

### Q1

What is a shell?

### Q2

What is the difference between a terminal emulator and a shell?

### Q3

Conceptually, what happens when you type:

```bash
date
```

and press Enter?

### Q4

What does `$` at the end of a shell prompt indicate?

### Q5

If you run:

```bash
foobar
```

and receive:

```text
foobar: command not found
```

what does that tell you?

---

## Chapter Complete When

You can:

1. Open a terminal.
2. Recognize the shell prompt.
3. Run commands.
4. Read basic command output.
5. Recognize `command not found`.
6. Recall and edit previous commands.
7. Cancel input with `Ctrl+C`.
8. Inspect basic system information.
9. Exit the shell.
10. Explain the terminal → shell → operating system relationship.

**Next:** Chapter 2 — Navigation.
