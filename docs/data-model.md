
# Data Model

Entity-relationship diagram for the car wash booking system.

```mermaid
erDiagram
    USERS ||--o| OPERATORS : "has profile"
    USERS ||--o{ APPOINTMENTS : "books as client"
    OPERATORS ||--o{ APPOINTMENTS : performs
    VEHICLE_TYPES ||--o{ APPOINTMENTS : "booked for"
    APPOINTMENTS ||--|{ APPOINTMENT_SERVICES : contains
    SERVICES ||--o{ APPOINTMENT_SERVICES : "chosen in"

    USERS {
        bigint id PK
        varchar email UK
        varchar password_hash
        varchar first_name
        varchar last_name
        varchar phone
        varchar role
        timestamptz created_at
    }

    OPERATORS {
        bigint id PK
        bigint user_id FK
        varchar display_name
        boolean active
    }

    VEHICLE_TYPES {
        bigint id PK
        varchar name UK
        numeric multiplier
    }

    SERVICES {
        bigint id PK
        varchar name UK
        text description
        int base_duration_minutes
        numeric base_price
        boolean bookable
        boolean active
    }

    BUSINESS_HOURS {
        smallint day_of_week PK
        time open_time
        time close_time
    }

    APPOINTMENTS {
        bigint id PK
        bigint client_id FK
        bigint operator_id FK
        bigint vehicle_type_id FK
        varchar vehicle_description
        timestamptz start_time
        timestamptz end_time
        int occupied_minutes
        numeric total_price
        varchar status
        timestamptz created_at
        timestamptz cancelled_at
    }

    APPOINTMENT_SERVICES {
        bigint appointment_id PK
        bigint service_id PK
        numeric price_at_booking
        int duration_at_booking
    }
```

## Notes

- **Price/duration snapshot:** `appointment_services` stores the price and duration at booking time, so later catalog changes do not alter past appointments.
- **Money:** stored as `numeric` (`BigDecimal` in Java), `double` leads to rounding errors.
- **Time:** stored as `timestamptz`; slot generation uses the `Europe/Bucharest` time zone.
- **Concurrency:** a PostgreSQL exclusion constraint on (`operator_id`, time range) prevents overlapping appointments at the database level.
