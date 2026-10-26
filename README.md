# DineDesk — Restaurant Ordering Platform

A full-stack restaurant ordering and table reservation starter built with Next.js, React, TypeScript, Tailwind CSS, PostgreSQL and Prisma.

## Features included

- Responsive restaurant landing page and digital menu
- Menu search, category filters and cart
- Order creation with server-side pricing and 5% demo tax
- Order tracking by order number
- Table reservation request form
- Secure password-hashed staff login and signed HTTP-only session cookie
- Role-aware Admin and Kitchen dashboards
- Order status workflow: Pending → Confirmed → Preparing → Ready → Completed
- PostgreSQL schema for users, menu categories/items, orders, order items, payments, restaurant tables and reservations
- Seed data for demo menu, 12 tables, admin and kitchen accounts

## Requirements

- Node.js 20+
- PostgreSQL 14+
- npm

## Run locally

1. Install dependencies:

   ```bash
   npm install
   ```

2. Create a PostgreSQL database named `dinedesk` (or use an existing database).

3. Copy `.env.example` to `.env` and set `DATABASE_URL`. Set `SESSION_SECRET` to a unique random string with at least 32 characters.

   ```bash
   cp .env.example .env
   ```

4. Generate Prisma client and apply the development migration:

   ```bash
   npx prisma generate
   npx prisma migrate dev --name init
   ```

5. Load demo categories, menu items, tables and staff accounts:

   ```bash
   npm run db:seed
   ```

6. Start the development server:

   ```bash
   npm run dev
   ```

7. Open `http://localhost:3000`.

## Demo staff accounts

After seeding:

- Admin: `admin@dinedesk.local` / `ChangeMe123!`
- Kitchen: `kitchen@dinedesk.local` / `ChangeMe123!`

Change/remove demo accounts before deployment. The seed currently uses the same temporary password for both roles for convenient local testing.

## Main routes

- `/` — home
- `/menu` — menu and cart
- `/reservations` — reservation request
- `/orders/track` — track an order
- `/login` — staff sign in
- `/admin` — admin overview
- `/kitchen` — kitchen display

## API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/menu` | List available menu items |
| POST | `/api/orders` | Create an order |
| GET | `/api/orders/track?number=DD-...` | Track an order |
| PATCH | `/api/orders/:id` | Update order status (staff only) |
| POST | `/api/auth/login` | Sign in |
| POST | `/api/auth/logout` | Sign out |
| GET | `/api/admin/overview` | Admin metrics (admin only) |
| GET | `/api/kitchen/orders` | Active kitchen orders (staff only) |
| POST | `/api/reservations` | Create reservation request |

## Important scope notes

This is a runnable starter project, not a production-certified restaurant system.

- Online payments are represented by a `DEMO` payment record. No real card/payment processing is enabled. Integrate a payment provider such as Razorpay or Stripe and verify provider webhooks before marking a payment paid.
- Reservations are saved as pending requests. Availability conflict checking, table assignment and staff reservation-management UI should be added before live restaurant use.
- The public order tracking endpoint exposes order details to anyone who knows an order number. For production, add a private tracking token or verify phone/email before returning details.
- Add rate limiting, CSRF/origin protections as appropriate, monitoring, automated tests, backups, privacy notices and production deployment configuration before launch.
- The seed uses sample food/menu data and emoji illustrations. Replace with the restaurant's real content and images.
- This project uses Next.js route handlers as the Node.js backend, so a separate Express server is not required for this version.

## Project structure

```text
dinedesk/
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── admin/overview/route.ts
│   │   │   ├── auth/login/route.ts
│   │   │   ├── auth/logout/route.ts
│   │   │   ├── kitchen/orders/route.ts
│   │   │   ├── menu/route.ts
│   │   │   ├── orders/route.ts
│   │   │   ├── orders/[id]/route.ts
│   │   │   ├── orders/track/route.ts
│   │   │   └── reservations/route.ts
│   │   ├── admin/page.tsx
│   │   ├── kitchen/page.tsx
│   │   ├── login/page.tsx
│   │   ├── menu/page.tsx
│   │   ├── orders/track/page.tsx
│   │   ├── reservations/page.tsx
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   └── lib/
│       ├── auth.ts
│       └── prisma.ts
├── .env.example
├── package.json
└── README.md
```
