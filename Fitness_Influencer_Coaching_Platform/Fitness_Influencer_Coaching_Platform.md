
title Fitness Influencer Coaching Platform

TRAINER  [icon: user-check, color: blue] {
        int trainer_id PK 
        string full_name
        string brand_name
        string email
        string phone
        string specialization
        string bio
        date join_date
        string status
    }


CLIENT [icon: user, color: blue ] {
        int client_id PK
        string full_name
        string email
        string phone
        string gender
        date date_of_birth
        string fitness_goal
        string activity_level
        date join_date
        string status
    }



TRAINER_CLIENT [icon: link, color: blue] {
   
        int assignment_id PK
        int trainer_id FK 
        int client_id FK
        date assigned_date
        string role
        boolean active_flag
    }

PLAN  [icon: 	clipboard-list, color: green] {
  
        int plan_id PK
        int trainer_id FK
        string plan_name
        string plan_type
        int duration_weeks
        decimal price
        string description
        boolean includes_workout
        boolean includes_diet
        boolean includes_live_sessions
        string status
    }


SUBSCRIPTION  [icon: refresh-cw, color: purple] {
  
        int subscription_id PK
        int client_id FK
        int plan_id FK
        date start_date
        date end_date
        string billing_cycle
        decimal amount_paid
        boolean auto_renew
        string subscription_status
    }


CONSULTATION  [icon: video, color: coral] {
    
        int consultation_id PK
        int client_id FK
        int trainer_id FK
        datetime scheduled_datetime
        string meeting_link
        string consultation_type
        string status
        string summary_notes
    }


LIVE_SESSION [icon: play-circle, color: coral] {
    
        int session_id PK
        int trainer_id FK
        string title
        datetime session_datetime
        int duration_minutes
        string meeting_link
        int max_participants
        string status
    }

SESSION_ATTENDANCE  [icon: check-square, color: coral] {
    
        int attendance_id PK
        int session_id FK
        int client_id FK
        string attendance_status
        datetime joined_at
    }


CHECK_IN [icon: activity, color: amber] {
    
        int checkin_id PK
        int client_id FK
        int trainer_id FK
        date checkin_date
        int energy_level
        int sleep_quality
        int diet_adherence
        int workout_adherence
        string mood
        string comments
        string next_action
    }


PROGRESS_LOG [icon: trending-up, color: amber] {
     
        int progress_id PK
        int client_id FK
        date log_date
        decimal weight
        decimal body_fat_percent
        decimal chest
        decimal waist
        decimal hips
        decimal arms
        decimal thighs
        string progress_photos_url
        string remarks
    }


TRAINER_NOTE [icon: file-text, color: amber] {
    
        int note_id PK
        int client_id FK
        int trainer_id FK
        date note_date
        string note_text
    }


SPAYMENT [icon: credit-card, color: purple] {
    
        int payment_id PK
        int client_id FK
        int subscription_id FK
        int consultation_id FK
        decimal amount
        date payment_date
        string payment_method
        string transaction_ref
        string payment_status
    }



TRAINER_CLIENT.trainer_id > TRAINER.trainer_id 
TRAINER_CLIENT.client_id > CLIENT.client_id
PLAN.trainer_id > TRAINER.trainer_id
SUBSCRIPTION.client_id > CLIENT.client_id
SUBSCRIPTION.plan_id > PLAN.plan_id
CONSULTATION.client_id > CLIENT.client_id
CONSULTATION.trainer_id > TRAINER.trainer_id
LIVE_SESSION.trainer_id > TRAINER.trainer_id
SESSION_ATTENDANCE.session_id > LIVE_SESSION.session_id
SESSION_ATTENDANCE.client_id > CLIENT.client_id
CHECK_IN.client_id > CLIENT.client_id
CHECK_IN.trainer_id > TRAINER.trainer_id
PROGRESS_LOG.client_id > CLIENT.client_id
TRAINER_NOTE.client_id > CLIENT.client_id
TRAINER_NOTE.trainer_id > TRAINER.trainer_id
SPAYMENT.client_id > CLIENT.client_id
SPAYMENT.subscription_id > SUBSCRIPTION.subscription_id
SPAYMENT.consultation_id > CONSULTATION.consultation_id


