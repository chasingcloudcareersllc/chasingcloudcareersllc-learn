---
title: "Shell Scripting"
description: "Automate tasks with Bash scripts — variables, conditionals, loops, pipes, redirects, and cron jobs."
position: 6
icon: "file-code"
---

# Shell Scripting

Once you're comfortable with the Linux terminal, the next step is automating repetitive tasks. Shell scripts let you combine commands into reusable programs that save time and reduce human error.

## What Is Shell Scripting?

A shell script is a file containing a series of commands that the shell executes in sequence. Instead of typing commands one at a time, you write them into a file and run the file. [Bash (Bourne Again Shell)](https://www.gnu.org/software/bash/manual/) is the most common shell on Linux systems and the default on nearly every server you will encounter in production. Writing Bash scripts is how you automate system administration, deployments, data processing, and the dozens of small repetitive tasks that consume engineering time.

## Why It Matters

Automation is at the heart of DevOps and infrastructure work. Before you use tools like Terraform or GitHub Actions, you need to understand the scripting fundamentals they are built on. Shell scripts appear in CI/CD pipelines, container entrypoints, cron jobs, and deployment workflows. When you move into [CI/CD](/learn/foundations/cicd/) and [Infrastructure as Code](/learn/foundations/iac/), every tool you use will either call shell scripts or expect you to write them for custom steps. Mastering shell scripting now gives you a foundation that pays dividends across every section that follows.

## What You'll Learn

- Variables and environment variables
- Conditionals (`if`, `elif`, `else`, `case`)
- Loops (`for`, `while`, `until`)
- Functions and script organization
- Pipes and redirects (`|`, `>`, `>>`, `<`)
- Exit codes and error handling
- Scheduling tasks with `cron`

---

## Your First Script

Every Bash script starts with two things: a shebang line and the commands you want to run.

### The Shebang

The first line of a shell script should be the **shebang** (also called a hashbang):

```bash
#!/bin/bash
```

This tells the operating system which interpreter to use when executing the file. Without it, the system may try to run your script with a different shell, which can produce unexpected behavior. Always include it.

### Creating and Running a Script

Create a file called `hello.sh`:

```bash
#!/bin/bash

echo "Hello, world!"
echo "Today is $(date)"
echo "You are logged in as: $USER"
```

Before you can run it, you need to make it executable:

```bash
chmod +x hello.sh
```

Now run it:

```bash
./hello.sh
```

```
Hello, world!
Today is Sat Feb  7 10:32:15 UTC 2026
You are logged in as: cloudchase
```

The `./` tells the shell to look in the current directory. Without it, the shell searches your `PATH` and will not find the script unless the current directory is in `PATH`.

You can also run a script without making it executable by passing it directly to Bash:

```bash
bash hello.sh
```

This works but is less common. Making scripts executable with `chmod +x` is standard practice because it lets you treat the script like any other command.

> **Try It**: Create `hello.sh` with the content above. Make it executable with `chmod +x hello.sh` and run it with `./hello.sh`. Then modify it to also print the hostname with `echo "Hostname: $(hostname)"` and run it again.

---

## Variables

Variables store values that you reference throughout your script. They eliminate duplication and make scripts configurable.

### Declaring Variables

In Bash, you assign a variable with `=` and **no spaces** around the equals sign:

```bash
#!/bin/bash

name="cloudchase"
greeting="Hello"
count=5

echo "$greeting, $name!"
echo "Count is: $count"
```

```
Hello, cloudchase!
Count is: 5
```

The no-spaces rule is critical. Writing `name = "cloudchase"` (with spaces) causes Bash to interpret `name` as a command and `=` as an argument, resulting in an error.

### Referencing Variables

Use `$VAR` or `${VAR}` to access a variable's value. The curly braces are required when the variable name is adjacent to other text:

```bash
file="report"
echo "$file_final"      # Bash looks for a variable called file_final (empty)
echo "${file}_final"    # Correct: outputs report_final
```

### Quoting Rules

The difference between single quotes, double quotes, and no quotes is one of the most common sources of bugs in shell scripts.

**Double quotes** (`"..."`) allow variable expansion and command substitution:

```bash
name="cloudchase"
echo "Hello, $name"       # Hello, cloudchase
echo "Date: $(date)"      # Date: Sat Feb  7 10:32:15 UTC 2026
```

**Single quotes** (`'...'`) treat everything literally -- no expansion occurs:

```bash
name="cloudchase"
echo 'Hello, $name'       # Hello, $name
echo 'Date: $(date)'      # Date: $(date)
```

**No quotes** work but are dangerous because word splitting and glob expansion can produce unexpected results:

```bash
message="hello   world"
echo $message              # hello world (extra spaces collapsed)
echo "$message"            # hello   world (spaces preserved)
```

The rule of thumb: always double-quote your variables unless you have a specific reason not to. This prevents word splitting, glob expansion, and a whole category of subtle bugs.

### Special Variables

Bash provides several built-in variables that give you information about the script and its arguments:

| Variable | Meaning |
|---|---|
| `$0` | The name of the script |
| `$1`, `$2`, ... | Positional arguments passed to the script |
| `$#` | Number of arguments passed |
| `$@` | All arguments as separate words |
| `$*` | All arguments as a single string |
| `$?` | Exit code of the last command |
| `$$` | Process ID of the current script |
| `$!` | Process ID of the last background command |

Example using positional arguments:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"
echo "All arguments: $@"
```

```bash
./args.sh hello world
```

```
Script name: ./args.sh
First argument: hello
Second argument: world
Total arguments: 2
All arguments: hello world
```

### Exporting Variables

A variable set in a script is local to that script's shell. If the script launches a child process (another script, a program), the child does not see the variable. Use `export` to make it available to child processes:

```bash
export DATABASE_URL="postgres://localhost/mydb"
export LOG_LEVEL="debug"
```

You encountered `export` in the [Linux](/learn/foundations/linux/) section when modifying `PATH`. The same concept applies here: any variable you want child processes to inherit must be exported.

> **Try It**: Create a script called `greet.sh` that takes a name as the first argument and prints `Hello, <name>! You are user number <random>.` Use `$1` for the name and `$RANDOM` for the number. Run it with `./greet.sh cloudchase`.

---

## Reading Input

Scripts can prompt the user for input using the `read` command:

```bash
#!/bin/bash

read -p "Enter your name: " username
read -p "Enter your project: " project

echo "Setting up project '$project' for user '$username'..."
```

```
Enter your name: cloudchase
Enter your project: webapp
Setting up project 'webapp' for user 'cloudchase'...
```

The `-p` flag displays a prompt. The variable name after the prompt stores whatever the user types. You can also read silently (useful for passwords) with `-s`:

```bash
read -sp "Enter password: " password
echo ""  # Add a newline since -s suppresses it
echo "Password received (length: ${#password})"
```

---

## Conditionals

Conditionals let your scripts make decisions based on the state of variables, files, and command results.

### if / elif / else

The basic structure:

```bash
#!/bin/bash

if [ "$1" = "start" ]; then
    echo "Starting the service..."
elif [ "$1" = "stop" ]; then
    echo "Stopping the service..."
elif [ "$1" = "status" ]; then
    echo "Service is running."
else
    echo "Usage: $0 {start|stop|status}"
    exit 1
fi
```

Key syntax details:
- `[ ]` is the test command. Spaces inside the brackets are **required**: `[ "$1" = "start" ]`, not `["$1"="start"]`.
- `then` goes on the same line as `if` when separated by `;`, or on the next line without the semicolon.
- Every `if` block ends with `fi`.

### Test Expressions

Bash provides `[ ]` (POSIX compatible) and `[[ ]]` (Bash-specific, more features). For most scripts, `[[ ]]` is preferred because it handles empty variables more gracefully and supports pattern matching.

#### String Tests

| Expression | True When |
|---|---|
| `-z "$var"` | String is empty (zero length) |
| `-n "$var"` | String is not empty |
| `"$a" = "$b"` | Strings are equal |
| `"$a" != "$b"` | Strings are not equal |

#### Numeric Tests

| Expression | True When |
|---|---|
| `"$a" -eq "$b"` | Equal |
| `"$a" -ne "$b"` | Not equal |
| `"$a" -lt "$b"` | Less than |
| `"$a" -gt "$b"` | Greater than |
| `"$a" -le "$b"` | Less than or equal |
| `"$a" -ge "$b"` | Greater than or equal |

#### File Tests

| Expression | True When |
|---|---|
| `-f "$path"` | Path is a regular file |
| `-d "$path"` | Path is a directory |
| `-e "$path"` | Path exists (file or directory) |
| `-r "$path"` | File is readable |
| `-w "$path"` | File is writable |
| `-x "$path"` | File is executable |
| `-s "$path"` | File exists and is not empty |

Example combining file tests:

```bash
#!/bin/bash

config_file="/etc/myapp/config.yaml"

if [[ -f "$config_file" ]]; then
    echo "Loading configuration from $config_file"
elif [[ -f "./config.yaml" ]]; then
    echo "Using local config.yaml"
else
    echo "Error: No configuration file found."
    exit 1
fi
```

### Logical Operators

Combine conditions with `&&` (AND), `||` (OR), and `!` (NOT):

```bash
if [[ -f "$file" && -r "$file" ]]; then
    echo "File exists and is readable."
fi

if [[ ! -d "$dir" ]]; then
    echo "Directory does not exist. Creating..."
    mkdir -p "$dir"
fi

if [[ "$status" = "active" || "$status" = "running" ]]; then
    echo "Service is up."
fi
```

### case Statements

When you have many possible values to check, `case` is cleaner than a chain of `elif` blocks:

```bash
#!/bin/bash

case "$1" in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    status)
        echo "Checking status..."
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
        ;;
