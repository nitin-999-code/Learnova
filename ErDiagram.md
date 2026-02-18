# 🗂️ Entity Relationship Diagram (ERD) - Learnova

This document defines the database schema structure, focusing on data integrity, relationships, and scalability.

```mermaid
erDiagram
    USERS {
        UUID id PK
        VARCHAR email UK
        VARCHAR password_hash
        VARCHAR full_name
        ENUM role "STUDENT, MENTOR, ADMIN"
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    MENTOR_PROFILES {
        UUID user_id PK, FK
        TEXT bio
        DECIMAL hourly_rate
        JSON social_links
        BOOLEAN is_verified
    }

    SKILLS {
        UUID id PK
        VARCHAR name UK
        VARCHAR category
        TEXT description
    }

    MENTOR_SKILLS {
        UUID mentor_id PK, FK
        UUID skill_id PK, FK
        INT years_experience
    }

    AVAILABILITY_SLOTS {
        UUID id PK
        UUID mentor_id FK
        TIMESTAMP start_time
        TIMESTAMP end_time
        BOOLEAN is_booked
    }

    BOOKINGS {
        UUID id PK
        UUID student_id FK
        UUID mentor_id FK
        UUID slot_id FK
        ENUM status "PENDING, CONFIRMED, COMPLETED, CANCELLED"
        DECIMAL price_paid
        TIMESTAMP created_at
    }

    REVIEWS {
        UUID id PK
        UUID booking_id FK
        UUID mentor_id FK
        UUID student_id FK
        INT rating
        TEXT comment
        TIMESTAMP created_at
    }

    %% Relationships

    USERS ||--o| MENTOR_PROFILES : "has optional"
    USERS ||--o{ BOOKINGS : "makes (as student)"
    USERS ||--o{ REVIEWS : "writes"

    MENTOR_PROFILES ||--o{ MENTOR_SKILLS : "offers"
    SKILLS ||--o{ MENTOR_SKILLS : "classified as"

    MENTOR_PROFILES ||--o{ AVAILABILITY_SLOTS : "defines"
    MENTOR_PROFILES ||--o{ BOOKINGS : "receives (as mentor)"
    
    AVAILABILITY_SLOTS ||--o| BOOKINGS : "resource for"

    BOOKINGS ||--o| REVIEWS : "generates"
```

## Key Schema Decisions
1.  **Normalization**: The schema is normalized to 3NF to reduce redundancy (e.g., `SKILLS` are separate from `MENTOR_PROFILES`).
2.  **Foreign Keys**: Strict referential integrity on all relationships (e.g., `BOOKINGS` relies on valid `USERS` and `SLOTS`).
3.  **Scalability**: UUIDs are used for Primary Keys instead of auto-incrementing integers to allow for distributed database scaling and security.
4.  **Flexibility**: `MENTOR_SKILLS` is a junction table allowing Many-to-Many relationships between Mentors and Skills.
