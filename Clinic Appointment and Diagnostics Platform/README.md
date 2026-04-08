# Clinic Appointment and Diagnostics Platform ER Diagram

## Project Overview
This project presents an Entity Relationship Diagram (ERD) for a modern clinic management system. The design focuses on the core clinic workflow: managing doctors, patients, appointments, consultations, prescribed diagnostic tests, generated reports, bills, and payments. The objective is to keep the system clean, practical, and scalable without turning it into a large hospital-level design.

## Business Scope
The clinic may have multiple doctors working across different departments and specialties. Patients can visit the clinic multiple times, either through booked appointments or follow-up visits. During a consultation, a doctor may prescribe one or more diagnostic tests. These tests may later generate reports, which are linked back to the original consultation. Billing and payment are also included so that the financial side of each visit can be tracked properly.

## Key Business Questions Supported
This ERD is designed to answer important clinic questions, such as:
- Who are the doctors and what are their specialties?
- Which patient booked which appointment?
- What is the appointment status?
- Did an appointment result in an actual consultation?
- Which diagnostic tests were prescribed during a consultation?
- What reports were generated for those tests?
- Can one patient have many visits over time?
- Can one doctor attend many patients?
- Can one consultation lead to multiple prescribed tests?
- How are bills and payments connected to consultations?

## Main Design Decisions
A clear distinction is made between **appointment** and **consultation**. An appointment represents a scheduled booking, while a consultation represents the actual doctor visit. This is important because not every appointment will result in a consultation; some may be cancelled or marked as no-show.

Diagnostic tests are linked to **consultations**, not directly to appointments, because tests are usually prescribed after the doctor sees the patient. Reports are linked to **prescribed tests**, which allows the system to track whether a prescribed test has produced a report yet.

Doctor specialty is modeled as a separate entity instead of a simple text attribute. A junction table is used to support the fact that one doctor can have multiple specialties and one specialty can belong to multiple doctors.

Payments are linked to **bills**, and bills are linked to **consultations**. This makes the billing structure cleaner and more realistic than attaching payments directly to appointments.

## Entities Included
The ERD contains the following entities:
- **DEPARTMENT** – stores clinic departments
- **SPECIALTY** – stores doctor specialties
- **DOCTOR** – stores doctor details
- **DOCTOR_SPECIALTY** – junction table between doctor and specialty
- **PATIENT** – stores patient information
- **APPOINTMENT** – stores booking details
- **CONSULTATION** – stores actual visit details
- **TEST_CATALOG** – master list of available diagnostic tests
- **PRESCRIBED_TEST** – stores tests prescribed during consultations
- **TEST_REPORT** – stores diagnostic reports generated later
- **BILL** – stores billing information
- **PAYMENT** – stores payment transactions

## Relationship Logic
The core relationships in the system are:
- One patient can book many appointments
- One doctor can receive many appointments
- One patient can have many consultations
- One doctor can conduct many consultations
- One appointment may or may not result in one consultation
- One consultation can prescribe many tests
- One test from the catalog can be prescribed many times
- One prescribed test may later generate one report
- One consultation may generate one bill
- One bill can have many payments
- One department can have many doctors
- Doctors and specialties have a many-to-many relationship through `DOCTOR_SPECIALTY`

## Relationship Map

| Parent Entity | Child Entity | Relationship | Foreign Key |
|---|---|---|---|
| PATIENT | APPOINTMENT | One patient can book many appointments | `APPOINTMENT.patient_id` |
| DOCTOR | APPOINTMENT | One doctor can receive many appointments | `APPOINTMENT.doctor_id` |
| PATIENT | CONSULTATION | One patient can have many consultations | `CONSULTATION.patient_id` |
| DOCTOR | CONSULTATION | One doctor can conduct many consultations | `CONSULTATION.doctor_id` |
| APPOINTMENT | CONSULTATION | One appointment may or may not result in one consultation | `CONSULTATION.appointment_id` |
| CONSULTATION | PRESCRIBED_TEST | One consultation can prescribe many tests | `PRESCRIBED_TEST.consultation_id` |
| TEST_CATALOG | PRESCRIBED_TEST | One test type can be prescribed many times | `PRESCRIBED_TEST.test_id` |
| PRESCRIBED_TEST | TEST_REPORT | One prescribed test may generate one report | `TEST_REPORT.prescribed_test_id` |
| CONSULTATION | BILL | One consultation may generate one bill | `BILL.consultation_id` |
| PATIENT | BILL | One patient can have many bills | `BILL.patient_id` |
| BILL | PAYMENT | One bill can have many payments | `PAYMENT.bill_id` |
| DEPARTMENT | DOCTOR | One department can have many doctors | `DOCTOR.department_id` |
| DOCTOR | DOCTOR_SPECIALTY | One doctor can map to many specialty records | `DOCTOR_SPECIALTY.doctor_id` |
| SPECIALTY | DOCTOR_SPECIALTY | One specialty can map to many doctor records | `DOCTOR_SPECIALTY.specialty_id` |

## Primary Key and Foreign Key Design
Each entity uses a primary key for unique identification. Foreign keys are used to connect related tables and maintain referential integrity. For example:
- `APPOINTMENT.patient_id` links to `PATIENT.patient_id`
- `APPOINTMENT.doctor_id` links to `DOCTOR.doctor_id`
- `CONSULTATION.appointment_id` links to `APPOINTMENT.appointment_id`
- `PRESCRIBED_TEST.consultation_id` links to `CONSULTATION.consultation_id`
- `TEST_REPORT.prescribed_test_id` links to `PRESCRIBED_TEST.prescribed_test_id`
- `BILL.consultation_id` links to `CONSULTATION.consultation_id`
- `PAYMENT.bill_id` links to `BILL.bill_id`

## Workflow Covered by the ERD
The overall clinic flow represented by this design is:
Patient registers → books appointment with doctor → attends consultation → doctor records diagnosis → doctor prescribes tests if needed → test reports are generated later → bill is created → payment is collected.

## Why This Design Is Suitable
This design is appropriate for a clinic because it is focused, readable, and practical. It avoids unnecessary hospital-level complexity while still supporting scheduling, visits, diagnostics, reporting, and billing. It also separates master data, transactional data, and financial data properly, making the model easier to extend in the future.

## Assumptions
- A consultation may exist only if a patient actually visits the doctor
- An appointment may be cancelled or may not result in a consultation
- A consultation can prescribe zero, one, or many tests
- A report is generated only after a prescribed test is processed
- Billing is tied to consultation activity rather than just booking
- A bill may be paid fully or in multiple payment transactions

## Conclusion
This ER diagram provides a clean and scalable database design for a clinic appointment and diagnostics platform. It successfully models patient visits, doctor consultations, test prescriptions, report generation, and payment tracking in a structured and realistic way.