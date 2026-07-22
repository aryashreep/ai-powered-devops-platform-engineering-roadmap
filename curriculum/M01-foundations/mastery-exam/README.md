# M01 Foundations — Mastery Exam

> **30 multiple-choice questions** · 3 sections · 10 questions each  
> **Pass:** 21/30 (70%) · **Honours:** 27/30 (90%)  
> **Time limit:** 45 minutes

---

## Part 1: Linux Internals (Days 1–12)

**1.** Which of the following best describes a container?
- A) A lightweight virtual machine with its own kernel
- B) A Linux process isolated by namespaces and constrained by cgroups
- C) A Docker-specific runtime that runs on Windows
- D) A hypervisor-managed virtual environment

**Ans: B**

---

**2.** What command shows all listening TCP ports and the processes that opened them?
- A) `ps aux`
- B) `ss -tlnp`
- C) `ls -la /proc`
- D) `systemctl list-sockets`

**Ans: B**

---

**3.** Which file stores user account information (UID, GID, home dir, shell)?
- A) `/etc/shadow`
- B) `/etc/group`
- C) `/etc/passwd`
- D) `/etc/sudoers`

**Ans: C**

---

**4.** What does the sticky bit (`+t`) do on a directory like `/tmp`?
- A) Files are automatically deleted after 24 hours
- B) Only the file owner (or root) can delete their files
- C) All users can read but not write
- D) Files are compressed on disk

**Ans: B**

---

**5.** Which signal is sent by `kill -9` and cannot be caught or ignored?
- A) SIGTERM
- B) SIGHUP
- C) SIGKILL
- D) SIGINT

**Ans: C**

---

**6.** What is `systemd` in modern Linux?
- A) A kernel module for device management
- B) The init system (PID 1), managing services and targets
- C) A replacement for the Bash shell
- D) A filesystem checker

**Ans: B**

---

**7.** Which directory is a virtual filesystem that exposes kernel data structures?
- A) `/etc`
- B) `/var`
- C) `/proc`
- D) `/boot`

**Ans: C**

---

**8.** To create a new SSH key with modern cryptography, which command should you use?
- A) `ssh-keygen -t rsa -b 1024`
- B) `ssh-keygen -t dsa`
- C) `ssh-keygen -t ed25519`
- D) `openssl genrsa -out key.pem 2048`

**Ans: C**

---

**9.** What does `set -euo pipefail` do in a Bash script?
- A) Enables verbose error logging
- B) Exits on errors, undefined variables, and pipeline failures
- C) Sets environment variables for the script
- D) Optimizes the script for parallel execution

**Ans: B**

---

**10.** In LVM, what is the correct order of the storage stack?
- A) LV → VG → PV → Disk
- B) PV → VG → LV → Filesystem
- C) Disk → LV → VG → PV
- D) VG → PV → LV → Filesystem

**Ans: B**

---

## Part 2: Networking & Languages (Days 13–20)

**11.** Which layer of the TCP/IP model handles routing between different networks?
- A) Application
- B) Transport
- C) Internet
- D) Link

**Ans: C**

---

**12.** What does `dig +trace google.com` do?
- A) Checks if google.com is reachable
- B) Shows the complete DNS resolution path from root servers
- C) Tests the HTTP response time
- D) Traces the TCP handshake

**Ans: B**

---

**13.** Which DNS record type maps a domain name to an IPv6 address?
- A) A
- B) MX
- C) CNAME
- D) AAAA

**Ans: D**

---

**14.** During the TCP three-way handshake, what does the client send first?
- A) SYN-ACK
- B) ACK
- C) SYN
- D) FIN

**Ans: C**

---

**15.** What HTTP status code indicates a resource has been permanently moved?
- A) 200
- B) 301
- C) 403
- D) 500

**Ans: B**

---

**16.** Which curl flag shows the full request and response headers?
- A) `-H`
- B) `-v`
- C) `-L`
- D) `-d`

**Ans: B**

---

**17.** In the TCP/IP model, what is the data unit at the Transport layer called?
- A) Frame
- B) Packet
- C) Segment
- D) Bit

**Ans: C**

---

**18.** Which IP address is NOT in the RFC 1918 private range?
- A) `10.0.0.1`
- B) `172.16.0.1`
- C) `192.168.1.1`
- D) `8.8.8.8`

**Ans: D**

---

**19.** Which tool is built on eBPF and provides Kubernetes networking and security?
- A) Docker
- B) Cilium
- C) HAProxy
- D) systemd

**Ans: B**

---

**20.** Which language is considered best for writing high-performance CLI tools that cross-compile to a single binary?
- A) Bash
- B) Python
- C) Go
- D) JavaScript

**Ans: C**

---

## Part 3: Git Deep (Days 21–24)

**21.** What does `git cat-file -p HEAD` show?
- A) The content of the current branch
- B) The commit object that HEAD points to
- C) The contents of the working directory
- D) The remote repository status

**Ans: B**

---

**22.** In Git internals, what does a tree object store?
- A) File contents
- B) A directory listing (blob → name → mode mappings)
- C) Commit metadata (author, timestamp, message)
- D) Diff between two commits

**Ans: B**

---

**23.** What is a Git branch?
- A) A copy of all files in the repository
- B) A mutable pointer to a specific commit
- C) A tag that cannot be moved
- D) A separate repository with its own objects

**Ans: B**

---

**24.** Which of the following correctly describes `git rebase main`?
- A) It creates a merge commit between current branch and main
- B) It replays current branch's commits on top of main, creating linear history
- C) It deletes all commits on the current branch
- D) It overwrites main with the current branch

**Ans: B**

---

**25.** What is a valid Conventional Commits format for a bug fix?
- A) `fix: resolve null pointer in login`
- B) `bugfix: resolve null pointer in login`
- C) `patch: resolve null pointer in login`
- D) `hotfix: resolve null pointer in login`

**Ans: A**

---

**26.** What does the `.gitignore` file do?
- A) Lists files to commit
- B) Specifies intentionally untracked files that Git should ignore
- C) Stores Git configuration
- D) Lists all files in the repository

**Ans: B**

---

**27.** What is the purpose of signing a Git commit?
- A) To encrypt the commit contents
- B) To cryptographically prove the commit came from you
- C) To make the commit visible to other users
- D) To increase the commit's priority

**Ans: B**

---

**28.** Which command temporarily saves uncommitted changes and cleans the working directory?
- A) `git save`
- B) `git stash`
- C) `git backup`
- D) `git freeze`

**Ans: B**

---

**29.** What does `git cherry-pick abc123` do?
- A) Creates a new branch from commit abc123
- B) Applies the changes from commit abc123 to the current branch
- C) Deletes commit abc123 from the repository
- D) Tags commit abc123 as a release

**Ans: B**

---

**30.** Which interactive rebase command combines a commit with the previous one but keeps the original message?
- A) `pick`
- B) `squash`
- C) `fixup`
- D) `reword`

**Ans: B**

---

## Results

| Score | Result |
|-------|--------|
| 27–30 (90%+) | 🏆 Honours — Mastery achieved |
| 21–26 (70%+) | ✅ Pass — Ready to move on |
| 0–20 (< 70%) | 🔄 Review weak areas and retry |

**Review weak areas:** Revisit the day folders for questions you missed. Each day has a hands-on lab to reinforce the concepts.
