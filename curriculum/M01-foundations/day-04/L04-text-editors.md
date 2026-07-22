---
module: "M01"
unit: "Linux internals"
lesson: "Ch 11: Text Editors (vim/nano)"
day: 4
lf_reference: "LFS101 Chapter 11"
status: draft
est_minutes: 45
---

# Chapter 11: Text Editors — vim & nano

> **Learning goal:** Master two essential Linux text editors — vim for power editing on servers, nano for quick config changes — so you can edit any file on any Linux system without needing a GUI.

---

## Story (2 min)

### The Server Has No GUI

**Golu:** *"Jagu, I need to edit the nginx config on the production server. Can you send me the file? I'll edit it in VS Code and send it back."*

**Jagu:** *"Golu... the production server is in a data center. There's no VS Code. There's no GUI. There's just you, a terminal, and a text file that needs editing. What do you do?"*

**Golu:** *"...I... cry?"*

**Jagu:** *"Close. You open `nano` or `vim`. Every Linux server has one of these installed. If you don't know how to use them, you can't fix a config file at 3 AM when the server is on fire."*

**Golu:** *"So you're telling me I need to learn a text editor that doesn't even have a menu bar?"*

**Jagu:** *"It doesn't have a menu bar. But it can edit a file on a server 10,000 miles away faster than you can download it, edit it, and upload it back. And when the network is down — which is when you need to fix things — it's your only option."*

> **Lesson:** A Linux admin without vim/nano is a carpenter without a hammer. You can survive without it, but you can't build anything.

---

## Real-world Problem (3 min)

### What Breaks Without Text Editor Skills?

- **"I can't edit server configs"** — You can `ls` and `cd` all day, but the moment you need to change an nginx config or update a cron job, you're stuck.
- **"The `sed` hack"** — You try to edit a file with `sed -i` because you don't know vim. You mess up the regex. The config breaks. The server goes down.
- **"The download-edit-upload dance"** — You download a file, edit it locally, upload it back. Takes 5 minutes. In that time, the senior already fixed it with vim in 30 seconds.
- **"No syntax highlighting"** — You edit a YAML file blindly. One wrong space. Kubernetes rejects it. You stare at it for 10 minutes before seeing the extra space.
- **"Interview embarrassment"** — "How do you edit a file on a Linux server?" You say "I use VS Code." The interviewer nods and moves on — but they've already judged.

**Golu:** *"OK, I get it. No GUI = need vim or nano. But which one? There's two of them?!"*

**Jagu:** *"Learn both. Use nano for quick edits (5 seconds, in and out). Use vim for real work (syntax highlighting, macros, searching). One is a bicycle, the other is a sports car."*

---

## Visualization (2 min)

### The Three Vim Modes (Draw This)

```
┌──────────────────────────────────────────────────────┐
│                   VIM — Three Modes                   │
├──────────────────────────────────────────────────────┤
│                                                       │
│  ┌─────────────┐     i     ┌──────────────┐          │
│  │   NORMAL    │ ───────→  │   INSERT     │          │
│  │ (navigation │   Esc     │  (editing)   │          │
│  │  & commands)│ ←─────── │              │          │
│  └─────────────┘           └──────────────┘          │
│        │  :                                            │
│        │  v                                            │
│        ▼                                               │
│  ┌─────────────┐                                       │
│  │   COMMAND   │  :w  = save                           │
│  │   (: mode)  │  :q  = quit                           │
│  │             │  :wq = save & quit                    │
│  └─────────────┘  :q! = quit without saving           │
│                                                       │
└──────────────────────────────────────────────────────┘
```

**Analogy:** Vim is like a **DSLR camera** — lots of modes, steep learning curve, but incredibly powerful once you master it. Nano is like a **phone camera** — point, shoot, done. Both take pictures. Choose the right tool.

---

## Theory (5 min)

### Nano — The Bicycle

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Save (write out) |
| `Ctrl+X` | Exit |
| `Ctrl+W` | Search |
| `Ctrl+K` | Cut line |
| `Ctrl+U` | Paste line |
| `Ctrl+G` | Help (all shortcuts shown at bottom) |

**Best for:** Quick config edits, beginners, one-time changes.

### Vim — The Sports Car

| Mode | What You Can Do |
|------|----------------|
| **Normal** | Navigate, delete, copy, paste, search — default mode |
| **Insert** | Type text — enter with `i`, `a`, `o` |
| **Visual** | Select text — enter with `v` |
| **Command** | Save, quit, search/replace — enter with `:` |

**Essential vim navigation (Normal mode):**

| Key | Action |
|-----|--------|
| `h j k l` | Move left, down, up, right |
| `w b` | Next word, previous word |
| `0 $` | Start of line, end of line |
| `gg G` | Start of file, end of file |
| `dd` | Delete line |
| `yy` | Copy (yank) line |
| `p` | Paste below |
| `/search` | Search for text |
| `n N` | Next match, previous match |
| `u` | Undo |
| `Ctrl+r` | Redo |

**Best for:** Serious editing, programming, config management, server-side work.

---

## Live Demo (13 min)

**Jagu:** *"Open your terminal. Today we do this together — I'll use vim, you use nano. Or vice versa. Then switch. Learn both."*

### Step 1 — Create a File with Nano

```bash
# Create a new file
nano my-first-edit.txt

# Inside nano:
# Type: "Hello Linux! This is my first nano edit."
# Ctrl+O to save, Enter to confirm, Ctrl+X to exit

# Verify:
cat my-first-edit.txt
```

### Step 2 — Edit with Vim

