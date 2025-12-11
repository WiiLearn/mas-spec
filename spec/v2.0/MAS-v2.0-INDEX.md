# Mermaid Abstract Syntax (MAS) v2.0 Specification

**Status:** Draft Specification  
**Version:** 2.0.0  
**Date:** 2025-11-21  
**Editors:** MAS Specification Editors  
**Previous Version:** [MAP-v1.0](./MAS-Mermaid-artifact-spect_v1_0.md)

---

## Abstract

The Mermaid Abstract Syntax (MAS) v2.0 defines an industry-standard, machine-parsable protocol for encoding software architecture as executable specifications using Mermaid diagram syntax. MAS serves as a bidirectional contract between LLM code generators, autonomous agents, and visual development environments, enabling semantic-rich architecture-as-code workflows with formal grammar, extensibility, and security guarantees.

This specification draws on industry standards including W3C specification guidelines, IETF RFC formats, OpenAPI structure, GraphQL formal grammar, Protocol Buffers syntax, Model Context Protocol (MCP) architecture, and Agent2Agent (A2A) interoperability patterns.

---

## Multi-Part Specification Structure

The MAS v2.0 specification is organized into **seven complementary parts**, each addressing distinct concerns while maintaining coherent integration:

### [Part 1: Core Specification](./MAS-Core-v2.0.md)

**Purpose:** Foundation protocol, formal grammar, conformance requirements  
**Audience:** Specification implementers, parser developers, standards bodies  
**Key Sections:**
- Introduction & Terminology
- Conformance Requirements (RFC 2119)
- Formal Grammar (EBNF)
- Core Type System
- Base Diagram Types
- Versioning & Compatibility
- Security Considerations
- Normative References

### [Part 2: Diagram Types](./MAS-DiagramTypes-v2.0.md)

**Purpose:** Extensible catalog of supported diagram types  
**Audience:** Diagram authors, tool developers, domain specialists  
**Key Sections:**
- Architecture Diagrams (topology, sequence, class)
- State & Workflow Diagrams (stateDiagram, journey, gitGraph)
- Deployment Diagrams (C4 deployment, infrastructure)
- Data Flow Diagrams (flowchart with data annotations)
- Entity-Relationship Diagrams (erDiagram)
- Timeline & Planning Diagrams (gantt, timeline)
- Custom Diagram Type Registration

### [Part 3: LLM Integration Protocol](./MAS-LLMProtocol-v2.0.md)

**Purpose:** Standardized agent-diagram communication patterns  
**Audience:** LLM application developers, agent framework builders  
**Key Sections:**
- Context Extraction Specification
- Prompt Engineering Templates
- Code Generation Contracts
- Bidirectional Translation (code ↔ diagram)
- Streaming Support
- Capability Negotiation
- Error Recovery Strategies

### [Part 4: Extensions & Plugins](./MAS-Extensions-v2.0.md)

**Purpose:** Plugin architecture for custom behaviors  
**Audience:** Extension developers, tool vendors  
**Key Sections:**
- Extension Points Catalog
- Custom Node Type Registration
- Custom Edge Type Registration
- Style Plugin System
- Validation Plugin Interface
- Transformation Plugin Interface
- Extension Registry Protocol

### [Part 5: Security & Validation](./MAS-Security-v2.0.md)

**Purpose:** Security considerations and validation algorithms  
**Audience:** Security engineers, compliance teams, validators  
**Key Sections:**
- Threat Model & Attack Vectors
- Input Sanitization Rules
- Execution Sandboxing
- Syntax Validation Algorithms
- Semantic Validation Rules
- Authentication & Authorization
- Privacy Considerations

### [Part 6: Reference Implementation](./MAS-Reference-v2.0.md)

**Purpose:** Implementation guidance and conformance testing  
**Audience:** Parser implementers, tool builders, QA engineers  
**Key Sections:**
- Parser Implementation Requirements
- Validator Implementation
- Code Generator Interface
- JSON Schema Definitions
- Conformance Test Suite
- Performance Benchmarks
- Interoperability Matrix

### [Part 7: Examples & Best Practices](./MAS-Examples-v2.0.md)

**Purpose:** Real-world usage patterns and migration guide  
**Audience:** Practitioners, solution architects, educators  
**Key Sections:**
- Complete System Examples
- Anti-patterns to Avoid
- Performance Optimization
- Accessibility Considerations
- Migration Guide from v1.0
- Case Studies
- FAQ

---

## Conformance

The MAS v2.0 specification uses **RFC 2119** keywords to indicate requirement levels:

- **<em class="rfc2119">MUST</em>:** Absolute requirement for conformance
- **<em class="rfc2119">MUST NOT</em>:** Absolute prohibition
- **<em class="rfc2119">SHOULD</em>:** Recommended practice, valid reasons may exist to ignore
- **<em class="rfc2119">SHOULD NOT</em>:** Not recommended, valid reasons may exist to proceed
- **<em class="rfc2119">MAY</em>:** Optional feature

