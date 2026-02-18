# 🎭 Use Case Diagram - Learnova

This document outlines the interactions between the primary actors (Student, Mentor, Admin) and the Learnova system.

```mermaid
usecaseDiagram
    actor "Student" as S
    actor "Mentor" as M
    actor "Admin" as A

    package "Learnova System" {
        
        %% Authentication
        usecase "Register" as UCS1
        usecase "Login (JWT)" as UCS2
        usecase "Manage Profile" as UCS3

        %% Mentorship & Skills
        usecase "List Skills" as UCM1
        usecase "Set Availability" as UCM2
        usecase "Browse Mentors" as UCS4
        usecase "View Mentor Profile" as UCS5

        %% Booking & Session
        usecase "Book Session" as UCS6
        usecase "Manage Bookings" as UCCommon1
        usecase "Complete Session" as UCM3
        usecase "Cancel Session" as UCCommon2

        %% Review & Feedback
        usecase "Leave Review" as UCS7
        usecase "View Reviews" as UCCommon3

        %% Admin Actions
        usecase "Moderate Users" as UCA1
        usecase "Moderate Content" as UCA2
        usecase "View Analytics" as UCA3
    }

    %% Relationships - Student
    S --> UCS1
    S --> UCS2
    S --> UCS3
    S --> UCS4
    S --> UCS5
    S --> UCS6
    S --> UCS7
    S --> UCCommon1
    S --> UCCommon2
    S --> UCCommon3

    %% Relationships - Mentor
    M --> UCS1
    M --> UCS2
    M --> UCS3
    M --> UCM1
    M --> UCM2
    M --> UCM3
    M --> UCCommon1
    M --> UCCommon2
    M --> UCCommon3

    %% Relationships - Admin
    A --> UCS2
    A --> UCA1
    A --> UCA2
    A --> UCA3

    %% Includes & Extends
    UCS6 ..> UCS2 : <<include>>
    UCM1 ..> UCS2 : <<include>>
    UCA1 ..> UCS2 : <<include>>
```

## Description of Actors
1.  **Student**: A user seeking to learn new skills. Can browse, book, and review mentors.
2.  **Mentor**: A user offering expertise. Can list skills, set availability, and conduct sessions.
    *   *Note: A user can potentially hold both roles, but interactions are distinct.*
3.  **Admin**: System administrator responsible for platform health, moderation, and user management.