esac
```

Each pattern ends with `)`. Each block ends with `;;`. The `*` pattern is the default, matching anything not caught above. The entire `case` block ends with `esac` (`case` spelled backward).

Patterns support globs:

```bash
case "$filename" in
    *.tar.gz)
        tar xzf "$filename"
        ;;
    *.zip)
        unzip "$filename"
        ;;
    *.txt)
        cat "$filename"
        ;;
    *)
        echo "Unknown file type: $filename"
        ;;
esac
```

> **Try It**: Write a script called `filecheck.sh` that takes a file path as the first argument. If the path is a regular file, print its size with `wc -c`. If it is a directory, print how many items it contains with `ls | wc -l`. If it does not exist, print an error message and exit with code 1.

---

## Loops

Loops let you repeat actions across lists of items, ranges of numbers, or until a condition is met.

### for Loops

The `for` loop iterates over a list of values:

```bash
#!/bin/bash

# Iterate over a list
for color in red green blue; do
    echo "Color: $color"
done
```

```
Color: red
Color: green
Color: blue
```

Iterate over a range:

```bash
for i in {1..5}; do
    echo "Iteration: $i"
done
```

```
Iteration: 1
Iteration: 2
Iteration: 3
Iteration: 4
Iteration: 5
```

Iterate over files using a glob pattern:

```bash
for file in /var/log/*.log; do
    echo "Log file: $file ($(wc -l < "$file") lines)"
done
```

C-style `for` loop (useful when you need a counter with custom increments):

```bash
for ((i = 0; i < 10; i += 2)); do
    echo "Even number: $i"
done
```

### while Loops

A `while` loop runs as long as its condition is true:

```bash
#!/bin/bash

count=1
while [[ "$count" -le 5 ]]; do
    echo "Count: $count"
    ((count++))
done
```

A common pattern is reading a file line by line:

```bash
while IFS= read -r line; do
    echo "Line: $line"
done < /etc/hostname
```

Here, `IFS=` prevents leading/trailing whitespace from being stripped, and `-r` prevents backslash interpretation. The `< /etc/hostname` redirects the file into the loop's standard input.

### until Loops

An `until` loop runs as long as its condition is **false** (the inverse of `while`):

```bash
#!/bin/bash

count=1
until [[ "$count" -gt 5 ]]; do
    echo "Count: $count"
    ((count++))
done
```

This produces the same output as the `while` example above. Use whichever reads more naturally for your situation.

### break and continue

`break` exits the loop entirely. `continue` skips the rest of the current iteration and moves to the next one:

```bash
#!/bin/bash

for i in {1..10}; do
    if [[ "$i" -eq 3 ]]; then
        continue    # Skip 3
    fi
    if [[ "$i" -eq 8 ]]; then
        break       # Stop at 8
    fi
    echo "Number: $i"
done
```

```
Number: 1
Number: 2
Number: 4
Number: 5
Number: 6
Number: 7
```

> **Try It**: Write a script that loops through all `.sh` files in the current directory and prints whether each one is executable or not. Use a `for` loop with a glob pattern and an `if` statement with the `-x` file test.

---

## Functions

Functions let you organize your script into reusable blocks. They make scripts easier to read, test, and maintain.

### Defining Functions

```bash
#!/bin/bash

greet() {
    echo "Hello, $1! Welcome to $2."
}

greet "cloudchase" "the server"
greet "admin" "production"
```

```
Hello, cloudchase! Welcome to the server.
Hello, admin! Welcome to production.
```

Function arguments use the same `$1`, `$2`, `$#`, `$@` positional variables as script arguments, but scoped to the function call.

### Local Variables

By default, variables in Bash are global. Inside functions, use `local` to prevent variables from leaking out and overwriting values in the rest of your script:

```bash
#!/bin/bash

process_file() {
    local filename="$1"
    local line_count
    line_count=$(wc -l < "$filename")
    echo "$filename has $line_count lines"
}

process_file "/etc/passwd"
echo "filename is: $filename"    # Empty — local variable is not visible here
```

Always use `local` for variables inside functions. This is one of the most important habits for writing reliable scripts.

### Return Values

Functions in Bash return an **exit code** (0-255), not a string or number. Use `return` to set the exit code and capture it with `$?`:

```bash
is_valid_file() {
    if [[ -f "$1" && -r "$1" ]]; then
        return 0    # success
    else
        return 1    # failure
    fi
}

if is_valid_file "/etc/passwd"; then
    echo "File is valid."
else
    echo "File is not valid."
fi
```

To return data (strings, numbers), print to stdout and capture with command substitution:

```bash
get_disk_usage() {
    df -h / | awk 'NR==2 {print $5}'
}

usage=$(get_disk_usage)
echo "Root disk usage: $usage"
```

This pattern -- function prints a value, caller captures it with `$()` -- is the standard way to get data out of functions in Bash.

> **Try It**: Write a function called `backup_file` that takes a file path as an argument, copies it to `<filename>.bak`, and prints a confirmation message. Call it on a test file and verify the backup was created.

---

## Pipes and Redirects

Pipes and redirects are how you connect commands together and control where their output goes. This is one of the most powerful features of the Unix shell.

### Pipes

The pipe operator `|` sends the output of one command as input to the next:

```bash
# Count how many processes are running
ps aux | wc -l

# Find all running python processes
ps aux | grep python

# Sort a file, remove duplicates, count unique lines
sort data.txt | uniq | wc -l

# Find the 5 largest files in /var/log
du -sh /var/log/* | sort -rh | head -5
```

Each command in a pipeline runs simultaneously. The shell connects the stdout of each command to the stdin of the next. This lets you build powerful data processing chains from simple, single-purpose tools.

### Output Redirects

| Operator | Action |
|---|---|
| `>` | Redirect stdout, overwriting the file |
| `>>` | Redirect stdout, appending to the file |
| `2>` | Redirect stderr |
| `2>>` | Redirect stderr, appending |
| `&>` | Redirect both stdout and stderr |
| `&>>` | Redirect both, appending |

```bash
# Save command output to a file (overwrites)
echo "Log started at $(date)" > /var/log/myapp.log

# Append to a file
echo "New entry" >> /var/log/myapp.log

# Redirect errors to a separate file
find / -name "*.conf" 2> /tmp/find-errors.log

# Redirect both stdout and stderr to the same file
./deploy.sh &> /var/log/deploy.log

# Discard output entirely
./noisy-command > /dev/null 2>&1
```

`/dev/null` is a special file that discards everything written to it. Redirecting to `/dev/null` is how you silence a command.

### Input Redirects

The `<` operator feeds a file into a command's stdin:

```bash
# Feed a file into a command
wc -l < /etc/passwd

# Sort a file via input redirect
sort < unsorted.txt > sorted.txt
```

### Heredocs

A **heredoc** (`<<`) lets you embed multi-line input directly in your script:

```bash
cat <<EOF
============================
  Server Status Report
  Generated: $(date)
  Host: $(hostname)
============================
EOF
```

Everything between `<<EOF` and the closing `EOF` is sent as input to the command. Variables and command substitutions are expanded inside a heredoc. To prevent expansion, quote the delimiter: `<<'EOF'`.

Heredocs are especially useful for generating configuration files, SQL queries, or multi-line messages inside scripts.

### Practical Pipeline Examples

```bash
# Find the top 10 IP addresses hitting a web server
cat /var/log/nginx/access.log | awk '{print $1}' | sort | uniq -c | sort -rn | head -10

# Get all unique shells used on the system
cut -d: -f7 /etc/passwd | sort -u

# Check which ports are listening
ss -tlnp | grep LISTEN

# Monitor memory usage every 2 seconds
watch -n 2 'free -h'
```

These pipelines demonstrate the Unix philosophy: each command does one thing well, and you compose them with pipes to solve larger problems. This same philosophy carries forward into [CI/CD](/learn/foundations/cicd/) pipelines, where individual steps are combined into workflows.

---

## Exit Codes and Error Handling

Every command in Linux returns an **exit code** when it finishes. By convention, `0` means success and any non-zero value means failure. Proper error handling is what separates a quick hack from a reliable script.

### Checking Exit Codes

The `$?` variable holds the exit code of the most recently executed command:

```bash
grep "root" /etc/passwd
echo "Exit code: $?"    # 0 (found a match)

grep "nonexistent" /etc/passwd
echo "Exit code: $?"    # 1 (no match found)
```

You can use exit codes directly in conditionals:

```bash
if grep -q "nginx" /etc/passwd; then
    echo "nginx user exists."
else
    echo "nginx user does not exist."
fi
```

The `-q` flag makes `grep` quiet (no output). The `if` statement checks whether the exit code was 0 (true) or non-zero (false).

### Setting Exit Codes

Use `exit` to terminate your script with a specific code:

```bash
#!/bin/bash

if [[ -z "$1" ]]; then
    echo "Error: No filename provided." >&2
    exit 1
fi

if [[ ! -f "$1" ]]; then
    echo "Error: File '$1' not found." >&2
    exit 2
fi

echo "Processing $1..."
exit 0
```

Notice that error messages are directed to stderr with `>&2`. This is best practice because it keeps error output separate from normal output, which matters when the script's stdout is piped or redirected.

### set -euo pipefail

This is the single most important line you can add to any script after the shebang. It enables strict mode:

```bash
#!/bin/bash
set -euo pipefail
```

Each flag does something different:

| Flag | Behavior |
|---|---|
| `-e` | Exit immediately if any command fails (returns non-zero) |
| `-u` | Treat unset variables as an error and exit |
| `-o pipefail` | A pipeline fails if **any** command in the pipe fails (by default, only the last command's exit code matters) |

Without `set -e`, a failing command in the middle of your script is silently ignored and the script keeps running -- potentially doing damage with incorrect data. Without `set -u`, a typo in a variable name silently becomes an empty string. Without `pipefail`, `curl http://bad-url | wc -l` reports success because `wc` succeeds even though `curl` failed.

```bash
#!/bin/bash
set -euo pipefail

# This script will exit immediately if any command fails
api_url="$1"                        # -u catches missing argument
response=$(curl -sf "$api_url")     # -e catches curl failure
echo "$response" | jq '.items'      # pipefail catches jq failure
```

Add `set -euo pipefail` to every script you write. It catches entire categories of bugs before they cause real problems.

### trap for Cleanup

The `trap` command lets you run code when your script exits, whether it finishes normally or is interrupted. This is essential for cleaning up temporary files, releasing locks, or printing a final status:

```bash
#!/bin/bash
set -euo pipefail

tmp_file=$(mktemp)
trap 'rm -f "$tmp_file"' EXIT

echo "Working with temp file: $tmp_file"
# ... do work that might fail ...
echo "Some data" > "$tmp_file"
# The temp file is automatically deleted when the script exits
```

The `trap '...' EXIT` syntax says "run this command when the script exits for any reason." You can also trap specific signals:

```bash
trap 'echo "Caught Ctrl+C"; exit 1' INT     # Ctrl+C
trap 'echo "Caught termination"; exit 1' TERM  # kill command
```

> **Try It**: Create a script with `set -euo pipefail` at the top. Add a command that will fail (like `ls /nonexistent/path`). Run the script and observe that it exits immediately with an error. Then remove the `set` line, run again, and notice that the script continues past the failure.

---

## Scheduling with Cron

Cron is the built-in job scheduler on Linux. It runs commands at specific times and intervals, which makes it the standard tool for recurring tasks like log rotation, backups, health checks, and data syncs. If you ever need to verify a cron expression, [crontab.guru](https://crontab.guru/) is an excellent interactive tool for testing schedule syntax.

### Cron Syntax

A cron schedule is defined by five time fields followed by the command to run:

```
* * * * * command
| | | | |
| | | | +--- Day of week (0-7, where 0 and 7 are Sunday)
| | | +----- Month (1-12)
| | +------- Day of month (1-31)
| +--------- Hour (0-23)
+----------- Minute (0-59)
```

| Field | Values | Special Characters |
|---|---|---|
| Minute | 0-59 | `*` (every), `,` (list), `-` (range), `/` (step) |
| Hour | 0-23 | `*`, `,`, `-`, `/` |
| Day of Month | 1-31 | `*`, `,`, `-`, `/` |
| Month | 1-12 | `*`, `,`, `-`, `/` |
| Day of Week | 0-7 | `*`, `,`, `-`, `/` |

### Common Schedule Examples

| Schedule | Meaning |
|---|---|
| `* * * * *` | Every minute |
| `0 * * * *` | Every hour (at minute 0) |
| `0 0 * * *` | Every day at midnight |
| `0 6 * * *` | Every day at 6:00 AM |
| `0 0 * * 0` | Every Sunday at midnight |
| `0 0 1 * *` | First day of every month at midnight |
| `*/5 * * * *` | Every 5 minutes |
| `0 9-17 * * 1-5` | Every hour from 9 AM to 5 PM, Monday through Friday |
| `30 2 * * *` | Every day at 2:30 AM |
| `0 0 1 1 *` | January 1 at midnight (once a year) |

### Managing Cron Jobs

Edit your crontab (personal cron table) with:

```bash
crontab -e
```

This opens your crontab in your default editor. Each line is one scheduled job. List your current cron jobs with:

```bash
crontab -l
```

Remove all your cron jobs with:

```bash
crontab -r
```

### Logging Cron Output

By default, cron sends the output of each job to the user's email (if a mail service is configured). In practice, you usually redirect output to a log file:

```bash
# Log stdout and stderr to a file
0 2 * * * /home/cloudchase/scripts/backup.sh >> /var/log/backup.log 2>&1

