# ⚠️ Deprecated — Merged into ember-tribe

This repository has been merged into [ember-tribe](https://github.com/tribe-framework/ember-tribe) and is no longer maintained here.

Please use ember-tribe going forward:
👉 https://github.com/tribe-framework/ember-tribe

# What is Storylang?

Storylang is a language that can be used for storyboarding user experiences. It brings precision and clarity to the collaborative process for people involved in UX design and frontend development.

Storylang is JSON-compatible.

Long-term goal: Storylang must evolve into a written script notation framework that can help imagine, annotate and communicate.

Inspiration to create this was drawn from music notations.<br><br>

![Storylang notations](https://wildfiretech.co/theme/assets/img/bach-notes.png)<br>
<em>Above: Hand-written musical notation by J. S. Bach (1685–1750).</em>

# Storylang.json Documentation

## Overview

Storylang.json is a structured configuration file used in the ember-tribe ecosystem to define the frontend implementation of your application. It works in conjunction with your `simplified-types.json` (which defines your data types) to create a complete frontend specification.

## Purpose

The storylang.json file serves as a blueprint for frontend developers to understand:

- What routes, components, services, helpers, modifiers and types are required
- How data flows through the application

## File Structure

The storylang.json file contains seven main sections:

```json
{
  "implementation_approach": "...",
  "types": [...],
  "components": [...],
  "routes": [...],
  "services": [...],
  "helpers": [...],
  "modifiers": [...]
}
```

## Section Definitions

### 1. Implementation Approach

**Purpose**: Provides a high-level technical overview of how the frontend interface would work.

**Format**:

```json
{
  "implementation_approach": "Two-paragraph description explaining technical approach and key functionality."
}
```

---

### 2. Types

**Purpose**: Declares which data types from `simplified-types.json` are used in this application, and maps them to the components, routes, services, helpers and modifiers that consume them. This creates a traceable link between your data layer and your UI implementation.

**Format**:

```json
{
  "types": [
    {
      "name": "type-name",
      "used_in": {
        "components": ["component-name"],
        "routes": ["route-name"],
        "services": ["service-name"]
      }
    }
  ]
}
```

**Type Properties**:

- `name`: The type name as defined in `simplified-types.json`
- `used_in`: An object mapping the type to the parts of the app that reference it
  - `components`: Components that receive or manipulate this type
  - `routes`: Routes that load or use this type
  - `services`: Services that operate on this type

**Example**:

```json
{
  "types": [
    {
      "name": "json_file",
      "used_in": {
        "components": ["file-summary-card", "file-list"],
        "routes": ["files", "files.show"],
        "services": ["file-manager"]
      }
    },
    {
      "name": "user",
      "used_in": {
        "components": ["profile-card"],
        "routes": ["profile"],
        "services": ["session"]
      }
    }
  ]
}
```

---

### 3. Components

**Purpose**: Defines reusable UI components that will be built for the application.

**Format**:

```json
{
  "components": [
    {
      "name": "component-name",
      "type": "component-type",
      "tracked_vars": [{ "variable_name": "data_type" }],
      "inherited_args": [{ "arg_name": "type" }],
      "actions": ["action1", "action2"],
      "helpers": ["helper1", "helper2"],
      "modifiers": ["modifier1"],
      "services": ["service1", "service2"]
    }
  ]
}
```

**Component Properties**:

- `name`: Kebab-case name of the component
- `type`: Type from the built-in components list
- `tracked_vars`: State variables tracked within the component
- `inherited_args`: Arguments passed from parent components
- `actions`: User interactions the component handles
- `helpers`: Template helpers used within the component
- `modifiers`: DOM modifiers applied to elements
- `services`: Ember services injected into the component

**Built-in Component Types**:

- **Layout**: `table`, `figure`, `accordion`, `card`, `list-group`, `navbar`, `nav`, `tab`, `breadcrumb`
- **Interactive**: `button`, `button-group`, `dropdown`, `modal`, `collapse`, `offcanvas`, `pagination`, `popover`, `tooltip`, `swiper-carousel`, `videojs-player`, `howlerjs-player`, `input-field`, `input-group`, `textarea`, `checkbox`, `radio`, `range`, `select`, `multi-select`, `date`, `file-uploader`, `alert`, `badge`, `toast`, `placeholder`, `progress`, `spinner`, `scrollspy`

**Example**:

```json
{
  "components": [
    {
      "name": "file-summary-card",
      "type": "card",
      "tracked_vars": [{ "isSelected": "bool" }, { "isExpanded": "bool" }],
      "inherited_args": [
        { "file": "var" },
        { "onEdit": "fn" },
        { "onDelete": "fn" }
      ],
      "actions": ["toggleSelection", "expandDetails", "editFile", "deleteFile"],
      "helpers": ["formatDate", "truncateText"],
      "modifiers": ["tooltip"],
      "services": ["store", "router"]
    }
  ]
}
```

---

### 4. Routes

**Purpose**: Defines the application's routes and their requirements.

**Format**:

```json
{
  "routes": [
    {
      "name": "route-name",
      "tracked_vars": [{ "variable_name": "data_type" }],
      "get_vars": [{ "param_name": "data_type" }],
      "actions": ["action1", "action2"],
      "helpers": ["helper1"],
      "services": ["service1"],
      "components": ["component1", "component2"],
      "types": ["type1", "type2"]
    }
  ]
}
```

**Route Properties**:

- `name`: The route name (matches Ember router)
- `tracked_vars`: Route-level state variables
- `get_vars`: URL query parameters and their types
- `actions`: Route-level actions
- `helpers`: Template helpers used in route templates
- `services`: Services used by the route
- `components`: Components rendered in this route
- `types`: Data types loaded by this route

**Example**:

```json
{
  "routes": [
    {
      "name": "files",
      "tracked_vars": [
        { "selectedFileType": "string" },
        { "sortOrder": "string" }
      ],
      "get_vars": [
        { "page": "int" },
        { "search": "string" },
        { "filter": "string" }
      ],
      "actions": ["filterFiles", "sortFiles", "searchFiles"],
      "helpers": ["pluralize", "formatDate"],
      "services": ["store", "router"],
      "components": ["file-list", "file-filter", "search-box", "pagination"],
      "types": ["json_file"]
    }
  ]
}
```

---

### 5. Services

**Purpose**: Defines custom Ember services needed by the application.

**Format**:

```json
{
  "services": [
    {
      "name": "service-name",
      "tracked_vars": [{ "variable_name": "data_type" }],
      "actions": ["action1", "action2"],
      "helpers": ["helper1"],
      "services": ["dependency1", "dependency2"]
    }
  ]
}
```

**Service Properties**:

- `name`: Service name
- `tracked_vars`: Service-level tracked properties
- `actions`: Methods provided by the service
- `helpers`: Utility functions
- `services`: Other services this service depends on

**Built-in Services**:

- `store`: Ember Data store for CRUD operations
- `router`: Ember router service for navigation

**Third-party npm packages pre-installed**:

- `bootstrap`: Bootstrap CSS framework with Popper.js
- `papaparse`: CSV parsing library
- `sortablejs`: Drag-and-drop sorting
- `swiperjs`: Touch slider/carousel
- `videojs`: Video player
- `howlerjs`: Audio player

**Example**:

```json
{
  "services": [
    {
      "name": "visualization-builder",
      "tracked_vars": [
        { "currentVisualization": "object" },
        { "availableTypes": "array" }
      ],
      "actions": [
        "createVisualization",
        "updateVisualization",
        "deleteVisualization"
      ],
      "helpers": ["validateConfig", "generatePreview"],
      "services": ["store", "router"]
    }
  ]
}
```

---

### 6. Helpers

**Purpose**: Defines custom template helpers — pure functions used in templates to format, compute or transform data for display.

**Format**:

```json
{
  "helpers": [
    {
      "name": "helper-name",
      "description": "What this helper does",
      "args": [{ "arg_name": "data_type" }],
      "returns": "data_type"
    }
  ]
}
```

**Helper Properties**:

- `name`: Kebab-case name of the helper
- `description`: Brief description of what the helper computes or transforms
- `args`: Input arguments the helper accepts
- `returns`: The data type the helper outputs

**Example**:

```json
{
  "helpers": [
    {
      "name": "format-date",
      "description": "Formats a raw ISO date string into a human-readable date",
      "args": [{ "isoString": "string" }, { "format": "string" }],
      "returns": "string"
    },
    {
      "name": "truncate-text",
      "description": "Truncates a string to a given character limit and appends an ellipsis",
      "args": [{ "text": "string" }, { "limit": "int" }],
      "returns": "string"
    },
    {
      "name": "pluralize",
      "description": "Returns singular or plural form of a word based on a count",
      "args": [{ "count": "int" }, { "word": "string" }],
      "returns": "string"
    }
  ]
}
```

---

### 7. Modifiers

**Purpose**: Defines custom Ember modifiers — functions that directly interact with DOM elements to attach behaviour, third-party libraries or event listeners.

**Format**:

```json
{
  "modifiers": [
    {
      "name": "modifier-name",
      "description": "What DOM behaviour this modifier applies",
      "args": [{ "arg_name": "data_type" }],
      "services": ["service1"]
    }
  ]
}
```

**Modifier Properties**:

- `name`: Kebab-case name of the modifier
- `description`: Brief description of the DOM behaviour it attaches
- `args`: Arguments passed into the modifier from the template
- `services`: Ember services injected if needed

**Example**:

```json
{
  "modifiers": [
    {
      "name": "tooltip",
      "description": "Initialises a Bootstrap tooltip on the target element using the provided label",
      "args": [{ "label": "string" }, { "placement": "string" }],
      "services": []
    },
    {
      "name": "sortable-list",
      "description": "Attaches SortableJS drag-and-drop behaviour to a list element",
      "args": [{ "items": "array" }, { "onReorder": "fn" }],
      "services": []
    },
    {
      "name": "autofocus",
      "description": "Sets focus on the target element when it is inserted into the DOM",
      "args": [],
      "services": []
    }
  ]
}
```

---

## Data Types Reference

### Common Data Types

- `string`: Text values
- `int`: Integer numbers
- `bool`: Boolean true/false
- `array`: List of items
- `object`: Complex data structure
- `var`: Variable (any type)
- `fn`: Function reference

### Argument Types

- `var`: Passed data/state
- `fn`: Callback function
- `action`: User interaction handler

---

## Integration with Other Files

### With simplified-types.json

- Type names used in routes should match type names from `simplified-types.json`
- The `types` section in storylang.json is the explicit bridge between your data types and your UI — always keep it in sync with `simplified-types.json`
- Component `inherited_args` often reference type data pulled from store, and/or their fields
- Types are the gateway to persistent storage on the backend

---

## Best Practices

**Always begin your thought process with routes → then move repeatable template logic into components → then move repeatable app-wide logic from components and routes to services → then extract reusable template and component functions into helpers → then extract template and component DOM behaviour into modifiers**

1. **Start with Routes**: Match route names to user mental models and use consistent naming conventions
2. **Minimize Route Logic**: Preferably fetch (read) type data in routes and then pass that data to components or services. Other than fetching type data, minimize the use of javascript in routes — Javascript is meant to be in components and services more than in routes
3. **Route Parameters**: Keep get_vars minimal and meaningful, and load only necessary types for each route
4. **Component Focus**: Keep components focused on single responsibilities and use descriptive, kebab-case names
5. **Data Flow**: Receive backend data down from routes (via inherited_args) rather than fetching (reading) in components
6. **Component Actions**: Non-read functions — create, update, delete — can all happen well at component-level
7. **Service Integration**: Use services directly in both components and routes for app-wide logic
8. **Service Architecture**: Keep services stateless when possible and use dependency injection for service composition
9. **Service Role**: Services interact with both routes and components and store the core logic of the application
10. **Helpers**: Keep helpers pure and stateless — they should only receive input and return output with no side effects
11. **Modifiers**: Use modifiers to isolate all direct DOM manipulation and third-party library initialisation from component logic
12. **Types Mapping**: Populate the `types` section as you define your routes and components — it is your traceability layer between data types and UI

## Tips for Manual Creation

1. **Start with implementation_approach**: Write a clear use case first
2. **Declare types early**: List the types from `simplified-types.json` you'll need before designing routes and components
3. **Identify key user journeys**: Map these to routes
4. **Break down UI into components**: Focus on reusability
5. **Define data flow**: Ensure types align with your `simplified-types.json`
6. **Plan service architecture**: Keep core application logic separate from UI logic
7. **Extract helpers**: Any formatting or computation repeated across templates should become a named helper
8. **Extract modifiers**: Any direct DOM interaction or third-party library setup should become a named modifier