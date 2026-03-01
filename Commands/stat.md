# stat command
## What is stat used for?

`stat` displays detailed information (metadata) about a file or directory.

It shows things like:
- size on disk,
- permissions,
- owner and group,
- timestamps,
- filesystem details,

It gives more information than `ls -l`.

### Syntax
`stat [OPTIONS] FILE`

#### Example output:
```bash
File: learn_vim.txt
Size: 2641        Blocks: 8     IO Block: 4096   regular file
Device: fd23h/33749d   Inode: 19792257   Links: 1
Access: (0664/-rw-rw-r--)  Uid: (55687/aluzniak)  Gid: (55687/aluzniak)
Access: 2026-02-25 10:46:47
Modify: 2026-02-25 10:46:46
Change: 2026-02-25 10:46:46
Birth:  2026-02-25 10:46:46
```
- **File** - filename,
- **Size** - file size in bytes,
- **Blocks** - disk blocks actually used by the file
- **IO Block** - filesystem block size (usually 4096 bytes),
- **regular file** - file type (can be directory, symlink, etc.),
- **Device** - device ID where the file is stored
- **Inode** - unique file identifier in the filesystem,
- **Links** - number of hardlinks to the file
- **Access (0664 / -rw-rw-r--)** - permissions (octal and symbolic),
- **Access** - last time the file was read,
- **Modify** - last time file content was changed,
- **Change** - last time metadata changed (permissions, owner),
- **Birth** - file creation time (if supported).

#### Effect of chmod on stat:
`chmod 660 learn_vim.txt`
```bash
stat learn_vim.txt
  File: learn_vim.txt
  Size: 2641      	Blocks: 8          IO Block: 4096   regular file
Device: fd01h/64769d	Inode: 19792257    Links: 1
Access: (0660/-rw-rw----)  Uid: (55687/aluzniak)   Gid: (55687/aluzniak)
Access: 2026-02-25 10:46:47.002259304 +0100
Modify: 2026-02-25 10:46:46.406308955 +0100
Change: 2026-03-01 11:29:50.313194025 +0100
 Birth: 2026-02-25 10:46:46.406308955 +0100
```
What changed:
- **Access permissions** changed from 0664 to 0660
- **Change timestamp** updated
- **Modify timestamp** did NOT change (content unchanged)

#### Checking filesystem informations:
stat -f learn_vim.txt
```bash
stat -f learn_vim.txt
  File: "learn_vim.txt"
    ID: b287d4f61ab78f99 Namelen: 255     Type: ext2/ext3
Block size: 4096       Fundamental block size: 4096
Blocks: Total: 105510566  Free: 104706901  Available: 99328905
Inodes: Total: 26869760   Free: 26804704
```
Important fields
**File system type** – e.g. **ext4**,
**Block size** – filesystem block size,
**Total blocks** – total space,
**Free blocks** – available space,
**Inodes** – file capacity of filesystem.

#### Formating output (-c option)
`stat` supports output formatting, we can use the **linux most common format sequences** table to extract the necessary fields from it.

##### Display specific fields
`stat -c %y learn_vim.txt`

`%y` → modification time

**Output:**
`2026-02-25 10:46:46.406308955 +0100`

##### Custom formatted output
`stat -c "Modified: %y" learn_vim.txt`

**Output:**
`Modified: 2026-02-25 10:46:46.406308955 +0100`

##### Multiple fields in one line
```stat -c "Access: %x, Modify: %y, Change: %z" learn_vim.txt```

`%x` → access time
`%y` → modify time
`%z` → change time

**Output:**
```
Access: 2026-02-25 10:46:47.002259304 +0100, Modify: 2026-02-25 10:46:46.406308955 +0100, Change: 2026-03-01 11:29:50.313194025 +0100
```

##### Multiple fields in multiple lines
```
stat -c $'Access: %x\nModify: %y\nChange: %z' learn_vim.txt
```

**Output:**
```
Access: 2026-02-25 10:46:47.002259304 +0100
Modify: 2026-02-25 10:46:46.406308955 +0100
Change: 2026-03-01 11:29:50.313194025 +0100
```

##### Sort files by modification time
`stat -c "%y %n" *.txt | sort -n`

`sort -n` → numeric sort by date

##### Check files modified today
`stat -c "%y %n" *.txt | grep $(date +%Y-%m-%d)`

**Output:**
`2026-03-01 11:44:38.555396787 +0100 test.txt`

##### Working with multiple files
`stat *.txt`
