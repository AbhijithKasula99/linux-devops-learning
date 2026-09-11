# Chapter 10 — Processes

## What is a Process?

A **process** is a running instance of a program. The kernel maintains information about processes, including their process ID (PID), memory, readiness, and ownership.

```text
Program          Process
-------          -------
bash      →      running bash
python    →      running python
nginx     →      running nginx
```

- **Program** = instructions stored on disk.
- **Process** = those instructions currently running.
- Every process has a **PID**.

```bash
echo $$
```

`$$` gives the PID of the current Bash shell.

---

## Viewing Processes

### `ps`

`ps` gives a snapshot of processes.

```bash
ps
```

Typical columns:

```text
PID   TTY   TIME   CMD
```

### `ps x`

```bash
ps x
```

Shows your running processes, including processes without a terminal.

### `ps aux`

```bash
ps aux
```

Shows processes belonging to every user and provides more information.

Important columns:

| Column | Meaning |
|---|---|
| `USER` | Owner of the process |
| `PID` | Process ID |
| `%CPU` | CPU usage percentage |
| `%MEM` | Memory usage percentage |
| `VSZ` | Virtual memory size |
| `RSS` | Resident set size — physical RAM used by the process |
| `TTY` | Terminal |
| `STAT` | Process state |
| `START` | Start time |
| `TIME` | CPU time consumed |
| `COMMAND` | Command that launched the process |

### `ps -ef`

```bash
ps -ef
```

Useful for viewing processes together with their **PPID** (parent process ID).

### Inspecting one process

```bash
ps -o pid,stat,cmd -p <PID>
```

---

## Process States

The `STAT` column describes process state and additional characteristics.

Common values:

- `R` — Running
- `S` — Sleeping
- `s` — Session leader
- `+` — Foreground process group

For example:

```text
Ss  → sleeping + session leader
R+  → running + foreground process group
```

Process state is a snapshot and can change quickly.

---

## `top` — Dynamic Process Monitoring

`ps` provides a snapshot. `top` provides a continuously updating view of system activity.

```bash
top
```

`top` displays:

- system summary
- tasks/processes
- CPU usage
- memory usage
- process activity

Press:

```text
q
```

to quit `top`.

---

## Foreground and Background Processes

### Foreground

```bash
sleep 60
```

Bash waits for the process to finish, so the prompt does not return.

### Background

```bash
sleep 60 &
```

The `&` places the command in the background.

Example:

```text
[1] 672
```

- `[1]` — Bash job number
- `672` — PID

### `jobs`

```bash
jobs
```

Shows jobs launched from the current shell.

### `fg`

```bash
fg %1
```

Brings job 1 to the foreground.

### `Ctrl+Z`

Suspends a foreground process.

### `bg`

```bash
bg %1
```

Resumes a stopped job in the background.

Basic flow:

```text
Foreground
   │
   ├── Ctrl+Z → Stopped
   │
   └── Ctrl+C → Interrupted
   │
Stopped
   └── bg %1 → Running in background
   │
Background
   └── fg %1 → Foreground
```

---

## Process Priority and `nice`

Linux has a process attribute called **niceness**, which affects scheduling priority.

```bash
nice
```

Default observed:

```text
0
```

Start a process with a different nice value:

```bash
nice -n 10 sleep 60 &
```

A **higher nice value means lower scheduling priority**.

### `renice`

Changes the niceness of an already-running process:

```bash
renice -n <value> <PID>
```

---

## Signals

The `kill` command does not literally mean "kill" a process. It **sends a signal** to it.

### `kill`

```bash
kill <PID>
```

If no signal is specified, `TERM` (SIGTERM) is sent by default.

It can also use a shell jobspec:

```bash
kill %1
```

Important signals practiced:

```text
SIGTERM → request graceful termination
SIGSTOP → stop/suspend a process
SIGCONT → continue a stopped process
SIGKILL → forcefully terminate a process
```

Examples:

```bash
kill <PID>
kill -STOP <PID>
kill -CONT <PID>
kill -KILL <PID>
```

### SIGTERM vs SIGKILL

Normal approach:

```text
SIGTERM
   ↓
allow graceful termination / cleanup
   ↓
still running?
   ↓ yes
SIGKILL
```

SIGKILL is a last resort because the process does not get an opportunity to perform normal cleanup.