# Discard all output
*/5 * * * * /home/cloudchase/scripts/healthcheck.sh > /dev/null 2>&1

# Log only errors
0 * * * * /home/cloudchase/scripts/sync.sh > /dev/null 2>> /var/log/sync-errors.log
```

Important cron considerations:
- Cron jobs run with a minimal environment. If your script depends on `PATH` or other environment variables, set them explicitly at the top of the script or in the crontab.
- Always use absolute paths in cron jobs. Cron does not know what your current directory is.
- Test your script manually before adding it to cron. Debug interactively first.

> **Try It**: Run `crontab -l` to see if you have any existing cron jobs. Then run `crontab -e` and add a line: `*/1 * * * * echo "Cron is working at $(date)" >> /tmp/cron-test.log`. Save and exit. Wait two minutes, then check `/tmp/cron-test.log`. When done, remove it with `crontab -e`.

---

## Practical Script: System Health Check

Here is a complete script that combines everything from this section -- variables, functions, conditionals, loops, pipes, and exit codes -- into a practical system health check tool. This is the kind of script you would run manually or schedule with cron on a production server.

```bash
#!/bin/bash
set -euo pipefail

# -----------------------------------------------------------
# system_health.sh
# A system health check script that reports on disk, memory,
# CPU load, and critical services.
# Usage: ./system_health.sh [warning_threshold]
# -----------------------------------------------------------

