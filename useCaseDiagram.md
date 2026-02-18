# Use Case Diagram - Learnova

This document outlines the interactions between the primary actors (Student, Mentor, Admin) and the Learnova system.

```mermaid
usecaseDiagram
    actor Student
    actor Mentor
    actor Admin

    usecase "Register" as UCS1
    usecase "Login (JWT)" as UCS2
    usecase "Manage Profile" as UCS3
    usecase "List Skills" as UCM1
    usecase "Set Availability" as UCM2
    usecase "Browse Mentors" as UCS4
    usecase "View Mentor Profile" as UCS5
    usecase "Book Session" as UCS6
    usecase "Manage Bookings" as UCCommon1
    usecase "Complete Session" as UCM3
    usecase "Cancel Session" as UCCommon2
    usecase "Leave Review" as UCS7
    usecase "View Reviews" as UCCommon3
    usecase "Moderate Users" as UCA1
    usecase "Moderate Content" as UCA2
    usecase "View Analytics" as UCA3

    %% Relationships - Student
    Student --> UCS1
    Student --> UCS2
    Student --> UCS3
    Student --> UCS4
    Student --> UCS5
    Student --> UCS6
    Student --> UCS7
    Student --> UCCommon1
    Student --> UCCommon2
    Student --> UCCommon3

    %% Relationships - Mentor
    Mentor --> UCS1
    Mentor --> UCS2
    Mentor --> UCS3
    Mentor --> UCM1
    Mentor --> UCM2
    Mentor --> UCM3
    Mentor --> UCCommon1
    Mentor --> UCCommon2
    Mentor --> UCCommon3

    %% Relationships - Admin
    Admin --> UCS2
    Admin --> UCA1
    Admin --> UCA2
    Admin --> UCA3

    %% Includes & Extends
    UCS6 ..> UCS2 : include
    UCM1 ..> UCS2 : include
    UCA1 ..> UCS2 : include
```

## Description of Actors
1.  **Student**: A user seeking to learn new skills. Can browse, book, and review mentors.
2.  **Mentor**: A user offering expertise. Can list skills, set availability, and conduct sessions.
    *   *Note: A user can potentially hold both roles, but interactions are distinct.*
3.  **Admin**: System administrator responsible for platform health, moderation, and user management.
