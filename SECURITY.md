# Security Policy

## Reporting a Vulnerability

The MAS Specification is a documentation project, not executable code. However, we take security seriously, especially regarding:

1. **Injection vulnerabilities** in diagram syntax
2. **XSS risks** when rendering user-provided Mermaid code
3. **Security guidance** accuracy in the specification

### How to Report

**DO NOT** open a public issue for security vulnerabilities.

Instead, email: **security@wiilearn.com**

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

### Response Timeline

- **Acknowledgment**: Within 48 hours
- **Initial assessment**: Within 7 days
- **Resolution**: Depends on severity

### Scope

In scope:
- Security issues in MAS specification text
- Vulnerabilities enabled by following MAS guidance
- Security best practices omissions

Out of scope:
- Vulnerabilities in Mermaid.js itself (report to [mermaid-js/mermaid](https://github.com/mermaid-js/mermaid))
- Vulnerabilities in third-party implementations

---

## Security Considerations in MAS

The MAS specification includes security guidance in:

- **MAS-Core-v2.0.md**: Input validation rules
- **MAS-INTEGRATION-Patterns.md**: Secure integration patterns

Key security principles:
1. Always sanitize user input before rendering
2. Use allowlists for diagram element types
3. Validate diagram syntax before processing
4. Limit diagram complexity to prevent DoS

---

## Acknowledgments

We thank security researchers who responsibly disclose vulnerabilities.

Contributors who report valid security issues will be acknowledged (with permission) in our security advisories.
