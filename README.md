# Car Wash Booking

Online appointment system for a car wash: customers pick a vehicle type,
services, date/time and (optionally) an operator. The system calculates total
duration and price, shows free slots and prevents double-booking, including
concurrent requests.

Inspired by a real car wash in Brașov; independent from its website.

## Planned stack

Java 21, Spring Boot 3 (Web, Data JPA, Validation, Security), PostgreSQL,
Flyway, JWT, JUnit 5, Mockito, Testcontainers, Docker Compose, OpenAPI,
GitHub Actions.

## Status

Work in progress. Currently: requirements and data model.
See [business rules](docs/business-rules.md).