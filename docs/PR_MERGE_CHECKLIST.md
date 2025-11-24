# PR Merge Checklist for Main Branch

When merging a PR from any branch (like `ankit`) to `main`, follow this checklist to ensure everything works correctly:

## 🔍 Pre-Merge Checklist

### 1. **Commit Message Format** ⚠️ CRITICAL
   - ✅ Ensure all commits follow **semantic commit format**
   - ✅ Format: `type(scope): description`
   - ✅ Valid types: `feat`, `fix`, `perf`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`, `build`
   - ✅ Example: `feat: add user authentication` or `fix: resolve login bug`

   **Why?** The release bot (`dhwani-release-bot`) analyzes commit messages to determine:
   - `feat:` → Creates **minor** release (1.0.0 → 1.1.0)
   - `fix:` → Creates **patch** release (1.0.0 → 1.0.1)
   - `BREAKING CHANGE:` → Creates **major** release (1.0.0 → 2.0.0)

   **If commits don't follow format:**
   - No release will be created automatically
   - You'll need to manually trigger release or fix commit messages

### 2. **CI/CD Checks**
   - ✅ All CI checks are **passing** (green checkmarks)
   - ✅ Code quality checks passed
   - ✅ Security scans completed
   - ✅ Test coverage meets requirements
   - ✅ No merge conflicts

### 3. **Code Review**
   - ✅ `dhwani-ankit` has reviewed and approved (auto-requested)
   - ✅ All required reviewers approved
   - ✅ All review comments addressed

### 4. **DevOps Checklist** (in PR description)
   - ✅ Pre-Merge Checklist completed
   - ✅ Release Checklist completed (if applicable)
   - ✅ Deployment Checklist completed (if applicable)

### 5. **Documentation**
   - ✅ Code is documented
   - ✅ CHANGELOG.md updated (if needed)
   - ✅ Breaking changes documented (if any)

## 🚀 What Happens After Merge

### Automatic Actions:

1. **Release Bot Triggers** (`dhwani-release-bot`)
   - Workflow runs automatically on push to `main`
   - Analyzes commits since last release
   - If semantic commits found → Creates release automatically
   - If no semantic commits → No release (workflow completes silently)

2. **Release Creation Process:**
   ```
   Merge PR → Push to main → Release workflow runs
   → Analyzes commits → Determines version
   → Creates formatted release notes
   → Creates git tag (v1.1.0)
   → Creates GitHub Release
   → Updates CHANGELOG.md
   → Commits back to repository
   ```

3. **Codecov Upload**
   - Coverage reports uploaded automatically
   - Coverage status shown in PR

## 📝 Step-by-Step Merge Process

### Option 1: Merge Commit (Recommended)
```bash
# On GitHub:
1. Click "Merge pull request"
2. Select "Create a merge commit"
3. Click "Confirm merge"
```

### Option 2: Squash and Merge
```bash
# If using squash merge:
1. Ensure the squash commit message follows semantic format
2. Format: "feat: description" or "fix: description"
3. Click "Squash and merge"
```

### Option 3: Rebase and Merge
```bash
# If using rebase merge:
1. Ensure all commit messages follow semantic format
2. Click "Rebase and merge"
```

## ⚠️ Important Notes

### If Release Doesn't Appear:

1. **Check Commit Messages:**
   ```bash
   # View commits that will be analyzed
   git log --oneline -10
   ```
   - Commits must follow: `feat:`, `fix:`, `perf:`, etc.
   - Commits like `chore:`, `docs:`, `style:` won't trigger releases

2. **Check Workflow Logs:**
   - Go to Actions → "Generate Semantic Release"
   - Look for: "No release will be created"
   - Check debug output for commit analysis

3. **Manual Trigger:**
   - Comment: `@dhwani-release-bot release` in the PR
   - Or: Actions → "Generate Semantic Release" → "Run workflow"

### If Code Reviewer Not Added:

1. **Check Workflow:**
   - Go to Actions → "Auto Request Review"
   - Check logs for errors

2. **Manual Add:**
   - Click "Reviewers" in PR
   - Add `dhwani-ankit` manually

### If Codecov Not Working:

1. **Check Coverage File:**
   - Ensure `coverage.xml` is generated
   - Check CI workflow logs

2. **Check Token:**
   - Verify `CODECOV_TOKEN` secret exists
   - Or ensure Codecov App is installed

## ✅ Quick Checklist Before Merging

- [ ] All commits follow semantic format (`feat:`, `fix:`, etc.)
- [ ] All CI checks passing
- [ ] Code reviewed by `dhwani-ankit`
- [ ] DevOps checklist completed
- [ ] No merge conflicts
- [ ] Documentation updated
- [ ] Ready for release (if applicable)

## 🎯 Example Merge Scenario

**Scenario:** Merging PR with these commits:
- `feat: add user authentication`
- `fix: resolve login bug`
- `docs: update API documentation`

**What happens:**
1. ✅ Merge PR to `main`
2. ✅ Release bot analyzes commits
3. ✅ Detects: 1 `feat` + 1 `fix` = **Minor release** (1.0.0 → 1.1.0)
4. ✅ Creates release: **v1.1.0**
5. ✅ Release notes show:
   - ✨ Features: add user authentication
   - 🐛 Bug Fixes: resolve login bug
   - 📝 Documentation: update API documentation

## 🆘 Troubleshooting

**Problem:** Release not created after merge
- **Solution:** Check commit messages follow semantic format

**Problem:** Reviewer not auto-added
- **Solution:** Check `dhwani-ankit` has repository access

**Problem:** Codecov not showing
- **Solution:** Check `CODECOV_TOKEN` secret or Codecov App installation

---

**Remember:** The most important thing is **semantic commit messages**. Without them, no automatic release will be created!

