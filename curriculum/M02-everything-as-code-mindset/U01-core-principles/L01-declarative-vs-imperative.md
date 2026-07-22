---
module: "M02"
unit: "Core principles"
lesson: "Declarative vs imperative"
status: reviewed            # draft | reviewed | recorded | published
est_minutes: 48          # Story(2) + Problem(3) + Visualization(2) + Theory(5) + Demo(13) + Mistakes(5) + Interview(5) + Assignment(10) + Concept Check(2) + Foundation(3)
---

# Declarative vs Imperative

> **Learning goal:** Understand the difference between telling a computer *what* you want (declarative) vs *how* to do it (imperative) — and why this distinction is the foundation of modern DevOps automation.

---

## 🎯 Goal

Build a mental model of declarative vs imperative thinking — so you can recognize which approach fits any task, and shift from "step-by-step scripting" to "desired-state definition" as your default mode.

## 🧠 Key Learnings

| # | Topic | Why It Matters |
|---|-------|-------------|
| 1 | Declarative vs imperative — the core distinction | Every DevOps tool choice depends on this. Terraform, Kubernetes, and Ansible are declarative; Bash and Python scripts are imperative. |
| 2 | The declaration spectrum | Not everything is 100% one or the other — understanding where you are on the spectrum helps you pick the right tool. |
| 3 | Reconciliation — the secret sauce | Declarative tools *compare* desired state to current state and fix differences. This is what makes them idempotent and drift-resistant. |
| 4 | Why "everything as code" starts here | The "as code" movement (config, policy, docs, cost, guardrails) is built on declarative thinking. This lesson plants the seed for the whole course. |

---

## 🏛️ Foundation Context (3 min)

### The Recipe That Doesn't Exist

**Golu:** *"Jagu, I've been SSHing into servers every day running commands. Isn't that just... how you do DevOps?"*

**Jagu:** *"Let me ask you something. If you won the lottery tomorrow and quit — could the person replacing you rebuild everything you've done exactly the same way?"*

**Golu:** *"...I mean, they have my bash history. Kind of?"*

**Jagu:** *"Bash history. That's not a recipe, Golu. That's a grocery receipt with half the items smudged. You need a recipe — a declarative, reproducible description of what the system should look like. That's what 'Everything as Code' means."*

**Fun fact:** The M02 module anchor is this: **If it isn't in Git, it doesn't exist** — like a recipe nobody wrote down; it dies with the cook.

---

## Story (2 min)

### The 47-Step Deployment

