# Demo Release Guide



This guide will help you create a demo release to test the `dhwani-release-bot` setup.

## 🎯 Quick Demo Release (5 minutes)

### Option 1: Create a Test Commit and Push (Recommended)

1. **Create a test file or make a small change:**
   ```bash
   # Create a simple test file
   echo "# Test Release" > test-release.md
   git add test-release.md
   ```


2. **Commit with semantic format:**
   ```bash
   # For a feature (creates minor release: 1.0.0 → 1.1.0)
   git commit -m "feat: add demo release test file"
   
   # OR for a fix (creates patch release: 1.0.0 → 1.0.1)
   git commit -m "fix: update demo release documentation"
   ```

3. **Push to main/master:**
   ```bash
   git push origin main
   # or
   git push origin master
   ```

4. **Watch the magic happen:**
   - Go to **Actions** tab → "Generate Semantic Release"
   - Watch the workflow run
   - Check **Releases** tab for new release
   - Check **Tags** for new tag

### Option 2: Manual Trigger via Workflow

1. **Go to Actions:**
   - Navigate to your repository
   - Click **Actions** tab
   - Select **"Generate Semantic Release"** workflow

2. **Run workflow:**
   - Click **"Run workflow"** button
   - Select branch: `main` or `master`
   - Click **"Run workflow"**

3. **Note:** This will only create a release if there are semantic commits since the last release.

### Option 3: Use Bot Command

1. **Create an issue or PR:**
   - Go to Issues or Pull Requests
   - Create a new issue/PR

2. **Comment with bot command:**
   ```
   @dhwani-release-bot release
   ```

3. **Bot will trigger the release workflow**

## 📝 Step-by-Step Demo (Detailed)

### Step 1: Check Current Version

```bash
# Check if there are any existing tags
git tag -l

# Check last release
git describe --tags --abbrev=0 2>/dev/null || echo "No tags found"
```

### Step 2: Make a Test Change

**Option A: Create a test file**
```bash
# Create a simple markdown file
cat > docs/demo-release.md << 'EOF'
# Demo Release

This is a test file for demo release.
Created: $(date)
EOF

git add docs/demo-release.md
```

**Option B: Update README**
```bash
# Add a line to README
echo "" >> README.md
echo "<!-- Demo release test -->" >> README.md
git add README.md
```

**Option C: Update a config file**
```bash
# Add a comment to a config file
echo "# Demo release test" >> .gitignore
git add .gitignore
```

### Step 3: Commit with Semantic Format

Choose one based on what you want to test:

**For Minor Release (1.0.0 → 1.1.0):**
```bash
git commit -m "feat: add demo release documentation"
```

**For Patch Release (1.0.0 → 1.0.1):**
```bash
git commit -m "fix: update demo release guide"
```

**For Major Release (1.0.0 → 2.0.0):**
```bash
git commit -m "feat: add new authentication system

BREAKING CHANGE: Old authentication method is deprecated"
```

### Step 4: Push to Main/Master

```bash
# Make sure you're on main/master
git checkout main  # or master

# Push your commit
git push origin main
```

### Step 5: Monitor the Release

1. **Check Actions:**
   ```
   Repository → Actions → "Generate Semantic Release"
   ```
   - Watch the workflow run
   - Check for any errors
   - Look for "Release created" messages

2. **Check Releases:**
   ```
   Repository → Releases
   ```
   - New release should appear
   - Check release notes format
   - Verify version number

3. **Check Tags:**
   ```
   Repository → Tags
   ```
   - New tag should be created (e.g., `v1.1.0`)

4. **Check CHANGELOG.md:**
   ```
   Repository → CHANGELOG.md
   ```
   - Should be updated with new release

## 🧪 Testing Different Release Types

### Test Patch Release (1.0.0 → 1.0.1)

```bash
git commit -m "fix: resolve demo release issue"
git push origin main
```