### Keyboard signals

- `Ctrl+C` sends an interrupt signal (`SIGINT`).
- `Ctrl+Z` sends a terminal stop signal (`SIGTSTP`).

---

## `killall`

`killall` sends signals to multiple processes matching a specified program name or username.

Example:

```bash
killall sleep
```

If two `sleep` processes are running, both can be targeted.

---

## Process Hierarchy

Processes have **parent-child relationships**.

### PID and PPID

```bash
ps -o pid,ppid,cmd
```

Example:

```text
PID   PPID   CMD
327   326    -bash
1095  327    ps -o pid,ppid,cmd
```

This means:

```text
326
└── 327 bash
    └── 1095 ps
```

- **PID** — identifies the process itself.
- **PPID** — identifies its parent process.

PPID is useful when debugging because it helps determine which process launched another process.

### `pstree`

```bash
pstree -p
```

Shows parent-child relationships as a tree.

---

## Identifying Processes by Name

### `pgrep`

```bash
pgrep sleep
```

Returns the PID(s) of matching processes.

A practical workflow:

```text
Know process name
       ↓
pgrep sleep
       ↓
get PID
       ↓
kill PID
```

---

## Shutting Down the System

The book explains that shutting down involves orderly termination of processes and filesystem housekeeping.

Commands discussed include:

```text
halt
poweroff
reboot
shutdown
```

Examples from the book:

```bash
sudo reboot
sudo shutdown -h now
sudo shutdown -r now
```

Do not run these casually in a learning WSL terminal.

---

## Other Process-Related Commands

| Command | Purpose |
|---|---|
| `pstree` | Process list arranged as a parent-child tree |
| `vmstat` | Snapshot of system resource usage including memory, swap, and disk I/O |
| `xload` | Graphical system-load display |
| `tload` | Terminal-based load display |

Example:

```bash
vmstat 5
```

Stop continuous output with `Ctrl+C`.

---

# Practical Command Reference

```bash
echo $$

ps
ps x
ps aux
ps -ef

ps -o pid,stat,cmd -p <PID>
ps -o pid,ppid,cmd

top

jobs
command &
fg %1
bg %1

kill <PID>
kill -STOP <PID>
kill -CONT <PID>
kill -KILL <PID>

killall <name>
pgrep <name>

nice
nice -n 10 <command>
renice -n <value> <PID>

pstree -p
```

---

# Mental Model

```text
PROGRAM
   │
   └── becomes a PROCESS
           │
           ├── PID
           ├── PPID
           ├── STATE
           ├── CPU / MEMORY usage
           └── PRIORITY (niceness)
```

Process-control model:

```text
Process
   │
   ├── SIGTERM → graceful termination request
   ├── SIGSTOP → stop
   ├── SIGCONT → continue
   └── SIGKILL → force termination
```

---

# Chapter 10 Interview Takeaways

### CPU-heavy process

Start with:

```bash
top
```

or:

```bash
ps aux
```

Find the PID, inspect the process, investigate its parent if needed, and then decide whether termination is appropriate.

### PID vs PPID

- PID identifies the process.
- PPID identifies its parent.
- PPID helps trace process ancestry.

### Graceful vs forced termination

```bash
kill <PID>
```

sends SIGTERM by default.

```bash
kill -KILL <PID>
```

sends SIGKILL.

Prefer SIGTERM first because it allows graceful cleanup.

### Foreground vs background

Foreground processes occupy the shell until they finish or are stopped.

Background processes allow Bash to return the prompt and continue accepting commands.

### `nice`

Higher nice value → lower scheduling priority.

### `VSZ` vs `RSS`

- VSZ → virtual memory size.
- RSS → physical RAM currently used by the process.

### Process states

```text
R → Running
S → Sleeping
s → Session leader
+ → Foreground process group
```

---

# Chapter 10 Status

**Completed:** Processes

**Interview score:** 9/10

**Practical skills demonstrated:**

- `ps`
- `ps x`
- `ps aux`
- `ps -ef`
- `top`
- `jobs`
- `fg`
- `bg`
- foreground/background execution
- `Ctrl+C`
- `Ctrl+Z`
- `nice`
- `renice`
- `kill`
- signals
- `killall`
- `pgrep`
- PID / PPID
- `pstree`
- process states
