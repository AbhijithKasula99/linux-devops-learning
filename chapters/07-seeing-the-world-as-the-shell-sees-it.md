# Chapter 7 — Seeing the World as the Shell Sees It

## What this chapter is about

Bash performs **expansions** and other interpretation before it executes a command.

Mental model:

```text
You type
   ↓
Bash interprets / expands
   ↓
Actual command is produced
   ↓
Command executes
```

## 1. Pathname Expansion

The `*` and `?` characters can be used as pathname patterns.

```bash
echo *
echo *.txt
echo ?
echo ???.txt
```

- `*` matches zero or more characters.
- `?` matches exactly one character.
- The patterns match **existing pathnames**.
- If a pattern has no match, Bash leaves the pattern unchanged by default.

Example:

```text
*.txt → existing .txt pathnames
```

Pathname expansion happens before the command runs.

## 2. Tilde Expansion

`~` is shorthand for the current user's home directory.

```bash
echo ~
echo ~/linux-devops-learning/
```

Example:

```text
~ → /home/abhijith
```

## 3. Arithmetic Expansion

Bash can perform arithmetic using:

```bash
$((expression))
```

Examples:

```bash
echo $((5 + 3))
echo $((10 * 4))
echo $((20 / 5))
```

Results:

```text
8
40
4
```

## 4. Brace Expansion

Brace expansion generates text.

```bash
echo {1..5}
echo {a..e}
echo file{1..3}.txt
```

Results:

```text
1 2 3 4 5
a b c d e
file1.txt file2.txt file3.txt
```

### Brace expansion vs pathname expansion

```text
*.txt
```

looks at the filesystem for matching existing pathnames.

```text
file{1..3}.txt
```

generates text; the files do not need to exist.

## 5. Parameter Expansion

A parameter can be expanded with `$name`.

```bash
name="Linux"
echo "$name"
echo "Learning $name"
```

Results:

```text
Linux
Learning Linux
```

`${name}` is another form of parameter expansion.

## 6. Command Substitution

Command substitution inserts the output of one command into another command.

Syntax:

```bash
$(command)
```

Example:

```bash
echo "Today is $(date)"
```

Bash runs `date`, takes its output, and inserts it into the `echo` command.

Command substitution can also contain a pipeline:

```bash
echo "There are $(ls | wc -l) entries here"
```

The pipeline runs first, its output is substituted, and then the outer command executes.

## 7. Quoting

Quoting changes how Bash interprets characters.

### Double quotes

```bash
echo "*"
echo "$(pwd)"
echo "$name"
```

Double quotes prevent pathname expansion and other word-splitting-related interpretation, but allow parameter, arithmetic, and command substitution.

Example:

```bash
echo "$(pwd)"
```

performs command substitution.

### Single quotes

```bash
echo '*'
echo '$(pwd)'
echo '${name}'
```

Single quotes preserve the enclosed text literally.

Example:

```text
'$(pwd)' → $(pwd)
```

No command substitution occurs.

## 8. Escaping Characters

A backslash escapes the next character.

```bash
echo \*
echo \?
echo \"hello\"
```

Results:

```text
*
?
"hello"
```

For example, `\*` prevents `*` from being interpreted as a pathname expansion pattern.

## 9. Backslash Escape Sequences

With `echo -e`, common escape sequences include:

```text
\n → newline
\t → tab
```

Examples:

```bash
echo -e "one\ntwo"
echo -e "one\ttwo"
```

## Key Takeaways

```text
* / ?          → pathname expansion
~              → home-directory expansion
$((...))       → arithmetic expansion
{...}          → brace expansion
$name          → parameter expansion
$(...)         → command substitution
"..."          → double quotes
'...'          → single quotes
\character     → escape the character
```

The central idea of the chapter is that Bash performs these expansions before carrying out the command.

## Interview Lessons

- `*` expands to matching **pathnames**, not file contents.
- Brace expansion generates text; it does not inspect the filesystem.
- Command substitution runs a command and inserts its output.
- Double quotes still allow parameter, arithmetic, and command substitution.
- Single quotes treat enclosed text literally.
- A backslash can prevent a character from being interpreted specially.

## Practice Commands Used

```bash
echo *
echo *.txt
echo ?
echo ???.txt
touch abc.txt ab.txt
echo c*
echo ~
echo ~/linux-devops-learning/
echo $((5 + 3))
echo $((10 * 4))
echo {1..5}
echo {a..e}
echo file{1..3}.txt
echo "Today is $(date)"
echo "I am currently in: $(pwd)"
echo "There are $(ls | wc -l) entries here"
echo "*"
echo '?'
echo "$(pwd)"
echo '$(pwd)'
echo \*
echo \?
echo \"hello\"
echo -e "one\ntwo"
echo -e "one\ttwo"
```

## Chapter Status

**Completed — interview passed (8/8).**
