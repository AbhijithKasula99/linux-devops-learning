# Chapter 6 — Redirection

## Core Idea

I/O means **input/output**. Linux lets us redirect command input and output to files and connect commands through pipelines. The chapter introduces `cat`, `sort`, `uniq`, `grep`, `wc`, `head`, `tail`, and `tee`. fileciteturn10file2L194-L208

## Standard Streams

| Stream | Number | Normal destination/source |
|---|---:|---|
| stdin | 0 | Keyboard |
| stdout | 1 | Terminal |
| stderr | 2 | Terminal |

Normally, command results go to stdout and status/error messages go to stderr. fileciteturn10file2L209-L226

## Output Redirection — `>`

Redirect stdout to a file:

```bash
echo "hello" > output.txt
```

`>` creates the file if needed and **overwrites** existing contents.

```text
command → stdout → file
```

## Append Output — `>>`

Append stdout to a file:

```bash
echo "third" >> output.txt
```

```text
>  → overwrite
>> → append
```

## Input Redirection — `<`

Take stdin from a file:

```bash
cat < output.txt
```

```text
file → stdin → command
```

Normally stdin is attached to the keyboard; redirection changes its source. fileciteturn10file2L217-L226

## Standard Error — `2>`

Redirect stderr to a file:

```bash
ls does-not-exist 2> error.txt
```

The error is written to `error.txt` instead of the terminal.

```text
command → stderr (2) → error.txt
```

stdout and stderr are separate streams. fileciteturn10file2L209-L226

## Redirect stdout and stderr Together — `2>&1`

```bash
ls output.txt does-not-exist > combined.txt 2>&1
```

First stdout is redirected to `combined.txt`. Then `2>&1` sends stderr to the same destination as stdout.

```text
stdout ──┐
         ├──→ combined.txt
stderr ──┘
```

## Pipelines — `|`

A pipeline connects stdout of one command to stdin of another:

```bash
ls | wc
```

```text
ls → stdout → wc → stdout → terminal
```

The `|` operator lets multiple commands work together. fileciteturn10file2L194-L208

### Multiple Commands

```bash
echo -e "banana
apple
banana
cherry
apple" | sort | uniq
```

Flow:

```text
echo → sort → uniq → terminal
```

Result:

```text
apple
banana
cherry
```

## Commands Used with Redirection and Pipelines

### `cat`

Concatenates files and can pass stdin to stdout.

```bash
printf "hello
world
" | cat
```

### `sort`

Sorts lines:

```bash
printf "banana
apple
cherry" | sort
```

### `uniq`

Reports or omits repeated lines. Commonly used after `sort`:

```bash
printf "banana
apple
banana
cherry
apple" | sort | uniq
```

### `grep`

Prints lines matching a pattern:

```bash
printf "apple
banana
apricot
cherry" | grep "ap"
```

Result:

```text
apple
apricot
```

### `wc`

Prints newline, word, and byte counts:

```bash
printf "apple
banana
cherry
" | wc
```

Example:

```text
3  3  20
```

### `head`

Outputs the first part:

```bash
printf "1
2
3
4
5
6
7
8
9
10
" | head -3
```

Result:

```text
1
2
3
```

### `tail`

Outputs the last part:

```bash
printf "1
2
3
4
5
6
7
8
9
10
" | tail -3
```

Result:

```text
8
9
10
```

## `tee`

`tee` reads stdin and writes it to both stdout and a file. fileciteturn10file2L200-L208

```bash
printf "hello
world
" | tee tee-output.txt
```

```text
             ┌──→ file
input → tee ─┤
             └──→ stdout
```

Append with:

```bash
printf "third
" | tee -a tee-output.txt
```

`-a` appends to the file while still sending input to stdout.

## Combined Example

```bash
printf "apple
banana
apple
cherry
banana
" | sort | uniq > fruits.txt
```

Flow:

```text
printf → sort → uniq → fruits.txt
```

Result:

```text
apple
banana
cherry
```

This combines pipelines, sorting, duplicate removal, and output redirection.

## Quick Reference

| Syntax / Command | Purpose |
|---|---|
| `stdin` | Standard input |
| `stdout` | Standard output |
| `stderr` | Standard error |
| `<` | stdin from a file |
| `>` | stdout to file, overwrite |
| `>>` | stdout to file, append |
| `2>` | stderr to file |
| `2>&1` | stderr to stdout's current destination |
| `|` | stdout → stdin |
| `cat` | Concatenate/pass text |
| `sort` | Sort lines |
| `uniq` | Report/omit repeated lines |
| `grep` | Print matching lines |
| `wc` | Count lines, words, bytes |
| `head` | Output beginning |
| `tail` | Output end |
| `tee` | File + stdout |
| `tee -a` | Append to file + stdout |

## Interview Takeaways

- `stdin` = input, `stdout` = normal output, `stderr` = error output.
- `>` overwrites; `>>` appends.
- `<` takes stdin from a file.
- `2>` redirects stderr.
- `2>&1` sends stderr to stdout's current destination.
- `|` connects one command's stdout to another command's stdin.
- Pipelines combine small commands into larger workflows.
- `tee` writes to a file while continuing to stdout.
