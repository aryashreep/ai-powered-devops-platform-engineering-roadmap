---
module: "M02"
unit: "Core principles"
lesson: "Idempotency"
status: reviewed            # draft | reviewed | recorded | published
est_minutes: 48          # Story(2) + Problem(3) + Visualization(2) + Theory(5) + Demo(13) + Mistakes(5) + Interview(5) + Assignment(10) + Concept Check(2) + Foundation(3)
---

# Idempotency

> **Learning goal:** Understand why running the same configuration five times should produce the same result — and why this property is the superpower that makes automation safe, predictable, and auditable.

---

## 🎯 Goal

Build a deep intuition for idempotency — so you can distinguish safe automation from dangerous automation, and write scripts/configs that can be run over and over without side effects.

## 🧠 Key Learnings

| # | Topic | Why It Matters |
|---|-------|-------------|
| 1 | Idempotency defined — run N times, same result | The fundamental property that makes automation *safe* to run on a schedule or from an automated trigger |
| 2 | Non-idempotent operations and their risks | `mkdir` (fails on second run), `>>` (appends forever), `apt install` (reinstalls) — each breaks when run twice |
| 3 | How declarative tools achieve idempotency | Terraform, Ansible, Kubernetes use *reconciliation loops* — compare desired state vs current state, only change what's different |
| 4 | Idempotency as a design principle | Not just a tool feature — a mindset: always ask \"will running this twice cause problems?\" |

---

## 🏛️ Foundation Context (3 min)

### The Vending Machine Problem

**Golu:** *"Jagu, I get declarative vs imperative. But you said something at the end of last lesson — *idempotency*. What's that? Sounds like a dinosaur."*

**Jagu:** *"Close. It's the most important property your automation can have. Let me ask you: if you press the elevator button that's already lit, does the elevator come faster?"*

**Golu:** *"...No? It's already on its way. Pressing it again does nothing."*

**Jagu:** *"Exactly. That button is *idempotent*. Press it once, the elevator comes. Press it 50 times, the elevator still comes once. No side effects. No double-charges. Now contrast with a vending machine that charges your card every time you press the button — even if the candy already dropped."*

**Golu:** *"Okay, so idempotent = safe to press multiple times?"*

**Jagu:** *"Bingo. And in DevOps, that means your automation can run on a schedule, from a CI trigger, or after a failure — and you won't get duplicate configs, broken services, or unexpected bills."*

**Fun fact:** The word comes from mathematics (idem = same, potence = power). In computing, it means "an operation that produces the same result regardless of how many times it's performed."

---

## Story (2 min)

### The Cron Job Disaster

