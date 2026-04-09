# Comic-Con India Event Parking System ER Diagram

## 1. Project Title
**Database Design for a Multi-Zone Event Parking System – Comic-Con India Convention Venue**

---

## 2. Business Understanding
Comic-Con India is a large multi-day convention where thousands of visitors arrive using different types of vehicles such as bikes, cars, SUVs, cabs, and EV vehicles.  
The venue provides a structured parking facility divided into multiple **zones** and **levels**. Some parking spots are specially reserved for **cosplayers with props, exhibitors, creators, VIP guests, staff members, and EV charging vehicles**.

The parking system must support the complete vehicle parking workflow:

- vehicle enters the venue
- a suitable parking spot is assigned
- a parking ticket is generated
- entry and exit timestamps are recorded
- parking fee is calculated
- payment is recorded
- spot availability is updated

The system should also support repeated visits by the same vehicle across multiple event days and reuse of the same parking spot across different parking sessions.

---

## 3. Objective of the ER Design
The purpose of this ER diagram is to model a parking system that can answer questions such as:

- What vehicles entered the parking facility?
- What type of vehicle entered?
- Which parking spot was assigned?
- Which zone and level does that parking spot belong to?
- Was the spot reserved for VIP, staff, exhibitor, cosplayer, or EV charging?
- When did the vehicle enter and exit?
- What parking ticket was issued?
- Can one vehicle visit multiple times?
- Can one parking spot be reused multiple times?
- How is availability tracked?
- How are parking charges and payment recorded?
- Which vehicles are currently parked inside the venue?

---

## 4. Main Entities in the Design

### 4.1 VEHICLE_CATEGORY
Stores the type of vehicle.

**Examples:**
- Bike
- Car
- SUV
- Cab
- EV

**Purpose:**  
Separates vehicle classification from individual vehicle records.

---

### 4.2 VEHICLE
Stores the actual vehicle details.

**Important attributes may include:**
- vehicle_id
- vehicle_category_id
- plate_number
- owner_name
- contact_number
- brand_model

**Purpose:**  
Represents each real vehicle entering the venue.

---

### 4.3 ACCESS_CATEGORY
Stores special access types used during the event.

**Examples:**
- General
- Cosplayer with Props
- Exhibitor
- Creator
- VIP
- Staff
- EV Charging

**Purpose:**  
Allows the system to represent special access or reserved parking categories without mixing them into vehicle data.

---

### 4.4 PARKING_ZONE
Stores major parking divisions of the venue.

**Examples:**
- Zone A
- Zone B
- Basement Zone
- Outdoor Zone

**Purpose:**  
Helps organize the facility into high-level parking areas.

---

### 4.5 PARKING_LEVEL
Stores parking levels/floors inside a zone.

**Important attributes may include:**
- level_id
- zone_id
- level_name
- floor_number

**Purpose:**  
Allows each zone to contain multiple parking levels.

---

### 4.6 PARKING_SPOT_CATEGORY
Stores the category/type of parking spot.

**Examples:**
- Bike Spot
- Standard Car Spot
- Large Vehicle Spot
- EV Charging Spot
- Oversized / Prop Vehicle Spot

**Purpose:**  
Helps define what kind of vehicle can use a particular spot.

---

### 4.7 SPOT_VEHICLE_COMPATIBILITY
A bridge entity between vehicle categories and parking spot categories.

**Purpose:**  
Shows which vehicle types are allowed in which parking spot categories.

**Example:**  
- Bike → Bike Spot  
- EV Car → EV Charging Spot  
- SUV → Large Vehicle Spot

---

### 4.8 PARKING_SPOT
Stores each individual parking spot.

**Important attributes may include:**
- spot_id
- level_id
- spot_category_id
- reserved_access_category_id
- spot_code
- availability_status
- is_active

**Purpose:**  
Represents the real parking spaces where vehicles are parked.

---

### 4.9 RATE_PLAN
Stores parking pricing rules.

