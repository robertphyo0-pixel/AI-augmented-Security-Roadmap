## 1. inspect_environment.sh
#!/usr/bin/env bash

echo "Current shell: $SHELL"
echo
echo "PATH:"
echo "$PATH"
echo 
echo "Location of ls:"
which ls
echo 
echo "Location of python3:"
which python3

## 2. file_inspector.sh
#!/usr/bin/env bash

FILE="$1"

if [[ -z "$FILE" ]]; then
	echo "Usage: $0 <file>"
	exit 1
fi

echo "=== File Information ==="
stat "$FILE"

echo
echo "=== Inode ==="
ls -li "$FILE"

You press Enter
         │
        ▼
Zsh reads your command
         │
        ▼
Parses:
Program = ./arguments.sh
Argument = file
         │
        ▼
Kernel asked to execute
         │
        ▼
Kernel checks:
✓ File exists
✓ Execute permission
         │
        ▼
Kernel creates a new process
         │
        ▼
Bash interpreter starts
         │
        ▼
$0 = ./arguments.sh
$1 = file
$# = 1
         │
        ▼
Bash expands variables
         │
        ▼
echo receives the expanded text
         │
        ▼
Output appears in your terminal
         │
        ▼
Script exits with a status code