**Characters:** Golu (junior, loves automating everything) and Jagu (senior, has cleaned up after Golu's cron jobs)

**Scene:** A Friday afternoon. Golu is proud of his automation.

**Golu:** *"Jagu! Check it out — I wrote a cron job that cleans up old logs and runs every hour!"*

**Jagu:** *"Let me see..."*

```bash
# Golu's cron job — runs every hour
0 * * * * /home/golu/cleanup.sh
```

**Jagu:** *"What does cleanup.sh do?"*

**Golu:** *"Just adds a timestamp to the log file so we know it ran!"*

```bash
#!/bin/bash
echo "[$(date)] Cleanup ran" >> /var/log/cleanup.log
```

**Jagu:** *"Golu... how many times has this cron job run so far?"*

**Golu:** *"It's been running for a week. So about 168 times?"*

**Jagu:** *"And how many lines are in /var/log/cleanup.log?"*

**Golu:** *"...168?"*

**Jagu:** *"Now imagine instead of a 50-byte log file, this was adding firewall rules. Or appending to a config file. Or inserting rows into a database. You'd have 168 duplicate firewall rules, a config file so bloated it crashes, or 168,000 duplicate database rows."*

**Golu:** *"Oh. OH. So my cron job isn't idempotent?"*

**Jagu:** *"No, Golu. It's a *monotonic disaster waiting to happen*. Every run adds more data. The first run and the 168th run produced *different* results. That's the opposite of idempotent."*

```bash
# Fix it — truncate instead of append (idempotent!)
#!/bin/bash
echo "[$(date)] Cleanup ran" > /var/log/cleanup.log
```

**Golu:** *"Wait — now it overwrites instead of appending. Running it 10 times still gives 1 line. That's idempotent!"*

**Jagu:** *"Now you're thinking. Every cron job, every deploy script, every automation — ask yourself: 'What happens if this runs twice?'"*

> **Lesson:** Idempotency is what makes automation safe to run on autopilot. Without it, every repeated run is a gamble.

---

## Real-world Problem (3 min)

### What Breaks Without Idempotency?

- **"The 3 AM Pager Duplicate"** — Your CI/CD pipeline failed mid-deploy and retried. The retry succeeded — but also created a duplicate monitoring alert, a duplicate firewall rule, and a duplicate DNS record. You're woken up at 3 AM to clean up the mess.
- **"The Monday Morning Script"** — The team runs a weekly server hygiene script. Someone forgot to make it idempotent. After 6 months, `/etc/hosts` has 6,000 lines (the same entries repeated weekly). SSH into those servers now takes 30 seconds.
- **"The Configuration Drift Bomb"** — Ansible playbook installs `nginx` with `state: latest`. Every run *upgrades* nginx to the latest version. After a year, nginx has been upgraded 12 times — and one of those upgrades broke a compatibility. Nobody knows which one.
- **"The Terraform Import Surprise"** — You write a Terraform config that creates an S3 bucket. You apply it. It works. You apply it again. Terraform says "no changes" (idempotent!). But your colleague's script that creates the *same* bucket with AWS CLI fails on the second run — because the bucket already exists. Non-idempotent automation = fragile automation.

### Topic Flow

```
🏛️ Foundation → 💥 Story → ❓ Problem → 👁️ Visualization → 📖 Theory → ⌨️ Demo → ⚠️ Mistakes → 🎤 Interview → 📝 Assignment
```

---

## Visualization (2 min)

### The Idempotency Spectrum

Draw this on a whiteboard:

```
NOT IDEMPOTENT                          FULLY IDEMPOTENT
(Side effects on every run)      (Same result every run)
         │                                    │
         ▼                                    ▼
   > mkdir test                   > mkdir -p test
     (fails if exists)              (succeeds silently)
   > echo "data" >> file           > echo "data" > file
     (appends forever)              (overwrites — same result)
   > apt install nginx             > apt install -y nginx
     (prompts, may reinstall)        (skips if installed)
   > INSERT INTO logs              > UPSERT (INSERT ... ON CONFLICT)
     (duplicate rows)                (updates, no duplicates)
   > cp file backup/               > rsync -a file backup/
     (copies every time)             (skips if identical)
```

**Golu's Summary:** *"So idempotent is like making your bed — do it once, it's done. Doing it again doesn't mess it up. Non-idempotent is like squeezing toothpaste — every squeeze changes the tube forever."*

**Jagu:** *"That's... actually perfect. And if you squeeze 20 times, you've got a mess. Just like your cron job."*

### The Reconciliation Loop

```
            ┌─────────────────────────────────┐
            │         DESIRED STATE           │
            │    (defined in Git/config)      │
            └────────────┬────────────────────┘
                         │
                         ▼
            ┌─────────────────────────────────┐
            │   TOOL COMPARES: Current vs.    │
            │         Desired                 │
            └────────────┬────────────────────┘
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
    ┌─────────────────┐  ┌─────────────────┐
    │  MATCH:         │  │  MISMATCH:      │
    │  Do nothing     │  │  Apply changes  │
    │  (idempotent!)  │  │  to reach       │
    │                 │  │  desired state  │
    └─────────────────┘  └─────────────────┘
```

---

## Theory (5 min)

### Idempotency Defined

> **Idempotency:** An operation is idempotent if performing it multiple times has the same effect as performing it once.

| Type | Property | Example |
|------|----------|---------|
| **Read idempotent** | GET the same resource, same result | `cat /etc/hosts` — always shows the file |
| **Write idempotent** | PUT the same data, same state | Terraform `apply` — converge to desired state |
| **Conditional idempotent** | Check first, then act | `mkdir -p` — "create if not exists" |
| **Non-idempotent** | Every run changes state | `echo >>` — appends forever |

### Why Idempotency Matters for DevOps

| Scenario | Non-Idempotent | Idempotent |
|----------|---------------|------------|
| **Cron job runs every hour** | Adds 24× daily duplicates | Same state after every run |
| **Pipeline retries after failure** | Creates orphaned resources, duplicate alerts | Safe to retry any step |
| **Scaling from 1 to 100 servers** | Each server slightly different (drift) | All servers identical |
| **Disaster recovery** | Manual recovery, error-prone | Re-apply config, get identical system |
| **Audit** | "Which run created this state?" | "The config IS the state" |

### How Tools Achieve Idempotency

| Tool | Idempotency Mechanism |
|------|----------------------|
| **Terraform** | State file tracks what exists → only creates/updates what differs from desired state |
| **Ansible** | Modules check current state before acting (e.g., `state: started` checks if service is already running) |
| **Kubernetes** | Reconciliation loop — controllers constantly compare actual vs desired state |
| **Dockerfile** | Layer caching — unchanged build steps are reused, not re-executed |
| **mkdir -p** | Shell-level — creates if missing, succeeds silently if exists |

### Forward Ref: Why This Connects to CI/CD (M03)

**Jagu:** *"In M03, you'll build CI/CD pipelines. Every pipeline stage should be idempotent. The build step should produce the same artifact hash for the same source code. The deploy step should succeed whether it's the first deployment or the 50th. Idempotency is what makes pipelines reliable."*

---

## Live Demo (13 min)

**Jagu:** *"Open your terminal. We'll write non-idempotent code, watch it break, then fix it. You'll *feel* the difference."*

### Step 1 — The Non-Idempotent Script

Create a "deploy" script that does things the wrong way:

```bash
cd ~
mkdir -p demo-idempotency
cd demo-idempotency

# Create a NON-idempotent deploy script
cat > deploy-bad.sh << 'EOF'
#!/bin/bash
echo "=== Deploy Script (Non-Idempotent) ==="

# Step 1: Create config directory
echo "Creating /tmp/myapp-config..."
mkdir /tmp/myapp-config

# Step 2: Write config
echo "Writing config..."
echo "server_name=myapp" > /tmp/myapp-config/app.conf
echo "port=8080" >> /tmp/myapp-config/app.conf

# Step 3: Add firewall rule (simulated)
echo "Adding firewall rule for port 8080..."
echo "iptables -A INPUT -p tcp --dport 8080 -j ACCEPT" >> /tmp/myapp-config/firewall.log

# Step 4: Log success
echo "Deploy complete at $(date)" >> /tmp/myapp-config/deploy.log

echo "=== Done ==="
EOF

chmod +x deploy-bad.sh
```

**Golu:** *"Looks fine to me. What's wrong with it?"*

**Jagu:** *"Run it twice."*

```bash
./deploy-bad.sh
echo "--- First run done ---"
./deploy-bad.sh
```

**Golu:** *"The first run worked. The second run failed! 'mkdir: cannot create directory'. And the firewall log has 2 lines. And deploy.log has 2 timestamps!"*

**Jagu:** *"Exactly. Every run changes the system. Run it 100 times, you get 100 deploy.log lines. This script is NOT safe to run on a schedule or from a pipeline retry."*

### Step 2 — Make It Idempotent

```bash
# Create an IDEMPOTENT deploy script
cat > deploy-good.sh << 'EOF'
#!/bin/bash
echo "=== Deploy Script (Idempotent) ==="

# Step 1: Create config directory (idempotent!)
echo "Ensuring /tmp/myapp-config exists..."
mkdir -p /tmp/myapp-config
echo "  → mkdir -p: always succeeds, never fails"

# Step 2: Write config (idempotent — overwrite with same content)
echo "Writing config (overwrite mode)..."
cat > /tmp/myapp-config/app.conf << 'CONF'
server_name=myapp
port=8080
CONF
echo "  → Same content written every time"

# Step 3: Add firewall rule (idempotent — check first)
echo "Ensuring firewall rule for port 8080..."
if ! grep -q "dport 8080" /tmp/myapp-config/firewall.log 2>/dev/null; then
  echo "iptables -A INPUT -p tcp --dport 8080 -j ACCEPT" >> /tmp/myapp-config/firewall.log
  echo "  → Rule added"
else
  echo "  → Rule already exists, skipping"
fi

# Step 4: Log success (idempotent — overwrite, don't append)
echo "Logging deployment..."
echo "Deploy complete at $(date)" > /tmp/myapp-config/deploy.log
echo "  → deploy.log overwritten (1 line, no matter how many runs)"

echo "=== Done ==="
EOF

chmod +x deploy-good.sh
```

```bash
./deploy-good.sh
echo "--- First run done ---"
./deploy-good.sh
echo "--- Second run done ---"
./deploy-good.sh
echo "--- Third run done ---"

# Check the state
echo ""
echo "=== Final State ==="
cat /tmp/myapp-config/deploy.log
echo "Deploy log has $(wc -l < /tmp/myapp-config/deploy.log) line(s)"
echo "Firewall log has $(wc -l < /tmp/myapp-config/firewall.log) line(s)"
```

**Golu:** *"3 runs, 1 deploy.log line, 1 firewall rule. It's the same every time!"*

**Jagu:** *"That's idempotency. Now imagine running this on 100 servers, once a week, for a year. No drift. No duplication. No surprises."*

### Step 3 — Ansible Preview: Idempotency in Action

Let's revisit the Ansible demo from L01, but focus on the idempotency:

```bash
# Run the Ansible playbook from L01 (still in ~/demo-declarative/)
cd ~/demo-declarative

echo "=== Run 1 ==="
ansible-playbook ensure-nginx.yml
echo ""

echo "=== Run 2 ==="
ansible-playbook ensure-nginx.yml
echo ""

echo "=== Run 3 ==="
ansible-playbook ensure-nginx.yml
```

**Golu:** *"Run 1 says 'changed=1'. Run 2 and 3 say 'changed=0'. It's doing nothing after the first run!"*

**Jagu:** *"Exactly. Ansible checked the current state, saw it matched the desired state, and did ZERO work. That's the power of idempotent declarative tools. Now imagine this is a 500-line Terraform config managing your entire cloud infrastructure — same principle."*

### Step 4 — Kubernetes Preview: Desired State Reconciliation

```bash
# Create a Kubernetes pod definition (preview — full depth in M06)
cat > ~/demo-idempotency/pod.yaml << 'EOF'
apiVersion: v1
kind: Pod
metadata:
  name: idempotent-demo
spec:
  containers:
  - name: nginx
    image: nginx:latest
EOF

echo "Preview of Kubernetes YAML — a DESIRED STATE declaration"
echo "Apply it once: pod is created"
echo "Apply it again: 'no changes' (idempotent)"
echo "Delete the pod manually: apply RE-CREATES it (self-healing!)"
echo ""
echo "This is the reconciliation loop in action."
```

**Jagu:** *"Kubernetes doesn't just apply your config once. It runs a *continuous reconciliation loop* — always checking that the actual state matches the desired state. If someone deletes the pod, Kubernetes recreates it. That's idempotency at the cluster level."*

---

## Common Mistakes (5 min)

### ❌ Mistake 1: Appending Instead of Overwriting

```bash
# 😬 WRONG — Appending creates unbounded growth
echo "[$(date)] Health check passed" >> /var/log/health.log
# ← 1 year later: 8,760 lines. The log file is 500KB of "passed".

# ✅ RIGHT — Overwrite or use a fixed-size log
echo "[$(date)] Health check passed" > /var/log/health.log
# ← 1 year later: still 1 line. Same information, less noise.
```

**Jagu:** *"`>>` is a red flag in any automated script. Ask yourself: 'Will this file grow forever?' If yes, it's not idempotent."*

### ❌ Mistake 2: Assuming Fresh State

```bash
# 😬 WRONG — Fails on second run
mkdir /app/uploads
# ← Second run: "File exists"

cp data.txt /app/uploads/
# ← Second run: overwrites silently (actually idempotent, but risky)

# ✅ RIGHT — Use idempotent alternatives
mkdir -p /app/uploads
# ← Succeeds every time

cp -n data.txt /app/uploads/
# ← -n = no overwrite. Only copies if target doesn't exist.
```

### ❌ Mistake 3: Not Testing the Second Run

```bash
# 😬 WRONG — Only test once
echo "Deploy script works!"
./deploy.sh  # ← works
# But did you test running it TWICE?

# ✅ RIGHT — Always test idempotency
echo "Testing idempotency..."
./deploy.sh
FIRST=$(md5sum /etc/myapp/config.json)
./deploy.sh
SECOND=$(md5sum /etc/myapp/config.json)
if [ "$FIRST" = "$SECOND" ]; then
  echo "✅ Idempotent: Second run produced identical state"
else
  echo "❌ NOT idempotent: State changed on second run"
fi
```

**Jagu:** *"The first rule of idempotency: test your script by running it twice. If the second run changes anything, you've got a bug. Most engineers never do this."*

---

## Interview Questions (5 min)

1. **Q: What does idempotency mean in the context of DevOps automation?**
   **A:** An operation is idempotent if running it multiple times produces the same result as running it once. In DevOps, this means your deployment scripts, configuration management, and infrastructure code can be applied repeatedly without side effects, duplicates, or errors.

2. **Q: Give an example of a non-idempotent operation and how to fix it.**
   **A:** `mkdir /tmp/logs` is non-idempotent — it fails if the directory already exists. The fix is `mkdir -p /tmp/logs`, which succeeds whether the directory exists or not. Another example: `echo "data" >> file` appends every time; `echo "data" > file` overwrites with the same content (idempotent).

3. **Q: How do declarative tools like Terraform and Kubernetes achieve idempotency?**
   **A:** They use a *reconciliation loop*: compare the current state against the desired state (defined in config), then only make changes to fix differences. If current state matches desired state, no changes are made. This makes `terraform apply` and `kubectl apply` idempotent — running them multiple times converges to the same state.

4. **Q: Why does idempotency matter for CI/CD pipelines?**
   **A:** Pipelines can fail, retry, or be re-triggered. Non-idempotent pipeline steps can create duplicate resources, corrupt state, or leave orphaned artifacts on retry. Idempotent steps are safe to retry — running the pipeline 10 times produces the same result as running it once. This is the foundation of reliable CI/CD (M03).

---

## Assignment (10 min)

**Task: Find and Fix Non-Idempotent Code**

**Golu:** *"This is the skill that separates safe automation from dangerous automation. Learn it now, thank yourself later."*

1. **Audit a script:** Take any shell script you've written (or use the `deploy-bad.sh` from the demo). Identify every line that is NOT idempotent. Circle the commands that use `>>`, `mkdir` (without `-p`), `cp` (without `-n`), or any `INSERT`-style operations.

2. **Fix it:** Rewrite the script so every operation is idempotent. Use `mkdir -p`, `>` instead of `>>`, check-before-act patterns, and idempotent flags.

3. **Prove it:** Run your fixed script 3 times. After each run, capture a checksum of the output state: `sha256sum /tmp/myapp-config/*`. Verify all 3 checksums match.

4. **Terraform example:** Read this Terraform snippet (even if you don't know Terraform):

   ```hcl
   resource "aws_s3_bucket" "data" {
     bucket = "my-app-data-2026"
     acl    = "private"
   }
   ```

   Why is this idempotent? (Hint: What happens when you `apply` it the second time? What does Terraform's state file do?)

5. **Research:** Find one DevOps tool that is NOT idempotent by design. Write 2-3 sentences about what breaks when it's run twice.

6. **🔥 Challenge (YouTube):** Write a Python or Bash script that:
   - Creates a file only if it doesn't exist
   - Appends data to the file only if that data isn't already there
   - Logs its actions to a file that never exceeds 100 lines (rotate by overwriting)
   - Run it 10 times. Prove the final state is the same as after 1 run.

**You know you're done when:** You can look at any shell command and tell immediately whether it's idempotent or not — and you know the fix.

---

## ✅ Concept Check (Micro-Assessment)

1. **Which command is idempotent?**
   - a) `mkdir /tmp/test`
   - b) `mkdir -p /tmp/test`
   - c) `echo "data" >> /tmp/test/log.txt`
   - d) `mv /tmp/a /tmp/b`
   - → **(b)** — `mkdir -p` succeeds whether the directory exists or not

