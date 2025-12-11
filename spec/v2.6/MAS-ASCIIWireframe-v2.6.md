# MAS v2.6 Part 10: ASCII Wireframe UI Specification

**Status:** Draft Specification  
**Version:** 2.6.0  
**Date:** 2025-11-29  
**Parent:** [MAS v2.6 Index](./MAS-v2.6-INDEX.md)

---

## 1. Introduction

### 1.1 Purpose

This specification defines the ASCII Wireframe UI diagram type for MAS v2.6. It provides a standardized text-based syntax for creating UI mockups and wireframes that:

- Work within any text editor, IDE, or documentation system
- Maintain visual fidelity through monospace font rendering
- Integrate with existing MAS v2.5 entity models (actors, screens, actions)
- Support accessibility annotations (WCAG 2.1)
- Enable AI-assisted wireframe generation

### 1.2 Background

Mermaid.js does not natively support wireframe diagrams (see [Issue #1184](https://github.com/mermaid-js/mermaid/issues/1184)). MAS v2.6 addresses this gap by defining an ASCII-based wireframe syntax inspired by:

- **PlantUML Salt**: Widget vocabulary and layout patterns
- **Mockdown**: ASCII art UI mockup syntax
- **Box Drawing Characters**: Unicode standard for visual structure

### 1.3 Scope

This specification covers:
- Wireframe code block syntax
- Widget vocabulary (15+ UI control types)
- Layout containers (grids, tabs, menus, trees)
- Annotation system for metadata
- Entity integration with MAS v2.5
- Validation rules and conformance

---

## 2. Formal Grammar (EBNF)

```ebnf
(* MAS v2.6 ASCII Wireframe Grammar *)

wireframe_block = "```wireframe" NEWLINE
                  { metadata_line }
                  { content_line }
                  "```" ;

metadata_line   = "@" identifier ":" value NEWLINE ;
identifier      = letter { letter | digit | "_" | "-" } ;
value           = { printable_char } ;

content_line    = ( widget | layout | annotation | separator | text ) NEWLINE ;

(* Widgets *)
widget          = button | textbox | checkbox | radio | dropdown 
                | textarea | link | image | slider | toggle | progress ;

button          = primary_btn | secondary_btn | icon_btn | disabled_btn ;
primary_btn     = "[" text "]" ;
secondary_btn   = "[[" text "]]" ;
icon_btn        = "[<" icon_name ">]" ;
disabled_btn    = "[-" text "-]" ;

textbox         = quoted_input | underscore_input | password_input ;
quoted_input    = '"' { printable_char } '"' ;
underscore_input = "[" { "_" } "]" ;
password_input  = "[" { "*" } "]" ;

checkbox        = unchecked_box | checked_box ;
unchecked_box   = "[ ]" ;
checked_box     = "[x]" | "[X]" ;

radio           = unselected_radio | selected_radio ;
unselected_radio = "( )" ;
selected_radio  = "(o)" | "(O)" ;

dropdown        = closed_dropdown | open_dropdown ;
closed_dropdown = "^" text "^" ;
open_dropdown   = "^" text "^" { "^" item "^" } ;

textarea        = "{+" { printable_char | NEWLINE } "+}" ;

link            = "[" text "](" url ")" ;

image           = "[img:" alt_text "]" ;

slider          = "[" { "=" } "o" { "=" } "]" ;

toggle          = "[OFF]" | "[ON]" ;

progress        = "[" { "#" } { "-" } "]" ;

(* Layouts *)
layout          = grid | groupbox | tabs | menu | tree | container ;

grid            = "{" row { "|" row } "}" ;
row             = cell { "|" cell } ;
cell            = widget | text ;

groupbox        = '{^"' title '"' { content_line } '^}' ;

tabs            = "{/" tab_item { "|" tab_item } "}" ;
tab_item        = text ;

menu            = "{*" menu_item { "|" menu_item } "}" ;
menu_item       = text ;

tree            = "{T" { tree_node } "}" ;
tree_node       = { "+" } text NEWLINE ;

container       = "{#" { content_line } "#}" ;

(* Annotations *)
annotation      = "@" annotation_type ":" annotation_value ;
annotation_type = "id" | "type" | "actor" | "action" | "a11y" 
                | "state" | "variant" | "screen" | "title" | "version"
                | "journey-section" | "flow-step" | "process-task"
                | "a11y-tab-order" ;

(* Separators *)
separator       = ".." | "--" | "==" | "~~" | "___" ;

(* Box Drawing (Optional) *)
box_char        = "┌" | "┐" | "└" | "┘" | "─" | "│" 
                | "├" | "┤" | "┬" | "┴" | "┼"
                | "═" | "║" | "╔" | "╗" | "╚" | "╝" ;

(* Terminals *)
text            = { printable_char } ;
printable_char  = ? any printable Unicode character ? ;
letter          = "A".."Z" | "a".."z" ;
digit           = "0".."9" ;
NEWLINE         = ? newline character ? ;
```

---

## 3. Widget Vocabulary

### 3.1 Button Widgets

| Type | Syntax | Rendering | Use Case |
|------|--------|-----------|----------|
| Primary Button | `[Submit]` | Filled, prominent | Main actions |
| Secondary Button | `[[Cancel]]` | Outlined/ghost | Secondary actions |
| Icon Button | `[<search>]` | Icon only | Toolbar actions |
| Disabled Button | `[-Submit-]` | Grayed out | Unavailable actions |

**Examples:**
```wireframe
[Submit Order]          # Primary
[[View Details]]        # Secondary  
[<menu>] [<settings>]   # Icons
[-Processing...-]       # Disabled
```

### 3.2 Input Widgets

| Type | Syntax | Rendering | Use Case |
|------|--------|-----------|----------|
| Text Input | `"Placeholder text"` | Input with placeholder | Short text |
| Sized Input | `[__________]` | Input with defined width | Form fields |
| Password | `[********]` | Masked input | Passwords |
| Textarea | `{+ Multi-line text +}` | Multi-line input | Long text |

**Examples:**
```wireframe
"Enter your email     "   # With placeholder
[____________________]    # Width = 20 chars
[****************]        # Password field
{+ Enter your message here...
   
   This supports multiple lines. +}
```

### 3.3 Selection Widgets

| Type | Syntax | Rendering | Use Case |
|------|--------|-----------|----------|
| Checkbox (unchecked) | `[ ] Label` | Empty checkbox | Multi-select |
| Checkbox (checked) | `[x] Label` | Checked checkbox | Selected option |
| Radio (unselected) | `( ) Label` | Empty radio | Single-select |
| Radio (selected) | `(o) Label` | Filled radio | Selected option |
| Dropdown (closed) | `^Label^` | Collapsed select | Selection list |
| Dropdown (open) | `^Label^^ Item1 ^^ Item2 ^` | Expanded select | Showing options |

**Examples:**
```wireframe
[x] I agree to Terms
[ ] Subscribe to newsletter

(o) Credit Card  ( ) PayPal  ( ) Bank Transfer

^Select Country^
^Country^^ United States ^^ United Kingdom ^^ Germany ^
```

### 3.4 Display Widgets

| Type | Syntax | Rendering | Use Case |
|------|--------|-----------|----------|
| Label | Plain text | Static text | Labels, descriptions |
| Link | `[text](#url)` | Clickable text | Navigation |
| Image | `[img:alt]` | Image placeholder | Media |
| Slider | `[===o====]` | Range control | Value selection |
| Toggle | `[ON]` / `[OFF]` | Switch | Boolean setting |
| Progress | `[####----]` | Progress bar | Loading state |

**Examples:**
```wireframe
Welcome to Example App!        # Label
[Forgot password?](#reset)  # Link
[img:Profile Photo]         # Image placeholder
Volume: [====o=====]        # Slider at 40%
Notifications: [ON]         # Toggle enabled
Loading: [######----] 60%   # Progress bar
```

---

## 4. Layout Containers

### 4.1 Grid Layout

Organize content in columns using `|` separator:

```wireframe
{ Column 1  | Column 2  | Column 3  }
{ Cell A    | Cell B    | Cell C    }
{ [Button1] | [Button2] | [Button3] }
```

**Form Layout Example:**
```wireframe
{ First Name:  | "John           " }
{ Last Name:   | "Doe            " }
{ Email:       | "john@example.com" }
{ Password:    | "****************" }
{              | [Register]        }
```

### 4.2 Group Box

Titled container for grouping related content:

```wireframe
{^"Personal Information"
  First Name: | "John       "
  Last Name:  | "Doe        "
  Birth Date: | [__]/[__]/[____]
^}

{^"Preferences"
  [x] Receive emails
  [ ] Enable dark mode
  
  Language: ^English^
^}
```

### 4.3 Tab Strip

Horizontal tab navigation:

```wireframe
{/ General | Security | Privacy | Notifications | Advanced }

# Active tab styling (optional)
{/ *General* | Security | Privacy }
```

### 4.4 Menu Bar

Application menu bar:

```wireframe
{* File | Edit | View | Tools | Help }

# With keyboard shortcuts
{* File (Alt+F) | Edit (Alt+E) | View (Alt+V) }
```

### 4.5 Tree Widget

Hierarchical tree structure:

```wireframe
{T
  + Documents
  ++ Work
  +++ Reports
  ++++ Q1 Report.pdf
  ++++ Q2 Report.pdf
  +++ Presentations
  ++ Personal
  + Downloads
  + Pictures
}
```

### 4.6 Container

Generic container for grouping:

```wireframe
{#
  This content is inside a container.
  [Button A] [Button B]
#}
```

---

## 5. Annotation System

### 5.1 Metadata Annotations

Top-level wireframe metadata:

```wireframe
@title: User Dashboard
@screen: DashboardPage
@actor: Authenticated User
@version: 2.1
@author: UX Team
```

### 5.2 Element Annotations

Annotations for specific elements:

```wireframe
@id: submit-btn
@action: submit-form
[Submit]

@id: email-field
@a11y: aria-label="Email address"
"Enter your email"

@state: disabled
[-Submit-]

@variant: danger
[Delete Account]
```

### 5.3 Cross-Reference Annotations

Link to other MAS diagrams:

```wireframe
@journey-section: Onboarding
@flow-step: registration-step-3
@process-task: validate-user

# This wireframe represents step 3 of the onboarding flow
# and corresponds to the "validate-user" business process task
```

### 5.4 Accessibility Annotations

WCAG 2.1 compliance annotations:

```wireframe
@a11y-tab-order: 1
Email: "user@example.com"

@a11y-tab-order: 2
@a11y: aria-required="true"
Password: "************"

@a11y-tab-order: 3
@a11y: aria-label="Sign in to your account"
@a11y: role="button"
[Login]

@a11y: aria-live="polite"
@type: toast
Success! You have logged in.
```

---

## 6. Entity Integration (MAS v2.5 Compatibility)

### 6.1 Actor Entity Mapping

MAS v2.5 defines actors in `UXIntentResult.actors` and `BusinessIntentResult.roles`. Wireframes reference these:

```wireframe
@actor: Customer
@actor: Admin

# Customer sees:
+---------------------------+
|  Welcome, Customer!       |
|  [View Orders]            |
+---------------------------+

# Admin additionally sees:
+---------------------------+
|  Admin Panel              |
|  [Manage Users]           |
|  [View Reports]           |
+---------------------------+
```

### 6.2 Screen Entity Mapping

MAS v2.5 `UXIntentResult.screens` defines screen names. Wireframes implement these:

```wireframe
@screen: LoginPage
# Implements the "LoginPage" screen from user flow diagram

@screen: DashboardPage
# Implements the "DashboardPage" screen
```

### 6.3 Action Entity Mapping

MAS v2.5 `UXIntentResult.actions` defines user actions. Wireframes trigger these:

```wireframe
@action: login
[Login]

@action: register
[Create Account]

@action: forgot-password
[Forgot password?](#reset)
```

### 6.4 Satisfaction Integration

MAS v2.5 `UXIntentResult.satisfaction_hints` maps to wireframe annotations:

```wireframe
@satisfaction: high
# User is delighted at this screen

@satisfaction: low
@pain-point: confusing-layout
# User experiences frustration here
```

---

## 7. Screen Region Types

### 7.1 Standard Regions

```wireframe
@type: header
+----------------------------------+
|  Logo   {* Nav1 | Nav2 | Nav3 }  |
|  [<search>] [<user>]             |
+----------------------------------+

@type: sidebar
| Filters    |
| ========== |
| [ ] Option1|
| [ ] Option2|

@type: content
| Main content area                |
| [Card 1] [Card 2] [Card 3]       |

@type: footer
| Terms | Privacy | Contact        |
| © 2025 Example Corp                  |
```

### 7.2 Modal/Overlay

```wireframe
@type: modal
@id: confirm-dialog

+----------------------------------+
|  ⚠️ Confirm Action               |
+----------------------------------+
|                                  |
|  Are you sure you want to        |
|  delete this item?               |
|                                  |
|  [[Cancel]]    [Delete]          |
|                @variant: danger  |
+----------------------------------+
```

### 7.3 Toast/Notification

```wireframe
@type: toast
@variant: success
┌────────────────────────────────┐
│ ✓ Changes saved successfully   │
└────────────────────────────────┘

@type: toast
@variant: error
┌────────────────────────────────┐
│ ✗ Failed to save changes       │
│   [Retry] [Dismiss]            │
└────────────────────────────────┘
```

---

## 8. Validation Rules

### 8.1 Syntax Validation

| Rule ID | Description | Severity |
|---------|-------------|----------|
| SYN-001 | Widget syntax must match defined patterns | ERROR |
| SYN-002 | Layout containers must be properly closed | ERROR |
| SYN-003 | Annotations must follow `@key: value` format | ERROR |
| SYN-004 | Grid rows should have consistent column count | WARNING |
| SYN-005 | Box drawing characters should form closed shapes | WARNING |

### 8.2 Semantic Validation

| Rule ID | Description | Severity |
|---------|-------------|----------|
| SEM-001 | `@actor` should exist in linked MAS v2.5 diagram | WARNING |
| SEM-002 | `@screen` should match user flow screen names | WARNING |
| SEM-003 | `@action` should match defined actions | WARNING |
| SEM-004 | `@journey-section` should exist in user journey | WARNING |

### 8.3 Accessibility Validation

| Rule ID | Description | Severity |
|---------|-------------|----------|
| A11Y-001 | Interactive elements should have `@a11y` annotations | WARNING |
| A11Y-002 | Tab order should be sequential (1, 2, 3...) | WARNING |
| A11Y-003 | Form inputs should have associated labels | WARNING |
| A11Y-004 | Images should have alt text via `[img:alt]` | WARNING |

---

## 9. Rendering Strategies

### 9.1 Plain Text (Default)

Wireframes render as `<pre>` blocks with monospace font:

```html
<pre class="mas-wireframe">
@title: Login
+------------------+
| [Login] [Cancel] |
+------------------+
</pre>
```

### 9.2 HTML Form Generation (Optional)

MAS Compiler can optionally convert wireframes to interactive HTML:

```html
<div class="wireframe-login">
  <h2>Login</h2>
  <form>
    <label>Email</label>
    <input type="email" placeholder="user@example.com">
    <label>Password</label>
    <input type="password">
    <button type="button">Cancel</button>
    <button type="submit">Login</button>
  </form>
</div>
```

### 9.3 Design Token Integration (Future)

Wireframes can reference design system tokens:

```wireframe
@design-system: example-ds

[Submit]
@token-background: color.primary.500
@token-text: color.white
@token-radius: radius.md
```

---

## 10. Best Practices

### 10.1 Naming Conventions

- **Screens**: PascalCase (`LoginPage`, `UserDashboard`)
- **Actions**: kebab-case (`submit-form`, `delete-item`)
- **IDs**: kebab-case (`email-input`, `submit-btn`)

### 10.2 Layout Guidelines

1. Keep wireframes under 80 characters wide for readability
2. Use consistent grid column widths within a wireframe
3. Group related elements with group boxes
4. Use separators to create visual hierarchy

### 10.3 Accessibility Guidelines

1. Always annotate interactive elements with `@a11y`
2. Provide logical tab order with `@a11y-tab-order`
3. Label all form inputs
4. Include alt text for images

### 10.4 Entity Integration Guidelines

1. Define `@screen` for every wireframe
2. Link buttons to actions with `@action`
3. Specify primary actor with `@actor`
4. Cross-reference to user journeys with `@journey-section`

---

## 11. Conformance

An implementation conforms to MAS v2.6 Part 10 if it:

1. **MUST** parse wireframe code blocks
2. **MUST** recognize all defined widget syntax
3. **MUST** validate layout container nesting
4. **MUST** parse annotation syntax
5. **SHOULD** validate entity cross-references
6. **SHOULD** perform accessibility validation
7. **MAY** render wireframes as HTML

---

## 12. References

- [PlantUML Salt](https://plantuml.com/salt) - Widget syntax inspiration
- [Mockdown](https://github.com/sminnee/mockdown) - ASCII UI mockup language
- [WCAG 2.1](https://www.w3.org/TR/WCAG21/) - Accessibility guidelines
- [Unicode Box Drawing](https://unicode.org/charts/PDF/U2500.pdf) - Character reference
- [MAS v2.5 Index](./MAS-v2.5-INDEX.md) - Entity model reference

---

**Version:** 2.6.0 | **Status:** Draft Specification | **Last Updated:** 2025-11-29