**Characters:** Golu (junior, types fast but doesn't document) and Jagu (senior, has the scars)

**Scene:** A Monday morning. The production server needs a new nginx config.

**Golu:** *"Alright, let me just SSH in, edit the config, restart nginx, done in 30 seconds."*

**Jagu:** *"Stop. Write it down first."*

**Golu:** *"Why? I know what I'm doing. It's just: `vim /etc/nginx/nginx.conf`, change the proxy_pass, `systemctl restart nginx`. Three steps."*

**Jagu:** *"Okay, show me the three steps. On paper."*

Golu writes them down.

**Jagu:** *"Now do it — but follow exactly what you wrote."*

Golu follows his own notes. Halfway through, he realizes he forgot a step — the firewall rule. Then he remembers he needs to test the config before restarting. Then he needs to check the logs after.

**Golu:** *"...Okay, it's actually six steps. And I forgot the rollback plan."*

**Jagu:** *"Right. And that's just you, today. What happens when you're on PTO and there's an incident? Or when you get promoted and someone else has to do this? Or when you have 50 servers instead of 1?"*

**Golu:** *"So you're saying... I should write the recipe before I cook?"*

**Jagu:** *"EXACTLY. The 'recipe' — the declarative description of what you want — goes in Git. Then anyone (or any automation) can apply it anywhere, anytime, and get the same result."*

> **Lesson:** Imperative = you describe every step. Declarative = you describe the *desired state* and let the system figure out the steps. DevOps runs on declarative.

---

## Real-world Problem (3 min)

### What Breaks Without Declarative Thinking?

- **"The On-Call Horror"** — Your teammate is paged at 3 AM. The runbook says "configure nginx." But it doesn't say *what* the config should look like. They guess. They get it wrong. The site stays down.
- **"The Golden Server"** — Server A was manually configured 2 years ago. Server B was set up last week using memory. They have different nginx versions, different firewall rules, and different logrotate settings. You discover this during a security audit. Fun times.
- **"The Deployed-at-Midnight"** — Someone SSHes in, makes a "quick fix," forgets to tell anyone. Next deployment overwrites the fix. Feature breaks. Blame game begins.
- **"The Audit from Hell"** — Compliance asks: "Show us every change made to the production environment in the last 90 days." You have SSH logs. Thousands of lines. No context. No approvals. Good luck.

### Topic Flow

```
🏛️ Foundation → 💥 Story → ❓ Problem → 👁️ Visualization → 📖 Theory → ⌨️ Demo → ⚠️ Mistakes → 🎤 Interview → 📝 Assignment
```

---

## Visualization (2 min)

### Two Doors — Same Destination

Draw this on a whiteboard:

```
🏪 IMPERATIVE APPROACH (The DIY route)
"You, worker: take 5 steps left, then 3 steps right, 
 then push the door, then walk through, then..."

   ┌─────────────────────────────────────┐
   │  1. SSH into server                 │
   │  2. Edit /etc/nginx/nginx.conf      │
   │  3. Run nginx -t (test config)      │
   │  4. systemctl restart nginx         │
   │  5. Check logs for errors           │
   │  6. curl localhost to verify        │
   └─────────────────────────────────────┘


🏪 DECLARATIVE APPROACH (The Uber route)
"I want to be at the restaurant at 8 PM."

   ┌─────────────────────────────────────┐
   │  Desired State:                     │
   │  nginx should be running            │
   │  on port 443 with TLS cert X        │
   │  proxying to backend Y              │
   │  firewall should allow port 443     │
   └─────────────────────────────────────┘
          │
          ▼
   System does the walking for you
          │
          ▼
   ┌─ Terraform / Ansible / Kubernetes ─┐
   │  "Reconcile the current state with │
   │   the desired state automatically" │
   └────────────────────────────────────┘
```

**Golu's Summary:** *"So imperative is like giving someone turn-by-turn directions. Declarative is like giving them a GPS destination. One is fragile, the other works even if the roads change."*

**Jagu:** *"And that's the entire DevOps mindset in one sentence."*

---

## Theory (5 min)

### Declarative vs Imperative — The Core Distinction

| Aspect | Imperative | Declarative |
|--------|-----------|-------------|
| **What you specify** | *How* to get there (the steps) | *What* the result should be (the state) |
| **Mental model** | A checklist of commands | A blueprint or recipe |
| **Idempotency** | Not guaranteed — run twice, may break | Inherent — "desired state" is the same every time |
| **Drift** | Guaranteed over time | Auto-corrected on next apply |
| **Auditability** | Poor — who remembers why they ran that command? | Excellent — the desired state is in Git |
| **Learning curve** | Low — feels natural ("type this, then this") | Higher — need to think in *desired outcomes* |
| **Examples** | Bash script, `apt-get install`, manual SSH | Terraform, Kubernetes YAML, Ansible playbooks, Dockerfile |
| **Best for** | Quick experiments, one-off tasks | Production infrastructure, repeatable deployments |

### The Declaration Spectrum

Not everything is purely one or the other. Think of it as a spectrum:

```
100% Imperative                    100% Declarative
     │                                    │
     ▼                                    ▼
  Bash script ─→ Python script ─→ Ansible ─→ Terraform ─→ Kubernetes
  (manual steps)  (automated steps) (desired state (desired infra (desired app
                                    + steps)      state)         state)
```

**Jagu's Insight:** *"The more declarative your tooling, the more the system does the 'how' for you. Your job shifts from 'typing the right commands' to 'defining the right state.' That's the career trajectory from junior to senior."*

### Why This Matters Right Now

| Tool | How It's Declarative |
|------|---------------------|
| **Terraform** | `resource "aws_instance" "web" { ami = "ami-..." }` — describes the server, not the provisioning steps |
| **Kubernetes** | `apiVersion: apps/v1` — `replicas: 3` — describes the desired number of pods, not how to create them |
| **Ansible** | `state: started` — describes the service state, not `systemctl start` |
| **Dockerfile** | `FROM`, `RUN`, `CMD` — describes the image layers, not the container runtime commands |

**Forward ref:** *"You'll use each of these tools in later modules — Terraform in M05, Kubernetes in M06-M07, Ansible in M05, Docker in M04. But the *mindset* — the declarative mindset — starts right here."*

---

## Live Demo (13 min)

**Jagu:** *"Open your terminal. We'll do the same thing two ways — imperative, then declarative. Feel the difference."*

### Step 1 — Imperative: Create a Config File Manually

```bash
# Create a directory
mkdir -p ~/demo-imperative

# Write a config file with echo (imperative)
echo "server {" > ~/demo-imperative/nginx.conf
echo "    listen 80;" >> ~/demo-imperative/nginx.conf
echo "    server_name example.com;" >> ~/demo-imperative/nginx.conf
echo "    root /var/www/html;" >> ~/demo-imperative/nginx.conf
echo "}" >> ~/demo-imperative/nginx.conf

# Show the result
cat ~/demo-imperative/nginx.conf
```

**Golu:** *"Okay, that's straightforward. I typed the steps, I got the file."*

**Jagu:** *"Now run the exact same commands again."*

```bash
# Run it again — what happens?
echo "server {" > ~/demo-imperative/nginx.conf
echo "    listen 80;" >> ~/demo-imperative/nginx.conf
echo "    server_name example.com;" >> ~/demo-imperative/nginx.conf
echo "    root /var/www/html;" >> ~/demo-imperative/nginx.conf
echo "}" >> ~/demo-imperative/nginx.conf
```

**Golu:** *"It... worked the same. So what's the problem?"*

**Jagu:** *"Now change one thing — add `index index.html;` after `root`. Do it from memory."*

**Golu:** *"Uh... I'd need to find the right line, edit it... I might mess up the syntax."*

**Jagu:** *"Exactly. Now imagine 50 servers. Imperative doesn't scale."*

### Step 2 — Declarative: Define the Desired State

```bash
# Create a declarative config file
mkdir -p ~/demo-declarative

cat > ~/demo-declarative/desired-nginx.conf << 'EOF'
# DESIRED STATE: This is what nginx should look like
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html;
}
EOF

cat ~/demo-declarative/desired-nginx.conf
```

**Jagu:** *"This file IS the desired state. It doesn't say 'create file, then write line 1, then line 2...' It just says 'this is how it should be.' That's declarative."*

### Step 3 — Compare: Apply to Multiple Servers

```bash
# Imperative — must repeat on each server
echo "For 5 servers, imperative = 5 SSH sessions × N commands each"
echo "Any mistake on any server = drift"

# Declarative — the config is THE config
echo "For 5 servers, declarative = 1 config file applied everywhere"
echo "The config is the source of truth. No drift possible."
```

**Golu:** *"Wait — so with declarative, I define it ONCE and it works everywhere? What if a server already has a different config?"*

**Jagu:** *"Great question! Real declarative tools (Terraform, Ansible, Kubernetes) do something called **reconciliation** — they compare the current state to the desired state and only change what's different. If a server already has the right config, it does nothing. That's called **idempotency**, and it's the next lesson."*

### Step 4 — See It in Action (Ansible Preview)

```bash
# Install ansible (if not present)
# This is a PREVIEW — full depth in M05
sudo apt update && sudo apt install -y ansible

# Create a simple declarative playbook
cat > ~/demo-declarative/ensure-nginx.yml << 'EOF'
---
- name: Ensure nginx is configured correctly
  hosts: localhost
  tasks:
    - name: Ensure nginx config file exists with correct content
      copy:
        dest: /tmp/declarative-demo.conf
        content: |
          server {
              listen 80;
              server_name example.com;
              root /var/www/html;
          }
    - name: Verify the file was created
      command: cat /tmp/declarative-demo.conf
      register: output
    - name: Show the result
      debug:
        msg: "Declarative config applied: {{ output.stdout_lines }}"
EOF

# Run it
ansible-playbook ~/demo-declarative/ensure-nginx.yml

# Run it AGAIN — see what happens?
ansible-playbook ~/demo-declarative/ensure-nginx.yml
```

**Golu:** *"Wait... I ran it twice and the output says 'ok=2' both times, but 'changed=1' only the first time!"*

**Jagu:** *"That's idempotency in action. The first run *changed* the system. The second run verified it was *already correct* and did nothing. That's the power of declarative + idempotent together."*

---

## Common Mistakes (5 min)

### ❌ Mistake 1: Shell Scripting Everything

**Golu's old habit:** *"I have a 200-line bash script that deploys my app. It works on my machine!"*

```bash
# 😬 WRONG — Fragile imperative script
ssh server "cd /app && git pull && npm install && systemctl restart app"
# What happens when git pull fails? It proceeds anyway.
# What happens on a different OS? npm might not exist.
# What happens on server #2? You forgot to change the hostname.

# ✅ RIGHT — Declarative approach
# A Terraform config or Kubernetes manifest describes the desired state
# The tool handles ordering, errors, and differences between environments
# The SAME config works on dev, stage, and prod (with different variables)
```

### ❌ Mistake 2: Confusing "Storing Files" with "Being Declarative"

```bash
# 😬 WRONG — This is just imperative commands stored in a file
echo "This isn't declarative, it's imperative-with-extra-steps"
echo "mkdir -p /app" >> deploy.sh
echo "cp app.jar /app/" >> deploy.sh
echo "systemctl restart app" >> deploy.sh

# ✅ RIGHT — A real declarative approach
# A Terraform resource definition:
# resource "aws_instance" "app" {
#   ami = "ami-..."
#   instance_type = "t3.medium"
#   user_data = file("app-init.sh")
# }
# This describes WHAT the server should be, not HOW to create it
```

**Jagu:** *"The trap is thinking 'I put my commands in a file, therefore it's as code.' No — declarative means you describe the *outcome*, not the *steps*. Putting imperative commands in a file is still imperative."*

### ❌ Mistake 3: Trying to Write Declarative Configs Without Understanding the Underlying System

```yaml
# 😬 WRONG — Copy-pasted Terraform without understanding
resource "aws_security_group" "web" {
  # Copied from a blog post, never verified
  ingress {
    from_port = 0
    to_port   = 0
    protocol  = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
# This allows ALL traffic from the internet. The declarative syntax is correct.
# The *intent* is wrong. Declarative doesn't mean safe.

# ✅ RIGHT — Know what you're declaring
resource "aws_security_group" "web" {
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]  # Only internal SSH
  }
}
```

**Jagu:** *"Declarative doesn't replace understanding. It replaces *typing*. You still need to know what a good config looks like. The tool just makes sure it's applied consistently."*

---

## Interview Questions (5 min)

1. **Q: What is the difference between declarative and imperative? Give an example of each in DevOps.**
   **A:** Imperative means specifying *how* to do something step by step (e.g., a bash script with sequential commands). Declarative means specifying *what* the result should be (e.g., a Terraform resource block describing an EC2 instance). The system figures out the steps to reach the desired state.

2. **Q: Why is declarative preferred for production infrastructure?**
   **A:** Three reasons: (1) **Idempotency** — applying the same config multiple times gives the same result. (2) **Auditability** — the desired state is in Git, making every change reviewable. (3) **Drift prevention** — tools automatically reconcile actual state with desired state.

3. **Q: Can you write declarative configurations without understanding the underlying system?**
   **A:** No. Declarative configs still require understanding what a correct configuration looks like. The declarative approach ensures *consistency*, not *correctness*. You still need to know that port 443 is for HTTPS and a security group open to `0.0.0.0/0` is dangerous.

4. **Q: What tools do you know that use a declarative approach?**
   **A:** Terraform/OpenTofu for infrastructure provisioning, Kubernetes manifests for container orchestration, Ansible playbooks for configuration management, Dockerfiles for container images, ArgoCD for GitOps deployments. Each of these lets you describe desired state rather than execution steps.

---

## Assignment (10 min)

**Task: Convert Imperative to Declarative**

**Golu:** *"This will feel weird at first — thinking in outcomes instead of steps. But it's the single most important mental shift in this entire course."*

1. **Write an imperative deployment:** Write a bash script (5-10 lines) that sets up a simple web server: create a directory, write an index.html, install nginx, start it, verify it's running.

2. **Write the declarative version:** Don't run anything — just write a file called `desired-state.txt` that describes what the server *should look like* without any commands. Use plain English.

3. **Compare:** Label each command in your bash script as either "what" (declarative) or "how" (imperative). Count which one you have more of.

4. **Translation challenge:** Take this runbook — "SSH into server, run `apt-get install nginx`, edit config, restart" — and rewrite it as a declarative desired state description. What does "done" look like?

5. **Research:** Find one example of a declarative config file online (Terraform, Kubernetes, Ansible, or Docker). Save it to `~/devops-lab/m01/declarative-example.txt`. Write 2-3 sentences explaining what state it describes.

6. **🔥 Challenge (YouTube):** Take your imperative script from step 1 and try to write a simple Ansible playbook that does the same thing declaratively. Run `ansible-playbook` — note whether the second run reports "changed=0" (idempotent success).

   > **Note:** This step builds on the Ansible preview demo above. If the demo went well, try adapting it. If you're stuck, just write the desired-state description (step 2) — that's the core skill. The Ansible syntax comes in M05.

**You know you're done when:** You can explain the difference between declarative and imperative to someone else in 30 seconds using a non-technical analogy (recipe, GPS, ordering food).

---

## ✅ Concept Check (Micro-Assessment)

1. **Which of these is declarative?**
   - a) `echo "server { listen 80; }" > nginx.conf`
   - b) `resource "aws_instance" "web" { ami = "ami-..." }`
   - c) `ssh server "systemctl restart nginx"`
   - d) `mkdir -p /var/www && cp index.html /var/www/`
   - → **(b)** — it describes the desired state, not the steps