2. **True or False:** A Terraform `apply` that creates an S3 bucket is idempotent.
   - → **True.** Terraform's state file tracks the bucket. The second `apply` detects "bucket exists with matching config" and makes zero changes.

3. **What does the reconciliation loop in Kubernetes do?**
   - a) Restarts the cluster every hour
   - b) Compares desired state to actual state and fixes differences
   - c) Logs all changes for the audit trail
   - d) Deletes and recreates all pods on every cycle
   - → **(b)** — the reconciliation loop is the continuous idempotent cycle

---

## 🧭 Now in Tech Terms

**Mapping the vending machine/elevator analogy to exact commands:**

| Analogy | Tech Term | Command/Tool |
|---------|-----------|-------------|
| "Pressing the elevator button again" | Idempotent operation | `mkdir -p`, `cp -n`, `terraform apply` |
| "Vending machine charging twice" | Non-idempotent operation | `mkdir` (no -p), `echo >>`, `INSERT` without UPSERT |
| "The elevator already on its way" | Reconciliation loop | Ansible check mode, Terraform plan |
| "Making your bed — doing it again doesn't make it messier" | Desired state convergence | `kubectl apply -f pod.yaml` |

---

## 📝 Closing Summary

| Item | |
|------|---|
| 🎯 **Takeaway** | Idempotency means running the same operation multiple times produces the same result. It's what makes automation *safe* to run on autopilot. |
| ❓ **Interview Question** | What is idempotency, and why does it matter for CI/CD pipelines and infrastructure automation? |
| 📝 **Assignment** | Audit a script for non-idempotent operations, fix them, and prove correctness with checksums |
| 🔥 **Challenge** | Write a log-rotation script that never exceeds 100 lines — idempotent by design |
| 💥 **Today's Mistake** | Never assume a script works just because it worked once. Always test the SECOND run — that's where idempotency bugs reveal themselves. |
| ⚡ **Pro Tip** | Before writing any automation, ask: "Will running this twice cause problems?" If yes, redesign to be idempotent. Add `-p`, `-n`, `--ignore-existing` flags as your first reflex. |

---

## 🔗 What's Next

- **M02/L03 — Immutability:** Replace, don't mutate. Why "cattle, not pets" is the production philosophy. How containers and Kubernetes deployments use immutability for reliable rollouts.
- **M02/L04 — The '…as Code' Spectrum:** Config as code, docs as code, policy as code, cost as code, guardrails as code.
- **M02/L05 — Change Discipline:** PR-driven change, review culture, approval gates, and auditable history.

> *"Idempotency is what makes automation boring. And boring automation is the best kind — it never surprises you at 3 AM."* — Jagu
