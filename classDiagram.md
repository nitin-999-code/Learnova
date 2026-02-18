# 🏗️ Class Diagram - Learnova

This diagram represents the Object-Oriented structure of the backend, following Clean Architecture principles. It highlights encapsulation, inheritance, and polymorphism.

```mermaid
classDiagram
    %% Core Entities
    class User {
        +UUID id
        +String name
        +String email
        -String passwordHash
        +Role role
        +Date createdAt
        +login(credentials)
        +updateProfile(data)
    }

    class Student {
        +List~Booking~ bookingHistory
        +searchMentors(criteria)
        +bookSession(mentor, slot)
        +leaveReview(booking, rating, comment)
    }

    class Mentor {
        +String bio
        +List~Skill~ skills
        +List~TimeSlot~ availability
        +Double hourlyRate
        +addSkill(skill)
        +setAvailability(slots)
        +approveBooking(bookingId)
    }

    class Admin {
        +List~String~ permissions
        +banUser(userId)
        +approveContent(contentId)
        +viewSystemLogs()
    }

    class Skill {
        +UUID id
        +String name
        +Category category
        +String description
    }

    class Booking {
        +UUID id
        +UUID studentId
        +UUID mentorId
        +UUID skillId
        +DateTime startTime
        +DateTime endTime
        +BookingStatus status
        +cancel()
        +complete()
    }

    class Review {
        +UUID id
        +UUID bookingId
        +UUID reviewerId
        +Int rating
        +String comment
        +Date timestamp
    }

    class Notification {
        +UUID id
        +UUID userId
        +String message
        +Boolean isRead
        +send()
    }

    %% Inheritance Relationships
    User <|-- Student
    User <|-- Mentor
    User <|-- Admin

    %% Associations
    Mentor "1" o-- "*" Skill : lists
    Student "1" --> "*" Booking : makes
    Mentor "1" --> "*" Booking : receives
    Booking "1" *-- "1" Review : generates
    User "1" <-- "*" Notification : receives

    %% Enumerations
    class Role {
        <<enumeration>>
        STUDENT
        MENTOR
        ADMIN
    }

    class BookingStatus {
        <<enumeration>>
        PENDING
        CONFIRMED
        COMPLETED
        CANCELLED
    }
```

## Design Principles Applied
*   **Inheritance**: `Student`, `Mentor`, and `Admin` all inherit common properties (id, email, etc.) from the abstract `User` class.
*   **Encapsulation**: Private attributes like `passwordHash` are managed internally.
*   **Abstraction**: High-level actions like `bookSession` abstract away the complexity of database transactions and availability checks.
