# Eval: Groups and Themes

## User Prompt

Create a C4 Container diagram for a microservices-based CRM system. Group the containers into "Frontend" (React dashboard, mobile app), "Core Services" (contact service, deal service, activity service), "Data Layer" (PostgreSQL, Elasticsearch, Redis), and "Integration" (email connector, calendar sync). Use the default Structurizr theme.

## Expected Behavior

- Uses `group` blocks to organize containers into logical groups
- Groups are nested inside the softwareSystem
- Includes the `theme default` directive
- All containers have technology annotations
- Relationships connect containers across groups
- Container view includes all groups

## Validation Criteria

- [ ] Contains at least 4 `group` blocks inside the softwareSystem
- [ ] Each group contains 2-3 containers
- [ ] Total of at least 9 containers
- [ ] Contains `theme default` in the views section
- [ ] Each container has a technology field
- [ ] Cross-group relationships exist (e.g., services -> data layer)
- [ ] Container view uses `include *`
- [ ] Database-tagged elements exist
