# github-final-project

Final capstone project for IBM's **Introduction to Git and GitHub** course, demonstrating both GitHub UI-based open-source project setup and Git CLI-based collaborative workflows across two connected repositories.

## Simple Interest Calculator

A calculator that calculates simple interest given principal, annual rate of interest and time period in years.

```
Input:
   p, principal amount
   t, time period in years
   r, annual rate of interest
Output
   simple interest = p*t*r/100
```

Run it with:
```bash
bash simple-interest.sh
```

## What this project demonstrates

This capstone is split into two parts, covering the two primary ways developers interact with Git and GitHub: the web UI and the command line.

### Part 1 — Open-source project setup (GitHub UI)

Built this repository from scratch using GitHub's web interface, following standard open-source project conventions:

- **[README.md](./README.md)** — this file, describing the project's purpose and usage
- **[LICENSE](./LICENSE)** — Apache License 2.0
- **[CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md)** — Contributor Covenant template
- **[CONTRIBUTING.md](./CONTRIBUTING.md)** — contribution guidelines for the community
- **[simple-interest.sh](./simple-interest.sh)** — the actual Bash script, committed directly to `main`

### Part 2 — Collaborative Git CLI workflow

Using a separate, shared repository ([mcino-Introduction-to-Git-and-GitHub](https://github.com/Book0fEli/mcino-Introduction-to-Git-and-GitHub)), practiced the full fork-branch-commit-merge-revert-pull request cycle entirely from the command line:

**1. Forked the upstream repository** and verified the fork relationship via the GitHub API:
```bash
curl -s https://api.github.com/repos/Book0fEli/mcino-Introduction-to-Git-and-GitHub | jq -r '.parent.clone_url'
```

**2. Cloned the fork locally and created a fix branch:**
```bash
git clone https://github.com/Book0fEli/mcino-Introduction-to-Git-and-GitHub.git
git checkout -b bug-fix-typo
```

**3. Fixed a typo in the README footer** (`2022 XYZ, Inc.` to `2023 XYZ, Inc.`), committed it, and pushed the branch using a Personal Access Token for authentication (GitHub no longer supports password authentication for Git operations):
```bash
git add README.md
git commit -m "Fix copyright year typo in footer"
git push --set-upstream origin bug-fix-typo
```

**4. Merged the fix into `main`:**
```bash
git checkout main
git merge bug-fix-typo
```

**5. Created a second branch to revert the change**, using `git revert` on the specific commit rather than deleting history:
```bash
git checkout -b bug-fix-revert
git revert <commit-hash>
git push --set-upstream origin bug-fix-revert
```

**6. Opened a pull request** from the `bug-fix-revert` branch on the fork back to the original upstream repository's `main` branch: [PR #11349](https://github.com/ibm-developer-skills-network/mcino-Introduction-to-Git-and-GitHub/pull/11349)

**7. Verified the PR and branch state via the GitHub API and CLI:**
```bash
curl -s https://api.github.com/repos/ibm-developer-skills-network/mcino-Introduction-to-Git-and-GitHub/pulls/11349 | jq -r '.head.repo.clone_url'
git branch -vv
```

## Lessons learned

A few real-world snags came up along the way, which ended up being some of the most useful parts of the exercise:

- **GitHub deprecated password authentication for Git operations** — pushing over HTTPS requires a Personal Access Token instead of an account password.
- **A "fast-forward" merge and an ordinary merge look different in the terminal**, but both correctly update the target branch — worth recognizing both outputs rather than assuming something went wrong when "Already up to date" appears unexpectedly.
- **Cloud lab environments that reset between sessions** can leave a local repository out of sync with what's actually on GitHub — `git fetch` and `git pull` are essential for reconciling local state with the remote source of truth.
- **`git revert` creates a new commit that undoes a previous one**, rather than deleting history — this keeps a clean, auditable trail of exactly what changed and why, which matters in any real collaborative repository.

## Tech used

`Git` · `GitHub` (UI + CLI + REST API) · `Bash` · `Personal Access Tokens` · `Open Source project conventions`

---

*Elijah Cordova — working toward a DevOps/Cloud Engineering role. Capstone project for IBM's Introduction to Git and GitHub course.*
