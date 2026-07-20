
## Objectives

By the end of this lesson, you should be able to:

- Think about software before writing code.
- Break a problem into responsibilities.
- Design functions with a single responsibility.
- Model data before implementation.
- Understand how professionals approach building tools.
- Apply this process to our `security-toolkit`.
# The Engineering Problem

Imagine your manager says:

> "We need a tool that inventories files on employee laptops."

Your first instinct might be:

> "I'll start coding."

Professional engineers usually don't.

Instead, they ask questions.

---

## Step 1 — What Problem Are We Solving?

Not:

> Write Python.

Instead:

> Build a tool that inventories files.

The language is almost irrelevant at this stage.

---

## Step 2 — What Does Success Look Like?

Suppose the tool should produce:

```
{
    "path": "/home/alice/report.pdf",
    "name": "report.pdf",
    "size": 18502,
    "sha256": "..."
}
```

Immediately, we know the output before writing code.

---

## Step 3 — What Inputs Exist?

Let's brainstorm.

Possible inputs:

- A single file
- A directory
- Hash algorithm
- Output file
- Recursive or not
- Ignore hidden files?
- Ignore extensions?
- Maximum file size?

Notice something?

We haven't written a single line of Python.

Yet we've already made progress.

---

## Step 4 — What Data Do We Need?

This is where many beginners jump straight to dictionaries.

Instead, engineers ask:

> **What is the thing my program is working with?**

Our program works with **files**.

A file has:

- path
- filename
- size
- hash
- extension

That's a **data model**.

We'll later represent it using a `dataclass`, but first we identify the concept.

---

## Step 5 — Responsibilities

Now comes the most important part.

Imagine one huge function:

```
def main():
```

Inside it:

- Ask user
- Scan folders
- Read files
- Calculate hashes
- Save JSON
- Print progress
- Handle errors
- Build report

Can it work?

Yes.

Should we do it?

Absolutely not.

---

## Professional software is built by splitting responsibilities.

Example:

```
collect_files()

Only finds files.
```

```
calculate_hash()

Only calculates hashes.
```

```
save_json()

Only writes JSON.
```

```
print_summary()

Only prints information.
```

Each function has one job.

# Mental Model

Imagine a restaurant.

Would the chef:

- take reservations
- cook food
- wash dishes
- collect payment
- clean the tables

all at once?

No.

Each person has one responsibility.

Functions work the same way.

---

# Engineering Principle #1

> A function should have **one clear reason to change**.

This idea comes from the Single Responsibility Principle (SRP), one of the SOLID design principles. We won't dive into SOLID yet, but you'll see this principle throughout professional code.

---

# 3. Designing Our First Toolkit Module

Let's design the `hash` module.

Not code.

Design.

What responsibilities do we need?

Here's one possible decomposition:

```
main()
│
├── get_user_input()
│
├── collect_files()
│
├── calculate_hash()
│
├── build_report()
│
└── save_json()
```

Notice that `main()` doesn't do the work itself. It coordinates the work.

This distinction is important:

- **Coordinator functions** orchestrate the workflow.
- **Worker functions** perform specific tasks.

---

# 4. Professional Discussion

Imagine six months from now your manager asks:

> "Can we also export CSV?"

If your program is well-designed:

```
save_json()
```

stays exactly as it is.

You simply add:

```
save_csv()
```

Everything else keeps working.

Good design makes change easier.

---

# 5. Cybersecurity Application

This same pattern appears everywhere in security engineering.

## IOC Scanner

```
collect_files()

↓

calculate_hash()

↓

compare_with_iocs()

↓

generate_report()
```

---

## Port Scanner

```
load_targets()

↓

scan_ports()

↓

analyze_results()

↓

save_report()
```

---

## Log Parser

```
read_log()

↓

parse_lines()

↓

extract_events()

↓

generate_summary()
```

Different tools.

Same engineering thinking.

# Notes to Save

### Engineering Habit #1

Never start with code.

Start with the problem.

---

### Engineering Habit #2

Identify:

- Inputs
- Outputs
- Data
- Responsibilities
- Errors

before implementation.

---

### Engineering Habit #3

Functions should have one clear responsibility.

---

### Engineering Habit #4

`main()` should coordinate the workflow, not perform every task itself.

# AI Engineering Tip

Suppose you ask AI:

> Build a file hasher.

Most people stop there.

A better workflow is:

1. Design it yourself.
2. Write the function list.
3. Decide the responsibilities.
4. Then ask AI:

> "Here's my design. Critique it. What responsibilities are missing? Which functions violate single responsibility? What edge cases haven't I considered?"

Notice:

You're asking AI to review engineering.

Not replace engineering.

That difference is enormous.

# The Biggest Misconception About Software Design

Many people imagine that senior engineers sit down and design a perfect architecture from day one.

That almost never happens.

A typical real-world process looks more like this:

```
Version 1
    ↓
Users request new features
    ↓
Refactor
    ↓
Architecture improves
    ↓
More features
    ↓
Refactor again
    ↓
Eventually a mature codebase
```

Good architecture **evolves**.

The skill is not predicting everything—it's making change inexpensive.

# This Is Called Incremental Design

Professional software usually evolves like this.

```
Small project

↓

Slightly larger

↓

Refactor

↓

Larger

↓

Refactor

↓

Production system
```

Not:

```
Predict the next five years perfectly.
```

# The Folder Structure 

This is where we're heading over many months—not next week.

```
security-toolkit/
│
├── pyproject.toml          # Project configuration
├── README.md
├── LICENSE
├── .gitignore
│
├── src/
│   └── security_toolkit/
│       ├── hash/
│       ├── files/
│       ├── scanner/
│       ├── network/
│       ├── logs/
│       ├── web/
│       ├── windows/
│       ├── linux/
│       ├── cloud/
│       ├── report/
│       ├── plugins/
│       └── utils/
│
├── tests/
│
└── docs/
```

Notice something?

Almost every folder represents a **domain**, not a Python concept.

We organize by **what the code does**, not by the language features it uses.