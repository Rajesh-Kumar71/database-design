title LiftGrid Systems - Smart Elevator Control Platform ERD

BUILDING [icon: building, color: blue] {
        int building_id PK
        string building_name
        string building_type
        string address
        string city
        string state
        boolean is_active
    }


FLOOR [icon: layers, color: blue] {
        int floor_id PK
        int building_id FK
        int floor_number
        string floor_code
        string usage_type
    }


ELEVATOR_SHAFT [icon: layout, color: blue] {
        int shaft_id PK
        int building_id FK
        string shaft_code
        string zone_name
        boolean is_active
    }
ELEVATOR [icon: move, color: blue] {
        int elevator_id PK
        int building_id FK
        int shaft_id FK
        string elevator_code
        int capacity_persons
        decimal capacity_kg
        date installation_date
        boolean is_enabled
    }


ELEVATOR_FLOOR_SERVICE [icon: link, color: green] {
        int service_id PK
        int elevator_id FK
        int floor_id FK
        boolean can_stop
        boolean is_priority_stop
    }


FLOOR_REQUEST [icon: phone-call, color: green] {
        bigint request_id PK
        int building_id FK
        int request_floor_id FK
        int destination_floor_id FK
        string request_type
        string direction
        datetime requested_at
        string request_status
    }


RIDE_ASSIGNMENT [icon: check-circle, color: green] {
        bigint assignment_id PK
        bigint request_id FK
        int elevator_id FK
        datetime assigned_at
        string assignment_status
    }

RIDE_TRIP [icon: clock, color: green] {
        bigint trip_id PK
        bigint assignment_id FK
        int elevator_id FK
        int start_floor_id FK
        int end_floor_id FK
        datetime trip_start_time
        datetime trip_end_time
        string trip_status
        int passenger_count_estimate
    }


ELEVATOR_STATUS_LOG [icon: activity, color: orange] {
        bigint status_log_id PK
        int elevator_id FK
        string status_code
        int current_floor_id FK
        string direction
        datetime recorded_at
    }


ELEVATOR_MAINTENANCE [icon: tool, color: red] {
        bigint maintenance_id PK
        int elevator_id FK
        string maintenance_type
        datetime start_time
        datetime end_time
        string maintenance_status
        string vendor_name
        string notes
    }
FLOOR.building_id > BUILDING.building_id
ELEVATOR_SHAFT.building_id > BUILDING.building_id
ELEVATOR.building_id > BUILDING.building_id
ELEVATOR.shaft_id - ELEVATOR_SHAFT.shaft_id

ELEVATOR_FLOOR_SERVICE.elevator_id > ELEVATOR.elevator_id
ELEVATOR_FLOOR_SERVICE.floor_id > FLOOR.floor_id

FLOOR_REQUEST.building_id > BUILDING.building_id
FLOOR_REQUEST.request_floor_id > FLOOR.floor_id
FLOOR_REQUEST.destination_floor_id > FLOOR.floor_id

RIDE_ASSIGNMENT.request_id - FLOOR_REQUEST.request_id
RIDE_ASSIGNMENT.elevator_id > ELEVATOR.elevator_id

RIDE_TRIP.assignment_id - RIDE_ASSIGNMENT.assignment_id
RIDE_TRIP.elevator_id > ELEVATOR.elevator_id
RIDE_TRIP.start_floor_id > FLOOR.floor_id
RIDE_TRIP.end_floor_id > FLOOR.floor_id

ELEVATOR_STATUS_LOG.elevator_id > ELEVATOR.elevator_id
ELEVATOR_STATUS_LOG.current_floor_id > FLOOR.floor_id

ELEVATOR_MAINTENANCE.elevator_id > ELEVATOR.elevator_id