# Configuration
DISK_WARN_PERCENT="${1:-80}"    # Default: warn at 80% disk usage
REPORT_FILE="/tmp/health_report_$(date +%Y%m%d_%H%M%S).txt"

# ---- Functions ----

print_header() {
    local title="$1"
    echo ""
    echo "========================================"
    echo "  $title"
    echo "========================================"
}

check_disk_usage() {
    print_header "Disk Usage"
    local warn_count=0

    while IFS= read -r line; do
        local usage
        usage=$(echo "$line" | awk '{print $5}' | tr -d '%')
        local mount
        mount=$(echo "$line" | awk '{print $6}')
        local size
        size=$(echo "$line" | awk '{print $2}')

        if [[ "$usage" -ge "$DISK_WARN_PERCENT" ]]; then
            echo "  [WARNING] $mount is at ${usage}% (${size})"
            ((warn_count++))
        else
            echo "  [OK]      $mount is at ${usage}% (${size})"
        fi
    done < <(df -h --type=ext4 --type=xfs --type=btrfs 2>/dev/null | tail -n +2)

    if [[ "$warn_count" -gt 0 ]]; then
        echo ""
        echo "  ** $warn_count filesystem(s) above ${DISK_WARN_PERCENT}% threshold **"
        return 1
    fi
    return 0
}

