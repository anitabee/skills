# Eval: Container Diagram

## User Prompt

Generate a C4 Container diagram for a blog platform. It has a React frontend, a Node.js REST API, a PostgreSQL database, and a Redis cache. Blog authors write posts and readers can comment. The API also uses AWS S3 for image storage.

## Expected Behavior

- Generates a valid Structurizr DSL file
- Creates persons for Author and Reader
- Creates the blog platform as a softwareSystem with nested containers
- Containers include: React frontend, Node.js API, PostgreSQL DB, Redis cache
- AWS S3 is modeled as an external software system
- Relationships describe how each container interacts
- Creates a `container` view for the blog platform
- Uses appropriate shapes: Cylinder for databases, WebBrowser for frontend
- Includes technology annotations on all containers

## Validation Criteria

- [ ] Contains at least 2 `person` elements (author, reader)
- [ ] Contains 1 primary `softwareSystem` with nested containers
- [ ] Contains at least 4 `container` elements
- [ ] Each container has a technology annotation (3rd parameter)
- [ ] Database containers tagged with "Database"
- [ ] Contains `container` view (not `systemContext`)
- [ ] Styles include `shape Cylinder` for Database tag
- [ ] External system (S3) is outside the softwareSystem block
- [ ] All containers have at least one relationship
