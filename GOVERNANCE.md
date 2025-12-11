# MAS Specification Governance

This document describes the governance model for the MAS (Mermaid Abstract Syntax) Specification project.

## Principles

1. **Openness**: All discussions and decisions happen in public
2. **Meritocracy**: Contributions and expertise earn influence
3. **Consensus**: We seek broad agreement before making changes
4. **Stability**: Specification changes are deliberate and backward-compatible when possible

---

## Roles

### Contributors

Anyone who contributes to the project through:
- Issues and bug reports
- Pull requests
- Documentation improvements
- Community discussions

### Maintainers

Trusted contributors with:
- Write access to the repository
- Ability to merge pull requests
- Vote on SEP proposals

Current Maintainers:
- @phuongnguyen (Lead Maintainer)

### Technical Steering Committee (TSC)

For major decisions affecting the specification direction:
- Minimum 3 members
- Elected annually by maintainers
- Resolves disputes and sets roadmap

---

## Decision Making

### Minor Changes

- Documentation fixes, typos, clarifications
- Single maintainer approval required
- Merged within 48 hours if no objections

### Specification Changes

- New diagram types, grammar changes, conformance levels
- Requires SEP (Specification Enhancement Proposal)
- Minimum 14-day review period
- Requires 2/3 maintainer approval

### Breaking Changes

- Changes that affect backward compatibility
- Requires TSC approval
- Minimum 30-day review period
- Major version bump required

---

## SEP Process

1. **Draft**: Author creates SEP in `rfcs/` directory
2. **Review**: 14-day public comment period
3. **Revise**: Author addresses feedback
4. **Vote**: Maintainers vote (2/3 majority required)
5. **Merge**: Approved SEPs are merged

SEP States:
- `draft` - Under discussion
- `accepted` - Approved, pending implementation
- `implemented` - Merged into specification
- `rejected` - Not accepted
- `withdrawn` - Author withdrew proposal

---

## Versioning

MAS follows Semantic Versioning:

- **Major** (X.0.0): Breaking changes
- **Minor** (0.X.0): New features, backward compatible
- **Patch** (0.0.X): Bug fixes, clarifications

---

## Meetings

- **Monthly sync**: First Tuesday of each month (optional)
- **Meeting notes**: Posted in GitHub Discussions
- **Async-first**: Most work happens asynchronously

---

## Amendments

This governance document can be amended through the SEP process with TSC approval.

---

*Last updated: December 2025*
