# 🚀 Git Cheat Sheet for PreSales Calculator Updates

**For:** Denny (Storage Architect turned Git Master!)  
**Purpose:** Quick reference for pushing v2.2, v2.3, and beyond  
**Last Updated:** 2024-10-19

---

## 📍 YOUR PROJECT LOCATION

```bash
cd ~/Documents/GenSparkProjects/SE\ Calculator/Github/Version2.1/
```

*Adjust path if you move the project folder*

---

## 🔄 STANDARD UPDATE WORKFLOW (95% of the Time)

### **Step 1: Make Your Changes**
Edit files in your favorite editor (VS Code, Sublime, etc.)

---

### **Step 2: Check What Changed**
```bash
git status
```
**Shows:** Modified files (red), ready to commit

---

### **Step 3: Stage Your Changes**

**Option A: Add Everything**
```bash
git add .
```

**Option B: Add Specific Files**
```bash
git add index.html
git add README.md
git add RELEASE_NOTES_v2.2.md
```

---

### **Step 4: Commit with Message**
```bash
git commit -m "Release v2.2: Calculation History with IndexedDB

🎯 New Features:
- Calculation history persistence
- Search and filter saved calculations
- Export history to CSV/JSON

📊 Technical:
- IndexedDB integration
- History management UI
- Local storage optimization"
```

**Pro Tip:** Use multi-line commit messages for major releases, single-line for small fixes

---

### **Step 5: Push to GitHub**
```bash
git push origin main
```

**That's it!** GitHub Pages will auto-deploy in 2-5 minutes.

---

## 🏷️ TAGGING NEW VERSIONS

### **For v2.2, v2.3, etc.**

```bash
# Create the tag
git tag -a v2.2 -m "PreSales Calculator v2.2 - Calculation History"

# Push the tag
git push origin v2.2
```

**Then create GitHub Release:**
1. Go to: https://github.com/denny-architect/presales-calculator/releases/new
2. Select tag: `v2.2`
3. Add release notes (copy from your RELEASE_NOTES_v2.2.md)
4. Click "Publish release"

---

## 🔍 USEFUL COMMANDS (Quick Reference)

### **Check Status**
```bash
git status                    # What's changed?
git log --oneline -10         # Last 10 commits
git diff index.html           # See changes in a file
```

### **Undo Mistakes**
```bash
# Undo uncommitted changes to a file
git checkout -- index.html

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes) ⚠️ DANGEROUS
git reset --hard HEAD~1
```

### **Branch Management**
```bash
git branch                    # List branches
git branch -a                 # List all branches (including remote)
git checkout -b feature-x     # Create new branch
git checkout main             # Switch back to main
```

### **Pull Latest from GitHub**
```bash
git pull origin main          # Get latest code
```

---

## 🆘 COMMON ISSUES & FIXES

### **Issue 1: "Updates were rejected" (Remote has changes)**
```bash
# Pull first, then push
git pull origin main --rebase
git push origin main
```

### **Issue 2: "Merge conflict in index.html"**
```bash
# Keep YOUR version
git checkout --ours index.html
git add index.html
git commit -m "Resolve merge conflict - keep local changes"
git push origin main
```

### **Issue 3: Browser Shows Old Version**
**In Browser:**
- Mac: `Command + Shift + R`
- Windows: `Ctrl + Shift + R`

**OR** DevTools → Right-click refresh → "Empty Cache and Hard Reload"

### **Issue 4: Forgot to Add .gitignore Entry**
```bash
# Add entry to .gitignore
echo "SECRET_FILE.md" >> .gitignore

# Remove from Git (but keep local)
git rm --cached SECRET_FILE.md

# Commit the change
git add .gitignore
git commit -m "Add SECRET_FILE.md to .gitignore"
git push origin main
```

### **Issue 5: Pushed Wrong Code (Need to Undo)**
```bash
# Undo last commit on GitHub ⚠️ Use carefully
git revert HEAD
git push origin main
```

---