check_memory() {
    print_header "Memory Usage"
    local total used available percent

    read -r total used available <<< "$(free -m | awk 'NR==2 {print $2, $3, $7}')"
    percent=$(( (used * 100) / total ))

    echo "  Total:     ${total}MB"
    echo "  Used:      ${used}MB (${percent}%)"
    echo "  Available: ${available}MB"

    if [[ "$percent" -ge 90 ]]; then
        echo "  [WARNING] Memory usage is critically high."
        return 1
    else
        echo "  [OK] Memory usage is within normal range."
        return 0
    fi
}

check_load() {
    print_header "CPU Load"
    local cores load1 load5 load15

    cores=$(nproc 2>/dev/null || echo 1)
    read -r load1 load5 load15 <<< "$(awk '{print $1, $2, $3}' /proc/loadavg 2>/dev/null || echo "0 0 0")"

    echo "  CPU Cores:       $cores"
    echo "  Load (1 min):    $load1"
    echo "  Load (5 min):    $load5"
    echo "  Load (15 min):   $load15"
    echo "  [OK] Load reported."
}

check_services() {
    print_header "Service Status"
    local services=("sshd" "cron")
    local failed=0

    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            echo "  [OK]      $service is running"
        else
            echo "  [DOWN]    $service is not running"
            ((failed++))
        fi
    done

    if [[ "$failed" -gt 0 ]]; then
        echo ""
        echo "  ** $failed service(s) not running **"
        return 1
    fi
    return 0
}

