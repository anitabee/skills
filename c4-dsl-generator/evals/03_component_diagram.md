# Eval: Component Diagram

## User Prompt

Create a C4 Component diagram for the "Payment Service" container in an e-commerce system. The Payment Service is a Java/Spring Boot application with these components: PaymentController (Spring MVC), PaymentProcessor (service layer), FraudDetector (validates transactions), StripeAdapter (integrates with Stripe API), and PaymentRepository (Spring Data JPA). It connects to a PostgreSQL database and the external Stripe payment gateway.

## Expected Behavior

- Creates a softwareSystem with the Payment Service container and its components
- Defines 5 components inside the Payment Service container
- Each component has a description and technology
- Models the PostgreSQL database as a sibling container
- Models Stripe as an external softwareSystem
- Relationships show component-level interactions
- Creates a `component` view scoped to the Payment Service container
- Includes appropriate styles

## Validation Criteria

- [ ] Contains exactly 5 `component` elements inside the Payment Service container
- [ ] Each component has technology annotation (e.g., "Spring MVC Controller")
- [ ] Contains relationships between components (controller -> processor -> repo, etc.)
- [ ] StripeAdapter has relationship to external Stripe system
- [ ] PaymentRepository has relationship to database container
- [ ] View is `component paymentService` (scoped to the container)
- [ ] Component elements styled with lighter color than containers
- [ ] Database has Cylinder shape
