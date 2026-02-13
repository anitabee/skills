# Eval: Basic System Context Diagram

## User Prompt

Create a C4 System Context diagram for a todo application. The app is used by end users who manage their tasks. It integrates with Google Calendar for due dates and SendGrid for email reminders.

## Expected Behavior

- Generates a valid Structurizr DSL file
- Defines a `workspace` with a descriptive name
- Creates a `person` for the end user
- Creates a `softwareSystem` for the todo app
- Creates external systems for Google Calendar and SendGrid tagged as "Existing System"
- Defines meaningful relationships with descriptions and technologies
- Creates a `systemContext` view with `include *` and `autoLayout`
- Includes styles for Person, Software System, and Existing System
- Output is a complete, self-contained `.dsl` file

## Validation Criteria

- [ ] File starts with `workspace`
- [ ] Contains `model { }` block
- [ ] Contains at least 1 `person`
- [ ] Contains exactly 1 primary `softwareSystem` (the todo app)
- [ ] Contains 2 external software systems with "Existing System" tag
- [ ] Contains at least 3 relationships (user->app, app->calendar, app->email)
- [ ] Contains `views { }` block with `systemContext` view
- [ ] Contains `styles { }` block
- [ ] Person element has `shape Person`
- [ ] No containers or components defined in the view (system context level only)
