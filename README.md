# 🏥 Behavioral Training ERP System

*Deployment:* Private  
*Stack:* Next.js (TypeScript), Tailwind CSS, Radix UI, TanStack React Table, React Hook Form, Zod, Axios


---

## 🧠 Overview

**BTS (Behavioral Training System)** is a comprehensive web-based management platform built for behavioral health service centers. It streamlines the day-to-day operations of a center — from managing clients and staff to handling insurance authorizations, billing claims, and payroll — all within a single, role-aware interface.

The frontend consumes a fully documented REST API (Swagger) built by a separate backend team, with all integration handled via a centralized HTTP client with automatic JWT token refresh.


---

## 👥 Target Audience

BTS is designed for the operational staff of behavioral health centers, including:

- **Center Administrators** — manage clients, staff, appointments, and center settings
- **Billing Staff** — handle insurance claims, authorizations, and payment processing
- **Payroll Managers** — manage staff rate cards and process payments

Access is controlled through role-based dashboards, ensuring each user only sees what is relevant to their function.


---

## 🧩 Key Features

BTS covers the full operational workflow of a behavioral health center:

- **Authentication & Session Management**:
  - JWT-based login with access token + refresh token
  - Automatic token refresh on 401 responses with concurrent request deduplication
  - Auto-redirect to login on session expiry

- **Client Management**:
  - Full CRUD for client profiles including demographics, guardians, emergency contacts, and insurance info
  - Authorization tracking with diagnosis codes, referring providers, and service limits
  - File uploads per client, status filtering (Active/Inactive), and CSV export

- **Appointment Scheduling**:
  - Create and manage appointments with multi-staff assignment
  - Appointment series support, status tracking, and reimbursement logging
  - Delete confirmation dialogs and full audit trails (createdBy / updatedBy)

- **Staff Management**:
  - Complete staff profiles with credentials (NPI), licenses, certifications, and file uploads
  - Service assignments and availability tracking
  - Credential enforcement for scheduling

- **Insurance & Billing**:
  - End-to-end claim management: diagnosis codes, procedure codes, modifiers, session tracking, and clearing house submission
  - Authorization management with unit limits (weekly/monthly) and service limits
  - Hierarchical claim detail view across multiple providers

- **Payroll & Payments**:
  - Staff rate cards with service-specific and salary/hourly configurations
  - Payroll processing forms and CSV export
  - Staff payment history and pending payment tracking

- **Role-Based Dashboards**:
  - **Admin Dashboard**: Clients, staff, appointments, programs, center settings
  - **Billing Dashboard**: Insurance claims, staff payments, authorizations — with summary stats cards


---

## 🧱 Tech Stack Overview

- **Frontend**: Next.js 14 (TypeScript), styled with Tailwind CSS for a responsive and accessible UI
- **UI Components**: Radix UI primitives (modals, dropdowns, selects, switches, alert dialogs)
- **Tables**: TanStack React Table v8 — sortable, filterable, paginated data grids
- **Forms**: React Hook Form with Zod schema validation for type-safe, complex nested forms
- **HTTP Client**: Axios with a custom wrapper handling auth headers, token refresh, and normalized responses
- **Utilities**: date-fns for date handling, libphonenumber-js for phone validation, country-state-city for address fields
- **Icons**: Lucide React
- **Theme**: Next Themes for dark/light mode support
- **Notifications**: React Hot Toast


---

## 🔐 Role-Based Access Control

The application has two protected sections — **Admin** and **Billing** — each with their own layout and navigation. Route-level access is enforced by validating the user's role from the JWT payload. Unauthorized users are redirected to the login page on every protected route entry.

```
/admin     → Center operations (clients, staff, appointments, programs)
/billing   → Insurance claims, staff payments, authorizations
```

Both sections share a common sidebar + header shell, with navigation links conditionally rendered based on the active role.


---

## 🔗 API Integration via Swagger

The backend was built by a separate team and documented using **Swagger**. All frontend API integration was done by reading and consuming the Swagger spec at:

```
https://bts.mitmgwalior.in/swagger/index.html
```

A centralized HTTP service (`lib/http.ts`) handles all communication:

```typescript
// Normalized response wrapper used across all API calls
{ success: boolean, data?: T, error?: string }
```

Key behaviors of the HTTP client:
- Injects Bearer token from localStorage on every request
- Automatically retries with a refreshed token on 401
- Prevents duplicate refresh calls using a shared Promise reference
- Safely parses empty or malformed JSON responses


---

## 📊 Data Tables & List Management

All list views use **TanStack React Table** for advanced grid functionality:

- Column-level sorting and filtering
- Pagination (configurable page size per entity)
- Row selection with count display
- Hierarchical row expansion for claim detail views
- Client-side data transformation and display formatting


---

## 🧾 Form Architecture

Every create/edit flow uses **React Hook Form** paired with **Zod** schemas:

- Type-safe field definitions with schema-first validation
- Support for deeply nested objects (e.g. guardians, emergency contacts, claim line items)
- Array transformations (string → Date coercion, default value injection)
- 40+ modal components following a consistent `add-{entity}`, `edit-{entity}`, `view-{entity}` pattern


---

## 🚀 Deployment

BTS is deployed in a **private environment** for internal use by the client organization. The frontend is served as a Next.js application connected to a secured backend API.


---

## 🛠️ Challenges Faced

- **First-time Swagger Integration**:
  Working with a Swagger-documented API for the first time required learning how to read API contracts, understand request/response schemas, and map them accurately to frontend types — without direct access to the backend codebase. This significantly improved the ability to work independently from backend developers.

- **Token Refresh & Concurrency**:
  Implementing a robust JWT refresh flow that handles multiple simultaneous API calls on session expiry (without triggering multiple refresh requests) required careful Promise-based deduplication logic.

- **API Response Normalization**:
  The backend returned inconsistently named fields across endpoints (e.g. `totalPages` vs `totalPage` vs `pageCount`). Building a normalization layer that gracefully handles all variants without breaking any list view was a non-trivial data-mapping challenge.

- **Complex Nested Forms**:
  Entities like clients (with guardians, emergency contacts, and insurance sub-forms) and claims (with multiple line items) required deeply nested Zod schemas and careful React Hook Form field array management.


---

## 💡 Lessons Learned

- **API-First Frontend Development**: Learned to build entirely from a Swagger spec — reading contracts, constructing typed interfaces, and catching schema mismatches early.
- **Scalable Component Patterns**: Building 40+ modals and forms reinforced the value of consistent naming, shared modal infrastructure, and callback-based parent refresh patterns.
- **HTTP Client Architecture**: Gained deep understanding of token lifecycle management, retry logic, and safe error handling in a production API client.
- **Data Normalization**: Learned to write defensive transformation layers that abstract away backend inconsistencies from UI components.
- **TanStack React Table**: Gained hands-on experience with one of the most powerful headless table libraries in the React ecosystem.


---

## 🎯 Project Status

The defined scope for BTS has been **fully completed**. All planned modules — client management, appointments, staff, insurance billing, payroll, and role-based dashboards — have been built and delivered.

Future improvements may include:
- Real-time notifications for appointment or claim status changes
- Reporting and analytics dashboards with data visualizations
- Mobile-responsive enhancements for field staff usage


---

Thank you! <br>
**Shahad Hassan** <br>
Frontend Developer <br>
*Softles*
