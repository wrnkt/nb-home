# Shell Tricks


### Pattern-Matching Iteration

```bash
# Loop over all .md files with the MM_DD_YY pattern in the base directory
for file in ??_??_??.md; do
    # Skip if no files found
    [ -e "$file" ] || continue

    # Extract month, day, year from filename: MM_DD_YY.md
    IFS="_." read -r MM DD YY _ <<<"$file"
```

I had no idea something like this was possible. `read` is playing an interesting part here as well.
What is `read`?

### `read` Commmand

Shell builtin, retrieves data from `stdin`.

```bash
# Store typed data
read variable

# Store each of the next lines entered as values of an array:
read -a array

# Specify max characters to be read
read -n char_count variable

# Assign multiple values to multiple variables
read <<< "The surname is Bond" _ variable1 _ variable2

# Do not let backslash act as an escape char
read -r variable


# Display a prompt before the input:
read -p "Enter your input here: " variable

# Do not echo typed characters (silent mode):
read -s variable

# Perform an action on each line of a command's output:
    command | while IFS= read -r line; do echo|ls|rm|... "$line"; done

# I've given an overview of useful behaviors
# please consult `man read` for further info.

```
