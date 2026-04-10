# LiftGrid Systems - Smart Elevator Control Platform ER Diagram

## 1. Project Overview

This project is a database design for **LiftGrid Systems**, a fast-growing infrastructure technology company that builds intelligent elevator control platforms for large commercial buildings across India.

The platform is used in:

- Corporate towers
- Shopping malls
- Airports
- Hospitals
- High-rise residential complexes

Unlike a simple standalone lift system, this is a **multi-building, multi-elevator control platform** where many elevators operate together inside a building and handle large volumes of floor requests every day.

The purpose of this ER design is to support:

- building-wise elevator management
- floor-level request tracking
- ride allocation to elevators
- usage and ride history logging
- elevator status monitoring
- maintenance history tracking

---

## 2. Business Understanding

The system is designed for a real-world elevator infrastructure environment where:

- one platform manages **multiple buildings**
- one building contains **multiple floors**
- one building contains **multiple elevator shafts**
- one shaft contains **one elevator**
- one elevator can serve **multiple floors**
- one floor can be served by **multiple elevators**
- users generate requests from floors
- requests are assigned to elevators
- rides are logged for operational analytics
- maintenance history must be preserved without overwriting old records
- elevator status should be tracked separately from elevator configuration

This means the database should not store everything in one big table.  
Instead, it should separate:

- **master/configuration data**
- **transactional/request data**
- **history/log data**

---

## 3. Objective of the ER Diagram

The ER diagram is designed to answer questions such as:

- How many buildings are connected to the platform?
- How many elevators exist inside each building?
- Which floors belong to which building?
- Which elevator serves which floors?
- Which floor requests were generated and when?
- Which elevator handled each request?
- Which requests are still pending?
- What is the current or recent status of each elevator?
- How many rides did an elevator complete today?
- Which elevator handled the most rides?
- Can an elevator be disabled temporarily for maintenance?
- Can maintenance history be tracked separately?
- Can ride logs be stored for future analytics?

---

## 4. Main Entities in the Design

The design includes the following major entities:

### 4.1 BUILDING
Stores the details of each building connected to the LiftGrid platform.

**Purpose:**  
To identify and manage each property separately.

**Important attributes:**
- `building_id` (Primary Key)
- `building_name`
- `building_type`
- `address`
- `city`
- `state`
- `is_active`

---

### 4.2 FLOOR
Stores the floors inside a building.

**Purpose:**  
To define all valid floors available in a building.

**Important attributes:**
- `floor_id` (Primary Key)
- `building_id` (Foreign Key)
- `floor_number`
- `floor_code`
- `usage_type`
- `is_active`

**Business rule:**  
One building can have many floors.

---

### 4.3 ELEVATOR_SHAFT
Stores physical shaft information in a building.

**Purpose:**  
To represent the real physical shaft in which an elevator is installed.

**Important attributes:**
- `shaft_id` (Primary Key)
- `building_id` (Foreign Key)
- `shaft_code`
- `zone_name`
- `is_active`

**Business rule:**  
One building can have many shafts.

---

### 4.4 ELEVATOR
Stores elevator master data.

**Purpose:**  
To represent each elevator as a configurable asset.

**Important attributes:**
- `elevator_id` (Primary Key)
- `building_id` (Foreign Key)
- `shaft_id` (Foreign Key)
- `elevator_code`
- `capacity_persons`
- `capacity_kg`
- `installation_date`
- `is_enabled`

**Important note:**  
Dynamic ride information and status history are not stored here.  
This table should only contain stable elevator configuration details.

---

### 4.5 ELEVATOR_FLOOR_SERVICE
Bridge table between elevators and floors.

**Purpose:**  
To solve the many-to-many relationship between elevators and floors.

**Why needed?**
- one elevator can serve multiple floors
- one floor can be served by multiple elevators

**Important attributes:**
- `service_id` (Primary Key)
- `elevator_id` (Foreign Key)
- `floor_id` (Foreign Key)
- `can_stop`
- `is_priority_stop`

---

### 4.6 FLOOR_REQUEST
Stores requests raised by users from floors.

**Purpose:**  
To track all ride requests generated in the system.

**Important attributes:**
- `request_id` (Primary Key)
- `building_id` (Foreign Key)
- `request_floor_id` (Foreign Key)
- `destination_floor_id` (Foreign Key)
- `request_type`
- `direction`
- `requested_at`
- `request_status`

**Examples of request status:**
- pending
- assigned
- cancelled
- completed

---

### 4.7 RIDE_ASSIGNMENT
Stores which elevator was assigned to handle a request.

**Purpose:**  
To separate request generation from request handling.

**Important attributes:**
- `assignment_id` (Primary Key)
- `request_id` (Foreign Key)
- `elevator_id` (Foreign Key)
- `assigned_at`
- `assignment_status`

**Examples of assignment status:**
- assigned
- accepted
- rejected
- reassigned

---

### 4.8 RIDE_TRIP
Stores completed or ongoing elevator trip logs.

**Purpose:**  
To keep ride execution history for analytics and reporting.