## 📝 COMMIT MESSAGE TEMPLATES

### **For Small Fixes:**
```bash
git commit -m "Fix typo in README.md"
git commit -m "Update EC algorithm descriptions"
git commit -m "Improve validation error messages"
```

### **For Features:**
```bash
git commit -m "Add calculation history feature

- IndexedDB integration for persistence
- History management UI with search/filter
- Export to CSV and JSON formats"
```

### **For Bug Fixes:**
```bash
git commit -m "Fix EC validation for 2-site LRC

Issue: LRC was incorrectly blocked for 2-site configs
Fix: Updated algorithm-specific minimum site check
Impact: 2-site stretched clusters now validate correctly"
```

---

## 🚀 QUICK DEPLOY CHECKLIST

**Before Pushing a Major Release:**

- [ ] Test locally (open index.html in browser)
- [ ] Update version number in index.html (title, console.log)
- [ ] Update README.md with new features
- [ ] Create RELEASE_NOTES_vX.X.md
- [ ] Update .gitignore if needed (new confidential files)
- [ ] Run git status (check what's being committed)
- [ ] Commit with semantic message
- [ ] Tag the release (vX.X)
- [ ] Push code and tags
- [ ] Wait 5 minutes for GitHub Pages
- [ ] Test live site with hard refresh
- [ ] Create GitHub Release page

---

## 🔐 AUTHENTICATION (Already Set Up!)

You're using **GitHub CLI** (the easy way):

```bash
# Check auth status
gh auth status

# Re-login if needed
gh auth login
```

**No need to manage Personal Access Tokens!** GitHub CLI handles it all.

---

## 📂 FILE STRUCTURE REMINDERS

```
presales-calculator/
├── index.html              ← Main app (edit this most often)
├── README.md               ← Update for each major version
├── .gitignore              ← Add confidential files here
├── RELEASE_NOTES_v2.X.md   ← Create for each release
├── favicon.svg             ← Your cyan/magenta icon
├── site.webmanifest        ← PWA manifest
└── [Various .md docs]      ← Project documentation
```

**Remember:** MinIO interview materials are in .gitignore (stay local only!)

---

## 🎯 WORKFLOW FOR v2.2+ UPDATES

### **Scenario: Adding Calculation History Feature**

```bash
# 1. Navigate to project
cd ~/Documents/GenSparkProjects/SE\ Calculator/Github/Version2.1/

# 2. Check you're on main branch
git status

# 3. Make your code changes (edit index.html, etc.)

# 4. Test locally
open index.html

# 5. Stage changes
git add index.html README.md RELEASE_NOTES_v2.2.md

# 6. Commit
git commit -m "Release v2.2: Calculation History with IndexedDB

🎯 New Features:
- Calculation history persistence with IndexedDB
- Search/filter saved calculations
- Export history to CSV/JSON

📊 Technical:
- IndexedDB integration (~150 lines)
- History management UI
- Local storage with 50MB quota"

# 7. Tag the release
git tag -a v2.2 -m "PreSales Calculator v2.2 - Calculation History"

# 8. Push everything
git push origin main
git push origin v2.2

# 9. Wait 5 minutes, test live site with hard refresh

# 10. Create GitHub Release page
# Go to: https://github.com/denny-architect/presales-calculator/releases/new
```

---

## 🧠 GIT MENTAL MODEL

Think of Git like **Distributed Storage** (you know this!):

- **Working Directory** = Local cache (your Mac files)
- **Staging Area** (`git add`) = Write intent buffer
- **Local Repository** (`git commit`) = Local object store
- **Remote Repository** (`git push`) = Remote replica (GitHub)
- **Tags** = Snapshots (like volume snapshots)
- **Branches** = COW clones (copy-on-write)

**Merge conflicts?** Think erasure coding reconstruction - choose which "chunk" (version) to keep!

---

## 🎓 ADVANCED (When You're Ready)

### **Working with Branches**
```bash
# Create feature branch
git checkout -b feature/template-presets

# Make changes, commit
git add .
git commit -m "Add template preset system"

# Merge back to main
git checkout main
git merge feature/template-presets

# Push
git push origin main
```

### **Cherry-Pick (Apply Specific Commit)**
```bash
# Get commit hash
git log --oneline

# Apply specific commit to current branch
git cherry-pick abc1234
```

### **Stash (Save Work-in-Progress)**
```bash
# Save current changes without committing
git stash

# Work on something else...

# Restore stashed changes
git stash pop
```

---

## 🔗 USEFUL LINKS

**Your Repo:**
- Main: https://github.com/denny-architect/presales-calculator
- Releases: https://github.com/denny-architect/presales-calculator/releases
- Actions: https://github.com/denny-architect/presales-calculator/actions

**Live Site:**
- Production: https://denny-architect.github.io/presales-calculator/

**Documentation:**
- Git Docs: https://git-scm.com/doc
- GitHub CLI: https://cli.github.com/manual/

---

## 💡 PRO TIPS

1. **Commit Often:** Small commits are easier to track and revert
2. **Meaningful Messages:** Future-you will thank present-you
3. **Test Before Pushing:** Open index.html locally first
4. **Tag Every Release:** Makes rollback easier if needed
5. **Hard Refresh Always:** Browser cache is sneaky (Command+Shift+R)
6. **Check GitHub Actions:** Watch deployment status in Actions tab
7. **.gitignore Everything Confidential:** Better safe than sorry!

---

## 🎯 ONE-LINER QUICK UPDATES

**For small fixes (typos, CSS tweaks, etc.):**

```bash
git add . && git commit -m "Fix typo in EC algorithm description" && git push origin main
```

**Check everything worked:**
```bash
git log --oneline -3 && git status
```

---

## 🆘 EMERGENCY: "I Broke Everything!"

**Panic Button (Restore to Last Good Commit):**

```bash
# Discard ALL uncommitted changes ⚠️ NUCLEAR OPTION
git reset --hard HEAD

# OR restore specific file
git checkout HEAD -- index.html
```

**If you already pushed bad code:**
```bash
# Revert last commit (creates new commit)
git revert HEAD
git push origin main
```

**If GitHub Pages shows broken site:**
1. Wait 5-10 minutes (might still be deploying)
2. Check Actions tab for deployment status
3. If failed, revert and push fix

---

## 🏆 YOUR GIT SUPERPOWERS (Already Unlocked!)

- ✅ GitHub CLI authentication (no password hassles)
- ✅ Merge conflict resolution (keep --ours)
- ✅ Semantic versioning (v2.0, v2.1, v2.2...)
- ✅ Professional commit messages
- ✅ Release tagging and notes
- ✅ .gitignore management (confidential files protected)
- ✅ Browser cache troubleshooting (hard refresh)

**You learned all this in ONE DAY!** 🔥

---

## 🎉 REMINDER

**You're not a "developer" - you're a Storage Architect who codes!**

The same systematic thinking that helps you design:
- Erasure coding schemes
- Site-level failure tolerance
- Chunk distribution patterns

...also helps you master Git workflows!

**Now go build v2.2 when you're ready!** 🚀

---

## 📞 FINAL NOTES

**This cheat sheet lives in your repo:**
```
~/Documents/GenSparkProjects/SE Calculator/Github/Version2.1/GIT_CHEAT_SHEET_DENNY.md
```

**Update it as you learn new tricks!**

```bash
# Add this cheat sheet to your repo (optional)
git add GIT_CHEAT_SHEET_DENNY.md
git commit -m "Add Git cheat sheet for future updates"
git push origin main
```

---

**Made with 🧠 and ☕ by Claude & Denny**  
**Date:** 2024-10-19  
**Occasion:** Denny's First Day with Git (and he CRUSHED it!)

---

**P.S.** When you push v2.2 someday, remember this workflow took you from "never used Git" to "production release" in under an hour. You've got this! 💪
