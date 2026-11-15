# SDP_SGA_2: Software Development Practice Assignment 2

**Student Name:** Rushabh Savla  
**Course:** Software Development Practice (SDP)  
**Public Repository:** [https://github.com/Rushabh-Savla/SDP_SGA_2](https://github.com/Rushabh-Savla/SDP_SGA_2)  
**Execution Environment:** WSL Ubuntu (Linux) / Bash Terminal  

---

## Repository Overview

This repository demonstrates an authentic Git version control workflow and test engineering design for **SDP SGA 2**, encompassing two core questions:

1. **Question 1: Git & GitHub Workflow (10 Marks):** Local repository setup, feature branching, merging, `.gitignore` configuration, dual undo demonstrations (`git reset` local reflog audit and `git revert` public commit), command documentation, and remote GitHub integration.
2. **Question 2: Test Case Design (10 Marks):** Formal test engineering design for a Login System covering 5 functional requirements through Equivalence Partitioning (EP), Boundary Value Analysis (BVA), and State Transition testing.

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
└── temp/                      # Sample ignored directory (tested with temp/cache.tmp)
```

---

## Question 1: Git & GitHub Workflow Verification

### 1. Commit History Topology
The repository contains genuine branching and merge history with non-fast-forward commits:
- `65e93e9`: Initial commit adding `index.html` and `about.html`.
- `b057cc8`: Second commit updating `about.html` with company bio and author information.
- `b6f7dc4`: Feature branch commit on `feature` introducing responsive navigation and service cards.
- `435227c`: Explicit merge commit merging `feature` into `main` (`--no-ff`).
- `6923688`: Added `.gitignore` excluding `*.log` and `temp/` folder.
- `52f5747`: Added `git_commands.txt` command documentation file.
- `be22f5c`: Navigation rectification adding `#contact` section and bidirectional navigation.
- `f06fab0`: Additional test commit for undo demonstration.
- `7bcaa2d`: Public undo commit via `git revert HEAD --no-edit` reverting `f06fab0`.
- `d424135`: Updated documentation and added `README.md`.

### 2. .gitignore Verification
The `.gitignore` rules can be verified using `git check-ignore`:
```bash
git check-ignore -v sample.log
# Output: .gitignore:2:*.log	sample.log

git check-ignore -v temp/sample.txt
# Output: .gitignore:6:temp/	temp/sample.txt
```

### 3. Undo Demonstration Audit
Two distinct undo paradigms are verified:
1. **Local Branch Reset (`git reset HEAD~1`):** Recorded in local Git reflog at `6fe18b2`, proving that commit `6fe18b2` was successfully rewound to `6923688` while preserving working directory cleanliness.
2. **Remote-Safe Revert (`git revert`):** Commit `f06fab0` was reversed on `main` via `7bcaa2d`, providing a transparent, auditable undo node visible on the public GitHub repository.

---

## Question 2: Login System Test Case Specification (14 Scenarios)

The test suite covers the three inputs (**Username**, **Password**, **Login button**) and all five functional requirements (**FR-1** through **FR-5**) using Equivalence Partitioning (EP), Boundary Value Analysis (BVA), and State Transition testing.

### Specification Assumptions & Test Account Fixtures
- **Test Account Fixtures:** To ensure logical consistency across boundary tests, distinct accounts are used for tests requiring valid authentication:
  - Account 1: `user_nominal` with registered valid password `"SecurePass123"` (nominal length 13 > 6 chars)
  - Account 2: `user_bva_min` with registered valid password `"Pass01"` (exact minimum boundary length = 6 chars)
  - Account 3: `user_bva_strong` with registered valid password `"DevSecure2026"` (length 13 chars)
- **Security Attempt Counting:** Failed attempts are incremented only when authentication is evaluated (valid-format inputs submitted with mismatched credentials). Validation failures (empty fields or passwords < 6 chars) are blocked at the client/form layer and do not increment the security counter.
- **Counter Reset:** Any valid login immediately resets the consecutive failed attempt counter to 0.
- **Lockout Persistence:** Once locked after 3 consecutive failed attempts, all subsequent login attempts are rejected immediately, bypassing password verification until unlocked by an administrator.

---

## Verification Commands (WSL Ubuntu)

To independently inspect this repository in WSL Ubuntu:
```bash
# Clone the repository
git clone https://github.com/Rushabh-Savla/SDP_SGA_2.git
cd SDP_SGA_2

# Inspect commit graph and branch topology
git log --oneline --graph --decorate --all

# Inspect reflog history for local reset audit
git reflog --date=local

# Verify .gitignore rules
git check-ignore -v sample.log temp/sample.txt

# Verify branches
git branch -a

# Inspect remote configuration
git remote -v
```
