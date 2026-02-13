# Eval: Edge Case — Many External Systems

## User Prompt

Create a C4 System Context diagram for an enterprise integration platform. It connects to 8 external systems: Salesforce CRM, SAP ERP, Workday HR, Slack, Jira, GitHub, Datadog monitoring, and PagerDuty alerting. Three types of users interact with it: business analysts, developers, and platform administrators.

## Expected Behavior

- Handles a large number of external systems cleanly
- All 8 external systems are defined and tagged as "Existing System"
- All 3 user types are defined as persons
- Relationships are specific (not generic "uses")
- System context view uses `autoLayout` to handle the many elements
- May suggest `lr` layout for better horizontal spread

## Validation Criteria

- [ ] Contains exactly 3 `person` elements
- [ ] Contains exactly 1 primary `softwareSystem`
- [ ] Contains exactly 8 external `softwareSystem` elements tagged "Existing System"
- [ ] All external systems have unique, descriptive names
- [ ] At least 11 relationships (3 person->system + 8 system->external)
- [ ] Relationship descriptions are specific to each external system's purpose
- [ ] `systemContext` view with `autoLayout`
- [ ] Styles differentiate Person, Software System, and Existing System