### Conformance Classes

**Class 1: Minimal Conformance** (Core Only)
- <em class="rfc2119">MUST</em> implement Part 1 (Core Specification)
- <em class="rfc2119">MUST</em> support at least one diagram type from Part 2

**Class 2: Standard Conformance** (Production Use)
- <em class="rfc2119">MUST</em> implement Class 1 requirements
- <em class="rfc2119">MUST</em> implement Part 5 (Security & Validation)
- <em class="rfc2119">SHOULD</em> implement Part 3 (LLM Integration Protocol)

**Class 3: Full Conformance** (Enterprise/Extension)
- <em class="rfc2119">MUST</em> implement Class 2 requirements
- <em class="rfc2119">MUST</em> implement Part 4 (Extensions & Plugins)
- <em class="rfc2119">MUST</em> pass conformance tests from Part 6

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0.0 | 2025-11-21 | Major revision: Multi-part structure, formal EBNF grammar, LLM protocol, security framework, extension architecture |
| 1.0.0 | 2025-11-20 | Initial MAP-v1 specification |

---

## Compatibility Promise

**Backward Compatibility:**
- MAS v2.0 parsers <em class="rfc2119">MUST</em> accept valid MAS v1.0 documents
- MAS v2.0 introduces additive features only (no breaking syntax changes)
- Deprecated v1.0 features <em class="rfc2119">MUST</em> emit warnings

**Forward Compatibility:**
- Unknown extensions <em class="rfc2119">MUST</em> be ignored gracefully
- Versioning follows semantic versioning: `MAJOR.MINOR.PATCH`
- Protocol version negotiation follows MCP pattern (YYYY-MM-DD revision dates)

---

## Design Principles

Drawing from industry best practices:

1. **Determinism** (GraphQL): Identical architectural intent produces identical diagram structure
2. **Modularity** (MCP): Core protocol + optional feature negotiation
3. **Extensibility** (OpenAPI): Clear extension points without breaking changes
4. **Security by Default** (A2A): Input validation, sandboxing, auth support
5. **LLM-Friendly** (MCP): Machine-parsable with semantic annotations
6. **Composability** (Protocol Buffers): Diagrams reference and embed other diagrams
7. **Standards-Based** (RFC/W3C): Formal grammar, conformance requirements, normative language

---

## Implementation Status

| Feature | Status | Target Date |
|---------|--------|-------------|
| Part 1: Core Specification | ✅ Draft | 2025-11-21 |
| Part 2: Diagram Types | ✅ Draft | 2025-11-21 |
| Part 3: LLM Protocol | ✅ Draft | 2025-11-21 |
| Part 4: Extensions | ✅ Draft | 2025-11-21 |
| Part 5: Security | ✅ Draft | 2025-11-21 |
| Part 6: Reference Impl | ✅ Draft | 2025-11-21 |
| Part 7: Examples | ✅ Draft | 2025-11-21 |
| JSON Schema | 🚧 In Progress | 2025-11-30 |
| Conformance Tests | 📋 Planned | 2025-12-15 |
| Reference Parser | 📋 Planned | 2026-01-15 |

---

## Community & Governance

**Discussion:** GitHub Discussions ([link TBD])  
**Issues:** GitHub Issues ([link TBD])  
**Pull Requests:** Follow SEP (Specification Enhancement Proposal) process  
**License:** CC BY 4.0 (specification), Apache 2.0 (reference implementation)

---

## References

### Normative References

- **[RFC2119]** S. Bradner. "Key words for use in RFCs to Indicate Requirement Levels." RFC 2119, March 1997.
- **[Mermaid]** "Mermaid - Diagramming and charting tool." https://mermaid.js.org/
- **[JSON-RPC]** "JSON-RPC 2.0 Specification." https://www.jsonrpc.org/specification
- **[EBNF]** ISO/IEC 14977:1996. "Information technology — Syntactic metalanguage — Extended BNF."

### Informative References

- **[W3C-Manual]** "W3C Manual of Style." https://www.w3.org/guide/manual-of-style/
- **[RFC7322]** "RFC Style Guide." https://datatracker.ietf.org/doc/html/rfc7322
- **[OpenAPI]** "OpenAPI Specification v3.1." https://spec.openapis.org/oas/v3.1.1.html
- **[GraphQL]** "GraphQL Specification." https://spec.graphql.org/October2021/
- **[ProtoBuf]** "Protocol Buffers Edition 2024 Specification." https://protobuf.dev/reference/protobuf/edition-2024-spec/
- **[MCP]** "Model Context Protocol Specification." https://modelcontextprotocol.io/specification/
- **[A2A]** "Agent2Agent Protocol." https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/
- **[C4Model]** "C4 Model for Software Architecture." https://c4model.com/

---

## Copyright & License

Copyright © 2025 MAS Contributors. This specification is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

Reference implementations are licensed under Apache License 2.0.

---

**Next:** [Part 1: Core Specification](./MAS-Core-v2.0.md)
