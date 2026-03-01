
# uniq
**Purpose:** Removes or reports duplicate lines from input.

**Important:** uniq only works on sequential duplicates – lines that appear one after another.

`uniq` is simple and fast, but only works on consecutive (sequential) duplicates. Use sort for global duplicate removal.

**Popular options:**
`-u`	Show only unique lines (lines that appear once in sequence)
`-i`	Ignore case when comparing lines
`-d`	Show only duplicate lines (each sequence of duplicates once)
`wc -l`	Can be combined (`uniq file.txt

#### Examples of usage
#### Example uniq -i
`cat linux_distros_2.txt | sort | uniq`
Without `-i` flag:
```bash
AlmaLinuxOS
arch
Arch
debian
Debian
Fedora
openSUSe
pop!_OS
Raspberry Pi OS
Ubuntu
```
With `-i` flag:
```bash
AlmaLinuxOS
arch
debian
Fedora
openSUSe
pop!_OS
Raspberry Pi OS
Ubuntu
```
With `-i` option arch, Arch and debian, Debian are treated as duplicates, so duplicates were removed regardless of letter case
#### Working with `echo`:
- Working example - sequential duplicates:

Preparing data:
`echo -e "Cinnamon\nCOSMIC\nCOSMIC\nGNOME\nPlasma\nMATE\nMATE\nxfce"`

**Output:**
```bash
Cinnamon
COSMIC
COSMIC
GNOME
Plasma
MATE
MATE
xfce
```
Removing duplicates:
`echo -e "Cinnamon\nCOSMIC\nCOSMIC\nGNOME\nPlasma\nMATE\nMATE\nxfce" | uniq`

**Output:**
As you can see below, duplicates were removed:
```bash
Cinnamon
COSMIC
GNOME
Plasma
MATE
xfce
```

- Not working example - non-sequential duplicates:

Preparing data:
`echo -e "Cinnamon\nCOSMIC\nGNOME\nMATE\nCOSMIC\nPlasma\nMATE\nxfce"`

**Output:**
```bash
Cinnamon
COSMIC
GNOME
MATE
COSMIC
Plasma
MATE
xfce
```
Trying to remove duplicates:
`echo -e "Cinnamon\nCOSMIC\nGNOME\nMATE\nCOSMIC\nPlasma\nMATE\nxfce" | uniq`
In this example values are not sequential, that's why `uniq` did nothing.

**Output:**
```bash
Cinnamon
COSMIC
GNOME
MATE
COSMIC
Plasma
MATE
xfce
```

To make `uniq` works, first we need to sort values:
`echo -e "Cinnamon\nCOSMIC\nGNOME\nMATE\nCOSMIC\nPlasma\nMATE\nxfce" | sort`

**Output:**

```bash
Cinnamon
COSMIC
COSMIC
GNOME
MATE
MATE
Plasma
xfce
```
Then pass them to `uniq`.

`echo -e "Cinnamon\nCOSMIC\nGNOME\nMATE\nCOSMIC\nPlasma\nMATE\nxfce" | sort | uniq`

**Output:**
Duplicates were removed:
```bash
Cinnamon
COSMIC
GNOME
MATE
Plasma
xfce
```

#### Passing content from file:
- Sequential records

Displaying file content:
`cat linux_distros1.txt`

**Output:**
```bash
AlmaLinuxOS
Arch
Arch
Debian
Debian
Fedora
Pop!_OS
Raspberry Pi OS
Ubuntu
Ubuntu
openSUSE
```
Duplicates were removed:
`cat linux_distros1.txt | uniq`

**Output:**
```bash
AlmaLinuxOS
Arch
Debian
Fedora
Pop!_OS
Raspberry Pi OS
Ubuntu
openSUSE
```

- Non-sequential records
Displaying file content:
`cat linux_distros2.txt`

**Output:**
```bash
AlmaLinuxOS
Arch
Debian
Ubuntu
Fedora
openSUSe
pop!_OS
Debian
Raspberry Pi OS
Arch
Ubuntu
```
Duplicates were removed (we need to use `sort` first):
`cat linux_distros_2.txt | sort | uniq`

**Output:**
```bash
AlmaLinuxOS
Arch
Debian
Fedora
Pop!_OS
Raspberry Pi OS
Ubuntu
openSUSE
```

#### Working with logs
We can combine `uniq` with `wc -l` to check how many lines were reduced.

Before using `wc -l`:
```bash
cat access.log | wc -l
2699
```
After using `wc -l`:
```bash
cat access.log | wc -l
1484
```