```bash
# Create a new file with vim
vim my-first-vim-edit.txt

# Inside vim:
# Press i to enter INSERT mode
# Type: "Vim is powerful once you learn the modes."
# Press Esc to return to NORMAL mode
# Type :wq and press Enter (save and quit)

# Verify:
cat my-first-vim-edit.txt
```

### Step 3 — Essential Vim Practice

```bash
# Copy a config file to practice
cp /etc/hosts ~/practice-hosts.txt
vim ~/practice-hosts.txt

# Practice these moves:
# - h/j/k/l to navigate
# - w to jump between words
# - dd to delete a line (then u to undo)
# - yy then p to copy and paste a line
# - /localhost to search
# - :set number to show line numbers
# - :wq to save and quit
```

### Step 4 — System Config Edit (Simulated)

```bash
# Create a practice nginx config
cat > ~/practice-nginx.conf << 'EOF'
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
}
EOF

# Open in vim and make changes:
vim ~/practice-nginx.conf

# Change: listen 80 → listen 443
# Change: example.com → yoursite.com
# Add a new line after root: index index.html;
# Save and quit
```

**Golu:** *"Wait — I just edited a config file on a fake server. Was that... actually kind of satisfying?"*

**Jagu:** *"That's the feeling, Golu. Now imagine doing that on a real server at 3 AM while the CEO is calling. That's when you'll be glad you practiced."*

---

## Common Mistakes (5 min)

### ❌ Mistake 1: Being Stuck in Vim

**Golu's first time:** *"I opened vim. I couldn't type. I couldn't exit. I literally closed the terminal."*

```bash
# 😬 WRONG — Panic and close the terminal (you lose unsaved work)
# Just close the terminal window and hope for the best

# ✅ RIGHT — Know how to exit vim:
# Press Esc (multiple times to be sure)
# :q! to quit WITHOUT saving
# :wq to save and quit
```

### ❌ Mistake 2: Using Nano for Everything

```bash
# 😬 WRONG — Nano is great for quick edits, but:
# No syntax highlighting
# No search/replace with regex
# Can't record macros
# Limited for large files

# ✅ RIGHT — Use nano for < 30 second edits
# Use vim for anything more complex
```

### ❌ Mistake 3: Not Knowing `sudoedit`

```bash
# 😬 WRONG — Editing system files directly as root
sudo vim /etc/nginx/nginx.conf   # Better, but runs vim as root

# ✅ RIGHT — Use sudoedit (safer, keeps your vim config)
sudoedit /etc/nginx/nginx.conf
```

---

## Interview Questions (5 min)

1. **Q: What is the difference between vim and nano?**
   **A:** Nano is a beginner-friendly, modeless editor — all shortcuts are shown at the bottom, you start typing immediately. Vim is a modal editor with three main modes (Normal, Insert, Command). Vim has a steeper learning curve but offers syntax highlighting, macros, search/replace with regex, and is extremely efficient once mastered. Every Linux system has at least one of them.

2. **Q: How do you save and exit a file in vim?**
   **A:** Press `Esc` to ensure you're in Normal mode, then type `:wq` and press Enter. `:w` writes (saves), `:q` quits. Alternatively, `:x` does the same thing. To quit without saving: `:q!`.

3. **Q: What is `sudoedit` and why would you use it?**
   **A:** `sudoedit` allows you to edit a system file with elevated privileges while running the editor itself as your regular user. This is safer than `sudo vim` because if vim gets compromised (e.g., by a plugin), the exploit doesn't run as root. It also preserves your personal vim configuration.

4. **Q: You need to quickly change one line in a config file. Nano or vim?**
   **A:** For a one-line change, use nano — it's faster to open, edit, and exit. For any editing that requires search/replace, syntax review, or working with multiple files, use vim.

---

## Assignment (10 min)

**Task: Text Editor Warm-Up**

**Golu:** *"By the end of this, nano will feel like a 5-star hotel. Still scared of vim? Good. Do it anyway."*

1. Create a file with nano: `nano ~/nano-practice.txt`. Write 3 lines about what you learned today. Save and exit.
2. Create the same file with vim: `vim ~/vim-practice.txt`. Write 3 lines. Save and exit.
3. Practice vim navigation on a large file: Open `/etc/services` in vim (`vim /etc/services`). Navigate to line 50 (`50G`), then line 200 (`200G`). Search for "ssh" (`/ssh`). Exit without saving (`:q!`).
4. Create a YAML practice file with vim: `vim ~/practice-config.yml`. Write a simple nginx config. Practice `dd` (delete line), `yy` (copy), `p` (paste).
5. **🔥 Challenge:** Open `/etc/hosts` with `sudoedit` and add a comment at the top: `# Edited by [your name] on [date]`. Save and exit.

**You know you're done when:** You can edit a file in vim without Googling "how to exit vim" — and you know when to use nano vs vim.

---

## 🎯 Chapter Summary

| Item | |
|------|---|
| 🎯 **Takeaway** | Every Linux server has vim or nano. Knowing both means you can edit any file on any system from any terminal. |
| ❓ **Interview Question** | How do you save and exit a file in vim? (`:wq`, `:x`, or `ZZ`) |
| 📝 **Assignment** | Practice vim navigation on `/etc/services`, create files with both editors |
| 🔥 **Challenge** | Use `sudoedit` to edit a system file safely |
| 💥 **Today's Mistake** | Don't panic in vim — `:q!` is your emergency exit |
| ⚡ **Pro Tip** | Run `vimtutor` on your system — it's a built-in 30-minute interactive vim tutorial |
