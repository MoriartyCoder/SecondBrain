### Pipelines (Chaining Commands)

Pipelines, represented by the `|` symbol, allow you to connect multiple commands in a sequence, where the output of one command serves as the input for the next. This is particularly useful in Linux for handling and transforming data through a series of operations efficiently.

**When to use it:** Use pipelines to streamline workflows by combining commands. For instance, to filter, process, and display data in a single operation.

**How to use it:**

- Chain two commands: `command1 | command2`
- Chain multiple commands: `command1 | command2 | command3`

**Example:** To find and count lines in a file containing a specific word, you can use:

``` bash
grep 'word' filename | wc -l
```

Here, `grep` searches for 'word' in the file, and the results are then counted by `wc -l`.
### cd (Changing directories)

Use the `cd` command to navigate between directories in your file system.

**Important options:**

- To move to a specific directory: `cd /path/to/directory`
- To go to the parent directory: `cd ..`
- To return to the home directory: `cd` or `cd ~`

**Example:**
``` bash
cd /home/user/documents
```

### pwd (Print Working Directory)

The `pwd` command outputs the current directory's full path.

**Example:**

``` bash
pwd 
# Output: /home/user/documents
```

### touch (Create an Empty File)

Use `touch` to create an empty file or update the timestamp of an existing file.

**Example:**
``` bash
touch newfile.txt
```

### rm (Remove Files)

The `rm` command removes files or directories.

**Important options:**

- To delete a file: `rm filename`
- To delete a directory and its contents: `rm -r directoryname`
- Force delete without prompt: `rm -f filename`

**Example:**

``` bash
rm myfile.txt
```

### curl (Download Data from the Internet)

Use `curl` to download files or interact with web APIs.

**Important options:**

- To download and save with the same name: `curl -O URL`
- To save with a specific filename: `curl -o filename URL`

``` bash
curl -O https://example.com/data.csv
```

### nano (Text Editor)

`nano` is a basic text editor for command-line environments.

**Key commands in nano:**

- Save changes: `Ctrl + O`
- Exit nano: `Ctrl + X`

**Example:**

``` bash
nano myfile.txt
```

### cat (Concatenate and Display Files)

The `cat` command displays file content or concatenates multiple files.

**Important options:**

- Print with line numbers: `cat -n filename`
- Concatenate and redirect output: `cat file1 file2 > file3`

``` bash
cat myfile.txt
```

### less (View File Content Page by Page)

`less` displays file content one page at a time, useful for large files.

**Example:**

``` bash
less myfile.txt
```

### grep (Search for Patterns in Files)

Use `grep` to search for text patterns within files.

**Important options:**

- Search for a regular expression: `grep 'pattern' filename`
- Ignore case: `grep -i 'pattern' filename`

**Example:**

``` bash
grep 'searchterm' myfile.txt
```

### wc (Word Count)

The `wc` command counts lines, words, and characters in a file.

**Important options:**

- Count lines: `wc -l filename`
- Count words: `wc -w filename`

``` bash
wc myfile.txt
```

### sort (Sort Lines of Text Files)

`sort` orders lines in files.

**Important options:**

- Sort numerically: `sort -n filename`
- Sort and remove duplicates: `sort -u filename`

``` bash
sort datafile.txt
```

### head/tail (View the Start/End of Files)

Use `head` to see the beginning of a file and `tail` for the end.

**Important options:**

- View first 10 lines: `head -n 10 filename`
- View last 10 lines: `tail -n 10 filename`

``` bash
head myfile.txt
```

### uniq (Filter Out Repeated Lines)

`uniq` filters out or reports duplicate lines in a file.

**Important options:**

- Count duplicates: `uniq -c filename`
- Show only unique lines: `uniq -u filename`

``` bash
sort myfile.txt | uniq
```

### cut (Extract Specific Columns from a File)

Use `cut` to remove sections from each line in a file.

**Important options:**

- Specify delimiter: `cut -d ',' -f1,3 filename`
- Extract specific character positions: `cut -c 1-5 filename`

``` bash
cut -f 1,3 -d ',' data.csv
```

### sed (Stream Editor for Text Manipulation)

`sed` is useful for text replacements and manipulations.

**Important options:**

- Substitute text: `sed 's/old/new/' filename`
- Substitute globally in a line: `sed 's/old/new/g' filename`

``` bash
sed 's/oldtext/newtext/' myfile.txt
```

### awk (Pattern-Scanning and Processing Language)

`awk` processes and analyzes text files line by line.

**Important options:**

- Count lines: `awk 'END {print NR}' filename`
- Sum column values: `awk '{sum+=$1} END {print sum}' filename`

``` bash
awk '{print $1, $3}' data.csv
```