# ---- Main ----

{
    echo "System Health Report"
    echo "Host:    $(hostname)"
    echo "Date:    $(date)"
    echo "Uptime:  $(uptime -p 2>/dev/null || uptime)"

    issues=0

    check_disk_usage  || ((issues++))
    check_memory      || ((issues++))
    check_load        || ((issues++))
    check_services    || ((issues++))

    print_header "Summary"
    if [[ "$issues" -gt 0 ]]; then
        echo "  Health check completed with $issues warning(s)."
    else
        echo "  All checks passed. System is healthy."
    fi
    echo ""
} | tee "$REPORT_FILE"

echo "Report saved to: $REPORT_FILE"
```

This script demonstrates:
- **Strict mode** with `set -euo pipefail`
- **Default values** with `${1:-80}` (parameter expansion)
- **Functions** with `local` variables and meaningful return codes
- **Loops** iterating over filesystem data and a service list
- **Conditionals** for threshold checks
- **Pipes** with `awk`, `tr`, and `tee`
- **Redirects** to a timestamped report file
- **Process substitution** with `< <(command)` for feeding command output into a `while` loop

You could schedule this script to run every hour with a cron job:

```bash
0 * * * * /home/cloudchase/scripts/system_health.sh 85 > /dev/null 2>&1
```

---

## Parameter Expansion

Bash provides powerful string manipulation through **parameter expansion** — operations performed directly on variable values without external commands.

### Default Values

```bash
# Use default if variable is unset or empty
name="${1:-World}"
echo "Hello, $name"    # "Hello, World" if no argument given

