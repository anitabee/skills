# Eval: Edge Case — Minimal Diagram

## User Prompt

Create the simplest possible C4 System Context diagram for a personal notes app. Just one user and one system, no external dependencies.

## Expected Behavior

- Generates a valid, minimal but complete DSL file
- Contains only the essential elements
- Still includes proper structure: workspace, model, views, styles
- Does not add unnecessary external systems or containers

## Validation Criteria

- [ ] Valid `workspace` with name and description
- [ ] Contains exactly 1 `person`
- [ ] Contains exactly 1 `softwareSystem`
- [ ] Contains exactly 1 relationship (person -> system)
- [ ] Contains a `systemContext` view
- [ ] Contains a `styles` block with at least Person and Software System styles
- [ ] No `container`, `component`, or `deploymentNode` elements
- [ ] File is under 40 lines
