# What is Storylang?

Storylang is a language that can be used for storyboarding user experiences. It brings precision and clarity to the collaborative process for people involved in UX design and frontend development.

Storylang is JSON-compatible.

Long-term goal: Storylang must evolve into a written script notation framework that can help imagine, annotate and communicate.

Inspiration to create this was drawn from music notations.<br><br>

![Storylang notations](https://wildfiretech.co/theme/assets/img/bach-notes.png)<br>
<em>Above: Hand-written musical notation by J. S. Bach (1685–1750).</em>

# Storylang.json Documentation

## Overview

Storylang.json is a structured configuration file used in the ember-tribe ecosystem to define the frontend implementation of your application. It works in conjunction with your `simplified-types.json` (which defines your data models) and `navigator.json` (which defines user flows and navigation) to create a complete frontend specification.

## Purpose

The storylang.json file serves as a blueprint for frontend developers to understand:

- What routes, components and services are required
- How data flows through the application

## File Structure

The storylang.json file contains four main sections:

```json
{
  "implementation_approach": "...",
  "components": [...],
  "routes": [...],
  "services": [...]
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

### 2. Components

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

### 3. Routes

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
			"models": ["model1", "model2"]
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
- `models`: Data models loaded by this route

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
			"models": ["json_file"]
		}
	]
}
```

### 4. Services

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

## Integration with Other Files

### With simplified-types.json

- Model names used in routes should match type names from simplified-types.json
- Component inherited_args often reference model data pulled from store, and/or their fields
- Models are the gateway to persistent storage on the backend.

## Best Practices

**Always begin your thought process with routes → then move repeatable template route into components → then move your repeatable app-wide logic from components to services**

1. **Start with Routes**: Match route names to user mental models and use consistent naming conventions
2. **Minimize Route Logic**: Preferably fetch (read) model data in routes and then pass that data to components or services. Other than fetching model data, minimize the use of javascript in routes - Javascript is meant to be in components and services more than in routes
3. **Route Parameters**: Keep get_vars minimal and meaningful, and load only necessary models for each route
4. **Component Focus**: Keep components focused on single responsibilities and use descriptive, kebab-case names
5. **Data Flow**: Receive backend data down from routes (via inherited_args) rather than fetching (reading) in components
6. **Component Actions**: Non-read functions - create, update, delete - can all happen well at component-level
7. **Service Integration**: Use services directly in components for app-wide logic
8. **Service Architecture**: Keep services stateless when possible and use dependency injection for service composition
9. **Service Role**: Services interact with both routes and components and store the core logic of the application

## Tips for Manual Creation

1. **Start with implementation_approach**: Write a clear use case first
2. **Identify key user journeys**: Map these to routes
3. **Break down UI into components**: Focus on reusability
4. **Define data flow**: Ensure models align with your simplified-types.json
5. **Plan service architecture**: Keep core application logic separate from UI logic
6. **Validate interconnections**: Ensure all references match between files
7. **Test component composition**: Verify components work together logically
