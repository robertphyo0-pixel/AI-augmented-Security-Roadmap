## Objectives

By the end of today's lesson you will understand:

- ✅ What a module really is
- ✅ What a package really is
- ✅ Why `import` exists
- ✅ How Python projects grow
- ✅ How professionals organize code
- ✅ Why project organization matters in cybersecurity tools
# Mental Model

Imagine you're writing a book.

Would you put everything into one page?

```
Chapter 1
Chapter 2
Chapter 3
Chapter 4
...
Chapter 20
```

Of course not.

You split it into chapters.

**Python modules are exactly the same idea.**

A module is simply:

> **A Python file that groups related code together.**

Nothing magical.

# Engineering Principle #2

> **Group related things together.**

Not because Python says so.

Because humans need to read the code.

Remember:

> **Code is written once but read many times.**

# Think of it as your First Project

Let's pretend today is Day 1.

We only support hashing.

Should we create this?

```
security-toolkit/
├── scanner/
├── cloud/
├── windows/
├── linux/
├── web/
├── report/
├── plugins/
├── utils/
├── inventory/
├── network/
├── intel/
└── hash/
```

No.

That would be designing for features we don't have yet.

Instead, we start small.

---

## Version 1

```
security-toolkit/
│
├── main.py
├── hash.py
├── report.py
└── README.md
```

Let's discuss why each file exists.

---

### `main.py`

Responsibility:

> Coordinate everything.

It should answer questions like:

- What does the user want?
- Which functions need to run?
- In what order?

It should **not** calculate hashes itself.

---

### `hash.py`

Responsibility:

Everything related to hashing.

Eventually it might contain:

```
calculate_hash()

verify_hash()

supported_algorithms()

compare_hashes()
```

Notice the theme.

Everything belongs to **hashing**.

---

### `report.py`

Responsibility:

Turning data into output.

Maybe today:

- JSON

Later:

- CSV
- HTML
- Markdown
- PDF

Still one domain:

**Reporting.**

---

# 4. What Is an Import?

Now we can answer **why** `import` exists.

Suppose `calculate_hash()` lives in `hash.py`.

How does `main.py` use it?

That's exactly why Python has imports.

Conceptually:

```
main.py
        │
        ▼
needs calculate_hash()
        │
        ▼
ask hash.py for it
```

So an import is not just syntax.

It's how modules **communicate**.

---

# Mental Model

Imagine an office.

There is an HR department.

There is an IT department.

There is a Finance department.

HR doesn't rewrite Finance's work.

It asks Finance for information.

Modules behave the same way.

---

# 5. What Is a Package?

A package is simply:

> A directory that groups related modules.

Imagine this:

```
hash/
    calculate.py
    verify.py
    algorithms.py
```

Now we have:

```
hash
```

which contains multiple modules.

That's a package.

We'll build packages later, when our project has grown enough to justify them.

---

# 6. Engineering Discussion

Let's imagine two versions of a project.

## Project A

```
main.py

12,000 lines
```

Finding the hashing code:

---

## Project B

```
hash.py

report.py

scanner.py

network.py

web.py
```

Finding the hashing code:

That's why organization matters.

# Cybersecurity Connection

Let's think about a real vulnerability scanner.

Does one file do everything?

No.

It might look like:

```
scanner/
    engine.py
    targets.py
    scheduler.py

report/
    json.py
    html.py

plugins/
    cve.py
    ssl.py
    smb.py

network/
    dns.py
    tcp.py
```

Notice something?

The structure reflects **domains of responsibility**, not Python language features.

That pattern is common in mature security tools.

# AI Engineering

Here's how I want you to start using AI.

❌ Instead of:

> Build me a security toolkit.

Ask:

> I have these modules:
> 
> - `main.py`
> - `hash.py`
> - `report.py`
> 
> Review my architecture.
> 
> Which responsibilities are mixed?
> 
> Which modules violate the Single Responsibility Principle?
> 
> Would this scale if I added five more features?

That's how senior engineers use AI—as a design reviewer rather than just a code generator.

---

# Notes to Save

### Engineering Habit #8

A **module** is a Python file that groups related code.

---

### Engineering Habit #9

A **package** is a directory containing related modules.

---

### Engineering Habit #10

Modules communicate through imports.

---

### Engineering Habit #11

Organize projects by **responsibility**, not by "whatever fits."

## Coupling

Imagine this function:

```
calculate_hash()

↓

writes JSON

↓

prints output

↓

uploads to cloud

↓

emails manager
```

That's **highly coupled**.

Changing one part risks breaking many others.

Now compare:

```
calculate_hash()

↓

returns hash
```

Then:

```
save_json()

↓

writes JSON
```

Then:

```
upload_report()
```

Each piece is independent.

That's **low coupling**.

Low coupling is one of the reasons we split work into focused functions and modules.

---

# Another Important Concept

Let's compare two styles.

## Beginner Thinking

> "I need a function."

## Intermediate Thinking

> "I need several functions."

## Engineer Thinking

> "How do these functions collaborate while staying independent?"

You're beginning to move into that third way of thinking.

---

# AI Engineering Tip

Here's a prompt I would actually use as a senior engineer:

> "Here's my project structure. Evaluate it for cohesion and coupling. Which modules have mixed responsibilities? Which responsibilities should be separated before the project grows?"

Notice I'm not asking AI to generate code.

I'm asking it to review architecture.

That's a much more valuable use of AI.