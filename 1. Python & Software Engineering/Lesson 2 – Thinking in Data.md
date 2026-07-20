## Objective

Today's lesson is about one question:

> **How should my program represent information?**

Every program is fundamentally:

```
Input
    ↓
Data
    ↓
Processing
    ↓
Output
```

If you choose the wrong data representation, the rest of the program becomes harder to build and maintain.

# Mental Model

Imagine you work for a shipping company.

A package has:

- tracking number
- destination
- weight
- sender

When the company builds software, they don't think:

> "Let's use a dictionary."

They think:

> "A package has these properties."

Exactly the same thing happens in cybersecurity.

Our toolkit doesn't manipulate "dictionaries."

It manipulates:

- Files
- Network hosts
- Ports
- Users
- Processes
- IP addresses
- Log events
- Vulnerabilities

Those are **real-world entities**.

The code is simply how we represent them.

---

## Designing Our First Data Model

Our current project hashes files.

So ask yourself:

> **What information describes a file for our program?**

Not everything about a file.

Only what our program cares about.

For Version 1, maybe:

```
File
──────
name

path

size

sha256
```

Notice what we **didn't** include:

- creation date
- owner
- permissions
- MIME type
- timestamps
- entropy

Those might matter later, but not yet.

A good engineer models only what is needed today while leaving room to extend tomorrow.

---

##  How Could We Represent It?

Imagine we have a file:

```
report.pdf
```

There are several options.

### Option 1 — List

```
[
    "report.pdf",
    "/home/user/report.pdf",
    12345,
    "abc123..."
]
```

Question:

Which value is the size?

You have to remember it's the third element.

That's fragile.

---

### Option 2 — Tuple

```
(
    "report.pdf",
    "/home/user/report.pdf",
    12345,
    "abc123..."
)
```

Slightly better for immutable data, but still positional.

---

### Option 3 — Dictionary

```
{
    "name": "report.pdf",
    "path": "/home/user/report.pdf",
    "size": 12345,
    "sha256": "abc123..."
}
```

Now it's obvious.

This is why you instinctively chose a dictionary during the diagnostic.

That was a reasonable choice.

---

### Option 4 — Dataclass

Don't worry about the syntax yet.

Conceptually:

```
FileRecord
──────────
name

path

size

sha256
```

This says:

> "This is a FileRecord."

Instead of:

> "This is a dictionary with some keys."

We'll learn dataclasses later because they're extremely common in modern Python.

---

##  Engineering Thinking

Here's something senior engineers constantly ask:

> **What does this variable represent?**

Compare these:

```
data = {}
```

versus

```
file_record = {}
```

The second immediately communicates intent.

Good naming reduces the mental effort required to read code.

---

##  Cybersecurity Connection

Let's think ahead.

Imagine we're building a file inventory for 10,000 machines.

Each machine scans thousands of files.

Every file becomes one record.

Then we compare hashes against known malware IOCs.

Notice the pipeline:

```
Disk
   ↓
File Records
   ↓
Hash Comparison
   ↓
Threat Detection
   ↓
Security Report
```

Everything starts with a good data model.

The same idea will appear later with:

- Network scan results
- HTTP responses
- DNS records
- Windows event logs
- Linux audit logs
- Cloud resources

# Engineering Discussion

I want to introduce another habit.

When adding a new field, don't ask:

> "Can I add it?"

Ask:

> "Who is responsible for providing it?"

For example:

- `collect_files()` knows the path.
- `calculate_hash()` knows the hash.
- `build_report()` combines everything into a report.

Each function contributes only the data it owns.

This reinforces the single-responsibility principle we discussed in Lesson 1.

# Notes to Save

### Engineering Habit #5

Think in **real-world entities**, not Python types.

Instead of:

> "I have a dictionary."

Think:

> "I have a file record."

---

### Engineering Habit #6

Choose data structures based on the meaning of the data, not just convenience.

---

### Engineering Habit #7

Every field should have a clear owner.

Don't let every function modify every piece of data.

# Engineering Discussion

## Data Lives Longer Than Code

Suppose we write:

```
calculate_hash()
```

Maybe next year we replace it with a faster implementation.

The function changes.

But this doesn't change:

```
File

↓

Name

Path

Size

Hash
```

Your **data model** is often much more stable than your implementation.

That's why experienced engineers spend so much time thinking about data.

If the data model is good:

- changing algorithms is easier,
- adding features is easier,
- testing is easier,
- reporting is easier.

---

# AI Engineering Tip

If you ask AI:

> "Write a file scanner."

You'll usually get code immediately.

Instead, try this:

> "Help me identify the entities, fields, and responsibilities before we write code."

You'll get a design discussion instead of a code dump. That leads to better software and makes it much easier for you to evaluate the implementation later.

## Fields

Fields are **data**.

For example:

```
{
    "name": "report.pdf",
    "path": "...",
    "size": 1200
}
```

The fields are:

- `name`
- `path`
- `size`

These describe **what the object is**.

---

## Functions

Functions describe **what the program does**.

Examples:

```
collect_files()

calculate_hash()

save_json()

build_report()
```

These perform actions.

# This Difference Is Extremely Important

Think about a real person.

You have:

```
Name

Age

Address

Phone
```

Those are **fields**.

What can a person do?

```
Walk

Talk

Drive

Eat
```

Those are **actions**.

Programming works exactly the same way.

A File has fields.

Functions perform actions on files.