**Important attributes may include:**
- rate_plan_id
- vehicle_category_id
- access_category_id
- base_fee
- hourly_rate
- grace_minutes
- daily_cap
- effective_from
- effective_to

**Purpose:**  
Supports fee calculation based on vehicle type and special access category.

---

### 4.10 PARKING_SESSION
Stores one complete visit of a vehicle into the parking facility.

**Important attributes may include:**
- session_id
- vehicle_id
- spot_id
- access_category_id
- rate_plan_id
- entry_time
- exit_time
- session_status
- parked_minutes
- fee_amount

**Purpose:**  
This is the main transaction entity that tracks entry, exit, assigned spot, and parking charges.

---

### 4.11 PARKING_TICKET
Stores the ticket issued for a parking session.

**Important attributes may include:**
- ticket_id
- session_id
- ticket_number
- issue_time
- qr_code
- ticket_status

**Purpose:**  
Represents the issued parking ticket separately from the parking session.

---

### 4.12 PAYMENT
Stores payment records related to parking sessions.

**Important attributes may include:**
- payment_id
- session_id
- payment_time
- amount_paid
- payment_method
- payment_status
- transaction_ref

**Purpose:**  
Tracks whether and how a parking session was paid.

---

## 5. Relationships in the ER Design

### Core Relationships
- One **vehicle category** can classify many **vehicles**
- One **vehicle** can have many **parking sessions**
- One **parking zone** can contain many **parking levels**
- One **parking level** can contain many **parking spots**
- One **parking spot category** can classify many **parking spots**
- One **parking spot** can be reused across many **parking sessions**
- One **access category** can be assigned to many **parking sessions**
- One **access category** can reserve many **parking spots**
- One **rate plan** can be used by many **parking sessions**
- One **parking session** generates one **parking ticket**
- One **parking session** can have one or more **payment records**

### Compatibility Relationships
- One **vehicle category** can appear in many **spot-vehicle compatibility** records
- One **parking spot category** can appear in many **spot-vehicle compatibility** records

This bridge structure helps enforce correct parking assignment.

## 6. Relationship Map

| Parent Entity | Child Entity | Relationship Type | Meaning |
|---------------|--------------|-------------------|---------|
| VEHICLE_CATEGORY | VEHICLE | One-to-Many | One vehicle category can classify many vehicles. |
| VEHICLE_CATEGORY | RATE_PLAN | One-to-Many | One vehicle category can have many rate plans. |
| VEHICLE_CATEGORY | SPOT_VEHICLE_COMPATIBILITY | One-to-Many | One vehicle category can appear in many compatibility rules. |
| ACCESS_CATEGORY | PARKING_SPOT | One-to-Many | One access category can reserve many parking spots. |
| ACCESS_CATEGORY | PARKING_SESSION | One-to-Many | One access category can be assigned to many parking sessions. |
| ACCESS_CATEGORY | RATE_PLAN | One-to-Many | One access category can affect many rate plans. |
| PARKING_ZONE | PARKING_LEVEL | One-to-Many | One parking zone can contain many parking levels. |
| PARKING_LEVEL | PARKING_SPOT | One-to-Many | One parking level can contain many parking spots. |
| PARKING_SPOT_CATEGORY | PARKING_SPOT | One-to-Many | One parking spot category can classify many parking spots. |
| PARKING_SPOT_CATEGORY | SPOT_VEHICLE_COMPATIBILITY | One-to-Many | One parking spot category can appear in many compatibility rules. |
| VEHICLE | PARKING_SESSION | One-to-Many | One vehicle can have many parking sessions across multiple days. |
| PARKING_SPOT | PARKING_SESSION | One-to-Many | One parking spot can be reused in many parking sessions over time. |
| RATE_PLAN | PARKING_SESSION | One-to-Many | One rate plan can be applied to many parking sessions. |
| PARKING_SESSION | PARKING_TICKET | One-to-One | One parking session generates one parking ticket. |
| PARKING_SESSION | PAYMENT | One-to-Many | One parking session can have one or more payment records. |

