# Eval: Multi-Level Workspace

## User Prompt

Create a complete C4 workspace for a ride-sharing application with multiple levels: System Context (L1) showing riders, drivers, the ride-sharing system, a payment processor, and a mapping service. Container diagram (L2) showing a mobile app, web app, API gateway, ride matching service, payment service, notification service, PostgreSQL database, and Redis cache. Component diagram (L3) for the ride matching service showing its internal components. Include appropriate styles throughout.

## Expected Behavior

- Single workspace containing L1, L2, and L3 views
- Model is defined once and shared across all views
- System Context view shows people and systems only
- Container view shows internal containers
- Component view zooms into ride matching service
- Styles are defined once and apply to all views
- All identifiers are consistent across model and views

## Validation Criteria

- [ ] Single `workspace` block containing everything
- [ ] Contains at least 2 `person` elements (rider, driver)
- [ ] Contains at least 2 external `softwareSystem` elements
- [ ] Primary system has at least 6 `container` elements
- [ ] At least one container has nested `component` elements
- [ ] Views section contains a `systemContext` view
- [ ] Views section contains a `container` view
- [ ] Views section contains a `component` view
- [ ] Styles block includes Person, Software System, Container, Component, Database shapes
- [ ] All element identifiers used in views exist in the model
- [ ] External systems tagged "Existing System"
