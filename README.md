# Football Ticket Booking System - Database Schema




## 🛠️ Features & Core Logic

*   **Role-Based Access Control:** Restricts user roles strictly to `Football Fan` or `Ticket Manager`.
*   **Dynamic Inventory Tracking:** Matches follow controlled operational states (`Available`, `Selling Fast`, `Sold Out`, `Postponed`).
*   **Transactional Ledger:** Tracks complete customer checkout metrics, including seating information and specific `payment_status` flows.
*   **Anti-Double Booking Validation:** Uses a composite unique constraint over `(user_id, match_id, seat_number)` to logically map exactly one customer to one stadium seat per match, ensuring data safety.

---

## 📐 Relational Schema Architecture

The database structure consists of three normalized core tables:

### 1. `Users` Table
Tracks registered application users and permissions.
*   `user_id` (PK, Serial)
*   `full_name` (Varchar, Required)
*   `email` (Varchar, Unique, Required)
*   `role` (Varchar, Check constraint enforced)
*   `phone_number` (Varchar, Nullable)

### 2. `Matches` Table
Catalogs tournament events, matchups, and baseline logistics.
*   `match_id` (PK, Serial)
*   `fixture` (Varchar, Required)
*   `tournament_category` (Varchar, Required)
*   `base_ticket_price` (Int, Required)
*   `match_status` (Varchar, Check constraint enforced)

### 3. `Bookings` Table
The intersection engine linking users dynamically to match events.
*   `booking_id` (PK, Serial)
*   `user_id` (FK -> `Users.user_id`, On Delete Cascade)
*   `match_id` (FK -> `Matches.match_id`, On Delete Cascade)
*   `seat_number` (Varchar)
*   `payment_status` (Varchar, Check constraint enforced)
*   `total_cost` (Int, Required)

---

## 📋 Included Query Operations

The script includes several business intelligence and diagnostic queries to retrieve analytical data:

| Query ID | Objective Description | Relational Techniques Used |
| :--- | :--- | :--- |
| **Query 1** | Filter available Champions League matches | `WHERE ... AND ...` conditional filter |
| **Query 2** | Case-insensitive profile name lookups | `ILIKE` wildcard operators |
| **Query 3** | Catch missing or broken transaction records | `COALESCE` null-replacement handling |
| **Query 4** | Build detailed operational customer receipts | Multi-table `INNER JOIN` operations |
| **Query 5** | Audit marketing engagement (including fans with 0 bookings) | `LEFT OUTER JOIN` schema auditing |
| **Query 6** | Identify premium sales exceeding structural margins | Independent scalar `SUBQUERY` validation |
| **Query 7** | Find premium tier brackets for budget scaling | `ORDER BY`, `LIMIT`, and `OFFSET` pagination |

---
