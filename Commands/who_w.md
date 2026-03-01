# who and w commands
| Command | Description                                                                 |
|---------|-----------------------------------------------------------------------------|
| `who`   | Shows who is currently logged in. Simple and ideal for bash scripts.       |
| `w`     | Extended view: shows who is logged in, what they are doing, and idle time. Useful for system monitoring and analysis. |


### who

**Example output:**
```bash
aluzniak  tty1     2026-02-28 09:15 (:0)
bob       pts/0    2026-02-28 09:45 (192.168.1.5)
carol     pts/1    2026-02-28 10:00 (192.168.1.10)
dave      tty2     2026-02-28 10:05 (:0)
```
**Columns:**
1. **user** – username
2. **tty** – terminal (tty or pts)
3. **login time** – login timestamp
4. **remote host** – remote host or display

**Popular flags:**
`who am i` Shows info about current user session (am i = current terminal)
`who mom likes` Rarely used, similar to `who`
`who -b`  Shows **last system boot time** (`b` = boot) 
`who -H`  Adds **header row** to output (`H` = header) 
`who -q` Quick mode: shows **usernames and total number of users** (`q` = quick)

### Login sources and terminals in who

1️⃣ **tty1, tty2, …**
**ttyN** = virtual text terminals (/dev/tty1, /dev/tty2, …)
Used for local logins (Ctrl+Alt+F1…F6)
Managed by **systemd** / **init**

2️⃣ **GUI logins**
**tty7** (or higher) → X server session (graphical desktop)
**Managed by display managers:**
**GDM** – GNOME Display Manager
**LightDM** – lightweight DM (Xfce, Mate)
**SDDM** – Simple Desktop Display Manager (KDE)

3️⃣ **Remote logins**
pts/N = pseudo-terminals (virtual terminals for remote or programmatic sessions)

**Sources:**
**SSH** → via sshd
**Telnet** → via telnetd
**Rlogin** → via rlogind

**pts/N** without sshd can also appear from local programs spawning pseudo-terminals, e.g., screen, tmux, xterm

### w 

**Example output:**

```bash
 10:15:23 up  1:45,  4 users,  load average: 0.10, 0.15, 0.12
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
aluzniak tty1     :0               09:15    1:00m  0.05s  0.05s /bin/bash
bob      pts/0    192.168.1.5      09:45    2:30   0.10s  0.10s /usr/bin/vim
carol    pts/1    192.168.1.10     10:00    5:00   0.20s  0.15s /usr/bin/top
dave     tty2     :0               10:05    0:00   0.02s  0.02s /bin/bash
```
**Columns:**
**USER** – username,
**TTY** – terminal,
**FROM** – remote host,
**LOGIN@** – login time,
**IDLE** – idle time (since last input),
**JCPU** – CPU time for all processes attached to tty,
**PCPU** – CPU time for current process,
**WHAT** – current command.

**Popular flags:**
`w -h`	Hides header row (h = hide)
`w -s` Short output – removes **login time, JCPU, PCPU**, shows essentials (`s` = short) 
`w -p` Hides **processes spawned by others** (`p` = personal processes) 
