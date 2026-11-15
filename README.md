# SDP_SGA_2: Software Development Practice Assignment 2

**Student Name:** Rushabh Savla  
**Course:** Software Development Practice (SDP)  
**Public Repository:** [https://github.com/Rushabh-Savla/SDP_SGA_2](https://github.com/Rushabh-Savla/SDP_SGA_2)  
**Execution Environment:** WSL Ubuntu (Linux) / Bash Terminal  

---

## Repository Overview

This repository demonstrates an authentic Git version control workflow and test engineering design for **SDP SGA 2**, encompassing two core questions:

1. **Question 1: Git & GitHub Workflow (10 Marks):** Local repository setup, feature branching, merging, `.gitignore` configuration, dual undo demonstrations (`git reset` local reflog audit and `git revert` public commit), command documentation, and remote GitHub integration.
2. **Question 2: Test Case Design (10 Marks):** Formal test engineering design for a Login System covering 5 functional requirements through Equivalence Partitioning (EP), Boundary Value Analysis (BVA), and State Transition testing with independent fixtures.

---

## Project Structure

```text
SDP_SGA_2/
├── .git/                      # Local Git repository database
├── .gitignore                 # Exclusion rules for *.log and temp/ directory
├── index.html                 # Application landing page with responsive styling & #contact
├── about.html                 # Project information page with bidirectional navigation
├── git_commands.txt           # Catalog of all 22 Git commands with explanations
├── README.md                  # Project overview and verification documentation
├── app.log                    # Sample untracked log file (ignored by *.log rule)
├── debug.log                  # Sample untracked debug log (ignored by *.log rule)
├── error.log                  # Sample untracked error log (ignored by *.log rule)
└── temp/                      # Sample ignored directory (tested with temp/cache.tmp)
```

---

## Question 1: Git & GitHub Workflow Verification

### 1. Verified Commit History Topology
The repository contains genuine branching and merge history with non-fast-forward commits:
- `65e93e9`: Initial commit adding `index.html` and `about.html`.
- `b057cc8`: Second commit updating `about.html` with company bio and author information.
- `b6f7dc4`: Feature branch commit on `feature` introducing responsive navigation and service cards (Latest on `feature`: `b6f7dc4fdc25a891dc27e716013394742ba2310f`).
- `435227c`: Explicit merge commit merging `feature` into `main` (`--no-ff`).
- `6923688`: Added `.gitignore` excluding `*.log` and `temp/` folder.
- `52f5747`: Added `git_commands.txt` command documentation file.
- `be22f5c`: Navigation rectification adding `#contact` section and bidirectional navigation.
- `f06fab0`: Additional test commit for undo demonstration.
- `7bcaa2d`: Public undo commit via `git revert HEAD --no-edit` reverting `f06fab0`.
- `d424135`: Updated documentation and added `README.md`.
- `cb8f316`: Corrected command 10 syntax (`git merge <branch-name> --no-ff -m "<message>"`) and organized commands into Part 1 (executed workflow) and Part 2 (diagnostic/reference). Latest workflow commit on `main`: `cb8f31695839d196097a5716e2c75527f04b4a32`.

### 2. .gitignore Verification on Actual Files
The exclusion rules were verified using actual project files:
```bash
git check-ignore -v app.log temp/cache.tmp
# Output:
# .gitignore:2:*.log	app.log
# .gitignore:6:temp/	temp/cache.tmp

git status --ignored --short
# Output:
# !! app.log
# !! debug.log
# !! error.log
# !! temp/
```
Running `git status` confirms that ignored files remain untracked and the working tree remains clean.

### 3. Undo Demonstrations & Comparative Analysis
Two distinct undo paradigms are implemented and audited in this repository:

1. **Local Branch Reset (`git reset HEAD~1`):**
   - Audited from local Git reflog at `6fe18b2`:
     ```text
     6923688 HEAD@{Sun Nov 15 09:37:46 2026}: reset: moving to HEAD~1
     6fe18b2 HEAD@{Sun Nov 15 09:37:43 2026}: commit: Commit unwanted draft file (to test undo operation)
     ```
   - *Technical Mechanism:* The default mixed reset (`git reset HEAD~1`) moves the HEAD pointer to the preceding commit (`6923688`) and resets the staging index, leaving the modifications from `6fe18b2` in the working directory as unstaged changes. It does **not** automatically produce a clean working tree.
2. **Production-Grade Remote Revert (`git revert`):**
   - Implemented via `git revert HEAD --no-edit`, creating commit `7bcaa2d` which cleanly inverses temporary commit `f06fab0`.
   - *Comparison of `git reset` vs `git revert`:*
     - `git reset`: Rewinds branch pointers and modifies commit history. It is ideal for local, private branches prior to pushing. It must never be used on shared public branches because it rewrites history and requires dangerous force-pushing (`--force`).
     - `git revert`: Creates a new, forward-moving commit that records the mathematical inverse of a previous commit. It preserves the integrity of the commit history and is the standard, safe practice for public and collaborative branches.

---

## Question 2: Login System Test Case Specification (14 Scenarios)

The test suite covers the three inputs (**Username**, **Password**, **Login button**) and all five functional requirements (**FR-1** through **FR-5**) using Equivalence Partitioning (EP), Boundary Value Analysis (BVA), and State Transition testing.

### Specification Assumptions & Independent Test Account Fixtures
- **Test Account Fixtures:** To eliminate contradictory credentials and ensure complete scenario independence, five distinct test accounts are specified:
  - `user_nominal`: Registered with valid password `"SecurePass123"` (nominal valid length 13 > 6 chars) for baseline valid login and sequential lockout tracking.
  - `user_bva_min`: Registered with valid password `"Pass01"` (exact minimum boundary length = 6 chars) for BVA minimum testing.
  - `user_bva_strong`: Registered with valid password `"DevSecure2026"` (length 13 chars) for BVA strong testing.
  - `user_case`: Registered with valid password `"CasePass2026"` for dedicated case-sensitivity negative testing.
  - `user_recovery`: Registered with valid password `"DevSecurePass456"` for dedicated counter-reset verification following prior failures.
- **Security Attempt Counting:** Failed attempts are incremented only when authentication is evaluated (well-formed inputs with mismatched credentials). Validation failures (empty fields or passwords < 6 chars) are blocked at the client/form layer and do not increment the security lockout counter.
- **Counter Reset:** Any valid login immediately resets the consecutive failed attempt counter to 0.
- **Lockout Persistence:** Once locked after 3 consecutive failed attempts, the account transitions to `LOCKED` state in the database, and all subsequent login attempts are rejected immediately, bypassing password verification until unlocked by an authorized administrator.

---

## Verification Commands (WSL Ubuntu)

To independently inspect this repository in WSL Ubuntu:
```bash
# Clone or navigate to the repository
cd /mnt/d/Rushabh_Savla/Rushabh_Savla/SDP_SGA_2

# Inspect commit graph and branch topology
git log --oneline --graph --decorate --all

# Inspect reflog history for local reset audit
git reflog --date=local -10

# Verify .gitignore rules on actual ignored files
git check-ignore -v app.log temp/cache.tmp
git status --ignored --short

# Verify branches and remotes
git branch -a
git remote -v
git ls-remote origin
```
