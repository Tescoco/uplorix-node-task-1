**Project Brief: “TicketEase” – Event Ticketing Platform**

## 1. Project Overview

TicketEase is a RESTful API and companion frontend (optional) for selling tickets to live events (concerts, theater, sports, etc.). Users can browse upcoming events, view venue seat maps, select seats, purchase tickets with payment integration, and receive e‑tickets. Admins can create and manage events, venues, pricing tiers, and view sales reports.

---

## 2. Functional Requirements

### 2.1 User & Authentication

- **User Registration & Login**

  - Register with `username`, `email`, `password`.
  - Login returns JWT for subsequent calls.

- **Roles**

  - **Guest**: browse events, view details.
  - **Customer**: purchase tickets, view order history.
  - **Admin**: create/edit/delete events, venues, pricing, view reports.

### 2.2 Event Management (Admin)

- **CRUD** endpoints for events:

  - `POST /admin/events` – title, description, category, start/end datetime, venueId.
  - `PUT /admin/events/:id`, `DELETE /admin/events/:id`.

- **Venue & Seating**

  - Define venues with name, address, capacity.
  - Upload seat‑map layout (e.g. rows, sections, seat numbers).

- **Pricing Tiers**

  - Create multiple price tiers per event (e.g. VIP, General, Balcony).
  - Assign each tier to seat ranges or sections.

### 2.3 Event Discovery (Guest & Customer)

- `GET /events` – list upcoming events, filterable by date range, category, location, and keyword.
- `GET /events/:id` – detailed event info, including seat‑map with real‑time availability.

### 2.4 Seat Selection & Shopping Cart (Customer)

- **Seat Availability**

  - Real‑time lock of seats when added to cart (e.g. 5‑minute hold).
  - Automatic release of holds on expiration or cart abandonment.

- **Cart Endpoints**

  - `POST /cart` – add seat(s) to cart.
  - `GET /cart` – view current holds.
  - `DELETE /cart/:seatHoldId` – remove from cart.

### 2.5 Checkout & Payment

- **Order Creation**

  - `POST /orders/checkout` – confirm held seats, collect customer info.

- **Payment Integration**

  - Integrate a sandbox payment provider (e.g. Stripe test mode).
  - Handle `payment_success` and `payment_failed` webhooks to finalize or rollback the order.

- **E‑Ticket Generation**

  - Generate a PDF or QR‑code ticket for each seat.
  - Store unique ticket codes in the database.

### 2.6 Order Management

- **Customer**:

  - `GET /orders` – list past orders with status, tickets, download link.

- **Admin**:

  - `GET /admin/orders` – view all orders, filterable by event, date, status.

---

## 3. Non‑Functional Requirements

### 3.1 Data Persistence

- Use PostgreSQL or MongoDB.
- Model entities: Users, Events, Venues, Seats, SeatHolds, Orders, Tickets, PricingTiers.

### 3.2 Concurrency & Consistency

- Ensure **atomic** seat reservations to prevent double‑booking.
- Use database transactions or distributed locks (e.g. Redis) on seat‑hold and order confirmation.

### 3.3 Caching & Performance

- Cache event listings and static seat‑map data in Redis (with TTL).
- Invalidate cache on event or venue updates.

### 3.4 Validation & Error Handling

- Validate all request payloads (e.g. Joi or Zod).
- Centralized error middleware returning consistent JSON errors:

  ```json
  { "error": "Seat not available", "code": 409 }
  ```

### 3.5 Security

- Secure JWT in `Authorization` header (`Bearer` token).
- Rate‑limit critical endpoints (login, checkout) to prevent abuse.
- Sanitize inputs to prevent injection attacks.

### 3.6 Observability

- Structured logging (e.g. pino or bunyan) with request IDs.
- Expose simple `/health` and `/metrics` endpoints.

### 3.7 Deployment & DevOps

- Use environment variables for secrets (e.g. `.env` with `JWT_SECRET`, `DB_URL`, `REDIS_URL`, `STRIPE_KEY`).

---

## 4. Deliverables

1. **Git repository** with clear commit history.
2. **README** documenting:

   - Setup (environment, database migrations, seed data).
   - How to run in dev.
   - API endpoint list (could be a Postman/Insomnia collection link).

3. **ER Diagram** showing database schema relationships.
4. **Postman/Insomnia collection** demonstrating all workflows.
5. **Test coverage report** (e.g. Cobertura or HTML).

##Important

- Export your database when you are done and commit it as well

## Stripe Test Key

- publish_key:"pk_test_51**\*\***",
- secret_key: "sk_test_51Ll**\*\***",
- Ask your admin for the full stripe key
