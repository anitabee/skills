# C4 Diagram Examples

## Table of Contents

- [System Context Diagram (L1)](#system-context-diagram-l1)
- [Container Diagram (L2)](#container-diagram-l2)
- [Component Diagram (L3)](#component-diagram-l3)
- [Code Diagram (L4)](#code-diagram-l4)
- [Multi-Level Workspace](#multi-level-workspace)

## System Context Diagram (L1)

Shows the system in scope and its interactions with users and other systems. The highest level of abstraction — the system is a single box.

```
workspace "E-Commerce Platform" "System Context" {
    model {
        customer = person "Customer" "Browses products and places orders"
        admin = person "Administrator" "Manages products and orders" "Staff"

        ecommerce = softwareSystem "E-Commerce Platform" "Online store for selling products"

        paymentGateway = softwareSystem "Payment Gateway" "Processes credit card payments" "Existing System"
        emailService = softwareSystem "Email Service" "Sends transactional emails" "Existing System"
        shippingProvider = softwareSystem "Shipping Provider" "Handles order fulfillment" "Existing System"

        customer -> ecommerce "Browses and purchases products" "HTTPS"
        admin -> ecommerce "Manages catalog and orders" "HTTPS"
        ecommerce -> paymentGateway "Processes payments" "HTTPS/JSON"
        ecommerce -> emailService "Sends order confirmations" "SMTP"
        ecommerce -> shippingProvider "Creates shipping labels" "REST API"
    }
    views {
        systemContext ecommerce "SystemContext" "E-Commerce System Context" {
            include *
            autoLayout lr
        }
        styles {
            element "Person" {
                shape Person
                background #08427B
                color #ffffff
            }
            element "Staff" {
                background #999999
                color #ffffff
            }
            element "Software System" {
                background #1168BD
                color #ffffff
            }
            element "Existing System" {
                background #999999
                color #ffffff
            }
        }
    }
}
```

## Container Diagram (L2)

Zooms into a software system to show its containers — the separately deployable units (applications, databases, message queues, etc.).

```
workspace "E-Commerce Platform" "Container Diagram" {
    model {
        customer = person "Customer" "Browses products and places orders"

        ecommerce = softwareSystem "E-Commerce Platform" "Online store" {
            spa = container "SPA" "Single-page application for customers" "React" "Browser"
            adminPortal = container "Admin Portal" "Management interface" "Angular" "Browser"
            apiGateway = container "API Gateway" "Routes and authenticates requests" "Kong"
            productService = container "Product Service" "Manages product catalog" "Node.js"
            orderService = container "Order Service" "Handles order lifecycle" "Java/Spring Boot"
            userService = container "User Service" "Manages user accounts" "Java/Spring Boot"
            messageQueue = container "Message Queue" "Async communication" "RabbitMQ" "Queue"
            productDb = container "Product DB" "Product catalog data" "MongoDB" "Database"
            orderDb = container "Order DB" "Order and transaction data" "PostgreSQL" "Database"
            userDb = container "User DB" "User account data" "PostgreSQL" "Database"
            cache = container "Cache" "Session and product cache" "Redis" "Database"
        }

        paymentGateway = softwareSystem "Payment Gateway" "Processes payments" "Existing System"

        customer -> spa "Browses products" "HTTPS"
        spa -> apiGateway "Makes API calls" "HTTPS/JSON"
        adminPortal -> apiGateway "Manages catalog" "HTTPS/JSON"
        apiGateway -> productService "Routes product requests" "HTTP"
        apiGateway -> orderService "Routes order requests" "HTTP"
        apiGateway -> userService "Routes user requests" "HTTP"
        productService -> productDb "Reads/writes" "MongoDB Wire Protocol"
        productService -> cache "Caches product data" "TCP"
        orderService -> orderDb "Reads/writes" "SQL/TCP"
        orderService -> messageQueue "Publishes order events" "AMQP"
        userService -> userDb "Reads/writes" "SQL/TCP"
        userService -> cache "Caches sessions" "TCP"
        orderService -> paymentGateway "Processes payments" "HTTPS"
    }
    views {
        container ecommerce "Containers" "E-Commerce Container Diagram" {
            include *
            autoLayout lr
        }
        styles {
            element "Person" {
                shape Person
                background #08427B
                color #ffffff
            }
            element "Software System" {
                background #1168BD
                color #ffffff
            }
            element "Existing System" {
                background #999999
                color #ffffff
            }
            element "Container" {
                background #438DD5
                color #ffffff
            }
            element "Database" {
                shape Cylinder
                background #438DD5
                color #ffffff
            }
            element "Queue" {
                shape Pipe
                background #438DD5
                color #ffffff
            }
            element "Browser" {
                shape WebBrowser
                background #438DD5
                color #ffffff
            }
        }
    }
}
```

## Component Diagram (L3)

Zooms into a single container to show its internal components — the major structural building blocks (controllers, services, repositories, etc.).

```
workspace "E-Commerce Platform" "Component Diagram" {
    model {
        ecommerce = softwareSystem "E-Commerce Platform" {
            apiGateway = container "API Gateway" "Routes requests" "Kong"
            orderService = container "Order Service" "Handles orders" "Java/Spring Boot" {
                orderController = component "Order Controller" "Handles HTTP requests for orders" "Spring MVC Controller"
                orderLogic = component "Order Logic" "Business rules for order processing" "Spring Service"
                paymentAdapter = component "Payment Adapter" "Integrates with payment gateway" "Spring Component"
                inventoryChecker = component "Inventory Checker" "Validates stock availability" "Spring Component"
                orderRepo = component "Order Repository" "Data access for orders" "Spring Data JPA"
                orderEventPublisher = component "Event Publisher" "Publishes order domain events" "Spring Events"
            }
            orderDb = container "Order DB" "Order data" "PostgreSQL" "Database"
            messageQueue = container "Message Queue" "Async messaging" "RabbitMQ" "Queue"
            inventoryService = container "Inventory Service" "Manages stock" "Node.js"
        }

        paymentGateway = softwareSystem "Payment Gateway" "" "Existing System"

        apiGateway -> orderController "Forwards order requests" "HTTP/JSON"
        orderController -> orderLogic "Delegates to"
        orderLogic -> inventoryChecker "Checks stock via"
        orderLogic -> paymentAdapter "Processes payment via"
        orderLogic -> orderRepo "Persists orders via"
        orderLogic -> orderEventPublisher "Publishes events via"
        inventoryChecker -> inventoryService "Queries stock" "HTTP/JSON"
        paymentAdapter -> paymentGateway "Charges card" "HTTPS/JSON"
        orderRepo -> orderDb "Reads/writes" "SQL/TCP"
        orderEventPublisher -> messageQueue "Publishes events" "AMQP"
    }
    views {
        component orderService "Components" "Order Service Components" {
            include *
            autoLayout lr
        }
        styles {
            element "Component" {
                background #85BBF0
                color #000000
            }
            element "Container" {
                background #438DD5
                color #ffffff
            }
            element "Database" {
                shape Cylinder
            }
            element "Queue" {
                shape Pipe
            }
            element "Existing System" {
                background #999999
                color #ffffff
            }
        }
    }
}
```

## Code Diagram (L4)

Zooms into a single component to show classes, interfaces, and their relationships. Structurizr DSL does not have a dedicated `code` view, so L4 diagrams are modeled using fine-grained components with class-level technology annotations.

This is typically auto-generated from source code. When authored manually, use it only for architecturally significant components.

```
workspace "E-Commerce Platform" "Code Diagram — Order Processing" {
    model {
        ecommerce = softwareSystem "E-Commerce Platform" {
            orderService = container "Order Service" "Handles orders" "Java/Spring Boot" {
                orderComponent = component "Order Processing" "Processes and validates orders" "Spring" {
                    // L4: model classes as fine-grained components
                }
            }

            // Model the classes inside the Order Processing component
            // Using a dedicated container to represent the code-level view
            orderCodeView = container "Order Processing (Code View)" "Classes in the Order Processing component" "Java" {
                orderEntity = component "Order" "Order aggregate root" "Java Class,Entity"
                orderItem = component "OrderItem" "Line item in an order" "Java Class,Entity"
                orderStatus = component "OrderStatus" "Order state enum" "Java Enum"
                orderService2 = component "OrderService" "Order business logic" "Java Class,Service"
                orderRepository = component "OrderRepository" "Data access interface" "Java Interface,Repository"
                orderRepositoryImpl = component "OrderRepositoryJpa" "JPA implementation of OrderRepository" "Java Class,Repository"
                orderValidator = component "OrderValidator" "Validates order constraints" "Java Class"
                orderEvent = component "OrderEvent" "Domain event base class" "Java Abstract Class"
                orderCreatedEvent = component "OrderCreatedEvent" "Published when order is created" "Java Class,Event"
            }

            orderService2 -> orderRepository "Persists orders via"
            orderService2 -> orderValidator "Validates with"
            orderService2 -> orderEntity "Creates and manages"
            orderService2 -> orderCreatedEvent "Publishes"
            orderRepositoryImpl -> orderRepository "Implements"
            orderEntity -> orderItem "Contains"
            orderEntity -> orderStatus "Has"
            orderCreatedEvent -> orderEvent "Extends"
        }
    }
    views {
        component orderCodeView "CodeView" "Order Processing — Class Diagram" {
            include *
            autoLayout tb
        }
        styles {
            element "Component" {
                background #85BBF0
                color #000000
            }
            element "Entity" {
                shape Hexagon
                background #85BBF0
                color #000000
            }
            element "Java Interface" {
                shape RoundedBox
                border dashed
                background #B0D0F0
                color #000000
            }
            element "Repository" {
                shape Cylinder
                background #85BBF0
                color #000000
            }
            element "Java Abstract Class" {
                border dashed
                background #A0C4E8
                color #000000
            }
            element "Event" {
                shape RoundedBox
                background #FFD700
                color #000000
            }
            element "Java Enum" {
                shape Hexagon
                background #C0DCF0
                color #000000
            }
        }
    }
}
```

## Multi-Level Workspace

A single workspace containing L1, L2, and L3 views — the most common combination. L4 is typically generated separately from code.

```
workspace "Internet Banking System" "Multi-level C4 architecture" {
    model {
        customer = person "Personal Banking Customer" "A customer of the bank"

        banking = softwareSystem "Internet Banking System" "Allows customers to view account info and make payments" {
            webapp = container "Web Application" "Serves the single-page app" "Java/Spring MVC"
            spa = container "Single-Page App" "Banking UI in the browser" "JavaScript/React" "Browser"
            mobileApp = container "Mobile App" "Banking on mobile" "React Native" "Mobile"
            api = container "API Application" "Provides banking via JSON/HTTPS API" "Java/Spring Boot" {
                signinController = component "Sign In Controller" "Allows users to sign in" "Spring MVC Controller"
                accountsController = component "Accounts Controller" "Provides account information" "Spring MVC Controller"
                securityComponent = component "Security Component" "Authentication and authorization" "Spring Security"
                accountFacade = component "Account Facade" "Orchestrates account operations" "Spring Service"
            }
            database = container "Database" "Stores user and account data" "Oracle 19c" "Database"
        }

        mainframe = softwareSystem "Mainframe Banking System" "Core banking" "Existing System"
        email = softwareSystem "E-mail System" "Sends emails to customers" "Existing System"

        customer -> banking "Views account balances and makes payments"
        customer -> spa "Uses" "HTTPS"
        customer -> mobileApp "Uses"
        spa -> api "Makes API calls" "JSON/HTTPS"
        mobileApp -> api "Makes API calls" "JSON/HTTPS"
        webapp -> spa "Delivers to the browser"
        api -> database "Reads/writes" "SQL/TCP"
        api -> mainframe "Gets account info" "XML/HTTPS"
        api -> email "Sends emails" "SMTP"

        signinController -> securityComponent "Uses"
        accountsController -> accountFacade "Uses"
        accountFacade -> database "Reads/writes" "SQL"
        securityComponent -> database "Reads/writes" "SQL"
    }
    views {
        systemContext banking "SystemContext" "L1 — System Context" {
            include *
            autoLayout lr
        }
        container banking "Containers" "L2 — Container diagram" {
            include *
            autoLayout lr
        }
        component api "Components" "L3 — API Components" {
            include *
            autoLayout lr
        }
        styles {
            element "Person" {
                shape Person
                background #08427B
                color #ffffff
            }
            element "Software System" {
                background #1168BD
                color #ffffff
            }
            element "Existing System" {
                background #999999
                color #ffffff
            }
            element "Container" {
                background #438DD5
                color #ffffff
            }
            element "Browser" {
                shape WebBrowser
            }
            element "Mobile" {
                shape MobileDevicePortrait
            }
            element "Database" {
                shape Cylinder
            }
            element "Component" {
                background #85BBF0
                color #000000
            }
        }
    }
}
```
