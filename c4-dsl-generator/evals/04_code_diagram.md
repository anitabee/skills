# Eval: Code Diagram (L4)

## User Prompt

Create a C4 Code diagram (Level 4) for the "Authentication" component in a Java/Spring Boot API service. Show the following classes and interfaces: AuthController (REST controller), AuthService (business logic), UserRepository (interface for data access), UserRepositoryJpa (JPA implementation), User (entity class), JwtTokenProvider (utility class), and AuthenticationEvent (domain event). Show the relationships between them: implements, uses, creates, etc.

## Expected Behavior

- Models classes/interfaces as fine-grained components inside a container
- Each component has a technology annotation indicating its type (Java Class, Interface, etc.)
- Uses tags to distinguish element types (Entity, Interface, Abstract Class, etc.)
- Styles use different shapes/borders for interfaces vs classes vs entities
- Relationships describe OOP relationships (implements, uses, creates, extends)
- Uses a `component` view (since Structurizr has no `code` view type)

## Validation Criteria

- [ ] Contains at least 7 component elements representing classes/interfaces
- [ ] Components have technology annotations like "Java Class", "Java Interface", etc.
- [ ] Components are tagged by type (Entity, Interface, Repository, etc.)
- [ ] Relationships use OOP terms: "Implements", "Uses", "Creates", "Extends"
- [ ] UserRepositoryJpa -> UserRepository relationship labeled "Implements"
- [ ] Styles differentiate interfaces (dashed border or RoundedBox) from classes
- [ ] Uses `component` view type
- [ ] Contains `autoLayout`
- [ ] Diagram is focused on a single component's internals, not an entire system
