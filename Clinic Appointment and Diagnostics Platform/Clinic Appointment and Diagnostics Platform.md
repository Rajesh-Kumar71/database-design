title Clinic Appointment and Diagnostics Platform


    DEPARTMENT [icon: building, color: Teal]{
        int department_id PK
        string department_name
    }

    SPECIALTY [icon: shield, color: "#E0F2F1"]{
        int specialty_id PK
        string specialty_name
    }

    DOCTOR [icon: stethoscope, color: green]{
        int doctor_id PK
        int department_id FK
        string full_name
        string phone
        string email
        string license_no
        string room_no
        string status
    }

    DOCTOR_SPECIALTY [icon: shield, color: "#E0F2F1"]{
        int doctor_id PK, FK
        int specialty_id PK, FK
    }

    PATIENT [icon: user, color: blue]{
        int patient_id PK
        string full_name
        date dob
        string gender
        string phone
        string email
        string address
        datetime registered_at
    }

    APPOINTMENT [icon: calendar-check, color: yellow]{
        int appointment_id PK
        int patient_id FK
        int doctor_id FK
        datetime scheduled_at
        string status
        string reason
        string booked_channel
        datetime created_at
    }

    CONSULTATION [icon: clipboard-check, color: "#FCE4EC"]{
        int consultation_id PK
        int appointment_id FK
        int patient_id FK
        int doctor_id FK
        datetime consultation_at
        string visit_type
        string symptoms
        string diagnosis
        string notes
        string status
    }

    TEST_CATALOG [icon: list, color: pink]{
        int test_id PK
        string test_name
        string category
        decimal standard_fee
        string instructions
    }

    PRESCRIBED_TEST [icon: flask, color: "#EDE7F6"]{
        int prescribed_test_id PK
        int consultation_id FK
        int test_id FK
        datetime prescribed_at
        string priority
        string status
        string remarks
    }

    TEST_REPORT [icon: file-check, color: "#F3E5F5"]{
        int report_id PK
        int prescribed_test_id FK
        datetime generated_at
        string findings
        string report_file_url
        string status
    }

    BILL [icon: receipt, color: orange]{
        int bill_id PK
        int consultation_id FK
        int patient_id FK
        date bill_date
        decimal total_amount
        string bill_status
    }

    PAYMENT [icon: credit-card, color: "#FFE0B2"]{
        int payment_id PK
        int bill_id FK
        datetime paid_at
        decimal amount
        string payment_method
        string payment_status
        string transaction_ref
    }

PATIENT.patient_id > APPOINTMENT.patient_id: [color: orange]
DOCTOR.doctor_id > APPOINTMENT.doctor_id: [color: yellow]

PATIENT.patient_id > CONSULTATION.patient_id: [color: blue]
DOCTOR.doctor_id > CONSULTATION.doctor_id: [color: purple]
APPOINTMENT.appointment_id > CONSULTATION.appointment_id: [color: orange]

CONSULTATION.consultation_id > PRESCRIBED_TEST.consultation_id: [color: purple]
TEST_CATALOG.test_id > PRESCRIBED_TEST.test_id: [color: red]


PRESCRIBED_TEST.prescribed_test_id > TEST_REPORT.prescribed_test_id: [color: orange]

CONSULTATION.consultation_id > BILL.consultation_id: [color: yellow]
PATIENT.patient_id > BILL.patient_id: [color: blue]

BILL.bill_id > PAYMENT.bill_id: [color: green]

DEPARTMENT.department_id > DOCTOR.department_id: [color: orange]

DOCTOR.doctor_id > DOCTOR_SPECIALTY.doctor_id: [color: orange]
SPECIALTY.specialty_id > DOCTOR_SPECIALTY.specialty_id: [color: purple]