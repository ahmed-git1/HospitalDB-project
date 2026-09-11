# HospitalDB
 
A small hospital management web app built for the CS340 (databases) course project. Patients book appointments and check their bills and prescriptions, doctors manage their schedule and write prescriptions, and admins manage users, billing and reports. all on top of a [Supabase](https://supabase.com) (PostgreSQL) database.
 
The app is plain HTML, CSS and JavaScript with the Supabase JS client loaded from a CDN. There is no build step and no backend: every page talks to the database directly through the query helpers in `js/sql.js`.
 
## Features
 
- **Patient** — view appointments, bills and prescriptions; book an appointment with any doctor.
- **Doctor** — view schedule; delete an appointment or change its time; create a prescription for a patient.
- **Admin** — delete a user by ID; create and delete bills; generate a report listing every table.
Login is by ID only (for example `P001`, `D001`, `AD001`) — the prefix decides the role. The registration page is a placeholder and isn't wired to the database.
 
## Database
 
Six tables. Deleting a patient cascades to their appointments, medications and bills; a doctor with appointments or prescriptions can't be deleted.
 
```mermaid
erDiagram
    PATIENTS ||--o{ APPOINTMENTS : books
    DOCTORS  ||--o{ APPOINTMENTS : handles
    PATIENTS ||--o{ MEDICATIONS  : "is prescribed"
    DOCTORS  ||--o{ MEDICATIONS  : prescribes
    PATIENTS ||--o{ BILLING      : "is billed"
 
    PATIENTS {
        varchar p_id PK
        varchar full_name
        date    date_of_birth
        varchar gender
        varchar email
        varchar contact_number
        text    medical_history
    }
    DOCTORS {
        varchar d_id PK
        varchar full_name
        varchar specialization
        text    availability_schedule
        varchar email
        varchar contact_number
    }
    ADMINS {
        varchar admin_id PK
        varchar full_name
        varchar position
        varchar contact_number
        varchar email
    }
    APPOINTMENTS {
        varchar a_id PK
        varchar p_id FK
        varchar d_id FK
        date    appointment_date
        time    appointment_time
        varchar status
    }
    MEDICATIONS {
        varchar m_id PK
        varchar p_id FK
        varchar d_id FK
        varchar medication_name
        varchar dosage
        date    prescription_date
    }
    BILLING {
        varchar bill_id PK
        varchar p_id FK
        decimal total_amount
        varchar payment_status
        date    issue_date
    }
```
