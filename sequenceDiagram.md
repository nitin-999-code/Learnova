# 🔄 Sequence Diagrams - Learnova

This document details critical workflows within the Learnova system using sequence diagrams.

## 1. End-to-End Booking Flow

This flow illustrates how a Student finds a mentor and successfully books a session.

```mermaid
sequenceDiagram
    autonumber
    actor Student
    participant UI as Frontend
    participant BC as BookingController
    participant BS as BookingService
    participant MS as MentorService
    participant BR as BookingRepository

    Student->>UI: Selects Mentor & Time Slot
    UI->>BC: POST /bookings (mentorId, slotId)
    activate BC
    BC->>BS: createBooking(studentId, mentorId, slotId)
    activate BS
    
    BS->>MS: checkAvailability(mentorId, slotId)
    activate MS
    MS-->>BS: Returns Available/Unavailable
    deactivate MS

    alt Slot is Available
        BS->>BR: saveBooking(details)
        activate BR
        BR-->>BS: Booking Created (ID: 123)
        deactivate BR
        
        BS->>MS: markSlotReserved(slotId)
        activate MS
        MS-->>BS: Slot Updated
        deactivate MS

        BS-->>BC: Booking Confirmation
        BC-->>UI: 201 Created (Booking Details)
        UI-->>Student: Show Success Message
    else Slot Unavailable
        BS-->>BC: Error: Slot Taken
        BC-->>UI: 409 Conflict
        UI-->>Student: Show Error Message
    end
    deactivate BS
    deactivate BC
```

## 2. Authentication Flow (JWT)

This flow shows how a user logs in and receives a secure token.

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant UI as Frontend
    participant AC as AuthController
    participant AS as AuthService
    participant UR as UserRepository
    participant TS as TokenService

    User->>UI: Enters Email & Password
    UI->>AC: POST /auth/login
    activate AC
    AC->>AS: authenticate(email, password)
    activate AS
    
    AS->>UR: findByEmail(email)
    activate UR
    UR-->>AS: User Entity (hash)
    deactivate UR

    alt User Found & Password Matches
        AS->>TS: generateToken(userId, role)
        activate TS
        TS-->>AS: JWT Access Token
        deactivate TS
        
        AS-->>AC: AuthSuccess(token, userProfile)
        AC-->>UI: 200 OK (Set Cookie/Include Header)
        UI-->>User: Redirect to Dashboard
    else Invalid Credentials
        AS-->>AC: AuthFailed
        AC-->>UI: 401 Unauthorized
        UI-->>User: Show "Invalid Login"
    end
    deactivate AS
    deactivate AC
```

## 3. Session Completion Flow

This flow depicts a Mentor marking a session as complete, allowing the Student to leave a review.

```mermaid
sequenceDiagram
    autonumber
    actor Mentor
    participant UI as Frontend
    participant SC as SessionController
    participant SS as SessionService
    participant NR as NotificationService
    actor Student

    Mentor->>UI: Clicks "Mark Complete"
    UI->>SC: PATCH /sessions/{id}/complete
    activate SC
    SC->>SS: completeSession(sessionId, mentorId)
    activate SS
    
    SS->>SS: validateSessionOwner(sessionId, mentorId)
    SS->>SS: updateStatus(COMPLETED)
    
    SS->>NR: notifyStudent(studentId, "Session Completed")
    activate NR
    NR-->>Student: Email/Push: "Rate your session!"
    deactivate NR

    SS-->>SC: Success
    deactivate SS
    SC-->>UI: 200 OK
    UI-->>Mentor: Update Dashboard Status
```