**Important attributes:**
- `trip_id` (Primary Key)
- `assignment_id` (Foreign Key)
- `elevator_id` (Foreign Key)
- `start_floor_id` (Foreign Key)
- `end_floor_id` (Foreign Key)
- `trip_start_time`
- `trip_end_time`
- `trip_status`
- `passenger_count_estimate`

This table is useful for:
- daily ride count
- elevator usage analysis
- high-demand floors
- response and travel analysis

---

### 4.9 ELEVATOR_STATUS_LOG
Stores time-based status records of elevators.

**Purpose:**  
To track status changes over time without overwriting previous values.

**Important attributes:**
- `status_log_id` (Primary Key)
- `elevator_id` (Foreign Key)
- `current_floor_id` (Foreign Key)
- `status_code`
- `direction`
- `recorded_at`

**Examples of status code:**
- idle
- moving_up
- moving_down
- door_open
- maintenance
- disabled

---

### 4.10 ELEVATOR_MAINTENANCE
Stores maintenance and inspection history of elevators.

**Purpose:**  
To preserve all maintenance records for operational safety and audit tracking.

**Important attributes:**
- `maintenance_id` (Primary Key)
- `elevator_id` (Foreign Key)
- `maintenance_type`
- `start_time`
- `end_time`
- `maintenance_status`
- `vendor_name`
- `notes`

**Examples of maintenance type:**
- scheduled
- emergency
- inspection
- repair

---

## 5. Why This Design Is Correct

This design is strong because it separates different kinds of data properly.

### 5.1 Configuration Data
These tables describe the permanent structure of the system:
- BUILDING
- FLOOR
- ELEVATOR_SHAFT
- ELEVATOR

### 5.2 Service Mapping Data
This table defines elevator-floor coverage:
- ELEVATOR_FLOOR_SERVICE

### 5.3 Request / Transaction Data
These tables capture operational flow:
- FLOOR_REQUEST
- RIDE_ASSIGNMENT
- RIDE_TRIP

### 5.4 Historical / Time-Based Data
These tables preserve logs over time:
- ELEVATOR_STATUS_LOG
- ELEVATOR_MAINTENANCE

This avoids bad design practices such as:
- storing ride counts directly in elevator table
- storing maintenance remarks inside elevator table
- overwriting status instead of keeping history
- linking floors and elevators with only one foreign key when the relationship is many-to-many

---

## 6. Relation Map (Tabular Form)

| Parent Entity | Child Entity | Relationship Type | Foreign Key / Link | Meaning |
|---|---|---|---|---|
| BUILDING | FLOOR | One-to-Many | `FLOOR.building_id -> BUILDING.building_id` | One building contains many floors |
| BUILDING | ELEVATOR_SHAFT | One-to-Many | `ELEVATOR_SHAFT.building_id -> BUILDING.building_id` | One building contains many elevator shafts |
| BUILDING | ELEVATOR | One-to-Many | `ELEVATOR.building_id -> BUILDING.building_id` | One building contains many elevators |
| ELEVATOR_SHAFT | ELEVATOR | One-to-One / One-to-Many by implementation | `ELEVATOR.shaft_id -> ELEVATOR_SHAFT.shaft_id` | One shaft houses one elevator |
| ELEVATOR | ELEVATOR_FLOOR_SERVICE | One-to-Many | `ELEVATOR_FLOOR_SERVICE.elevator_id -> ELEVATOR.elevator_id` | One elevator can serve many floors |
| FLOOR | ELEVATOR_FLOOR_SERVICE | One-to-Many | `ELEVATOR_FLOOR_SERVICE.floor_id -> FLOOR.floor_id` | One floor can be served by many elevators |
| BUILDING | FLOOR_REQUEST | One-to-Many | `FLOOR_REQUEST.building_id -> BUILDING.building_id` | Requests belong to a building |
| FLOOR | FLOOR_REQUEST | One-to-Many | `FLOOR_REQUEST.request_floor_id -> FLOOR.floor_id` | Requests are generated from a floor |
| FLOOR | FLOOR_REQUEST | One-to-Many | `FLOOR_REQUEST.destination_floor_id -> FLOOR.floor_id` | Requests may target a destination floor |
| FLOOR_REQUEST | RIDE_ASSIGNMENT | One-to-One / One-to-Many by policy | `RIDE_ASSIGNMENT.request_id -> FLOOR_REQUEST.request_id` | A request gets assigned to an elevator |
| ELEVATOR | RIDE_ASSIGNMENT | One-to-Many | `RIDE_ASSIGNMENT.elevator_id -> ELEVATOR.elevator_id` | One elevator can handle many requests |
| RIDE_ASSIGNMENT | RIDE_TRIP | One-to-One / One-to-Many by implementation | `RIDE_TRIP.assignment_id -> RIDE_ASSIGNMENT.assignment_id` | One assignment creates a trip record |
| ELEVATOR | RIDE_TRIP | One-to-Many | `RIDE_TRIP.elevator_id -> ELEVATOR.elevator_id` | One elevator completes many trips |
| FLOOR | RIDE_TRIP | One-to-Many | `RIDE_TRIP.start_floor_id -> FLOOR.floor_id` | Trip starts from a floor |
| FLOOR | RIDE_TRIP | One-to-Many | `RIDE_TRIP.end_floor_id -> FLOOR.floor_id` | Trip ends at a floor |
| ELEVATOR | ELEVATOR_STATUS_LOG | One-to-Many | `ELEVATOR_STATUS_LOG.elevator_id -> ELEVATOR.elevator_id` | One elevator has many status records |
| FLOOR | ELEVATOR_STATUS_LOG | One-to-Many | `ELEVATOR_STATUS_LOG.current_floor_id -> FLOOR.floor_id` | Status may reference elevator’s current floor |
| ELEVATOR | ELEVATOR_MAINTENANCE | One-to-Many | `ELEVATOR_MAINTENANCE.elevator_id -> ELEVATOR.elevator_id` | One elevator can have many maintenance records |

