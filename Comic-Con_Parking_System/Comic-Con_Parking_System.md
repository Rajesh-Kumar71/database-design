title Comic-Con Parking System


    VEHICLE_CATEGORY [icon: car, color: blue]{
        int vehicle_category_id PK
        string category_name
        string size_class
        string description
    }

    ACCESS_CATEGORY [icon: access, color: blue]{
        int access_category_id PK
        string category_name
        boolean is_reserved_access
        int priority_rank
    }


     PARKING_ZONE [icon: parking, color: green]{
        int zone_id PK
        string zone_name
        string description
    }


    PARKING_LEVEL [icon: squarespace, color: green]{
        int level_id PK
        int zone_id FK
        string level_name
        int floor_number
    }
    PARKING_SPOT_CATEGORY [icon: building, color: green]{
        int spot_category_id PK
        string category_name
        boolean requires_charger
        string description
    }


    SPOT_VEHICLE_COMPATIBILITY [icon: git-merge, color: green]{
        int spot_category_id PK, FK
        int vehicle_category_id PK, FK
    }


    PARKING_SPOT [icon: parking, color: green]{
        int spot_id PK
        int level_id FK
        int spot_category_id FK
        int reserved_access_category_id FK
        string spot_code
        string availability_status
        boolean is_active
    }


    VEHICLE [icon: car, color: orange]{
        int vehicle_id PK
        int vehicle_category_id FK
        string plate_number UK
        string owner_name
        string contact_number
        string brand_model
    }


    RATE_PLAN [icon: receipt, color: blue]{
        int rate_plan_id PK
        int vehicle_category_id FK
        int access_category_id FK
        decimal base_fee
        decimal hourly_rate
        int grace_minutes
        decimal daily_cap
        date effective_from
        date effective_to
    }


    PARKING_SESSION [icon: clock, color: orange]{
        int session_id PK
        int vehicle_id FK
        int spot_id FK
        int access_category_id FK
        int rate_plan_id FK
        datetime entry_time
        datetime exit_time
        string session_status
        int parked_minutes
        decimal fee_amount
    }


    PARKING_TICKET [icon: ticket, color: orange]{
        int ticket_id PK
        int session_id FK
        string ticket_number UK
        datetime issue_time
        string qr_code
        string ticket_status
    }

    PAYMENT [icon: credit-card, color: purple]{
        int payment_id PK
        int session_id FK
        datetime payment_time
        decimal amount_paid
        string payment_method
        string payment_status
        string transaction_ref
    }

VEHICLE.vehicle_category_id > VEHICLE_CATEGORY.vehicle_category_id
RATE_PLAN.vehicle_category_id > VEHICLE_CATEGORY.vehicle_category_id
SPOT_VEHICLE_COMPATIBILITY.vehicle_category_id > VEHICLE_CATEGORY.vehicle_category_id

PARKING_SPOT.reserved_access_category_id > ACCESS_CATEGORY.access_category_id
PARKING_SESSION.access_category_id > ACCESS_CATEGORY.access_category_id
RATE_PLAN.access_category_id > ACCESS_CATEGORY.access_category_id

PARKING_LEVEL.zone_id > PARKING_ZONE.zone_id
PARKING_SPOT.level_id > PARKING_LEVEL.level_id

PARKING_SPOT.spot_category_id > PARKING_SPOT_CATEGORY.spot_category_id
SPOT_VEHICLE_COMPATIBILITY.spot_category_id > PARKING_SPOT_CATEGORY.spot_category_id

PARKING_SESSION.vehicle_id > VEHICLE.vehicle_id
PARKING_SESSION.spot_id > PARKING_SPOT.spot_id

PARKING_SESSION.rate_plan_id > RATE_PLAN.rate_plan_id
PARKING_TICKET.session_id - PARKING_SESSION.session_id
PAYMENT.session_id > PARKING_SESSION.session_id
