# Mermaid Abstract Syntax (MAS) v2.6 Specification

**Status:** Draft Specification  
**Version:** 2.6.0  
**Date:** 2025-11-29  
**Editors:** MAS Specification Editors  
**Previous Version:** [MAS v2.5](./MAS-v2.5-INDEX.md)

---

## Abstract

The Mermaid Abstract Syntax (MAS) v2.6 extends the v2.5 specification with **ASCII Text Wireframe UI specifications**. Since Mermaid.js does not natively support wireframe diagram types, MAS v2.6 introduces a standardized ASCII wireframe syntax that works within code blocks and maintains full backward compatibility with MAS v2.5 entities (actors, screens, actions, business entities).

**Key Enhancements in v2.6**:
- ✅ **Part 10**: ASCII Wireframe UI Specification
- ✅ Standardized widget vocabulary (buttons, inputs, checkboxes, dropdowns)
- ✅ Grid-based layout system with semantic annotations
- ✅ Full entity integration with MAS v2.5 actors, screens, and actions
- ✅ Cross-reference linking to user journeys, flows, and business processes
- ✅ WCAG accessibility annotation support

This specification maintains full backward compatibility with MAS v2.0, v2.5 while adding semantic-rich wireframe modeling for UX designers, frontend architects, and product teams.

---

## What's New in v2.6

### ASCII Wireframe UI Diagrams (Part 10)

**Problem Solved**: Mermaid.js lacks native wireframe/mockup support. MAS v2.6 provides a standardized ASCII syntax that:
- Preserves monospace layout for visual fidelity
- Uses semantic widget vocabulary inspired by PlantUML Salt
- Integrates with existing MAS v2.5 entity models
- Renders consistently in documentation, IDEs, and chat interfaces

**New Capabilities**:
1. **Wireframe Code Blocks** - Fenced blocks with `wireframe` type indicator
2. **Widget Vocabulary** - 15+ UI control types with consistent syntax
3. **Layout Containers** - Grids, group boxes, tabs, menus, trees
4. **Entity Annotations** - Link wireframes to actors, actions, accessibility
5. **Screen Regions** - Semantic section types (header, content, footer, modal)
6. **Cross-Reference** - Link to user journeys, flows, business processes

---

## Multi-Part Specification Structure

The MAS v2.6 specification consists of **ten complementary parts**:

### Core Specification (MAS v2.0)

| Part | Title | Audience | Status |
|------|-------|----------|--------|
| 1 | [Core Specification](../v2.0/MAS-Core-v2.0.md) | Implementers, parser developers | ✅ Stable |
| 2 | [Diagram Types](../v2.0/MAS-DiagramTypes-v2.0.md) | Diagram authors, architects | ✅ Stable |
| 3 | [LLM Integration Protocol](./MAS-LLMProtocol-v2.0.md) | Agent framework builders | ✅ Stable |
| 4 | [Extensions & Plugins](./MAS-Extensions-v2.0.md) | Extension developers | ✅ Stable |
| 5 | [Security & Validation](./MAS-Security-v2.0.md) | Security engineers | ✅ Stable |
| 6 | [Reference Implementation](./MAS-Reference-v2.0.md) | Parser implementers | ✅ Stable |
| 7 | [Examples & Best Practices](./MAS-Examples-v2.0.md) | Practitioners | ✅ Stable |

### UX & Business Modeling (MAS v2.5)

| Part | Title | Audience | Status |
|------|-------|----------|--------|
| 8 | [UX & User Experience Diagrams](./MAS-UXDiagrams-v2.5.md) | UX designers, frontend architects | ✅ Stable |
| 9 | [Business Operations & Process](./MAS-BusinessOperations-v2.5.md) | Product owners, business analysts | ✅ Stable |

### ASCII Wireframe (MAS v2.6) ⭐ NEW

| Part | Title | Audience | Status |
|------|-------|----------|--------|
| 10 | [ASCII Wireframe UI Specification](./MAS-ASCIIWireframe-v2.6.md) ⭐ | UX designers, frontend devs, product teams | 📋 Draft |

---

## Part 10: ASCII Wireframe UI Specification

### 10.1 Wireframe Block Syntax

