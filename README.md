# SDP_SGA_2: Software Development Practice Assignment 2

**Student Name:** Rushabh Savla  
**Course:** Software Development Practice (SDP)  
**Public Repository:** [https://github.com/Rushabh-Savla/SDP_SGA_2](https://github.com/Rushabh-Savla/SDP_SGA_2)  
**Execution Environment:** WSL Ubuntu (Linux) / Bash Terminal  

---

## Repository Overview

This repository demonstrates a complete, auditable Git version control workflow and test engineering design for **SDP SGA 2**, encompassing two core questions:

1. **Question 1: Git & GitHub Workflow (10 Marks):** Local repository setup, feature branching, merging, `.gitignore` configuration, dual undo demonstrations (`git reset` and `git revert`), command documentation, and remote GitHub integration.
2. **Question 2: Test Case Design (10 Marks):** Formal test engineering design for a Login System covering 5 functional requirements through Equivalence Partitioning (EP), Boundary Value Analysis (BVA), and State Transition testing.

---

## Project Structure

```text
SDP_SGA_2/
├── .git/                      # Local Git repository database
├── .gitignore                 # Exclusion rules for *.log and temp/ directory
├── index.html                 # Application landing page with responsive styling
├── about.html                 # Project information page with navigation bar
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
- `435227c`: Explicit merge commit merging `feature` into `main`.
- `6923688`: Added `.gitignore` excluding `*.log` and `temp/` folder.
- `52f5747`: Added `git_commands.txt` command documentation file.
- `be22f5c`: Navigation rectification adding `#contact` section and bidirectional navigation.
- `f06fab0`: Additional test commit for undo demonstration.
- `7bcaa2d`: Public undo commit via `git revert HEAD --no-edit` reverting `f06fab0`.

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

## Question 2: Login System Test Case Specification

| Test Case ID | Test Scenario | Input Data | Expected Output |
| :--- | :--- | :--- | :--- |
| **TC_01** | Valid Login (Nominal Case) | Username = "rushabh_user", Password = "SecurePass123", Click [Login] | Login successful. Session token issued; user redirected to Dashboard page (`/dashboard`); welcome banner displayed. |
| **TC_02** | Edge Case &mdash; Empty Username | Username = "" (blank), Password = "Password@123", Click [Login] | Login fails. Inline error displayed: *"Username must not be empty."* Submission blocked; focus remains on Username input. |
| **TC_03** | Edge Case &mdash; Empty Password | Username = "rushabh_user", Password = "" (blank), Click [Login] | Login fails. Inline error displayed: *"Password must be at least 6 characters long."* Submission blocked; focus set to Password input. |
| **TC_04** | Edge Case &mdash; Both Fields Empty | Username = "", Password = "", Click [Login] | Login fails. Dual errors displayed: *"Username must not be empty."* and *"Password must be at least 6 characters long."* No network request sent. |
| **TC_05** | Boundary Value &mdash; Password &lt; 6 Chars (N - 1) | Username = "rushabh_user", Password = "Ab123" (5 characters), Click [Login] | Login fails. Error message displayed: *"Password must be at least 6 characters long (5 characters provided)."* Password field highlighted in red. |
| **TC_06** | Boundary Value &mdash; Password == 6 Chars (N) | Username = "rushabh_user", Password = "Pass01" (6 characters), Click [Login] | Login successful. Validation passes minimum length threshold; user is authenticated and redirected to Dashboard (`/dashboard`). |
| **TC_07** | Invalid Scenario &mdash; Non-Existent Username | Username = "unregistered_99", Password = "ValidPass123", Click [Login] | Login fails. Error message displayed: *"Invalid username or password. Please verify your credentials and try again."* Password field cleared. |
| **TC_08** | Invalid Scenario &mdash; 1st Failed Login Attempt | Username = "rushabh_user", Password = "WrongPass_1", Click [Login] | Login fails. Error message displayed: *"Invalid username or password. 2 attempts remaining before account lockout."* Failed counter increments to 1. |
| **TC_09** | Invalid Scenario &mdash; 2nd Consecutive Failed Attempt | Username = "rushabh_user", Password = "WrongPass_2", Click [Login] | Login fails. Warning displayed: *"Invalid username or password. Warning: 1 attempt remaining before account lockout."* Failed counter increments to 2. |
| **TC_10** | Account Lockout &mdash; 3rd Consecutive Failed Attempt | Username = "rushabh_user", Password = "WrongPass_3", Click [Login] | Login fails. Critical alert displayed: *"Your account has been locked due to 3 consecutive failed login attempts. Please contact support or reset password."* Account status updated to LOCKED. |
| **TC_11** | Post-Lockout Behavior on Locked Account | Username = "rushabh_user", Password = "SecurePass123" (valid), Click [Login] | Login fails immediately. Error displayed: *"Access Denied: Account is currently locked. Authentication disabled. Please contact administrator to unlock."* |
| **TC_12** | Counter Reset on Successful Login | Step 1: Wrong password (counter = 1)<br>Step 2: Correct password "SecurePass123", Click [Login] | Login successful on Step 2. User redirected to Dashboard; failed login counter automatically resets from 1 back to 0. Subsequent failures start at attempt 1. |

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
