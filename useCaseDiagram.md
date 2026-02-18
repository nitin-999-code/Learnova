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

## Actors & Key Responsibilities

### Student
The core consumer of the platform seeking knowledge.
*   **Key Responsibilities**:
    *   Register and maintain a personal profile.
    *   Search for mentors based on skills and ratings.
    *   Book and pay for learning sessions.
    *   Attend sessions and provide reviews/ratings.

### Mentor
The expert provider sharing knowledge.
*   **Key Responsibilities**:
    *   Create a professional profile highlighting expertise.
    *   List specific skills and set hourly rates.
    *   Manage calendar availability for sessions.
    *   Conduct sessions and track earnings.
    *   *Note: A user can act as both a Student and a Mentor.*

### Admin
The system overseer ensuring quality and safety.
*   **Key Responsibilities**:
    *   Monitor platform health and analytics.
    *   Moderate user accounts (ban/suspend violators).
    *   Review and approve content or skill listings.
    *   Resolve disputes between Students and Mentors.
