# Contributing to MAS Specification

Thank you for your interest in contributing to the Mermaid Abstract Syntax (MAS) Specification! This document provides guidelines for contributing.

## Ways to Contribute

### 1. Report Issues

Found a problem? Open an issue with:
- Diagram type affected
- Expected behavior
- Actual behavior
- Minimal reproduction example

### 2. Suggest Enhancements

Have an idea? Start a [GitHub Discussion](https://github.com/WiiLearn/mas-spec/discussions) first to gather feedback.

### 3. Submit Pull Requests

Ready to contribute? Follow the process below.

---

## Contribution Process

### For Documentation Fixes

1. Fork the repository
2. Create a branch: `git checkout -b fix/typo-in-v30-spec`
3. Make your changes
4. Submit a pull request

### For Specification Changes (SEP Process)

Significant changes require a **Specification Enhancement Proposal (SEP)**:

1. **Discuss**: Open a GitHub Discussion with your proposal
2. **Draft SEP**: Create `rfcs/NNNN-your-proposal.md` using the template
3. **Review**: Community and maintainers review (minimum 14 days)
4. **Revise**: Address feedback
5. **Vote**: Maintainers vote on acceptance
6. **Implement**: Merge approved changes

---

## SEP Template

```markdown
# SEP-NNNN: Title

## Summary
One paragraph explanation.

## Motivation
Why is this needed?

## Specification
Detailed specification with EBNF grammar if applicable.

## Examples
Mermaid code examples.

## Backward Compatibility
Impact on existing diagrams.

## Alternatives Considered
Other approaches and why they were rejected.
```

---

## Pull Request Guidelines

### Commit Messages

Use conventional commits:
- `feat: add new diagram type for X`
- `fix: correct EBNF grammar for swimlane`
- `docs: improve quick start guide`
- `chore: update dependencies`

### PR Checklist

- [ ] All Mermaid diagrams render correctly
- [ ] Links are valid (run `npx markdown-link-check **/*.md`)
- [ ] Follows existing style and formatting
- [ ] CHANGELOG.md updated (for spec changes)
- [ ] Version numbers updated if applicable

---

## Style Guide

### Markdown

- Use ATX-style headers (`#`, `##`, `###`)
- One sentence per line (for better diffs)
- Use fenced code blocks with language identifiers
- Use reference-style links for repeated URLs

### Mermaid Diagrams

- Include `mermaid` language identifier in code blocks
- Test rendering in GitHub preview
- Use semantic node IDs (not `A`, `B`, `C`)

### Specification Text

- Use RFC 2119 keywords: MUST, SHOULD, MAY
- Define terms on first use
- Include examples for every rule

---

## Development Setup

```bash
# Clone your fork
git clone git@github.com:YOUR_USERNAME/mas-spec.git
cd mas-spec

# Add upstream remote
git remote add upstream git@github.com:WiiLearn/mas-spec.git

# Keep your fork updated
git fetch upstream
git merge upstream/main
```

---

## Code of Conduct

All contributors must follow our [Code of Conduct](CODE_OF_CONDUCT.md).

---

## Questions?

- **General questions**: [GitHub Discussions](https://github.com/WiiLearn/mas-spec/discussions)
- **Bug reports**: [GitHub Issues](https://github.com/WiiLearn/mas-spec/issues)

Thank you for contributing to MAS!
