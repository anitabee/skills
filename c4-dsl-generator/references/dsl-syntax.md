# Structurizr DSL Syntax Reference

## Table of Contents

- [Workspace](#workspace)
- [Model Elements](#model-elements)
- [Relationships](#relationships)
- [Views](#views)
- [Styles](#styles)
- [Themes](#themes)
- [Element Properties](#element-properties)
- [Perspectives](#perspectives)
- [Implied Relationships](#implied-relationships)
- [Groups](#groups)
- [Identifiers](#identifiers)
- [Code Diagrams (L4)](#code-diagrams-l4)

## Workspace

Top-level container for the entire architecture model.

```
workspace [name] [description] {
    !identifiers hierarchical  // optional: use hierarchical identifiers
    !impliedRelationships <strategy>  // optional

    model {
        // elements and relationships
    }
    views {
        // diagram definitions and styles
    }
}
```

### Workspace Properties

```
workspace {
    name "My Workspace"
    description "A description"
    properties {
        "key" "value"
    }
}
```

## Model Elements

The C4 model has four levels of abstraction. The first three (System Context, Container, Component) map directly to DSL keywords.

### Person

```
<identifier> = person <name> [description] [tags]
```

Examples:
```
user = person "User" "A generic user"
admin = person "Admin" "System administrator" "Staff"
```

### Software System (L1 — System Context)

```
<identifier> = softwareSystem <name> [description] [tags] {
    // optional: nested containers
}
```

Examples:
```
mySystem = softwareSystem "My System" "Does things"
external = softwareSystem "External System" "Third-party" "Existing System"
```

### Container (L2)

Must be nested inside a `softwareSystem` block.

```
<identifier> = container <name> [description] [technology] [tags] {
    // optional: nested components
}
```

Examples:
```
webapp = container "Web App" "User-facing application" "React"
api = container "API" "REST API" "Node.js/Express" "Backend"
db = container "Database" "Stores data" "PostgreSQL" "Database"
```

### Component (L3)

Must be nested inside a `container` block.

```
<identifier> = component <name> [description] [technology] [tags]
```

Examples:
```
authController = component "Auth Controller" "Handles authentication" "Spring MVC Controller"
userRepo = component "User Repository" "Data access for users" "Spring Data JPA"
```

### Ordering of Parameters

All elements follow this parameter order:
1. Name (required, quoted string)
2. Description (optional, quoted string)
3. Technology (optional, containers and components only, quoted string)
4. Tags (optional, quoted string, comma-separated)

When providing tags without technology (for person/softwareSystem), just supply the tags string directly:
```
user = person "User" "Description" "Tag1,Tag2"
```

## Relationships

### Basic Syntax

```
<source> -> <destination> [description] [technology] [tags]
```

All parameters after `->` are optional quoted strings.

### Examples

```
user -> webApp "Uses" "HTTPS"
webApp -> api "Makes API calls to" "JSON/HTTPS"
api -> db "Reads from and writes to" "SQL/TCP"
api -> emailSystem "Sends notifications via" "SMTP" "Async"
```

### Relationship Within Block

Relationships can be defined inside element blocks using `this`:

```
api = container "API" {
    this -> db "Reads from" "SQL"
}
```

## Views

### System Landscape

Shows all people and software systems (not a C4 level, but useful for enterprise context):

```
systemLandscape [key] [description] {
    include *
    autoLayout
}
```

### System Context (L1)

Scope: a single software system.

```
systemContext <softwareSystem> [key] [description] {
    include *
    autoLayout
}
```

### Container (L2)

Scope: a single software system, showing its containers.

```
container <softwareSystem> [key] [description] {
    include *
    autoLayout
}
```

### Component (L3)

Scope: a single container, showing its components.

```
component <container> [key] [description] {
    include *
    autoLayout
}
```

### Filtered

Creates a filtered copy of another view based on tags:

```
filtered <baseViewKey> include "Tag1" [key] [description]
filtered <baseViewKey> exclude "Tag2" [key] [description]
```

### View Keywords

#### include / exclude

Control which elements appear in a view:

```
include *                          // all elements in scope
include <identifier>               // specific element
include <identifier> -> <identifier>  // specific relationship
include ->                         // all relationships
exclude <identifier>               // remove specific element
exclude "Tag"                      // exclude by tag
exclude <id> -> <id>               // exclude specific relationship
```

#### autoLayout

```
autoLayout [direction] [rankSeparation] [nodeSeparation]
```

Directions: `tb` (top-bottom, default), `bt` (bottom-top), `lr` (left-right), `rl` (right-left).

```
autoLayout          // default: top-bottom
autoLayout lr       // left-to-right
autoLayout tb 300 200  // top-bottom with custom spacing
```

#### Other View Properties

```
title "Custom View Title"
description "View description"
```

## Styles

### Element Styles

```
styles {
    element "Tag" {
        shape <shape>
        icon <url>
        width <pixels>
        height <pixels>
        background <color>     // hex color, e.g. #438DD5
        color <color>          // text color
        colour <color>         // alias for color
        stroke <color>         // border color
        strokeWidth <pixels>
        fontSize <pixels>
        border <style>         // solid, dashed, dotted
        opacity <0-100>
        metadata <true|false>  // show metadata
        description <true|false>  // show description
    }
}
```

### Available Shapes

`Box` (default), `RoundedBox`, `Circle`, `Ellipse`, `Hexagon`, `Cylinder`, `Pipe`, `Person`, `Robot`, `Folder`, `WebBrowser`, `MobileDevicePortrait`, `MobileDeviceLandscape`, `Component`

### Relationship Styles

```
styles {
    relationship "Tag" {
        thickness <pixels>
        color <color>
        colour <color>
        style <style>         // solid, dashed, dotted
        routing <routing>     // Direct, Orthogonal, Curved
        fontSize <pixels>
        width <pixels>
        position <0-100>      // label position along line
        opacity <0-100>
    }
}
```

### Style Examples

```
styles {
    element "Person" {
        shape Person
        background #08427B
        color #ffffff
    }
    element "Database" {
        shape Cylinder
        background #438DD5
        color #ffffff
    }
    element "Browser" {
        shape WebBrowser
    }
    relationship "Async" {
        style dashed
        color #999999
    }
    relationship "Relationship" {
        thickness 2
        color #707070
    }
}
```

## Themes

Apply predefined visual themes:

```
views {
    theme default
    theme https://static.structurizr.com/themes/amazon-web-services-2023.01.31/theme.json
}
```

Common themes:
- `default` — Structurizr default styling
- Amazon Web Services: `https://static.structurizr.com/themes/amazon-web-services-2023.01.31/theme.json`
- Google Cloud Platform: `https://static.structurizr.com/themes/google-cloud-platform-v1.5/theme.json`
- Microsoft Azure: `https://static.structurizr.com/themes/microsoft-azure-2023.01.24/theme.json`
- Kubernetes: `https://static.structurizr.com/themes/kubernetes-v0.3/theme.json`

Multiple themes can be combined:

```
views {
    theme default
    theme https://static.structurizr.com/themes/amazon-web-services-2023.01.31/theme.json
}
```

## Element Properties

Attach key-value metadata to any element:

```
mySystem = softwareSystem "My System" {
    properties {
        "Owner" "Team Alpha"
        "Risk" "High"
    }
}
```

## Perspectives

Add architectural perspectives to elements:

```
mySystem = softwareSystem "My System" {
    perspectives {
        "Security" "All data encrypted at rest and in transit"
        "Performance" "99.9% uptime SLA, <200ms response time"
    }
}
```

## Implied Relationships

Control how relationships propagate through the hierarchy:

```
workspace {
    !impliedRelationships true   // default: create implied relationships
    !impliedRelationships false  // disable implied relationships
}
```

When enabled, a relationship between two components implies a relationship between their parent containers and software systems.

## Groups

Visually group elements:

```
model {
    group "Internal Systems" {
        systemA = softwareSystem "System A"
        systemB = softwareSystem "System B"
    }
    group "External Systems" {
        systemC = softwareSystem "System C" "Existing System"
    }
}
```

Groups can be nested inside software systems and containers:

```
mySystem = softwareSystem "My System" {
    group "Frontend" {
        webapp = container "Web App" "" "React"
        mobile = container "Mobile App" "" "React Native"
    }
    group "Backend" {
        api = container "API" "" "Java/Spring"
        worker = container "Worker" "" "Java"
    }
}
```

## Identifiers

### Flat (default)

All identifiers are globally unique:
```
api = container "API"
```

### Hierarchical

Identifiers are scoped to their parent:
```
workspace {
    !identifiers hierarchical
    model {
        system = softwareSystem "System" {
            api = container "API"     // referenced as system.api
        }
    }
}
```

### Rules

- Identifiers must be alphanumeric, starting with a letter
- Identifiers are case-insensitive
- Convention: use camelCase (e.g., `webApp`, `apiGateway`, `userService`)

## Code Diagrams (L4)

The C4 model's Level 4 (Code) represents the lowest level of detail — individual classes, interfaces, and their relationships. Structurizr DSL does not have a dedicated `code` view type.

### Approaches for L4 Code Diagrams

**Option 1 — Auto-generate from source code (recommended)**:
Use IDE tools or code analysis tools to generate UML class diagrams:
- IntelliJ IDEA / Eclipse UML plugins
- PlantUML with code parsing
- Dependency analysis tools (jdeps, deptrac, etc.)

**Option 2 — Manual DSL using fine-grained components**:
Model classes and interfaces as components with detailed technology annotations:

```
container "Order Service" "Order processing" "Java/Spring Boot" {
    component "Order" "Order entity" "Java Class,Entity"
    component "OrderRepository" "Data access interface" "Java Interface,Repository"
    component "OrderRepositoryImpl" "JPA implementation" "Java Class,Repository"
    component "OrderService" "Order business logic" "Java Class,Service"
    component "OrderController" "REST endpoint" "Java Class,Controller"
}
```

Use tags like "Java Class", "Interface", "Abstract Class", "Entity" to distinguish element types. Style them accordingly:

```
styles {
    element "Interface" {
        shape RoundedBox
        border dashed
    }
    element "Abstract Class" {
        border dashed
    }
    element "Entity" {
        shape Hexagon
    }
}
```

**When to use L4**: Only for architecturally significant components where class-level detail helps understanding. Most systems do not need L4 diagrams — the source code itself serves as the definitive L4 view.
