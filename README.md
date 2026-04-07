# 🏋️ Fitness Influencer Coaching Platform — ER Diagram

> Database design for an online fitness coaching ecosystem where trainers manage clients, sell programs, schedule sessions, and track progress.

---

## 📌 Project Overview

A fitness influencer has grown their business from Instagram DMs and video calls into a full online coaching platform. This database design supports the entire coaching lifecycle:

- Onboarding clients and trainers
- Selling and managing fitness programs
- Scheduling 1:1 consultation sessions
- Delivering personalized diet and workout plans
- Tracking client progress and weekly check-ins
- Managing payments and subscriptions

---

## 🗂️ Entity Summary

The design consists of **13 entities** organized into 5 functional groups:

### 👤 User & Role Entities
| Entity | Description |
|--------|-------------|
| `users` | Base entity storing common info for all platform users |
| `client` | Client-specific profile data (age, gender, height, food preference) |
| `trainer` | Trainer-specific data (experience, bio, achievements) |

### 📦 Program & Subscription Entities
| Entity | Description |
|--------|-------------|
| `programs` | Fitness programs/packages created by trainers |
| `program_subscriptions` | Tracks which client bought which program, with start/end dates and status |
| `payments` | Payment records for program purchases |

### 🥗 Coaching Content Entities
| Entity | Description |
|--------|-------------|
| `diet_plan` | Personalized diet plans assigned to clients |
| `workout_plan` | Weekly workout schedule assigned to clients |
| `workout_exercises` | Individual exercises within a workout plan (sets, reps, rest) |

### 📅 Sessions & Communication
| Entity | Description |
|--------|-------------|
| `sessions` | Scheduled 1:1 video consultations between trainer and client |
| `trainer_notes` | Trainer feedback, form corrections, and plan adjustments per client |

### 📊 Progress & Check-in Tracking
| Entity | Description |
|--------|-------------|
| `check_in` | Weekly subjective report submitted by the client (sleep, mood, energy, diet adherence) |
| `progress_tracking` | Objective body measurement records over time (weight, chest, waist, hips, etc.) |

---

## 🔗 Key Relationships & Cardinality

| Relationship | Cardinality | Notes |
|---|---|---|
| `users` → `client` | 1 : 1 | One user has one client profile |
| `users` → `trainer` | 1 : 1 | One user has one trainer profile |
| `trainer` → `programs` | 1 : Many | One trainer can create many programs |
| `programs` → `program_subscriptions` | 1 : Many | One program can be purchased by many clients |
| `client` → `program_subscriptions` | 1 : Many | One client can buy many programs over time |
| `client` → `sessions` | 1 : Many | One client can have many sessions |
| `programs` → `sessions` | 1 : Many | One program can have many sessions scheduled |
| `client` → `diet_plan` | 1 : Many | Each client gets personalized diets over time |
| `client` → `workout_plan` | 1 : Many | Each client gets personalized workout plans |
| `workout_plan` → `workout_exercises` | 1 : Many | One workout plan has many exercises |
| `client` → `check_in` | 1 : Many | One client submits many weekly check-ins |
| `client` → `progress_tracking` | 1 : Many | One client has many progress records |
| `trainer` → `trainer_notes` | 1 : Many | One trainer writes many notes |
| `client` → `trainer_notes` | 1 : Many | One client can have many notes written about them |
| `client` → `payments` | 1 : Many | One client can make many payments |

> **Note:** The `programs` ↔ `client` relationship is **Many-to-Many**, resolved through the `program_subscriptions` junction table.

---

## 🧠 Key Design Decisions

1. **Unified `users` table with role separation** — Common attributes (name, email, phone) live in `users`. Role-specific data lives in `client` and `trainer` tables, linked via `user_id` FK. This avoids data duplication.

2. **Diet and Workout are client-dependent, not program-dependent** — Since the trainer customizes plans per client's body stats, diet and workout plans are linked to `client_id`, not `program_id`.

3. **Check-in ≠ Progress Tracking** — Check-ins are subjective weekly reports (mood, sleep, energy). Progress tracking stores objective body measurements (weight, waist, chest). Kept separate intentionally.

4. **Trainer Notes are separate from Check-ins** — Trainer feedback and form corrections are stored independently to allow trainers to write notes at any time, not just in response to check-ins.

5. **`program_subscriptions` as junction table** — Resolves the many-to-many between programs and clients, while also storing subscription metadata (start date, end date, status, payment reference).

6. **Payments linked to subscriptions** — `program_subscriptions.payment_id` → `payments.payment_id`. This avoids circular references and keeps payment records clean.

7. **One trainer per program** — Kept intentionally simple. Collaboration between trainers was considered but excluded to keep the design practical and readable.

---
