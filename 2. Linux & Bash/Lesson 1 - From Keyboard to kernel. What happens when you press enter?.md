## Flow 
You
  │
 ▼
Type command
  │
 ▼
Shell receives input
  │
 ▼
Shell parses command
  │
 ▼
Shell expands variables
  │
 ▼
Shell checks built-ins
  │
 ▼
Shell searches PATH
  │
 ▼
Shell asks kernel
  │
 ▼
Kernel creates process
  │
 ▼
Program executes
  │
 ▼
Program exits
  │
 ▼
Shell displays prompt

## Example 

Let's follow this command:

```
ls -l
```

Step by step.

## Step 1 — Keyboard Input

You type:

```
ls -l
```

The shell receives those characters.

Nothing has executed yet.

---

## Step 2 — Parsing

The shell analyzes your input.

It recognizes:

Command:

```
ls
```

Argument:

```
-l
```

If you had typed:

```
echo $HOME
```

The shell would first replace `$HOME` with its value **before** running `echo`.

That's called **expansion**.

---

## Step 3 — Is it a Built-in?

The shell asks:

> "Is `ls` one of my built-in commands?"

Some commands are built into the shell itself.

Examples:

```
cd
pwd
export
alias
history
```

Why?

Let's think about `cd`.

If `cd` were a normal program, it would change _its own_ current directory, then exit. Your shell would still be in the old directory.

Because the shell's own state needs to change, `cd` must be a shell built-in.

This is an example of understanding _why_, not just _what_.

---

## Step 4 — Search the `PATH`

If it's not a built-in, the shell searches the directories listed in the `PATH` environment variable.

You can see yours with:

```
echo "$PATH"
```

It might look like:

```
/usr/local/bin:/usr/bin:/bin:/usr/sbin
```

The shell checks each directory in order:

```
Does /usr/local/bin/ls exist?
No.

Does /usr/bin/ls exist?
Yes.
```

Now the shell knows which executable file to use.

---

## Step 5 — Ask the Kernel

This is the critical point.

The shell does **not** execute the program itself.

Instead, it asks the kernel:

> "Please create a new process using `/usr/bin/ls`."

The kernel then:

- creates a new process
- allocates memory
- loads the executable into memory
- sets up the process environment
- schedules it to run on the CPU

The shell waits for the result (for a foreground command).

---

## Step 6 — Program Runs

Now `ls` is running as its own process.

It asks the kernel questions like:

- "Open this directory."
- "Read the directory entries."
- "Get each file's metadata."

Notice something important:

`ls` never talks directly to the disk.

It asks the kernel, and the kernel interacts with the filesystem and hardware.

---

## Step 7 — Output

`ls` writes its output to **standard output (stdout)**.

By default, the shell has connected stdout to your terminal, so you see the results on screen.

---

## Step 8 — Program Ends

`ls` exits.

The kernel cleans up the process.

Control returns to the shell.

The shell prints another prompt and waits for your next command.

# Why Security Engineers Care

This isn't just theory. It explains many real-world situations.

- **`command not found`**: The shell couldn't find the executable in `PATH`.
- **`Permission denied`**: The kernel refused to execute the file or access a resource.
- **Unexpected program behavior**: The shell may have expanded variables or wildcards differently than expected.
- **Malware detection**: Investigators check which executable was actually run and how it was located.
- **Privilege escalation**: Misconfigured `PATH` values or writable directories can allow a malicious executable to be run instead of the intended one.

Understanding _where_ a failure occurs helps you troubleshoot efficiently.

# Notes

Remember these key distinctions:

- **Shell**: Reads commands, performs expansions, checks built-ins, searches `PATH`, and asks the kernel to start programs.
- **Kernel**: Manages hardware, processes, memory, filesystems, networking, and enforces permissions.
- **Programs**: Run as separate processes created by the kernel.

A helpful way to think about it is:

> **The shell decides _what_ should run. The kernel decides _how_ it runs safely on the system.**

The shell parses the command, resolves what to execute (built-in, alias, function, or external command), and asks the kernel to execute it. The kernel creates the process and enforces permissions.

eg: cat file.txt

```
Shell

↓

Kernel starts cat

↓

cat asks kernel

↓

Kernel reads disk

↓

Kernel gives bytes

↓

cat prints bytes
```