**Expected Result:**
- Version: `v1.0.1`
- Release type: Patch
- Section: 🐛 Bug Fixes

### Test Minor Release (1.0.0 → 1.1.0)

```bash
git commit -m "feat: add new demo feature"
git push origin main
```

**Expected Result:**
- Version: `v1.1.0`
- Release type: Minor
- Section: ✨ Features

### Test Major Release (1.0.0 → 2.0.0)

```bash
git commit -m "feat: implement new API

BREAKING CHANGE: Old API endpoints are removed"
git push origin main
```

**Expected Result:**
- Version: `v2.0.0`
- Release type: Major
- Section: ✨ Features (with breaking change note)

## 🔍 Troubleshooting Demo Release

### Issue: No Release Created

**Check:**
1. Commit message format:
   ```bash
   git log --oneline -5
   ```
   - Must start with `feat:`, `fix:`, `perf:`, etc.

2. Workflow logs:
   - Go to Actions → "Generate Semantic Release"
   - Check for "No release will be created" message

3. Branch:
   - Must be pushed to `main` or `master`

**Solution:**
```bash
# Fix commit message
git commit --amend -m "feat: add demo feature"
git push origin main --force-with-lease
```

### Issue: Release Created But Not Visible

**Check:**
1. Refresh the Releases page
2. Check if release is in draft state
3. Verify GitHub App permissions

**Solution:**
- Check workflow logs for errors
- Verify `DHWANI_RELEASE_BOT_APP_ID` and `DHWANI_RELEASE_BOT_PRIVATE_KEY` secrets

### Issue: Wrong Version Number

**Check:**
1. Previous tags:
   ```bash
   git tag -l
   ```

2. Semantic-release determines version based on:
   - Last tag
   - Commit types since last tag

**Solution:**
- If no previous tag, first release will be `v1.0.0`
- Subsequent releases follow semantic versioning

## ✅ Demo Release Checklist

Before creating demo release:
- [ ] GitHub App is set up and installed
- [ ] Secrets are configured (`DHWANI_RELEASE_BOT_APP_ID`, `DHWANI_RELEASE_BOT_PRIVATE_KEY`)
- [ ] You're on `main` or `master` branch
- [ ] Commit message follows semantic format

After creating demo release:
- [ ] Workflow completed successfully
- [ ] Release appears in Releases tab
- [ ] Tag created (check Tags tab)
- [ ] CHANGELOG.md updated
- [ ] Release notes are formatted correctly

## 🎬 Quick Demo Script

Here's a complete script to create a demo release:

```bash
#!/bin/bash

# Demo Release Script
echo "🚀 Creating demo release..."

# Create test file
echo "# Demo Release Test" > demo-test.md
git add demo-test.md

# Commit with semantic format
git commit -m "feat: add demo release test file"

# Push to main
echo "📤 Pushing to main..."
git push origin main

echo "✅ Demo release triggered!"
echo "📊 Check Actions tab for workflow status"
echo "🏷️  Check Releases tab for new release"
```

Save as `demo-release.sh`, make executable, and run:
```bash
chmod +x demo-release.sh
./demo-release.sh
```

## 📊 Expected Output

After successful demo release, you should see:

1. **In Actions:**
   ```
   ✅ Release workflow completed
   ✅ Release created: v1.0.1
   ```

2. **In Releases:**
   ```
   v1.0.1 Latest
   @dhwani-release-bot released this just now
   
   ## v1.0.1 (2025-01-XX)
   
   🐛 Bug Fixes
   - fix: update demo release guide ([`abc1234`](link))
   ```

3. **In Tags:**
   ```
   v1.0.1
   ```

4. **In CHANGELOG.md:**
   ```markdown
   ## [1.0.1] - 2025-01-XX
   
   ### 🐛 Bug Fixes
   - fix: update demo release guide
   ```

---

**Need help?** Check the workflow logs in Actions tab for detailed information about what happened.

