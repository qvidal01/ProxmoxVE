# 🤖 Autolabeler Features

This document describes the automated features of our PR labeling system.

## 🎯 All Features

### 1. **Label Cleanup** ✨

Automatically removes outdated or conflicting labels when PR is updated.

**Example:**

- PR initially labeled with `feature` and `bugfix`
- User updates PR and only checks `bugfix`
- Bot removes `feature` label automatically

**Benefits:**

- Prevents label confusion
- Ensures accurate changelog categorization
- Maintains label consistency

---

### 2. **Commit Message Analysis** 📝

Automatically detects change types from conventional commit messages.

**Supported Formats:**

- `fix:` or `fix(scope):` → adds `bugfix` label
- `feat:` or `feat(scope):` → adds `feature` label
- `refactor:` or `refactor(scope):` → adds `refactor` label
- `BREAKING CHANGE:` or `feat!:` → adds `breaking change` label

**Example:**

```
feat(docker): add support for custom networks

This adds support for user-defined networks
```

→ Automatically labeled with `feature`

**Benefits:**

- Works even if checkboxes are forgotten
- Supports standard Git conventions
- Combines with template checkboxes

---

### 3. **Size Labels** 📏

Automatically adds size labels based on total lines changed.

**Size Categories:**

- `size: XS` → 1-10 lines (🟢 Green)
- `size: S` → 11-50 lines (🟢 Yellow-Green)
- `size: M` → 51-200 lines (🟡 Yellow)
- `size: L` → 201-500 lines (🟠 Orange)
- `size: XL` → 500+ lines (🔴 Red)

**Benefits:**

- Helps reviewers prioritize
- Quick visual indication of PR complexity
- Automatic and consistent

**Note:** Labels need to be created in repository first. See [SETUP_NEW_LABELS.md](SETUP_NEW_LABELS.md)

---

### 4. **First-Time Contributor Welcome** 🎉

Special welcome message for contributors making their first PR.

**Example:**

```markdown
## 🎉 Welcome to the Community!

Thank you @username for your first contribution! 🙌

A maintainer will review your PR soon. Here are some helpful resources:

- Contributing Guide
- Code of Conduct
```

**Benefits:**

- Better onboarding experience
- Increases contributor retention
- Builds community

---

### 5. **Documentation Check** 📚

Warns when code changes don't include documentation updates.

**Triggers Warning When:**

- Code files modified (`.sh`, `.func`, `.js`, `.go`, etc.)
- No documentation files modified (`.md`, `README`, etc.)
- Not a bugfix or refactor (features should have docs!)

**Example Warning:**

```markdown
📚 **Documentation update recommended**: Consider updating documentation
for these code changes.
```

**Benefits:**

- Improves documentation coverage
- Reminds contributors about docs
- Better project maintainability

---

### 6. **Related Issues Detection** 🔗

Automatically finds and links related open issues.

**How it works:**

- Extracts script names from changed files
- Searches open issues for matching names
- Links up to 5 most relevant issues

**Example:**

```markdown
## 🔗 Related Issues

This PR may be related to the following open issues:

- #123: Docker script fails on Ubuntu 22.04
- #456: Add network configuration options
```

**Benefits:**

- Better context for reviewers
- Automatic cross-referencing
- Helps track issue resolution

---

### 7. **Validation Warnings** ⚠️

Provides helpful warnings when PR information is incomplete or inconsistent.

**Checks:**

- ✅ Change type checkbox is selected for script updates
- ✅ Website checkbox matches file changes
- ✅ Required labels are present
- ✅ Documentation updated for new features

**Example Warning:**

```markdown
## ⚠️ Validation Warnings

⚠️ **Missing change type**: Please check one of the change type checkboxes
(Bug fix, New feature, Refactoring, or Breaking change) in the PR description.
```

---

### 8. **Changelog Preview** 📝

Shows contributors exactly how their PR will appear in the changelog.

**Example Preview:**

```markdown
## 📝 Changelog Preview

This PR will appear in the changelog as:

### 🚀 Updated Scripts

- #### 🐞 Bug Fixes
  - Fix docker script issue @username ([#123](url))
```

**Benefits:**

- Contributors see immediate feedback
- Reduces changelog errors
- Improves PR quality

---

## 🏷️ Label Priority System

When multiple change types are checked, only the highest priority label is applied:

1. 🐞 **Bugfix** (highest priority)
2. 🔧 **Refactor**
3. ✨ **Feature**
4. 💥 **Breaking Change** (lowest priority)

**Rationale:**

- Bug fixes are most important for users
- Prevents PRs from appearing in multiple subcategories
- Maintains clean changelog structure

**Note:** This applies to both template checkboxes AND conventional commit messages!

---

## 📊 Changelog Categories

The bot automatically determines which changelog category your PR belongs to:

| Files Changed                        | Category                        | Note               |
| ------------------------------------ | ------------------------------- | ------------------ |
| `ct/`, `vm/`, `install/`, `turnkey/` | 🚀 Updated Scripts              | Standard scripts   |
| `tools/`                             | 🛠️ Updated Tools                | Utility tools      |
| `misc/`                              | 🔧 Core Updates                 | Core functionality |
| `frontend/` (not JSON)               | 🌐 Website                      | Frontend changes   |
| `frontend/public/json/`              | 🌐 Website > Script Information | Metadata updates   |
| Documentation files                  | 🧰 Maintenance                  | Docs, README, etc. |

