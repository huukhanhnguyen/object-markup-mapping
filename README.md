# Object Markup Mapping Syntax (OMM)

**Object Markup Mapping (OMM)** is a convention concept for mapping native programming objects to HTML structure and behavior using a clean, intuitive syntax. It allows developers to define HTML elements, attributes, events, and styles as plain objects — usable across multiple languages.

## Why OMM?

Traditional HTML templating or JSX-based systems mix logic with markup, require special compilers, and are often limited to one language (like JavaScript). OMM provides:

- A language-agnostic, object-based markup definition
- Easy programmatic composition and reuse
- Consistent structure usable in backend or frontend
- Remmembarable without any custom syntax

## OMM Syntax

| Key          | Description                            | Example                        |
|--------------|----------------------------------------|--------------------------------|
| `"tag"`      | HTML tag name (first key of object)    | `"div": "Hello"`               |
| `attribute`  | HTML attributes                        | `"class": "my-box"`            |
| `"style"`    | CSS styling (supports nesting)         | `"style": { "color": "red" }`  |
| `onClick`    | DOM event handler (in JS only)         | `"onClick": fn()`              |

The first string key in the object determines the **HTML tag**. Other keys define **attributes**, **event handlers**, or **special fields** such as `style`.

## Children Rules

The value assigned to the tag key (e.g., `"div": ...`) is the **children**. It can be:

- An **array** — multiple children (standard)
- A **text string** — e.g. `"div": "Hello"` (shorthand)
- A **function** — for reactive rendering (in JavaScript)
- `null` — for void elements (`img`, `input`, etc.)

## Supported Languages
- JavaScript  
- Python  
- Ruby  
- PHP  
- (And others that support dictionary-style syntax)

## Python Example

```python
eelement = {
    "button": "Click Me",
    "class": "primary-button",
    "onClick": "alert('Clicked!')",  # Placeholder: JS event as string
    "style": {
        "background-color": "DodgerBlue",
        "&:hover": {
            "background-color": "RoyalBlue"
        }
    }
}
```
### Nested Example
```
layout = {
    "div": [
        {
            "h1": "Welcome to OMM"
        },
        {
            "p": "This is a paragraph inside a nested object.",
            "class": "text-muted"
        },
        {
            "ul": [  # Nested list
                {"li": "First item"},
                {"li": "Second item"},
                {"li": "Third item"}
            ],
            "class": "item-list"
        }
    ],
    "class": "container",
    "style": {
        "padding": "20px",
        "background-color": "#f0f0f0"
    }
}
```
## PHP Example 
```
$element = [
    "button" => "Click Me",
    "class" => "primary-button",
    "onClick" => "alert('Clicked!')", // Gắn JS trực tiếp nếu render client-side
    "style" => [
        "background-color" => "DodgerBlue",
        "&:hover" => [
            "background-color" => "RoyalBlue"
        ]
    ]
];
```
## Ruby Example 
```
element = {
  "button" => "Click Me",
  "class" => "primary-button",
  "onClick" => "alert('Clicked!')", # JS dưới dạng chuỗi
  "style" => {
    "background-color" => "DodgerBlue",
    "&:hover" => {
      "background-color" => "RoyalBlue"
    }
  }
}
```
## JavaScript Example

```javascript
let element = {
    button: "Click Me",
    class:"primary-button",
    onClick: (event) => alert("Clicked!"),
    style: {
        backgroundColor: "DodgerBlue",
        "&:hover": {
            backgroundColor: "RoyalBlue"
        }
    }
};
```
## Benefits

- Avoids mixing logic with HTML templates
- Works across languages (JS, Python, PHP, etc.)
- Promotes object-based thinking and reuse
- Compatible with server rendering and client interactivity
- Ideal for low-overhead custom renderers

## Syntax Principle Recommendations

- Use standard HTML and CSS naming (e.g., background-color, not bgColor)
- Avoid reserved language symbols ($, this, etc.)
- Prefix custom metadata with _ (e.g., _key, _meta)
- Prefer explicit object trees over sugar like turn array to `ul` or nested array to `table`
- Extract subtrees into variables for better readability

## Generation Process Recommendations

- Generate a **unique class name** for each element based on its style
- **Parse and flatten** CSS Nesting objects into standard CSS rules
- **Collect all CSS rules** into a centralized stylesheet
- **Separate HTML and CSS generation** generation for better maintainability
- ➜ Results in **cleaner HTML output** and **smaller overall string size**

## Comparison with Virtual DOM (VDOM)

VDOM structures are more verbose and less intuitive for daily use due to double depth each level:

```python
let element = {
  tagName: "button",
  attributes: {
    class: "primary-button",
    onClick: "alert('Clicked!')"
  },
  style: {
    "background-color": "DodgerBlue",
    "&:hover": {
      "background-color": "RoyalBlue"
    }
  },
  children: ["Click Me"]
}
```
## Comparison with JSX

JSX is the dominant syntax for building UI in JavaScript, especially with React. OMM offers a language-agnostic, object-based alternative suitable for both frontend and backend environments.

| Aspect           | JSX                          | OMM                                |
|------------------|-------------------------------|-------------------------------------|
| **Syntax Style** | XML-like                      | Plain object                        |
| **Language Support** | JavaScript / TypeScript only | Any (JS, Python, Ruby, etc.)        |
| **Requires Compiler** | Yes (e.g. Babel)           | No                                  |
| **Logic Separation** | Mixed in XML-like markup   | Cleanly embedded in native logic    |
| **Componentization** | Built-in and idiomatic     | Possible via object composition     |
| **Reactivity**   | Built-in (React, Solid, etc.) | Requires custom renderer (JS only)  |
| **Portability**  | Tied to JS ecosystem          | Can be used across many languages   |
| **Rendering Target** | DOM-focused                | DOM, HTML, or custom output         |

> OMM is not a replacement for JSX, but a structured, minimal, and flexible alternative — especially powerful in multi-language or server-rendering contexts.

## JavaScript Extensions (Optional)

When used in JavaScript environments, OMM can be extended with interactivity and dynamic behavior:

- **Standard Event Function**: Add properties like `onClick`, `onInput`, etc.
- **Lifecycle Hooks**: Optional `onAttach` hook provides access to the rendered DOM element.
- **Reactive Values**: Attributes and styles can be functions that return values dynamically
- **SSR/CSR Seamless**: The same tree structure can be used both server-side and client-side, enabling seamless event attachment.

These extensions require a custom renderer and do not apply to other languages like Python or PHP.
## Applicability

OMM can serve as an **abstract standard** for building a wide range of rendering and UI systems:
- **Server-side rendering (SSR)** — generate clean HTML from native objects using backend languages like Python, PHP, or Ruby
- **Email HTML generators** — ideal for complex templating logic with minimal markup
- **Low-code builders** — store only object-based JSON instead of raw HTML
- **PDF renderers**, **UI editors**, and **markdown + widget hybrid renderers**
- **Static site generators** for backend-driven frameworks
- **Design tokens → UI pipelines** — convert design configurations into interface output


License: MIT Copyright: 2025 Author: [Khánh Nguyễn](https://github.com/huukhanhnguyen)
