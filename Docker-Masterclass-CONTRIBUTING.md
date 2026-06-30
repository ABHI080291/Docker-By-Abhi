# Contributing to Docker-Masterclass 🐳

Thank you for your interest in contributing! We welcome contributions from everyone.

---

## How to Contribute

### 1. Report Errors or Issues

Found a mistake in the tutorials? Typo in code examples?

```
1. Check if issue already exists
2. Open a new issue with:
   - Clear title
   - Description of error
   - Where in course (section/page)
   - Your suggestion to fix it
```

### 2. Suggest Improvements

Have a better way to explain something?

```
1. Open an issue with suggestion
2. Describe improvement
3. Why it's better
4. Examples if possible
```

### 3. Add New Content

Want to add new sections or projects?

```
1. Open issue first (discuss)
2. Fork the repository
3. Create branch: git checkout -b add/new-content
4. Follow existing format
5. Commit: git commit -am 'Add new content'
6. Push: git push origin add/new-content
7. Open Pull Request
```

### 4. Fix Typos or Improve Examples

Small improvements welcome!

```
1. Fork repository
2. Create branch: git checkout -b fix/typo-or-example
3. Make changes
4. Commit with clear message
5. Push and open PR
```

---

## Content Style Guide

### Writing Style

- **Simple English** - Understandable for beginners
- **Clear explanations** - Don't assume knowledge
- **Practical examples** - Real code that works
- **Visual aids** - Diagrams help learning
- **Consistent format** - Follow existing sections

### Markdown Format

```markdown
# Section Title (H1)

**Bold** for important terms
- Bullet points for lists
- Easy to scan

```code blocks```
For showing commands/code

> Blockquotes for tips/notes

| Tables | For | Comparisons |
```

### Code Examples

```markdown
# Good example:
bash
# Show what it does
$ docker run -d nginx
Output or explanation

# Bad example:
Just showing code without explanation
```

---

## Pull Request Process

### Before You Start
- [ ] Fork the repository
- [ ] Create descriptive branch name
- [ ] Test your changes
- [ ] Follow style guide
- [ ] Run spell check

### Submitting PR

```markdown
## Description
Brief description of changes

## Type
- [ ] Bug fix
- [ ] Typo correction
- [ ] New section
- [ ] Improvement
- [ ] Better example

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
How did you test this?

## Related Issues
Fixes #123
```

### Review Process

1. Maintainers review PR
2. May request changes
3. Update PR if needed
4. Approved and merged
5. You'll be credited!

---

## Code of Conduct

### Our Commitment
We are committed to providing a welcoming and inclusive community.

### Expected Behavior
- Be respectful and inclusive
- Welcome feedback
- Focus on constructive discussion
- Respect different opinions

### Unacceptable Behavior
- Harassment or discrimination
- Offensive comments
- Trolling
- Unprofessional conduct

---

## Section Format Template

```markdown
# XX - Section Title 📚

**One line description**

---

## 📚 Table of Contents

1. [Topic 1](#topic-1)
2. [Topic 2](#topic-2)
3. [Topic 3](#topic-3)

---

## Topic 1

### Simple Explanation

**What is it?**
```
Clear, simple definition
```

### Visual Explanation

```
Diagram or visual aid
```

### Real Example

```bash
$ actual docker command
# output/explanation
```

---

## Summary 📝

```
Key points
```

---

## 🔗 Next Section

👉 **[Next Section](../XX-Section/README.md)**

---

Last updated: January 2025
```

---

## Common Contributions

### 1. Typo Fixes
```
Simple spelling/grammar fixes
No discussion needed
Direct PR is fine
```

### 2. Example Improvements
```
Better Docker examples
Clear, working code
Explain what it does
```

### 3. New Sections
```
Discuss in issue first
Follow existing format
Include all details
Table of contents
Links to next section
```

### 4. Diagram Improvements
```
ASCII diagrams
Visual explanations
Help beginners understand
```

---

## Git Workflow

### Initial Setup
```bash
# Fork on GitHub (click Fork button)

# Clone your fork
git clone https://github.com/YOUR-USERNAME/Docker-Masterclass.git
cd Docker-Masterclass

# Add upstream
git remote add upstream https://github.com/ORIGINAL-OWNER/Docker-Masterclass.git
```

### Making Changes
```bash
# Create feature branch
git checkout -b fix/typo-or-add/content

# Make your changes
# Edit files, add examples, fix typos

# Commit
git commit -am "Fix typo in section 5"
git commit -am "Add Podman example"

# Push
git push origin fix/typo-or-add/content
```

### Pull Request
```bash
# Go to GitHub
# Click "New Pull Request"
# Select your branch
# Add description
# Submit
```

### After Merge
```bash
# Update your fork
git checkout main
git pull upstream main
git push origin main

# Clean up
git branch -d fix/typo-or-add/content
```

---

## Commit Message Guidelines

### Good Commit Messages
```
Fix typo in Docker-Commands section
Add Kubernetes deployment example
Improve Docker-Compose explanation
Add health check example
```

### Format
```
[TYPE] Brief description (under 50 characters)

Longer explanation (optional)
- Point 1
- Point 2

Fixes #123
```

### Types
- `fix:` - Fix error or typo
- `add:` - Add new content
- `improve:` - Improve explanation
- `docs:` - Documentation changes
- `refactor:` - Reorganize content

---

## Review Checklist

Before submitting PR, check:

- [ ] Content is accurate
- [ ] Examples work correctly
- [ ] Grammar and spelling correct
- [ ] Follows style guide
- [ ] Markdown formats correctly
- [ ] No broken links
- [ ] Images/diagrams included (if needed)
- [ ] Clear and beginner-friendly
- [ ] Provides value to learners

---

## Common Contribution Types

### 1. Typo Fix
**Difficulty:** Easy
**Time:** 5 minutes
```
One typo correction
Obvious fix
Direct PR OK
```

### 2. Example Addition
**Difficulty:** Easy
**Time:** 15 minutes
```
Add working Docker example
Explanation included
Improves understanding
```

### 3. Explanation Improvement
**Difficulty:** Medium
**Time:** 30 minutes
```
Rewrite explanation
Clearer and simpler
Better for beginners
```

### 4. New Section
**Difficulty:** Hard
**Time:** 2-4 hours
```
Completely new section
Discuss first in issue
Follow template
Multiple examples
Diagrams
```

---

## Getting Help

- **Questions?** Open a discussion issue
- **Stuck?** Ask in PR comments
- **Ideas?** Discuss in issues first
- **Not sure?** Comment and ask!

---

## Contributor Recognition

Contributors will be:
- Listed in README
- Credited in changelog
- Thanked in commits
- Recognized in community

---

## FAQ for Contributors

**Q: Can I add completely new sections?**
A: Yes! Discuss in issue first.

**Q: What if my PR is rejected?**
A: We'll explain why and suggest improvements.

**Q: Can I edit other people's work?**
A: Yes, for improvements. Discuss first.

**Q: How long before review?**
A: Usually 3-5 days.

**Q: Do I need to know everything?**
A: No! Fix what you know.

---

## Resources

- [Markdown Guide](https://www.markdownguide.org/)
- [Docker Docs](https://docs.docker.com/)
- [Git Guide](https://git-scm.com/book/en/v2)
- [Community Standards](https://docs.github.com/en/communities)

---

## Special Thanks

To everyone who contributes, we say:

**🙏 Thank you!**

Your contributions make this guide better for everyone. Even small improvements help thousands of learners worldwide.

---

**Happy Contributing!** 🎉

Last updated: January 2025