# Set default AND assign it to the variable
: "${LOG_DIR:=/var/log/myapp}"

# Error if variable is unset
: "${API_KEY:?Error: API_KEY must be set}"
```

### String Operations

```bash
filename="backup-2025-01-15.tar.gz"

# Remove shortest match from beginning
echo "${filename#*.}"       # tar.gz

# Remove longest match from beginning
echo "${filename##*.}"      # gz

# Remove shortest match from end
echo "${filename%.*}"       # backup-2025-01-15.tar

# Remove longest match from end
echo "${filename%%.*}"      # backup-2025-01-15

# Substring extraction
echo "${filename:0:6}"      # backup

# String length
echo "${#filename}"         # 27

# Substitution
echo "${filename/2025/2026}"    # backup-2026-01-15.tar.gz
```

These operations are faster than calling external commands like `sed` or `awk` for simple string manipulation, and they work in any POSIX-compatible shell.

> **Try It**: Set `path="/home/clouduser/documents/report.pdf"` and use parameter expansion to extract just the filename (`report.pdf`), just the directory (`/home/clouduser/documents`), and just the extension (`pdf`).

### Associative Arrays

Bash 4+ supports associative arrays (hash maps / dictionaries) — arrays indexed by strings instead of numbers:

```bash
# Declare an associative array
declare -A servers

# Set values
servers[web]="192.168.1.10"
servers[db]="192.168.1.20"
servers[cache]="192.168.1.30"

# Access a value
echo "${servers[web]}"    # 192.168.1.10

# Iterate over keys
for role in "${!servers[@]}"; do
    echo "$role: ${servers[$role]}"
done

# Check if a key exists
if [[ -v servers[web] ]]; then
    echo "Web server is configured"
fi

# Get all keys and values
echo "Roles: ${!servers[@]}"     # web db cache
echo "IPs: ${servers[@]}"        # 192.168.1.10 192.168.1.20 192.168.1.30
```

Associative arrays are useful for configuration lookups, mapping hostnames to IPs, or any scenario where you need key-value pairs without an external tool.

---

## Argument Parsing with getopts

For scripts that accept command-line flags, `getopts` provides structured argument parsing:

```bash
#!/bin/bash