2. **True or False:** Storing your bash commands in a Git repo makes them declarative.
   - → **False.** Declarative is about *what* vs *how*, not about where the file lives.

3. **What does "reconciliation" mean in a declarative system?**
   - a) The system asks for confirmation before making changes
   - b) The system compares current state to desired state and fixes differences
   - c) The system logs all changes for auditing
   - d) The system deletes everything and starts fresh
   - → **(b)** — reconciliation is the core loop of declarative tools

---

## 🧭 Now in Tech Terms

**Mapping the analogy to exact commands:**

| Analogy | Tech Term | Command/Tool |
|---------|-----------|-------------|
| "Recipe" | Declarative desired state | Terraform `.tf`, Kubernetes `.yaml`, Ansible `.yml` |
| "Turn-by-turn directions" | Imperative steps | Bash script, SSH commands |
| "The GPS destination" | Resource definition | `resource "aws_instance" "web" {}` |
| "GPS recalculating route" | Reconciliation loop | `terraform plan`, `kubectl diff`, Ansible check mode |

---

## 📝 Closing Summary

| Item | |
|------|---|
| 🎯 **Takeaway** | Declarative = describe *what* you want, not *how* to get there. It's the foundation of scalable, auditable, drift-resistant infrastructure. |
| ❓ **Interview Question** | What's the difference between declarative and imperative, and why does DevOps prefer declarative? |
| 📝 **Assignment** | Convert an imperative deployment script into a declarative desired-state description |
| 🔥 **Challenge** | Write an Ansible playbook that's idempotent — run it twice, only one change on the first run |
| 💥 **Today's Mistake** | Don't confuse "storing commands in a file" with "being declarative." Declarative is about *what*, not *where*. |
| ⚡ **Pro Tip** | Before writing any deployment code, pause and ask: "Am I describing the *desired state* or the *steps to get there*?" The answer determines your tooling choice. |

---

## 🔗 What's Next

- **M02/L02 — Idempotency:** Running the same config 5 times gives the same result. The superpower that makes automation safe.
- **M02/L03 — Immutability:** Replace, don't mutate. Why "cattle, not pets" is the production philosophy.
- **M02/L04 — The '…as Code' Spectrum:** Config as code, docs as code, policy as code, guardrails as code.
- **M02/L05 — Change Discipline:** PR-driven change, review culture, and why everything needs an audit trail.

> *"The imperative approach gets you hired. The declarative mindset gets you promoted."* — Jagu
