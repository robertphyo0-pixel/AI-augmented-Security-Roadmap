# The Most Common Misconception

Most people think a file looks like this:

```
notes.txt
┌─────────────────┐
│ hello           │
└─────────────────┘
```

This is **not** how Linux sees it.

Linux sees three different things.

---

# Mental Model

Imagine a hotel.

There are three separate things:

```
Room Number
↓

Room Information

↓

Room Contents
```

These are different.

For example:

```
Room 305

↓

Occupied
Two beds
Ocean view

↓

Suitcases
Laptop
Clothes
```

Linux files work similarly.

---

## Part 1 — Filename

```
notes.txt
```

This is just a **name**.

Nothing more.

It exists inside a directory.

---

## Part 2 — Inode

Your file has:

```
3547254
```

That number identifies the file internally.

Linux cares far more about the inode than the filename.

The inode stores metadata like:

- owner
- permissions
- timestamps
- size
- number of links
- pointers to the data blocks

Notice again:

**The inode does not store the filename.**

---

## Part 3 — Data Blocks

Finally:

```
hello
```

lives here:

```
Disk
│
├── Block 1
├── Block 2
├── Block 3
└── ...
```

The inode simply says:

> "The contents of this file are stored in blocks 1021, 1022, and 1023."

---

# The Complete Picture

Here's a more accurate representation:

```
Directory
│
├── notes.txt
│        │
│        ▼
│   inode 3547254
│        │
│        ├── Owner
│        ├── Permissions
│        ├── Size
│        ├── Time
│        └── Points to...
│
▼
Data Blocks
┌───────────────┐
│ hello         │
└───────────────┘
```

This is one of the most important diagrams in Linux.

Everything else builds on it.

Why file copy completed so fast?
The filesystem updated the directory entry. The inode and data blocks remained the same, so no data copy was needed.
# Notes

Keep these distinctions clear:

- **Filename**: A human-friendly name stored in a directory.
- **Directory**: A mapping from names to inode numbers.
- **Inode**: Metadata plus pointers to where the data lives.
- **Data blocks**: The actual bytes of the file.

# The Rule Every Linux Engineer Should Know

> **A file is not deleted until its link count reaches zero and no process is still using it.**

That sentence explains:

- Hard links
- Deleted log files
- Running deleted executables
- Disk space not being freed
- Many forensic investigations