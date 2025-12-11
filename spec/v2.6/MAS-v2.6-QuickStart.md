# MAS v2.6 Quick Start Guide

**Get started with ASCII Wireframes in 5 minutes!**

---

## 1. Basic Wireframe Structure

Every wireframe starts with a fenced code block:

````markdown
```wireframe
@title: My Screen
@screen: ScreenName
@actor: User

+---------------------------+
|  Your content here        |
+---------------------------+
```
````

---

## 2. Widget Cheat Sheet

### Buttons
```wireframe
[Primary Button]     # Main action (filled)
[[Secondary Button]] # Secondary action (outlined)
[<icon>]             # Icon button
[-Disabled-]         # Disabled state
```

### Inputs
```wireframe
"Placeholder text    "  # Text input with placeholder
[__________________]    # Input with specific width
[****************]      # Password input
{+ Multi-line
   text area +}         # Textarea
```

### Selection Controls
```wireframe
[ ] Unchecked checkbox    [x] Checked checkbox
( ) Unselected radio      (o) Selected radio
^Dropdown^                # Closed dropdown
^Select^^ Option 1 ^^ Option 2 ^  # Open dropdown
```

### Other Widgets
```wireframe
[Link text](#url)         # Clickable link
[img:Alt text]            # Image placeholder
[===o====]                # Slider
[ON] [OFF]                # Toggle
[####----] 40%            # Progress bar
```

---

## 3. Layout Containers

### Grid (Columns)
```wireframe
{ Label:    | "Input value     " }
{ Password: | "****************" }
{ [Cancel]  | [Submit]          }
```

### Group Box
```wireframe
{^"Login Form"
  Email:    "user@example.com"
  Password: "****************"
  [Login]
^}
```

### Tabs & Menu
```wireframe
{/ Tab 1 | Tab 2 | Tab 3 }      # Tabs
{* File | Edit | View | Help }  # Menu bar
```

### Tree
```wireframe
{T
  + Folder
  ++ Subfolder
  +++ File.txt
}
```

---

## 4. Annotations

### Metadata
```wireframe
@title: Page Title
@screen: PageName
@actor: User Type
@version: 1.0
```

### Element Annotations
```wireframe
@action: submit-form
[Submit]

@a11y: aria-label="Search"
[<search>]

@state: disabled
[-Cannot Click-]
```

### Cross-References
```wireframe
@journey-section: Onboarding    # Links to user journey
@flow-step: step-3              # Links to user flow
@process-task: approve-order    # Links to business process
```

---

## 5. Separators

```wireframe
..        # Dotted line
--        # Dashed line
==        # Double line
~~        # Wavy line
___       # Solid underline
```

---

## 6. Box Drawing (Optional)

For prettier wireframes, use Unicode box characters:

```wireframe
┌────────────────────────────┐
│  Login                     │
├────────────────────────────┤
│  Email:    [____________]  │
│  Password: [************]  │
│                            │
│  [Cancel]  [Login]         │
└────────────────────────────┘
```

**Characters:**
- Corners: `┌ ┐ └ ┘`
- Lines: `─ │`
- Junctions: `├ ┤ ┬ ┴ ┼`

---

## 7. Complete Example

```wireframe
@title: User Registration
@screen: RegistrationPage
@actor: New User
@journey-section: Onboarding

┌════════════════════════════════════════════┐
│  🚀 Create Your Account                    │
├════════════════════════════════════════════┤
│                                            │
│  {^"Personal Information"                  │
│    First Name: | "John           "         │
│    Last Name:  | "Doe            "         │
│    Email:      | "john@example.com"        │
│  ^}                                        │
│                                            │
│  {^"Security"                              │
│    Password:        | "************"       │
│    Confirm Password:| "************"       │
│  ^}                                        │
│                                            │
│  [x] I agree to Terms of Service           │
│  [ ] Subscribe to newsletter               │
│                                            │
│  ..                                        │
│                                            │
│  @action: register-user                    │
│  [[Cancel]]         [Create Account]       │
│                                            │
│  Already have an account? [Login](#login)  │
│                                            │
└════════════════════════════════════════════┘
```

---

## 8. Region Types

```wireframe
@type: header    # Navigation, branding
@type: sidebar   # Secondary navigation, filters
@type: content   # Main content area
@type: form      # User input forms
@type: modal     # Overlay dialogs
@type: footer    # Links, copyright
@type: toast     # Notifications
@type: card      # Content containers
```

---

## 9. Tips

1. **Keep width under 80 characters** for readability
2. **Use consistent column widths** in grids
3. **Always annotate actions** with `@action:`
4. **Add accessibility annotations** for interactive elements
5. **Link to MAS v2.5 diagrams** with cross-reference annotations

---

## 10. Integration with MAS v2.5

Your wireframes should reference entities from other MAS diagrams:

| MAS v2.5 Entity | Wireframe Annotation |
|-----------------|----------------------|
| `UXIntentResult.screens` | `@screen: ScreenName` |
| `UXIntentResult.actors` | `@actor: ActorName` |
| `UXIntentResult.actions` | `@action: action-name` |
| `UXIntentResult.sections` | `@journey-section: Section` |
| `BusinessIntentResult.roles` | `@actor: RoleName` |

---

## Next Steps

- Read the [full specification](./MAS-ASCIIWireframe-v2.6.md)
- See the [MAS v2.6 Index](./MAS-v2.6-INDEX.md)
- Review [Part 8: UX Diagrams](./MAS-UXDiagrams-v2.5.md) for entity models

---

**Happy Wireframing! 🎨**
