# How Bash Reads a Line

One of the biggest mental models in Bash is this:

> **The shell reads a line before it executes anything.**

Its first job is to answer:

> **"Is this an assignment, or is this a command?"**

---

## Case 1: No spaces

```
NAME=Robert
```

Bash recognizes a special pattern:

```
word=value
```

It says:

> "Ah, this is a variable assignment."

No command is executed.

It simply stores:

```
NAME
↓

Robert
```

---

## Case 2: Spaces

Now look at:

```
NAME = Robert
```

Bash splits the line into **tokens** using spaces.

It sees:

```
Token 1 → NAME
Token 2 → =
Token 3 → Robert
```

Now it asks:

> **"What's the command?"**

Answer:

```
NAME
```

So internally it behaves something like:

```
Run command:

NAME

Arguments:

=

Robert
```

Then it searches your `PATH`.

Remember this diagram from Lesson 1?

```
Shell

↓

Search PATH

↓

No executable named NAME

↓

command not found
```

Everything connects together.

# This Is Why Your Error Makes Sense

The shell wasn't saying:

> "You wrote the assignment wrong."

It was saying:

> "I'm trying to execute a program called `NAME`, but I can't find one."

That's a huge difference.

# Linux Internals Connection

Notice something fascinating.

We've now built a chain of understanding:

```
You type a line

↓

Shell parses it

↓

If it's an assignment
    ↓
Store variable

If it's a command
    ↓
Find executable

↓

Ask kernel to execute

↓

Program runs
```

Once you understand the parser, many "weird" Bash behaviors suddenly become logical.

## #Wording

Instead of saying:

> "It considers it as a command."

Say:

> **"The shell parses the first token as the command name."**

Or even more precisely:

> **"Because of the spaces, Bash no longer recognizes a variable assignment. It tokenizes the line and treats `NAME` as the command to execute."**

That wording is much closer to what you'd hear in a systems programming or Linux interview.

# This Is the Foundation of Shell Parsing

Everything else you'll learn later builds on these same steps.

Eventually you'll see things like:

```
echo *.txt
```

or

```
echo $(pwd)
```

or

```
echo "$HOME"
```

They all follow the same pattern:

```
Read line

↓

Tokenize

↓

Expand

↓

Execute
```

We're building the foundation first.

---

# Security Engineering Connection

Now let's connect this to something you'll absolutely encounter.

Imagine a developer writes:

```
rm $FILE
```

Suppose:

```
FILE="important document.txt"
```

What does Bash do?

It expands the variable **before** running `rm`.

Without quotes, the expanded value is then split into separate arguments because it contains a space.

Conceptually:

```
rm important document.txt
```

Now `rm` receives **two** filenames instead of one:

- `important`
- `document.txt`

This is why experienced Bash programmers almost always write:

```
rm "$FILE"
```

The quotes tell Bash to treat the expanded value as a single argument, even if it contains spaces.

# The Big Picture

Let's connect every lesson we've done so far.

Imagine this command:

```
cat "$HOME/report.txt"
```

What actually happens?

```
1. You press Enter
        │
        ▼
2. Bash reads the line
        │
        ▼
3. Bash tokenizes it

cat

"$HOME/report.txt"
        │
        ▼
4. Bash expands $HOME

/home/phantom/report.txt
        │
        ▼
5. Bash finds cat

/usr/bin/cat
        │
        ▼
6. Bash asks kernel

"Please execute /usr/bin/cat"
        │
        ▼
7. Kernel creates a process
        │
        ▼
8. cat runs
        │
        ▼
9. cat asks kernel

"Open this file"
        │
        ▼
10. Kernel checks permissions
        │
        ▼
11. Kernel reads inode
        │
        ▼
12. Kernel reads data blocks
        │
        ▼
13. Kernel returns bytes
        │
        ▼
14. cat prints bytes
        │
        ▼
15. cat exits
        │
        ▼
16. Shell stores exit code in $?
```

**This** is the mental model I wanted you to build.

## Question:

If another user has **read permission on `passwords.txt`** but **no permission on the directory**, can they read the file?

The kernel first resolves the file path by traversing the directories. If it cannot access one of the directories in the path, it cannot reach the file's inode, so access is denied before the file permissions are even checked.