# Define options: v (verbose), f: (file, requires argument), h (help)
verbose=false
file=""

while getopts "vf:h" opt; do
    case $opt in
        v) verbose=true ;;
        f) file="$OPTARG" ;;
        h)
            echo "Usage: $0 [-v] [-f file] [-h]"
            echo "  -v  Enable verbose output"
            echo "  -f  Specify input file"
            echo "  -h  Show this help"
            exit 0
            ;;
        \?)
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
    esac
done

# Shift past processed options to access remaining arguments
shift $((OPTIND - 1))

if $verbose; then
    echo "Verbose mode enabled"
    echo "File: $file"
    echo "Remaining arguments: $@"
fi
```

The colon after `f` in `"vf:h"` means `-f` requires an argument. The value is available in `$OPTARG`. This pattern makes your scripts feel professional and is much more robust than manually parsing `$1`, `$2`, etc.

> **Try It**: Create a script with `getopts` that accepts `-n name` and `-g greeting` options, then prints the greeting with the name.

---

## Debugging Scripts

When a script does not behave as expected, Bash provides built-in debugging tools:

```bash
# Run entire script in debug mode (prints each command before executing)
$ bash -x myscript.sh

# Enable/disable debug mode within a script
set -x    # Turn on debug tracing
echo "This will be traced"
set +x    # Turn off debug tracing
echo "This will not"

# Customize the debug prompt (default is "+ ")
export PS4='+ ${BASH_SOURCE}:${LINENO}: '
bash -x myscript.sh
# Output: + myscript.sh:5: echo "hello"
```

Other useful debugging techniques:

```bash
# Exit immediately if any command fails (don't continue with bad state)
set -e

# Treat unset variables as errors
set -u

# Fail on pipe errors (not just the last command)
set -o pipefail

# Combine all three (recommended for production scripts)
set -euo pipefail
```

The `set -euo pipefail` combination at the top of a script is considered a best practice. It catches errors early instead of letting them cascade into confusing failures.

Beyond built-in debugging, [ShellCheck](https://www.shellcheck.net/) is a static analysis tool that catches common bugs, syntax issues, and portability problems in your shell scripts. You can run it locally or paste scripts into the web interface to get instant feedback. It is highly recommended as part of your scripting workflow.

> **Try It**: Create a simple script with a deliberate error (like referencing an unset variable). Run it normally, then with `bash -x`, and finally add `set -euo pipefail` at the top to see the difference.

---

## Key Takeaways

- Every script starts with `#!/bin/bash` (the shebang) and must be made executable with `chmod +x`. This two-step process applies to every script you write.
- Variables use `=` with no spaces. Always double-quote your variables (`"$var"`) to prevent word splitting and glob expansion. Use `${var}` when concatenating with other text.
- Special variables (`$0`, `$1`, `$?`, `$@`, `$#`) give you access to script arguments, exit codes, and process information. They appear in almost every script.
- Conditionals use `if/elif/else/fi` with test expressions in `[ ]` or `[[ ]]`. Know the string, numeric, and file test operators -- they are your decision-making toolkit.
- Loops (`for`, `while`, `until`) handle iteration. `for` is most common for lists and files. `while read` is the standard pattern for processing files line by line.
- Functions organize code into reusable blocks. Always use `local` for function variables. Return data via stdout and capture it with `$()`.
- Pipes (`|`) chain commands together. Redirects (`>`, `>>`, `2>`, `&>`) control where output goes. These are the connective tissue of Unix.
- `set -euo pipefail` enables strict mode. Add it to every script. It catches unset variables, failed commands, and broken pipelines before they cause damage.
- `trap` handles cleanup on exit. Use it for temporary files, locks, and graceful shutdown.
- Cron schedules recurring tasks using a five-field time syntax. Always use absolute paths and redirect output to log files.
- Shell scripting is the automation layer beneath every DevOps tool. The patterns you learn here -- variables, conditionals, loops, error handling -- reappear in [Programming](/learn/foundations/programming/), [CI/CD](/learn/foundations/cicd/), and [Infrastructure as Code](/learn/foundations/iac/).

## Resources & Further Reading

- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
- [ShellCheck (shell script linter)](https://www.shellcheck.net/)
- [explainshell.com](https://explainshell.com/)
- [Bash Guide for Beginners](https://tldp.org/LDP/Bash-Beginners-Guide/html/)
- [Google Shell Style Guide](https://google.github.io/styleguide/shellguide.html)
- [crontab.guru](https://crontab.guru/)