### Bridge / Compatibility Relationship
| Entity 1 | Entity 2 | Bridge Entity | Meaning |
|----------|----------|---------------|---------|
| VEHICLE_CATEGORY | PARKING_SPOT_CATEGORY | SPOT_VEHICLE_COMPATIBILITY | This bridge entity shows which vehicle categories are allowed in which parking spot categories. |
---

## 7. Important Business Rules Captured

1. **One vehicle can enter multiple times** across different days of the event.
2. **One parking spot can be reused** over time for different vehicles.
3. **Entry and exit timestamps are stored in parking sessions**.
4. **Ticket and session are modeled separately** for better design clarity.
5. **Vehicle type and reserved access category are not treated as the same thing**.
6. **Parking charges are based on a separate rate plan structure**.
7. **Payment is stored separately from session details**.
8. **Zone and level are modeled separately** to reflect actual venue parking layout.
9. **Availability can be tracked** through spot status and active session status.
10. **Currently parked vehicles can be identified** by sessions where exit time is NULL or session status is Active.

---

## 8. Why the Design is Normalized
This design avoids putting all details into one table.  
Instead, the system is divided into logical parts:

- **Master data**  
  Vehicle categories, access categories, spot categories

- **Parking structure**  
  Zones, levels, spots

- **Operational flow**  
  Sessions and tickets

- **Financial flow**  
  Rate plans and payments

- **Compatibility logic**  
  Spot-vehicle compatibility mapping

This makes the design more realistic, maintainable, and scalable.

---

## 9. Assumptions Used
The following assumptions were made while designing the ER diagram:

- a vehicle is identified by a unique plate number
- one parking session belongs to one vehicle
- one parking session uses one parking spot at a time
- one ticket is issued per parking session
- one parking spot may optionally be reserved for a special access category
- the same vehicle may come under different access categories on different visits
- one payment record belongs to one parking session
- parking availability is derived using spot status and active sessions
- different vehicle types may require different parking spot categories
- rate plans may vary by vehicle type and access category

---

## 10. How the Design Answers the Required Questions

| Question | Entity / Relationship Used |
|----------|-----------------------------|
| What vehicles entered the facility? | VEHICLE + PARKING_SESSION |
| What type of vehicle entered? | VEHICLE_CATEGORY |
| Which parking spot was assigned? | PARKING_SESSION → PARKING_SPOT |
| Which zone or level does that spot belong to? | PARKING_SPOT → PARKING_LEVEL → PARKING_ZONE |
| Was the spot reserved? | PARKING_SPOT → ACCESS_CATEGORY |
| When did the vehicle enter? | PARKING_SESSION.entry_time |
| When did the vehicle exit? | PARKING_SESSION.exit_time |
| What ticket was issued? | PARKING_TICKET |
| Can one vehicle visit multiple times? | VEHICLE → PARKING_SESSION |
| Can one spot be reused? | PARKING_SPOT → PARKING_SESSION |
| How is availability tracked? | PARKING_SPOT.availability_status + active sessions |
| How are charges calculated? | RATE_PLAN + PARKING_SESSION |
| How is payment recorded? | PAYMENT |
| Can special access categories be represented? | ACCESS_CATEGORY |
| Can currently parked vehicles be identified? | PARKING_SESSION where exit_time is NULL |

---

## 11. Final Justification
This ER design is suitable for a real multi-zone event parking system because it clearly separates:

- vehicle information
- parking location structure
- parking usage history
- ticket generation
- reservation/access handling
- pricing rules
- payment records

The design supports repeated visits, parking spot reuse, reserved categories, fee calculation, and real-time parking status tracking.  
Therefore, it is a practical and well-structured database design for the Comic-Con India event parking facility.

---

## 12. Conclusion
The final ER diagram successfully models the complete event parking workflow for a large convention venue.  
It captures all essential entities, relationships, and business rules needed to manage vehicle entry, spot allocation, reserved access, session tracking, ticket generation, payment handling, and availability monitoring in a clean and normalized way.