# 🧪 Lab: Convert a Manual Runbook to a Reviewed Git Repo

> **Goal:** Take a manual server setup process and move it entirely into a version-controlled, reviewed Git repository with a PR template and CODEOWNERS.

## Target Objectives

- Understand the difference between imperative (manual steps) and declarative (desired state)
- Create a Git repository with proper structure for operations documentation
- Write a PR template that enforces review quality
- Configure CODEOWNERS for accountability
- Experience the PR review workflow end-to-end

---

## Prerequisites

- A GitHub account (free tier)
- Git installed on your local machine (`git --version`)
- Basic familiarity with: `git init`, `git add`, `git commit`, `git push`

---

## Hands-on Challenges

### 1. Set Up the Repo

Create a new repository on GitHub called `server-runbook` (or similar). **Clone it locally** — don't use `git init` because you need the remote to push to:

```bash
# Clone (this creates the directory + adds the remote automatically)
git clone https://github.com/YOUR-USERNAME/server-runbook.git
cd ~/server-runbook

# Create the directory structure
mkdir -p runbooks templates
```

> 💡 **Why clone instead of init?** `git clone` sets up the remote (`origin`) automatically. If you use `git init`, you'll need `git remote add origin <url>` before you can push. Using clone avoids this extra step.

### 2. Write the Imperative Runbook

Create `runbooks/new-server-setup.md` — a step-by-step guide for setting up a new Ubuntu web server. Write it the way you'd actually do it manually:

```markdown
# New Server Setup Runbook

## Steps
1. SSH into the server as root
2. Run `apt update && apt upgrade -y`
3. Install nginx: `apt install -y nginx`
4. Create config file at `/etc/nginx/sites-available/myapp`
5. Enable the site: `ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/`
6. Test config: `nginx -t`
7. Restart nginx: `systemctl restart nginx`
8. Verify: `curl http://localhost`
```

**Now ask yourself:** Would someone following this at 3 AM get it right? What's missing? (Ports, firewall rules, TLS cert, monitoring checks, rollback plan?)

### 3. Write the Declarative Desired State

Create `runbooks/desired-state.md` — describe what the server SHOULD look like when setup is complete. No commands. Just the end state:

```markdown
# Desired State: Production Web Server

## What "Done" Looks Like
- nginx 1.24+ is installed and running
- Port 443 is open with a valid TLS certificate
- Port 80 redirects to 443
- The app is served from `/var/www/myapp`
- Logs rotate daily, kept for 30 days
- Monitoring checks pass (HTTP 200 on /health)
- Uptime is reported to the observability dashboard
```

### 4. Create a PR Template

Create `PULL_REQUEST_TEMPLATE.md` at the repo root:

```markdown
# Pull Request — Runbook Change

## Description
<!-- What changed and why -->

## Type of Change
- [ ] New runbook
- [ ] Update to existing runbook
- [ ] Fix (typo, broken step, missing detail)

## Review Checklist
- [ ] All steps are numbered and unambiguous
- [ ] Every command includes expected output or error handling
- [ ] Rollback steps are documented (if applicable)
- [ ] Security considerations noted (ports, keys, credentials)
- [ ] Someone new to the team could follow this at 2 AM

## Testing
- [ ] Steps verified on a clean Ubuntu 22.04 system
```

### 5. Add CODEOWNERS + Branch Protection

Create a `CODEOWNERS` file at the repo root:

```bash
# Global owner for all runbooks
* @YOUR-GITHUB-USERNAME
```

> ⚠️ Replace `@YOUR-GITHUB-USERNAME` with your actual GitHub handle. The `CODEOWNERS` file uses GitHub usernames — if the user doesn't exist, GitHub will reject it.

Now enable **branch protection** on GitHub (this enforces the review culture you're learning):
1. Go to your repo → **Settings** → **Branches** → **Add branch protection rule**
2. Branch name pattern: `main`
3. Check: **Require a pull request before merging**
4. Check: **Require approvals** (set to 1)
5. Click **Create**

> 💡 Without branch protection, you could bypass the entire PR workflow by pushing directly to `main`. Protection rules make the review culture *mandatory*, not optional.

### 6. Add .gitignore

Create a `.gitignore` file to keep secrets and logs out of your runbook repo:

```bash
# .gitignore — runbooks should never contain secrets
*.log
.env
node_modules/
*.pem
*.key
.DS_Store
```

### 7. Commit, Push, Review

```bash
# Stage everything
git add .
git commit -m "feat: initial runbook with PR template, CODEOWNERS, and gitignore"

# Create a branch and make a change
git checkout -b improve-setup-steps
# ... edit runbooks/new-server-setup.md with improvements ...

git add runbooks/new-server-setup.md
git commit -m "fix: add firewall and TLS steps to server setup"
git push origin improve-setup-steps
```

Now open a **Pull Request** on GitHub. Add a reviewer (a classmate, or yourself). Your PR template will auto-populate. Review the diff. Add comments. Approve. Merge.

### 8. 🔥 Challenge: Add a Second Runbook

Create a second runbook — `runbooks/deploy-rollback.md` — documenting the deploy and rollback process. Use the PR template to get it reviewed and merged. Make sure CODEOWNERS requires your review.

---

## Proof of Work

Save your findings in `solution/m01-lab-report.md`:

- The URL of your GitHub repo
- The PR template you created (copy the content)
- The CODEOWNERS file you configured
- The PR URL (or screenshot of the merged PR)
- A 3-sentence reflection: "What was different about describing desired state vs writing steps?"

Commit and push when done.

---

## Acceptance Criteria

You're done when:

- ✅ A GitHub repo exists with the runbook, PR template, and CODEOWNERS
- ✅ At least one PR was opened, reviewed, and merged
- ✅ The `desired-state.md` file contains zero commands — only descriptions of the end state
- ✅ You can explain why the PR template makes reviews more consistent

## Learn in Public

Share your repo on LinkedIn with **#LearnDevOpsIn90Days** and tag what surprised you about writing desired-state vs step-by-step instructions!