ASCII wireframes use fenced code blocks with `wireframe` language identifier:

````markdown
```wireframe
@title: Screen Title
@screen: ScreenIdentifier
@actor: User

+----------------------------------+
|  Content goes here               |
+----------------------------------+
```
````

**Block Structure:**
```
wireframe_block = "```wireframe" NEWLINE 
                  metadata*
                  content 
                  "```"

metadata = "@" KEY ":" VALUE NEWLINE
content  = (widget | layout | annotation | separator | box_drawing)*
```

### 10.2 Widget Vocabulary

MAS v2.6 defines a standardized vocabulary for UI controls:

| Widget | ASCII Syntax | Example | Notes |
|--------|--------------|---------|-------|
| **Button (Primary)** | `[Text]` | `[Submit]` | Main action button |
| **Button (Secondary)** | `[[Text]]` | `[[Cancel]]` | Secondary/ghost button |
| **Button (Icon)** | `[<icon>]` | `[<search>]` | Icon-only button |
| **Button (Disabled)** | `[-Text-]` | `[-Submit-]` | Disabled state |
| **Text Input** | `"Placeholder"` | `"Enter email"` | Single-line input |
| **Text Input (Width)** | `[____]` | `[__________]` | Width = underscore count |
| **Password Input** | `[****]` | `[********]` | Masked input |
| **Checkbox (Unchecked)** | `[ ]` | `[ ] Option` | Unchecked state |
| **Checkbox (Checked)** | `[x]` or `[X]` | `[x] Selected` | Checked state |
| **Radio (Unchecked)** | `( )` | `( ) Option A` | Unselected radio |
| **Radio (Selected)** | `(o)` or `(O)` | `(o) Option B` | Selected radio |
| **Dropdown (Closed)** | `^Label^` | `^Select country^` | Closed dropdown |
| **Dropdown (Open)** | `^Label^^ item ^^` | `^Country^^ USA ^^ UK ^` | Expanded dropdown |
| **Textarea** | `{+ text +}` | `{+ Enter description... +}` | Multi-line text |
| **Label/Text** | Plain text | `Username:` | Static text |
| **Link** | `[text](#)` | `[Forgot password?](#reset)` | Clickable link |
| **Image Placeholder** | `[img:alt]` | `[img:Profile Photo]` | Image placeholder |
| **Slider** | `[===o====]` | `[===o====]` | Range slider |
| **Toggle** | `[OFF]` / `[ON]` | `[ON]` | Toggle switch |
| **Progress** | `[####----]` | `[######--]` | Progress bar |

### 10.3 Layout Containers

#### Grid Layout (Columns with `|`)
```wireframe
{ Label:     | "Enter value    " }
{ Email:     | "user@example.com" }
{ Password:  | "************    " }
{ [Cancel]   | [Submit]          }
```

#### Group Box (Titled Container)
```wireframe
{^"Login Form"
  Email:    | "user@example.com"
  Password: | "************"
  ..
  [x] Remember me
  [Login]
^}
```

#### Tab Strip
```wireframe
{/ General | Security | Privacy | Advanced }
```

#### Menu Bar
```wireframe
{* File | Edit | View | Tools | Help }
```

#### Tree Widget
```wireframe
{T
  + Documents
  ++ Work
  +++ Project A
  +++ Project B
  ++ Personal
  + Downloads
}
```

#### Horizontal Separator Types
```wireframe
..        # Dotted separator
--        # Dashed separator
==        # Double line separator
~~        # Wavy separator
___       # Solid underline
```

### 10.4 Annotation System

Annotations add semantic metadata to wireframe elements:

```wireframe
@title: Login Screen
@screen: LoginPage
@actor: Guest User
@version: 1.0
@journey-section: Onboarding
@flow-step: step-2
```

**Annotation Types:**

| Annotation | Purpose | Example |
|------------|---------|---------|
| `@title` | Screen title | `@title: User Dashboard` |
| `@screen` | Screen identifier (links to MAS v2.5 screens) | `@screen: DashboardPage` |
| `@actor` | Primary actor (links to MAS v2.5 actors/roles) | `@actor: Admin` |
| `@version` | Wireframe version | `@version: 2.1` |
| `@id` | Element identifier | `@id: submit-btn` |
| `@type` | Region/component type | `@type: header` |
| `@action` | User action trigger (links to MAS v2.5 actions) | `@action: submit-form` |
| `@a11y` | Accessibility annotation | `@a11y: aria-label="Submit"` |
| `@state` | Widget state | `@state: disabled` |
| `@variant` | Visual variant | `@variant: primary` |
| `@journey-section` | Link to user journey section | `@journey-section: Checkout` |
| `@flow-step` | Link to user flow step | `@flow-step: payment-step` |
| `@process-task` | Link to business process task | `@process-task: approve-order` |

### 10.5 Entity Integration (MAS v2.5 Backward Compatibility)

MAS v2.6 wireframes integrate with all MAS v2.5 entities:

#### Actor Integration
```wireframe
@actor: Customer
@actor: System

+----------------------------------+
|  Welcome, {{actor.Customer}}!    |
|  [View Orders] [Account Settings]|
+----------------------------------+
```

#### Screen Integration
```wireframe
@screen: CheckoutPage
@flow-step: checkout-step-3

# This wireframe represents the "CheckoutPage" screen
# from the user flow diagram
```

#### Action Integration
```wireframe
{^"Payment Form"
  Card Number: | [________________]
  Expiry:      | [__]/[__]
  CVV:         | [___]
  ..
  @action: process-payment
  [Pay Now]  @a11y: aria-label="Complete payment"
^}
```

#### Accessibility Integration (WCAG 2.1)
```wireframe
@a11y-tab-order: 1
Email: | "email@example.com"

@a11y-tab-order: 2  
@a11y: aria-required="true"
Password: | "************"

@a11y-tab-order: 3
@a11y: aria-label="Sign in to your account"
[Login]
```

### 10.6 Screen Regions

Standard region types for consistent wireframe structure:

```wireframe
@title: E-commerce Product Page

+==========================================+
| @type: header                            |
| {* Home | Products | Cart | Account }    |
| [<search>] "Search products..."          |
+==========================================+
|         |                                |
| @type:  | @type: content                 |
| sidebar |                                |
|         | [img:Product Image]            |
| Filters | Product Title                  |
| ======= | $99.99                         |
| Price   | (o) Small ( ) Medium ( ) Large |
| [ ] $50 | Quantity: [__]                 |
| [ ] $100|                                |
|         | [Add to Cart] [[Wishlist]]     |
|         |                                |
+==========================================+
| @type: footer                            |
| [Terms] | [Privacy] | [Contact]          |
+==========================================+
```

**Standard Region Types:**

| Region | Annotation | Typical Contents |
|--------|------------|------------------|
| Header | `@type: header` | Navigation, branding, search |
| Sidebar | `@type: sidebar` | Filters, secondary nav |
| Content | `@type: content` | Main content area |
| Form | `@type: form` | User input forms |
| Card | `@type: card` | Content containers |
| List | `@type: list` | Item collections |
| Modal | `@type: modal` | Overlay dialogs |
| Footer | `@type: footer` | Legal links, copyright |
| Toast | `@type: toast` | Notification messages |
| Toolbar | `@type: toolbar` | Action buttons |

### 10.7 Box Drawing Characters (Optional)

For enhanced visual clarity, use Unicode box-drawing characters:

```wireframe
┌────────────────────────────────────┐
│  Login                             │
├────────────────────────────────────┤
│                                    │
│  Email:    ┌──────────────────┐    │
│            │ user@example.com │    │
│            └──────────────────┘    │
│                                    │
│  Password: ┌──────────────────┐    │
│            │ **************** │    │
│            └──────────────────┘    │
│                                    │
│  ☐ Remember me                     │
│                                    │
│  ┌──────────┐  ┌──────────────┐    │
│  │  Cancel  │  │    Login     │    │
│  └──────────┘  └──────────────┘    │
│                                    │
│  Forgot password?                  │
│                                    │
└────────────────────────────────────┘
```

**Box Drawing Character Reference:**

| Character | Code | Purpose |
|-----------|------|---------|
| `┌` | U+250C | Top-left corner |
| `┐` | U+2510 | Top-right corner |
| `└` | U+2514 | Bottom-left corner |
| `┘` | U+2518 | Bottom-right corner |
| `─` | U+2500 | Horizontal line |
| `│` | U+2502 | Vertical line |
| `├` | U+251C | Left T-junction |
| `┤` | U+2524 | Right T-junction |
| `┬` | U+252C | Top T-junction |
| `┴` | U+2534 | Bottom T-junction |
| `┼` | U+253C | Cross junction |
| `═` | U+2550 | Double horizontal |
| `║` | U+2551 | Double vertical |

### 10.8 Validation Rules

MAS v2.6 wireframes MUST pass these validation checks:

1. **Syntax Validity**
   - All widgets use defined syntax
   - Layout containers are properly closed
   - Annotations follow `@key: value` format

2. **Entity Consistency**
   - `@actor` values SHOULD exist in linked MAS v2.5 diagrams
   - `@screen` values SHOULD match user flow screens
   - `@action` values SHOULD match defined actions

3. **Accessibility Compliance**
   - Interactive elements SHOULD have `@a11y` annotations
   - Tab order SHOULD be logical (1, 2, 3...)
   - Form inputs SHOULD have labels

4. **Layout Integrity**
   - Grid columns SHOULD be consistent per row
   - Box drawing characters SHOULD form closed rectangles
   - Nested containers SHOULD be properly indented

---

## Conformance

### v2.6 Conformance Classes

**Class 2.6-Wireframe: ASCII Wireframe Conformance**
- <em class="rfc2119">MUST</em> implement MAS v2.5 Class 2.5-Full
- <em class="rfc2119">MUST</em> implement Part 10 (ASCII Wireframes)
- <em class="rfc2119">MUST</em> parse wireframe code blocks
- <em class="rfc2119">MUST</em> validate widget syntax
- <em class="rfc2119">SHOULD</em> support entity cross-references
- <em class="rfc2119">MAY</em> render wireframes as HTML mockups

**Class 2.6-Full: Complete v2.6 Conformance**
- <em class="rfc2119">MUST</em> implement Class 2.6-Wireframe
- <em class="rfc2119">MUST</em> validate cross-references to MAS v2.5 entities
- <em class="rfc2119">MUST</em> pass all v2.6 conformance tests

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| **2.6.0** | 2025-11-29 | **NEW**: Part 10 (ASCII Wireframe UI Specification) |
| 2.5.0 | 2025-11-27 | Part 8 (UX Diagrams), Part 9 (Business Operations) |
| 2.0.0 | 2025-11-21 | Multi-part structure, formal grammar, LLM protocol |
| 1.0.0 | 2025-11-20 | Initial MAP-v1 specification |

---

## Compatibility Promise

**Backward Compatibility:**
- MAS v2.6 parsers <em class="rfc2119">MUST</em> accept valid MAS v2.0, v2.5 documents
- All v2.0 and v2.5 conformance tests <em class="rfc2119">MUST</em> pass
- No breaking changes to existing syntax or semantics

**Forward Compatibility:**
- v2.5 parsers <em class="rfc2119">MAY</em> ignore wireframe code blocks gracefully
- Unknown annotations <em class="rfc2119">MUST</em> emit warnings, not errors
- Versioning follows semantic versioning: `2.6.x` for enhancements

---

## Quick Start Examples

### Example 1: Simple Login Form

```wireframe
@title: Login
@screen: LoginPage
@actor: Guest

+---------------------------+
|  Example Login           |
+---------------------------+
|                           |
|  Email:                   |
|  "user@example.com      " |
|                           |
|  Password:                |
|  "******************    " |
|                           |
|  [x] Remember me          |
|                           |
|  [[Cancel]]  [Login]      |
|                           |
|  [Forgot password?](#)    |
+---------------------------+
```

### Example 2: Dashboard with Sidebar

```wireframe
@title: Dashboard
@screen: DashboardPage
@actor: Authenticated User

+========================================+
| @type: header                          |
| Example  {* Chats | Brains | Studio } |
| [<bell>] [<settings>] [<user>]         |
+========================================+
|          |                             |
| @type:   | @type: content              |
| sidebar  |                             |
|          | Recent Chats                |
| Brains   | +--------------------------+|
| ======   | | Chat with Marketing AI   ||
| My Brain | | Yesterday                 ||
| Team AI  | +--------------------------+|
| Research | | Product Research          ||
|          | | 2 days ago                ||
| ======   | +--------------------------+|
| [+ New]  |                             |
|          | [Start New Chat]            |
+========================================+
```

### Example 3: Modal Dialog

```wireframe
@title: Confirm Delete
@type: modal
@action: delete-item

+----------------------------------+
|  ⚠️ Confirm Deletion             |
+----------------------------------+
|                                  |
|  Are you sure you want to delete |
|  "Project Alpha"?                |
|                                  |
|  This action cannot be undone.   |
|                                  |
|  [[Cancel]]     [Delete]         |
|                 @variant: danger |
+----------------------------------+
```

### Example 4: Form with Validation States

```wireframe
@title: Registration
@screen: RegisterPage
@actor: New User

{^"Create Account"
  
  Username:
  "johndoe        "
  @state: valid ✓
  
  Email:
  "invalid-email  "
  @state: error ✗ "Please enter valid email"
  
  Password:
  "********       "
  @state: warning "Password is weak"
  
  Confirm Password:
  "********       "
  
  ..
  
  [x] I agree to Terms of Service
  [ ] Subscribe to newsletter
  
  ..
  
  [Create Account]
  @action: register-user
  
^}
```

### Example 5: E-commerce Product Page (Complex)

```wireframe
@title: Product Details
@screen: ProductPage
@actor: Shopper
@journey-section: Evaluation

┌──────────────────────────────────────────────────────┐
│ @type: header                                        │
│ 🏠 Home > Electronics > Headphones                   │
│ ─────────────────────────────────────────────────── │
│ [<search>] "Search products..."   [<cart> (3)]      │
└──────────────────────────────────────────────────────┘
┌─────────────────────┬────────────────────────────────┐
│                     │                                │
│  [img:Product       │  Example Pro Headphones       │
│   Main Photo]       │  ⭐⭐⭐⭐☆ (128 reviews)         │
│                     │                                │
│  ┌───┐ ┌───┐ ┌───┐  │  $299.99  ~~$349.99~~          │
│  │ 1 │ │ 2 │ │ 3 │  │                                │
│  └───┘ └───┘ └───┘  │  Color:                        │
│                     │  (o) Black ( ) White ( ) Blue  │
│                     │                                │
│                     │  Size:                         │
│                     │  ^Standard^                    │
│                     │                                │
│                     │  Quantity: [_2_]               │
│                     │                                │
│                     │  [Add to Cart]                 │
│                     │  @action: add-to-cart          │
│                     │  [[Add to Wishlist]]           │
│                     │                                │
└─────────────────────┴────────────────────────────────┘
┌──────────────────────────────────────────────────────┐
│ @type: tabs                                          │
│ {/ Description | Specifications | Reviews (128) }   │
│ ─────────────────────────────────────────────────── │
│ Premium noise-canceling headphones with 40-hour     │
│ battery life and Hi-Res audio certification.        │
│                                                      │
│ • Active Noise Cancellation                         │
│ • 40-hour battery life                              │
│ • Bluetooth 5.3                                     │
│ • Foldable design                                   │
└──────────────────────────────────────────────────────┘
```

---

## MAS Compiler Integration

### v2.6 API Endpoints

#### Generate ASCII Wireframe
```http
POST /api/v1/compile/wireframe
Content-Type: application/json

{
  "description": "Login form with email, password, remember me checkbox, and forgot password link",
  "screen": "LoginPage",
  "actor": "Guest",
  "style": "box_drawing"  // or "ascii_simple"
}
```

**Response:**
```json
{
  "success": true,
  "wireframe_code": "```wireframe\n@title: Login\n...\n```",
  "diagram_type": "wireframe",
  "widgets_detected": ["textbox", "textbox", "checkbox", "button", "link"],
  "entity_links": {
    "screens": ["LoginPage"],
    "actors": ["Guest"],
    "actions": ["login", "forgot-password"]
  }
}
```

#### Validate Wireframe
```http
POST /api/v1/validate/wireframe
Content-Type: application/json

{
  "wireframe_code": "```wireframe\n@title: Test\n[Button]\n```",
  "validate_entities": true,
  "check_accessibility": true
}
```

---

## Migration Guide: v2.5 → v2.6

### For Existing v2.5 Users

**No changes required** - All v2.5 diagrams remain valid in v2.6.

**To adopt v2.6 wireframes**:

1. **Create Wireframe Blocks**:
   ````markdown
   ```wireframe
   @title: My Screen
   @screen: ScreenName
   
   [Button] "Input field"
   ```
   ````

2. **Link to Existing Diagrams**:
   ```wireframe
   @journey-section: Onboarding  # Links to user journey
   @flow-step: step-3            # Links to user flow
   @process-task: approve        # Links to business process
   ```

3. **Add Accessibility Annotations**:
   ```wireframe
   @a11y: aria-label="Submit form"
   @a11y-tab-order: 1
   ```

---

## Design Principles (Extended for v2.6)

Building on v2.5 principles:

1. **Text-First Rendering**: Wireframes render as plain text without dependencies
2. **Semantic Widgets**: Consistent vocabulary maps to real UI components
3. **Entity Continuity**: Wireframes link to actors, screens, actions from v2.5
4. **Progressive Enhancement**: Basic ASCII → Box Drawing → HTML rendering
5. **Accessibility Awareness**: Built-in WCAG annotation support
6. **IDE Compatibility**: Works in any text editor or documentation tool
7. **LLM Friendly**: Structured format for AI-assisted wireframe generation

---

## Implementation Status

| Feature | Status | Target Date |
|---------|--------|-------------|
| Part 1-7 (v2.0 Core) | ✅ Stable | 2025-11-21 |
| Part 8-9 (v2.5 UX/Business) | ✅ Stable | 2025-11-27 |
| **Part 10: ASCII Wireframe Spec** | ✅ Draft | 2025-11-29 |
| **Wireframe Parser** | 📋 Planned | 2025-12-15 |
| **Widget Validation Rules** | 📋 Planned | 2025-12-30 |
| **HTML Rendering Engine** | 📋 Planned | 2026-01-15 |
| **Entity Cross-Reference Validator** | 📋 Planned | 2026-01-30 |

---

## References

### Normative References

All v2.0 and v2.5 references plus:

- **[PlantUML-Salt]** Salt (Wireframe) Syntax. PlantUML. https://plantuml.com/salt
- **[Mockdown]** ASCII-art language for UI mock-ups. GitHub. https://github.com/sminnee/mockdown
- **[Unicode-BoxDrawing]** Box Drawing Characters. Unicode Consortium. https://unicode.org/charts/PDF/U2500.pdf
- **[ASCIIFlow]** ASCII Diagram Editor. https://asciiflow.com/

### Informative References

- **[Balsamiq]** Lo-fi Wireframing Principles. Balsamiq Studios.
- **[NielsenWireframes]** "Wireframes: An Overview." Nielsen Norman Group.
- **[MermaidIssue1184]** "Wireframes in mermaid." GitHub Issue. https://github.com/mermaid-js/mermaid/issues/1184

---

## Copyright & License

Copyright © 2025 MAS Contributors. This specification is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

Reference implementations are licensed under Apache License 2.0.

---

## Navigation

**Start Here:**
- [Part 1: Core Specification](../v2.0/MAS-Core-v2.0.md) - Foundation
- [Part 8: UX Diagrams](./MAS-UXDiagrams-v2.5.md) - User experience
- [Part 10: ASCII Wireframes](./MAS-ASCIIWireframe-v2.6.md) ⭐ NEW - UI prototypes

**For Implementers:**
- [Part 6: Reference Implementation](./MAS-Reference-v2.0.md)
- [Implementation Roadmap](./MAS-Implementation-Roadmap.md)

**For Users:**
- [Part 7: Examples & Best Practices](./MAS-Examples-v2.0.md)
- [v2.6 Quick Start Guide](./MAS-v2.6-QuickStart.md)

---

**Version:** 2.6.0 | **Status:** Draft Specification | **Last Updated:** 2025-11-29
