# Online Fitness Coaching Platform — ER Diagram

This project contains a color-coded ER diagram for an online fitness coaching platform.

## Purpose
The design models an online coaching system where trainers manage clients, offer plans, schedule consultations and live sessions, track progress, record check-ins, and manage payments.

## Main Entities
- CLIENT
- TRAINER
- TRAINER_CLIENT
- PLAN
- SUBSCRIPTION
- CONSULTATION
- LIVE_SESSION
- SESSION_ATTENDANCE
- CHECK_IN
- PROGRESS_LOG
- TRAINER_NOTE
- SPAYMENT

## Key Business Rules
- One trainer can coach many clients.
- A client can buy more than one plan over time.
- One plan can be subscribed to by many clients.
- Consultations and check-ins are stored separately.
- Progress tracking is stored separately from the client profile.
- Payments can be linked to subscriptions or consultations.

## Files
- `Fitness_Influencer_Coaching_Platform.png`
- `Fitness_Influencer_Coaching_Platform.md`
- `README.md`

## Color Scheme
- **Blue** — CLIENT, TRAINER, TRAINER_CLIENT
- **Green** — PLAN
- **Purple** — SUBSCRIPTION, SPAYMENT
- **Coral** — CONSULTATION, LIVE_SESSION, SESSION_ATTENDANCE
- **Amber** — CHECK_IN, PROGRESS_LOG, TRAINER_NOTE
