# UWEAR UK Platform

A production e-commerce fashion platform consisting of three applications: a customer-facing storefront, a backend API server, and an admin dashboard. Live and serving real customers.

[Live Site](https://uwearuk.com/)

My role: Junior Web Developer (September 2024 to present).

## Architecture

```
┌──────────────────┐     ┌──────────────────┐
│ Customer Browser │     │  Admin Browser   │
│                  │     │                  │
│ React Storefront │     │ Admin Dashboard  │
│    (Vercel)      │     │    (Vercel)      │
└────────┬─────────┘     └────────┬─────────┘
         │                        │
         └───────────┬────────────┘
                     │
            ┌────────┴─────────┐
            │  Express API     │
            │  (Render.com)    │
            └────────┬─────────┘
                     │
        ┌────────────┼────────────┐
        │            │            │
┌───────┴──┐  ┌──────┴─────┐  ┌──┴──────────┐
│ MongoDB  │  │  Stripe    │  │  SendGrid   │
│  Atlas   │  │ (Payments) │  │  (Email)    │
└──────────┘  └────────────┘  └─────────────┘
```

## Customer Storefront

**Stack:** React 18, Chakra UI, Tailwind CSS, Framer Motion, React Router DOM v6, Axios

**Features:**
- Product browsing with filtering and sorting (price, category, popularity)
- Shopping cart with guest and authenticated user support
- Cart persists in localStorage for guests, syncs to database on login
- Wishlist functionality with the same guest/auth sync pattern
- Multi-step checkout process with shipping address collection
- User account management: profile, order history, saved addresses
- JWT token-based authentication stored in localStorage
- Lazy-loaded images for faster page loads
- Responsive mobile-first design

**Pages:** Home, Products, Product Detail, Cart, Wishlist, Checkout, Order Confirmation, Account, About Us, Careers, Blog, Contact, FAQ, Returns, Shipping, Size Guide (15+ routes)

## Backend API

**Stack:** Node.js, Express 4, MongoDB (Mongoose), JWT, bcrypt, Stripe, SendGrid, Multer, Winston, express-validator

**Route Groups:**

| Group | Endpoints | Purpose |
|-------|-----------|---------|
| Users | 12 | Signup, login, logout, profile CRUD, address management, cart/wishlist sync, password reset |
| Products | 6 | Public browsing, admin CRUD with image upload, category filtering |
| Orders | 10 | Order creation, Stripe payment, delivery tracking, admin management, archival |
| Admin | 4 | Dashboard metrics, recent orders/users, low stock alerts |

**Database Models:**
- **User:** auth credentials, cart items, wishlist, addresses, admin flag, newsletter preference
- **Product:** name, price, images (array), categories (product, interest, gender, sale), stock, sizes, visibility toggle
- **Order:** items, shipping address, customer details, Stripe session ID, payment status, delivery status
- **OrderArchive:** historical order storage for completed/old orders

**Integrations:**
- Stripe payment processing with webhook handlers for payment confirmation
- SendGrid transactional emails (order confirmation, welcome, password reset) with branded HTML templates
- Multer file upload middleware (up to 10 product images, 5MB max per file)
- Winston logging for production debugging
- node-cron for scheduled order archival

## Admin Dashboard

**Stack:** React 18, Bootstrap, Chart.js (react-chartjs-2), XLSX, date-fns

**Features:**
- Session-based admin authentication (separate from customer JWT auth)
- Dashboard overview: revenue metrics, recent orders, recent users, low stock alerts
- Product management: create, edit, delete with image upload and category selection
- Order management: view all orders, update status, mark delivered, print order invoices
- User management: view and manage customer accounts
- Sales analytics: revenue charts and order trends via Chart.js
- Data export: download product and order data as Excel files (XLSX)
- Search and pagination across all management views

## Deployment

| Component | Platform |
|-----------|----------|
| Customer Storefront | Vercel |
| Admin Dashboard | Vercel |
| Backend API | Render.com |
| Database | MongoDB Atlas |
| Payments | Stripe |
| Email | SendGrid |

## Technical Highlights

**1. Stripe payment integration with webhook verification.**
Orders are created with a Stripe checkout session. The backend listens for Stripe webhook events to confirm payment. The webhook handler verifies the event signature before updating order status. This ensures payment confirmation comes from Stripe, not a spoofed request.

**2. Guest to authenticated cart sync.**
Guest users have a cart stored in localStorage. When they create an account or log in, the frontend sends the local cart to the backend. The backend merges it with any existing server-side cart. This prevents losing items when transitioning from guest to logged-in state.

**3. Role-based access control with dual auth systems.**
Customers use JWT tokens (30-day expiry) stored in localStorage. Admins use server-side sessions stored in MongoDB via connect-mongo. These two auth systems are completely separate. A customer token cannot access admin routes. An admin session cannot impersonate a customer.

**4. SendGrid email templates with branded HTML.**
Order confirmations, welcome emails, and password reset links use custom HTML templates with UWEAR branding. Templates include order details, tracking info, and direct links. The email service is abstracted into a reusable module.

**5. Sales analytics with Chart.js and Excel export.**
The admin dashboard displays revenue trends, order volume, and top products using Chart.js line and bar charts. Admins export product inventory or order history as Excel files (XLSX format) for offline analysis and reporting.

**6. Multer file upload pipeline.**
Product creation and editing supports up to 10 images per listing. Each file is validated for type (images only) and size (5MB max). The upload middleware processes files before they reach the route handler.

## My Contributions

- Developed and maintained the React customer storefront
- Designed backend API architecture and all endpoint logic
- Integrated Stripe payment processing with webhook handlers
- Built the JWT + RBAC authentication system
- Created the admin dashboard with analytics and data export
- Implemented guest-to-auth cart synchronization
- Set up SendGrid email automation for transactional emails
- Managed deployments on Vercel and Render

---

Source code is private (employer project). This repository showcases the architecture and my contributions to this production platform.