**Multi-Category PRs:**
PRs that change files in multiple categories will appear in each relevant category.

---

## 🔄 Bot Behavior

### When PR is Opened/Edited:

1. ✅ Analyzes changed files
2. ✅ Parses PR template checkboxes
3. ✅ Analyzes commit messages (conventional commits)
4. ✅ Calculates size label
5. ✅ Checks if first-time contributor
6. ✅ Applies appropriate labels
7. ✅ Removes conflicting labels
8. ✅ Checks for validation issues
9. ✅ Finds related issues
10. ✅ Posts/updates comment with all info

### Comment Structure:

A single bot comment contains (in order):

1. 🎉 Welcome message (if first-time contributor)
2. ⚠️ Validation warnings (if any)
3. 📝 Changelog preview (always)
4. 🔗 Related issues (if found)
5. ℹ️ Footer with PR size

### Comment Updates:

- Bot updates its own comment when PR is edited (no spam)
- Only posts new comment if none exists
- Welcome message is preserved across updates

---

## 📝 Complete Example

Here's what a first-time contributor might see:

```markdown
## 🎉 Welcome to the Community!

Thank you @newuser for your first contribution! 🙌

A maintainer will review your PR soon. Here are some helpful resources:

- Contributing Guide
- Code of Conduct

---

## ⚠️ Validation Warnings

📚 **Documentation update recommended**: Consider updating documentation
for these code changes.

---

## 📝 Changelog Preview

This PR will appear in the changelog as:

### 🚀 Updated Scripts

- #### ✨ New Features
  - Add Docker network support @newuser ([#789](url))

---

## 🔗 Related Issues

This PR may be related to the following open issues:

- #456: Docker networking not working
- #123: Request: Custom network support

---

_This is an automated message from the PR labeler bot. PR size: size: M_
```

---

## 🛠️ For Maintainers

### Debugging Label Issues

If labels aren't being applied correctly:

1. Check PR template checkboxes are properly formatted
2. Verify commit messages use conventional format
3. Look for bot comment explaining label decisions
4. Check GitHub Actions logs for errors
5. Verify all required labels exist in repository

### Modifying Label Behavior

**Config files:**

- `.github/autolabeler-config.json` - File path → label mappings
- `.github/changelog-pr-config.json` - Changelog categories
- `.github/workflows/autolabeler.yml` - Core logic

**Priority order** can be changed in `autolabeler.yml`:

```javascript
const priorityOrder = ["bugfix", "refactor", "feature", "breaking change"];
```

**Size thresholds** can be adjusted:

```javascript
if (totalChanges <= 10) labelsToAdd.add("size: XS");
else if (totalChanges <= 50) labelsToAdd.add("size: S");
// etc.
```

### Creating Required Labels

Run these commands to create all size labels:

```bash
gh label create "size: XS" --description "1-10 lines" --color "00ff00"
gh label create "size: S" --description "11-50 lines" --color "7fff00"
gh label create "size: M" --description "51-200 lines" --color "ffff00"
gh label create "size: L" --description "201-500 lines" --color "ff8c00"
gh label create "size: XL" --description "500+ lines" --color "ff0000"
```

See [SETUP_NEW_LABELS.md](SETUP_NEW_LABELS.md) for full setup guide.

---

## 🧪 Testing Scenarios

### Test 1: Conventional Commits

```bash
# Commit with conventional format
git commit -m "feat: add new feature"
# Expected: feature label, even without checkbox
```

### Test 2: Size Labels

```bash
# Small PR (< 50 lines)
# Expected: size: S label
```

### Test 3: First-Time Contributor

```bash
# Create PR from new contributor account
# Expected: Welcome message in bot comment
```

### Test 4: Documentation Check

```bash
# Change .sh file, no .md changes
# Expected: Warning about missing docs
```

### Test 5: Related Issues

```bash
# PR changes "docker.sh"
# Expected: Links to issues mentioning "docker"
```

---

## 🎉 Benefits Summary

✅ **Automation**: Less manual work for maintainers
✅ **Consistency**: All PRs follow same standard
✅ **Quality**: Multiple validation checks
✅ **Transparency**: Contributors see what happens
✅ **Community**: Better onboarding for new contributors
✅ **Context**: Automatic issue linking
✅ **Clean Changelogs**: Priority system prevents duplication

---

## 🐛 Troubleshooting

**Labels not applied?**

- Check if PR template checkboxes are properly formatted
- Try using conventional commit messages
- Ensure files changed match expected patterns
- Look for validation warnings in bot comment

**Size label missing?**

- Labels need to be created first (see setup guide)
- Check if label was removed manually
- Verify workflow has permissions to add labels

**Wrong category?**

- Verify file paths (tools/ vs ct/ vs misc/)
- Check if multiple categories apply (intended behavior)
- Review `.github/changelog-pr-config.json`

**Bot comment not appearing?**

- Check GitHub Actions logs
- Verify bot has comment permissions
- May take a few seconds after PR creation
- Check if PR is from a fork (requires pull_request_target)

**Related issues not found?**

- Script name must match issue title/body
- Only searches open issues
- Limited to top 5 results

---

**Questions?** Check the workflow logs or ask in the repository discussions.