---

## 7. Key Relationship Logic

### 7.1 Building to Floor
A building contains many floors, so this is a **one-to-many** relationship.

### 7.2 Building to Elevator
A building contains many elevators, so this is also **one-to-many**.

### 7.3 Elevator to Floor
This is **many-to-many**, because:
- one elevator serves many floors
- one floor can be served by many elevators

This is resolved using the bridge table **ELEVATOR_FLOOR_SERVICE**.

### 7.4 Floor Request to Elevator
A request is first generated, then allocated to an elevator using **RIDE_ASSIGNMENT**.

This keeps the system more realistic because:
- requests can be created before assignment
- requests can remain pending
- requests can be reassigned if needed

### 7.5 Assignment to Trip
The system separates **allocation** from **actual ride execution**.  
This is useful because an elevator may be assigned, but the ride may still be incomplete, interrupted, or logged later.

### 7.6 Elevator to Maintenance
This is **one-to-many**, because one elevator can go through many maintenance events over time.

### 7.7 Elevator to Status Log
This is **one-to-many**, because one elevator’s status changes many times during the day.

---

## 8. Design Assumptions

The following assumptions were used while designing the system:

1. Each building may contain multiple elevator shafts.
2. Each shaft contains one elevator.
3. Each elevator belongs to one building.
4. Each floor belongs to one building only.
5. A floor request is generated from one floor at a time.
6. A request may optionally include a destination floor.
7. Multiple elevators can serve the same floor.
8. One elevator can serve multiple floors.
9. Elevator status is time-based and should be logged separately.
10. Maintenance records should not overwrite historical records.
11. Ride history should be stored separately from elevator master data.
12. The system is intended for operational monitoring and future analytics.

---

## 9. Normalization and Data Quality

This design follows good normalization practices:

### First Normal Form (1NF)
- no repeating groups
- no multi-valued columns

### Second Normal Form (2NF)
- attributes depend on the full primary key
- many-to-many relationship resolved using bridge table

### Third Normal Form (3NF)
- non-key attributes depend only on their own entity
- status, maintenance, and rides are not mixed into elevator master table

This improves:
- data consistency
- easier updates
- lower redundancy
- better reporting

---

## 10. Queries This Design Can Support

This ERD supports important business queries such as:

- total number of buildings
- number of elevators in each building
- total floors in each building
- elevators serving a particular floor
- floors served by a particular elevator
- pending requests
- completed requests
- ride counts per elevator
- daily usage pattern of elevators
- elevator with maximum handled requests
- current and past elevator status tracking
- maintenance history of an elevator
- elevators currently under maintenance
- trip analytics for performance monitoring

---

## 11. Important Design Decisions

### Decision 1: Separate configuration from history
The elevator table contains only permanent details.  
Dynamic records like rides and status logs are stored in separate tables.

### Decision 2: Use bridge table for elevator-floor coverage
Directly storing one floor inside elevator or one elevator inside floor would be incorrect because the relationship is many-to-many.

### Decision 3: Keep requests, assignments, and trips separate
This reflects real-life elevator workflow:
1. request generated
2. elevator assigned
3. ride completed/logged

### Decision 4: Keep maintenance as historical data
Maintenance should not overwrite current elevator details.  
Separate maintenance records allow a full audit trail.

---

## 12. Final Justification

This ER diagram is suitable for a real smart elevator control platform because it:

- models a **multi-building environment**
- handles **multiple elevators per building**
- correctly supports **many-to-many elevator-floor service mapping**
- captures **floor requests and elevator allocation**
- stores **ride logs for future analytics**
- tracks **time-based status changes**
- preserves **maintenance history**
- avoids mixing static and dynamic data
- supports both operational monitoring and management reporting

Overall, this design is practical, scalable, and logically aligned with how intelligent elevator systems work in large commercial infrastructure environments.

---

## 13. Conclusion

The proposed ER diagram for LiftGrid Systems provides a well-structured database foundation for managing elevator infrastructure across multiple buildings.

It supports:

- operational efficiency
- maintenance visibility
- request and ride tracking
- status monitoring
- future performance analytics

The model is normalized, scalable, and suitable for a modern infrastructure technology platform handling large volumes of elevator activity daily.