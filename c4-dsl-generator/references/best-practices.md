# C4 Modeling Best Practices

## Table of Contents

- [Naming Conventions](#naming-conventions)
- [The Four Abstraction Levels](#the-four-abstraction-levels)
- [Relationships](#relationships)
- [Tags and Styles](#tags-and-styles)
- [Layout Tips](#layout-tips)
- [Common Mistakes](#common-mistakes)
- [When to Use Each Level](#when-to-use-each-level)

## Naming Conventions

### Identifiers

- Use camelCase: `orderService`, `apiGateway`, `userDb`
- Keep identifiers short but descriptive
- Avoid abbreviations that aren't universally understood
- Use consistent suffixes: `*Service`, `*Db`, `*Api`, `*Queue`, `*Cache`

### Element Names (Display Names)

- Use title case for systems and containers: "Order Service", "API Gateway"
- Be specific: "Order Database" not just "Database"
- Include the technology in the container name only when it helps distinguish: "Redis Cache" vs "PostgreSQL Database"
- People names should describe roles: "Customer", "System Administrator", "API Consumer"

### Descriptions

- Keep descriptions to one sentence
- Describe what it does, not how: "Manages user authentication and authorization" not "Java service that uses Spring Security"
- Technology goes in the technology field, not the description

## The Four Abstraction Levels

### Level 1 — System Context

- Show the system in scope at the center
- Include all direct users (people) and external systems
- Don't show internal details — the system is a single box
- External systems should have the "Existing System" tag
- This is the starting point for every C4 model

### Level 2 — Container

- Show all deployable units: web apps, APIs, databases, message queues, file systems
- A container is something that needs to be running for the system to work
- Don't go too granular — a microservice is one container, not separate containers for each class
- Include technology choices: "React", "Java/Spring Boot", "PostgreSQL"

### Level 3 — Component

- Only create component diagrams for containers that warrant it (complex business logic)
- Components map to major structural building blocks: controllers, services, repositories
- Don't model every class — focus on architecturally significant components
- Skip component diagrams for simple CRUD containers or databases

### Level 4 — Code

- The most detailed level — shows individual classes, interfaces, enums, and their relationships
- Typically auto-generated from source code using IDE tools or UML generators
- Only create manually for architecturally significant components where class structure matters
- In Structurizr DSL, model classes as fine-grained components with technology annotations like "Java Class", "Interface", "Abstract Class"
- Keep these diagrams small and focused — a single component, not an entire container

## Relationships

### Labeling

- Use verb phrases describing the interaction: "Reads from", "Sends notifications to", "Authenticates via"
- Describe the direction of the relationship: "Makes API calls to" not "API calls"
- Include technology/protocol when it adds value: "JSON/HTTPS", "SQL/TCP", "AMQP"
- Keep labels concise — under 5 words ideally

### Direction

- Relationships should flow in the direction of dependency or data flow
- User -> System -> External System is the typical flow
- Avoid circular relationships at the same abstraction level
- For bidirectional communication, pick the primary direction and label it "Reads from and writes to" or use two relationships

### Avoid

- Don't create relationships between elements at different abstraction levels (e.g., person directly to a component) unless at the view level that shows both
- Don't duplicate relationships that are implied by the hierarchy
- Don't label relationships with just the technology — include the purpose

## Tags and Styles

### Effective Tagging

Use tags to categorize elements for styling:

```
// Infrastructure categories
db = container "Database" "" "PostgreSQL" "Database"
queue = container "Queue" "" "RabbitMQ" "Queue"
cache = container "Cache" "" "Redis" "Database"

// Element roles
external = softwareSystem "External" "" "Existing System"

// Custom categories
webapp = container "Web App" "" "React" "Browser"
mobile = container "Mobile App" "" "React Native" "Mobile"
```

### Standard Tags and Their Styles

| Tag | Shape | Use For |
|-----|-------|---------|
| `Database` | Cylinder | Databases, data stores |
| `Queue` | Pipe | Message queues, event buses |
| `Browser` | WebBrowser | Web applications in browsers |
| `Mobile` | MobileDevicePortrait | Mobile applications |
| `Existing System` | Box (grey) | External/legacy systems |
| `Person` | Person | Users and actors |

### Color Palette (C4 Default)

| Element | Background | Text |
|---------|-----------|------|
| Person | #08427B | #ffffff |
| Software System | #1168BD | #ffffff |
| Container | #438DD5 | #ffffff |
| Component | #85BBF0 | #000000 |
| External System | #999999 | #ffffff |

## Layout Tips

### autoLayout Direction

- `lr` (left-right): Best for flow-oriented diagrams and pipelines
- `tb` (top-bottom): Best for hierarchical diagrams, system context with users at top
- Use consistent direction within a workspace when possible

### Spacing

Adjust rank and node separation for better readability:

```
autoLayout lr 300 200   // more horizontal space
autoLayout tb 200 150   // tighter vertical layout
```

Default values are usually fine. Only adjust when diagrams look cramped or too spread out.

### Selective Include/Exclude

For large diagrams, use selective includes instead of `include *`:

```
container mySystem "key" {
    include customer
    include spa
    include api
    include db
    exclude messageQueue
    autoLayout lr
}
```

## Common Mistakes

### 1. Mixing Abstraction Levels

**Wrong**: Showing components and software systems in the same diagram without the container level.

**Right**: Each C4 level focuses on one abstraction level. Zoom in progressively: L1 -> L2 -> L3 -> L4.

### 2. Too Much Detail in System Context (L1)

**Wrong**: Showing databases and internal services in the system context diagram.

**Right**: The system context only shows the system as a single box plus people and external systems.

### 3. Missing Technology Annotations

**Wrong**: `container "API" "Handles requests"`

**Right**: `container "API" "Handles requests" "Java/Spring Boot"`

### 4. Vague Relationship Labels

**Wrong**: `api -> db "Uses"`

**Right**: `api -> db "Reads from and writes to" "SQL/TCP"`

### 5. Forgetting Styles

**Wrong**: No styles block — everything looks the same.

**Right**: Always include styles with appropriate shapes and colors for each element type.

### 6. Duplicate Identifiers

**Wrong**: Using the same identifier in different software systems (in flat mode).

**Right**: Use unique identifiers or enable `!identifiers hierarchical`.

### 7. Orphaned Elements

**Wrong**: Defining elements that have no relationships and appear isolated in diagrams.

**Right**: Every element should have at least one relationship, or be excluded from the view.

### 8. Creating L4 Diagrams for Everything

**Wrong**: Manually authoring code-level diagrams for every component.

**Right**: Only create L4 for architecturally significant components. The source code is the authoritative L4 view — auto-generate when possible.

## When to Use Each Level

| Level | Audience | Purpose | When to Skip |
|-------|----------|---------|-------------|
| L1 — System Context | Everyone | Scope and external dependencies | Never — always start here |
| L2 — Container | Technical team | Technical building blocks and technology choices | Simple single-container systems |
| L3 — Component | Developers | Internal structure of complex containers | Simple CRUD services, databases |
| L4 — Code | Developers | Class/interface detail of a component | Almost always — auto-generate instead |
