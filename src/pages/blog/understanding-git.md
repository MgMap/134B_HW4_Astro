---
layout: ../../layouts/BlogPostLayout.astro
title: 'Understanding Git and Version Control'
description: 'Master Git fundamentals and learn best practices for version control in modern software development teams.'
publishDate: '2025-11-16'
author: 'Min Aung Paing'
authorRole: 'Software Engineer'
authorBio: 'Web client languages student, junior year'
tags: ['Git', 'Version Control', 'Development']
---

## Why Git Matters

Version control is essential for modern software development. Git, created by Linus Torvalds, has become the industry standard for managing code changes, collaborating with teams, and maintaining project history.

## Core Concepts

### The Three States

Git has three main states that your files can be in:

1. **Modified** - You've changed the file but not committed it
2. **Staged** - You've marked a modified file to go into your next commit
3. **Committed** - The data is safely stored in your local database

```bash
# Check file status
git status

# Stage changes
git add filename.js

# Commit changes
git commit -m "Add new feature"
```

### The Git Workflow

```bash
# 1. Make changes to your files
echo "console.log('Hello');" > app.js

# 2. Stage the changes
git add app.js

# 3. Commit with a descriptive message
git commit -m "Add console greeting"

# 4. Push to remote repository
git push origin main
```

## Essential Commands

### Starting a Repository

```bash
# Initialize a new Git repository
git init

# Clone an existing repository
git clone https://github.com/user/repo.git
```

### Daily Workflow

```bash
# Check current status
git status

# See what changed
git diff

# View commit history
git log --oneline

# Create a new branch
git checkout -b feature/new-feature

# Switch branches
git checkout main

# Merge changes
git merge feature/new-feature
```

### Undoing Changes

```bash
# Unstage a file
git reset HEAD filename.js

# Discard changes in working directory
git checkout -- filename.js

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1
```

## Branching Strategy

### Feature Branch Workflow

```bash
# Create feature branch from main
git checkout -b feature/user-authentication

# Work on your feature
git add .
git commit -m "Implement user login"

# Keep branch updated with main
git checkout main
git pull
git checkout feature/user-authentication
git merge main

# When ready, merge back to main
git checkout main
git merge feature/user-authentication
```

### Git Flow

A popular branching model for larger projects:

- `main` - Production-ready code
- `develop` - Integration branch for features
- `feature/*` - Individual features
- `release/*` - Release preparation
- `hotfix/*` - Emergency fixes

## Best Practices

### Writing Good Commit Messages

```bash
# Bad
git commit -m "fix stuff"

# Good
git commit -m "Fix login validation error

- Add email format validation
- Handle missing password field
- Update error messages for clarity"
```

### Commit Message Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

Examples:
```bash
feat(auth): Add password reset functionality
fix(api): Handle null response in user endpoint
docs(readme): Update installation instructions
refactor(utils): Simplify date formatting logic
```

### Small, Focused Commits

```bash
# Good - Each commit does one thing
git commit -m "Add user model"
git commit -m "Add user validation"
git commit -m "Add user tests"

# Bad - One huge commit
git commit -m "Add complete user system"
```

## Working with Remote Repositories

### Syncing Changes

```bash
# Fetch changes without merging
git fetch origin

# Pull changes and merge
git pull origin main

# Push your changes
git push origin feature/my-feature

# Push a new branch
git push -u origin feature/my-feature
```

### Managing Remotes

```bash
# List remotes
git remote -v

# Add a remote
git remote add upstream https://github.com/original/repo.git

# Remove a remote
git remote remove upstream
```

## Advanced Techniques

### Interactive Rebase

Clean up commit history before pushing:

```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Options:
# pick - keep commit as is
# reword - change commit message
# squash - combine with previous commit
# drop - remove commit
```

### Stashing Changes

Save work in progress without committing:

```bash
# Stash current changes
git stash

# List stashes
git stash list

# Apply most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{0}
```

### Cherry Picking

Apply specific commits to another branch:

```bash
# Get commit hash from log
git log --oneline

# Apply that commit to current branch
git cherry-pick abc123
```

## Troubleshooting Common Issues

### Merge Conflicts

```bash
# When you see conflicts
git status

# Edit conflicted files, then:
git add resolved-file.js
git commit
```

### Accidentally Committed to Wrong Branch

```bash
# Move commit to new branch
git branch feature/new-feature
git reset HEAD~ --hard
git checkout feature/new-feature
```

### Lost Commits

```bash
# View reference log
git reflog

# Recover lost commit
git checkout -b recovery abc123
```

## Conclusion

Git is a powerful tool that becomes more valuable as you master it. Start with the basics, practice regularly, and gradually incorporate advanced techniques as needed.

Remember:
- Commit often with clear messages
- Use branches for new features
- Keep your main branch stable
- Pull before you push
- Don't rewrite public history

With these fundamentals and best practices, you'll be well-equipped to manage version control effectively in any project!
