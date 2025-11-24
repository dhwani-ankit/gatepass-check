# Release Notes Guide - Getting Proper Formatted Releases

## 🎯 The Problem

When releases show merge commit messages like "Merge pull request #8" instead of actual changes, it's because:

1. **Individual commits in PR don't follow semantic format**
2. **PR merge method loses semantic commit information**
3. **Squash merge creates a single commit without semantic format**

## ✅ Solution: Proper PR Merging

### Option 1: Create a Merge Commit (Recommended)

**Best for preserving individual commits:**

1. When merging PR, select **"Create a merge commit"**
2. This preserves all individual commits
3. Each commit must follow semantic format: `feat:`, `fix:`, etc.

**Example:**
```
PR has commits:
- feat: add user authentication
- fix: resolve login bug

After merge → Release shows:
✨ Features
- add user authentication

🐛 Bug Fixes  
- resolve login bug
```

### Option 2: Squash and Merge

**If you use squash merge:**

1. When merging, select **"Squash and merge"**
2. **IMPORTANT:** Edit the squash commit message to follow semantic format
3. Format: `feat: description` or `fix: description`

**Example:**
```
Squash commit message should be:
feat: add user authentication and fix login bug

NOT:
Merge branch 'ankit' into main
```

### Option 3: Rebase and Merge

**Best for clean history:**

1. Select **"Rebase and merge"**
2. Ensures all commits follow semantic format
3. Preserves individual commit messages

## 📝 Ensuring Commits Follow Semantic Format

### In Your PR Branch:

Before merging, check your commits:

```bash
# View commits in your PR branch
git log --oneline origin/main..HEAD

# Should see:
# feat: add new feature
# fix: resolve bug
# NOT:
# added new feature
# fixed bug
```

### Fixing Commit Messages:

If commits don't follow format, fix them:

```bash
# Interactive rebase to fix last 3 commits
git rebase -i HEAD~3

# Change 'pick' to 'reword' for commits to fix
# Edit commit messages to follow semantic format
```

## 🎨 What Good Release Notes Look Like

### ✅ Good Release:

```
## v1.1.0 (2025-01-XX)

✨ Features
- add user authentication system ([`abc1234`](link))
- implement password reset functionality ([`def5678`](link))

🐛 Bug Fixes
- resolve login session timeout issue ([`ghi9012`](link))
- fix token expiration handling ([`jkl3456`](link))
```

### ❌ Bad Release:

```
## v2.0.0

Merge pull request #8 from dhwani-ankit/ankit
fix: verify release bot configuration
```

## 🔧 Quick Fix for Existing Releases

If you already have releases with bad notes:

1. **Delete the bad release** (if it's recent and not used)
2. **Fix commit messages** in your branch
3. **Create a new commit** with proper format:
   ```bash
   git commit --amend -m "fix: proper commit message"
   git push origin main --force-with-lease
   ```
4. **Trigger new release** - it will use the fixed commits

## 📋 Checklist Before Merging PR

- [ ] All commits in PR follow semantic format (`feat:`, `fix:`, etc.)
- [ ] Using "Create a merge commit" or "Rebase and merge"
- [ ] If using squash merge, squash commit message follows semantic format
- [ ] Commit messages are descriptive and clear

## 🚀 Best Practices

1. **Write semantic commits from the start:**
   ```bash
   git commit -m "feat: add new feature"
   git commit -m "fix: resolve bug"
   ```

2. **Use conventional commit format:**
   ```
   type(scope): description
   
   Examples:
   feat(auth): add login functionality
   fix(api): handle null responses
   ```

3. **Review commits before pushing:**
   ```bash
   git log --oneline -5
   ```

4. **Fix commits before merging:**
   ```bash
   # If needed, amend last commit
   git commit --amend -m "feat: proper message"
   ```

## 🎯 Example Workflow

```bash
# 1. Create feature branch
git checkout -b feat/add-authentication

# 2. Make changes and commit with semantic format
git add .
git commit -m "feat: add user authentication"
git commit -m "fix: resolve login validation bug"

# 3. Push and create PR
git push origin feat/add-authentication

# 4. When merging PR:
# - Select "Create a merge commit"
# - Or "Rebase and merge"
# - If squash, edit message to: "feat: add user authentication and fix login validation"

# 5. Release will automatically show:
# ✨ Features
# - add user authentication
# 🐛 Bug Fixes
# - resolve login validation bug
```

---

**Remember:** The release bot reads commit messages. If commits don't follow semantic format, release notes won't look good!

