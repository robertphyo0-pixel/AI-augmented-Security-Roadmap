# Lesson Objectives

By the end of today's lesson you'll understand:

- What a permission actually is
- Who checks permissions (Shell? Kernel?)
- Why read, write, and execute mean different things for **files** and **directories**
- Why "Permission denied" happens
- The first part of Linux's security model
# Mental Model

Imagine a secure office building.
Someone walks to a door.
Who decides whether they can enter?
The receptionist?
No.
The security guard.
Linux works the same way.

```
Program

↓

Kernel

↓

Permission Check

↓

Allowed / Denied
```

The kernel is the **security guard**.
Every program must go through it.
Even `sudo` eventually asks the kernel.
This is why malware cannot simply "ignore" Linux permissions.

# Linux Internals

Let's follow this command:

```
cat report.txt
```

What actually happens?

```
You

↓

Shell

↓

Find cat

↓

Kernel starts cat

↓

cat asks:

"Please open report.txt"

↓

Kernel checks permissions

↓

Allowed?

↓

Yes

↓

Kernel returns bytes

↓

cat prints bytes
```

Notice something.
Does `cat` decide whether it is allowed to read?
No.
The kernel decides.
`cat` simply asks.
This is the same API that Python, `vim`, `grep`, and your own programs use.

## Another concept

```
echo hello
```

There is **no file** involved.

So no bytes are read from disk.

The flow is actually:

```
You
        │
        ▼
Bash parses

Command = echo

Argument = hello
        │
        ▼
Kernel creates a process
        │
        ▼
echo starts
        │
        ▼
echo prints "hello"
        │
        ▼
echo exits with 0
```

No filesystem access happened.

Let's connect everything we've learned.

```
You type:

echo "$HOME"
```

Bash performs:

```
Read line

↓

Tokenize

↓

Expand variables

↓

Locate command

↓

Ask kernel to execute

↓

Program runs
```

Notice something.

The shell does **a lot** before the kernel gets involved.

# Security Application

Suppose you're writing a malware scanner in Python.
It walks through:

```
/home

/var

/etc
```

Eventually it reaches:

```
/root
```

Your program tries to open:

```
/root/secret.txt
```

What happens?
Does Python decide?
No.
Python asks the kernel to open the file.
The kernel checks the permissions of the current user and either returns a file handle or denies the request.
That's why understanding Linux permissions is essential for both offensive and defensive security.

# AI Workflow

Here's how an experienced engineer would use AI with today's topic:
❌ Bad prompt:

> My script says "Permission denied." Fix it.

✅ Better prompt:

> My Bash script gets "Permission denied" when reading `/var/log/auth.log`. Here's the script, the output of `ls -l`, `id`, and `stat`. Help me determine whether the failure is caused by file permissions, directory permissions, ownership, or something else.

Notice the difference?
The second prompt includes **evidence**, not just the error message.


|Symbol|Meaning|
|---|---|
|`-`|Regular file|
|`d`|Directory|
|`l`|Symbolic link|
|`c`|Character device|
|`b`|Block device|
|`p`|Named pipe (FIFO)|
|`s`|Unix domain socket|

Notice something.
Directories...
Links...
Sockets...
Devices...
Linux considers all of these to be **filesystem objects** with different types.
That leads to one of the most famous Unix ideas:

> **"Everything is a file."**

## Here's Something Cool

You saw:

```
-rw-rw-r--
```

The first character tells you **what kind of object** it is.

The remaining nine characters tell you **what permissions apply**.

So instead of thinking:

```
-rw-rw-r--
```

Think:

```
-

↓

Object Type
```

and

```
rw-

rw-

r--

↓

Permissions
```

Those are two separate pieces of information.

# Interview Mode 🔥

Imagine in the interview.

---

**Interviewer:**

Why does this command:

```
ls test_dir
```

show

```
secret.txt
```

while

```
ls -l test_dir
```

shows

```
?????????
```

Don't mention memorized permission rules.

Explain it using:

- directory entries
- inode
- pathname resolution
- kernel

Use those four terms.

#Answer: 
`ls` can read the directory entries because it has read (`r`) permission on the directory. However, when `ls -l` tries to retrieve metadata for each file, the kernel must traverse the directory while resolving the pathname. Because the execute (`x`) permission is missing, pathname resolution fails, so the kernel cannot access the file's inode to retrieve its metadata.

# Let's Draw It

## `ls test_dir`

```
ls

↓

Kernel

↓

Read directory entries

↓

secret.txt

↓

Print filename
```

No inode metadata is needed.

---

## `ls -l test_dir`

```
ls

↓

Read directory entries

↓

secret.txt

↓

Now get metadata

↓

Kernel resolves:

test_dir/secret.txt

↓

Needs execute permission

↓

❌ No execute

↓

Cannot access inode

↓

Print ?????????
```

Notice the difference?

The first command only needs the **directory entries**.

The second command needs the **inode metadata**.

# AI Workflow (Real World)

This is also how I'd want you to use AI in the future.

Instead of asking:

> "Why does `ls -l` show question marks?"

Ask something like:

> "I can list filenames in a directory, but `ls -l` shows `?????????` for each file. The directory lacks execute permission. Explain how pathname resolution, directory traversal, and inode access cause this behavior."

Notice the difference?

You're describing the **system behavior**, not just the symptom.

That leads to much better explanations and helps you verify AI's answer against your own mental model.

# Let's Build the Permission Matrix


## Directory has only `r`

```
dr--------
```

Can you:

- List filenames? ✅ Yes
- Open a known file? ❌ No

Why?

You can read the directory entries...

...but you cannot traverse the directory.

---

## Directory has only `x`

```
d--x------
```

Can you:

- List filenames? ❌ No
- Open a known file? ✅ Yes (if the file's own permissions allow it)

Why?

You cannot read the directory entries...

...but you **can traverse** the directory if you already know the path.

---

## Directory has `r` + `x`

```
dr-x------
```

Can you:

- List filenames? ✅
- Open known files? ✅

This is the most common read-only directory configuration.