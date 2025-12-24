# Planora – Event Management System (Directus)

Planora is an event management backend built using **Directus**.  
It demonstrates data modeling, permissions, business logic enforcement using Flows, and admin UI customization.

This project was created as part of a technical assessment to showcase practical usage of Directus as a headless CMS / backend platform.

## 🚀 Setup Instructions

### Prerequisites

- Docker
- Docker Compose
- Git

---

### Installation Steps

```bash
git clone https://github.com/InsafNilam/planora.git
cd planora
cp .env.example .env
docker compose up -d
```

This will start:

- Directus Admin & API
- Database services

---

### Access Directus Admin

```
http://localhost:8055
```

**Admin Credentials**

```
Email:    admin@example.com
Password: password
```

## 🗂️ ERD Diagram (Data Model Overview)

### Core Entities

- **events**
- **categories**
- **registrations**
- **events_categories** (junction table)

### Relationships

```
events ────< registrations
events ────< events_categories >──── categories
```

- One event can have many registrations
- One event can belong to many categories
- One category can contain many events

## 🧱 Data Model Explanation

### events

Stores all event-related information such as date, location, capacity, and status.

Key fields:

- `capacity`: Maximum allowed registrations
- `spots_remaining`: Auto-calculated field
- `status`: draft / published / cancelled

---

### categories

Used to classify events (Technology, Workshop, etc.).

Designed as a separate collection to allow:

- Reusability
- Easy filtering
- Many-to-Many relationship with events

---

### registrations

Represents a user registering for an event.

Each registration belongs to:

- One event
- (Optionally) one user

---

### events_categories (Junction Table)

Implements the Many-to-Many relationship between events and categories.

Why a junction table?

- Normalized schema
- Extensible (can add priority, metadata later)

## 🔐 Permissions Explanation

### Admin Role

- Full access to all collections
- Can manage events, categories, registrations

---

### Editor / Manager Role (if enabled)

- Create and update events
- Assign categories
- View registrations

---

### Public / API Role

- Read-only access to published events
- Can create registrations

Permissions were configured to:

- Prevent unauthorized writes
- Enforce business rules at backend level
- Keep admin UI safe and simple

## ⚙️ Flows Explanation

### 1️⃣ Prevent Event Overbooking

**Trigger**

- Before registration create (blocking)

**Logic**

- Count existing registrations for the event
- Compare with event capacity
- Reject if capacity is reached

**Result**

- Ensures no event exceeds its allowed capacity

### 2️⃣ Auto-Calculate `spots_remaining`

Triggered when:

- A registration is created
- A registration is deleted
- Event capacity is updated

**Formula**

```
spots_remaining = capacity - total_registrations
```

Why this approach?

- Single source of truth
- No counter drift
- Safe against deletes and updates

## 🧠 Challenges Faced

Initially, I didn’t have hands-on experience working with tools like Directus.
However, I had some conceptual understanding from platforms such as **Bubble.io** and **Payload CMS**.

I approached this project with a **learning-by-doing mindset**:

- Explored Directus data modeling deeply
- Understood how Flows work for real business logic
- Faced some difficulty implementing blocking logic at first, especially capacity checks
- Eventually figured out how to structure flows correctly using triggers, read operations, and scripts

Once the core concepts clicked, everything became smooth and intuitive.

Overall, it was a great learning experience, and I now feel confident working with Directus in real-world scenarios.

---

## ✅ Final Notes

- Business rules are enforced at backend level
- Schema is scalable and normalized
- Admin UI is customized for real users
- Flows ensure data integrity

---

**Author:** Insaf Nilam
