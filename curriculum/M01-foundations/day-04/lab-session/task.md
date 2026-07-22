# 🧪 Lab: Text Editors — vim & nano

## Objective
By the end of this lab, you'll be able to create, edit, and save files using both nano and vim on any Linux system.

## Prerequisites
- A Linux terminal (your VM or WSL)
- About 15 minutes

---

## Part 1: Nano Practice (5 min)

1. Create a new file: `nano ~/nano-practice.txt`
2. Type these three lines:
   ```
   Nano is simple.
   I can see all shortcuts at the bottom.
   Ctrl+O saves, Ctrl+X exits.
   ```
3. Save: `Ctrl+O`, then `Enter`
4. Exit: `Ctrl+X`

✅ You should see your file with `cat ~/nano-practice.txt`

---

## Part 2: Vim Survival (5 min)

1. Create a new file: `vim ~/vim-practice.txt`
2. Press `i` to enter INSERT mode
3. Type: "Vim is powerful but different."
4. Press `Esc` to return to NORMAL mode
5. Type `:wq` and press `Enter`

✅ You should see your file with `cat ~/vim-practice.txt`

---

## Part 3: Vim Navigation (5 min)

1. Open a large file: `vim /etc/services`
2. Practice navigation:
   - `50G` — jump to line 50
   - `200G` — jump to line 200
   - `/http` — search for "http"
   - `n` — next match
   - `N` — previous match
   - `:set number` — show line numbers
   - `:set nonumber` — hide line numbers
3. Exit without saving: `:q!`

✅ You navigated a large file using vim commands

---

## Part 4: Edit a Config File (Stretch)

1. Copy a config: `cp /etc/hosts ~/practice-edit.txt`
2. Open it with `sudoedit ~/practice-edit.txt`
3. Add a comment at the top: `# Edited by [your name]`
4. Save and exit
5. Verify: `cat ~/practice-edit.txt`

✅ You used `sudoedit` to edit a system file safely

---

## Acceptance Criteria

You're done when:
- ✅ You created and saved files with both nano and vim
- ✅ You navigated `/etc/services` using vim search and line jumps
- ✅ You can explain the difference between nano and vim
- ✅ You can exit vim without Googling
  
## Learn in Public

Share on LinkedIn: "Today I learned vim and nano! No more VS Code dependency for quick server edits. #LearnDevOpsIn90Days"
