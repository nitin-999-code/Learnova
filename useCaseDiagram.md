# Use Case Diagram - Learnova

This document outlines the interactions between the primary actors (Student, Mentor, Admin) and the Learnova system, utilizing a GitHub-compatible flowchart to represent use cases.

```mermaid
flowchart LR
    %% Actors
    Student((Student))
    Mentor((Mentor))
    Admin((Admin))

    %% System Boundary
    subgraph Learnova_System [Learnova System]
        direction TB
        
        %% Auth
        Register[Register]
        Login[Login / Auth]
        Profile[Manage Profile]

        %% Discovery & Booking
        Search[Browse Mentors & Skills]
        ViewMentor[View Mentor Profile]
        Book[Book Session]
        
        %% Session Management
        ManageBookings[Manage Bookings]
        Complete[Complete Session]
        Cancel[Cancel Session]

        %% Mentor Specific
        ListSkills[List Skills]
        Availability[Set Availability]

        %% Feedback
        Review[Leave Review]
        ViewSystem[View Reviews]

        %% Admin
        ModUser[Moderate Users]
        ModContent[Moderate Content]
        Analytics[View Analytics]
    end

    %% Relationships - Student
    Student --> Register
    Student --> Login
    Student --> Profile
    Student --> Search
    Student --> ViewMentor
    Student --> Book
    Student --> ManageBookings
    Student --> Cancel
    Student --> Review
    Student --> ViewSystem

    %% Relationships - Mentor
    Mentor --> Register
    Mentor --> Login
    Mentor --> Profile
    Mentor --> ListSkills
    Mentor --> Availability
    Mentor --> ManageBookings
    Mentor --> Complete
    Mentor --> Cancel
    Mentor --> ViewSystem

    %% Relationships - Admin
    Admin --> Login
    Admin --> ModUser
    Admin --> ModContent
    Admin --> Analytics

    %% Internal Dependencies (Includes)
    Book -.->|requires| Login
    ListSkills -.->|requires| Login
    ModUser -.->|requires| Login
```

## Description of Actors
1.  **Student**: A user seeking to learn new skills. Can browse, book, and review mentors.
2.  **Mentor**: A user offering expertise. Can list skills, set availability, and conduct sessions.
    *   *Note: A user can potentially hold both roles, but interactions are distinct.*
3.  **Admin**: System administrator responsible for platform health, moderation, and user management.
