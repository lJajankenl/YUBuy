# YUBuy
A York University campus marketplace for students to buy and sell used textbooks, furniture, electronics, and other student essentials.

---

## My Contributions

### **Frontend Pages**

Built four core pages from scratch in React (Vite), each wired into the application's routing and connected to backend endpoints where they existed at the time:

**Admin Dashboard (`Admin.jsx`)**
- Tab-based layout for managing listings and users
- Fetches live listings (`GET /api/listing`) and users (`GET /api/user`) on mount, with loading states for each
- Listing removal is fully backend-connected (`DELETE /api/listing`)
- Listing approval and user banning were built as local-state actions at the time (no approve/ban endpoints existed yet) — a teammate later connected these to their backend endpoints
- Status badges (Active / Flagged / Reported) styled to match the app's dark theme

**Checkout (`Checkout.jsx`)**
- Full checkout form: shipping info, payment details, and an order summary section (shipping, tax, total cost)
- Regex-based client-side validation across the form: name fields (letters/spaces/hyphens only), postal code, phone number (auto-inserted `-` separators, capped at 10 digits), card number and security code (digits only), and expiration date (auto-inserted `/`)
- On submit, validated at the time and redirected to the order confirmation page; a teammate later connected this flow to a real backend checkout endpoint

**Sell Item (`SellItem.jsx`)**
- Form for sellers to list an item: title, description, price, category, condition, and location
- Wired to `POST /api/listing` to create a real listing in the database
- Category list is intentionally hardcoded (reverted from an earlier backend fetch) to keep the form testable independent of category-seeding state
- Redirects to the seller's profile page on successful submission

**Order Confirmation (`OrderConfirmation.jsx`)**
- Confirmation page shown after a successful checkout submission, with a link back to listings
- Routed in from `Checkout.jsx` on successful form submission

---

### **Integration & Routing**

- Added and fixed routes in `App.jsx` for each new page as it was built, including resolving a merge conflict that briefly broke routing
- Fixed a typo'd import (`SellerItem` → `SellItem`) that was breaking the build
- Added a "+ Sell Item" button to `SellerProfile.jsx` (a page primarily built by a teammate) linking sellers directly to the Sell Item page
- Wired the "Sell an item" button on the Listings page to navigate to the Sell Item page

---

### **Design Documentation**

- Authored PlantUML sequence diagrams covering six major user flows — Buyer, Seller, Admin, Authentication, Password Reset, and Wishlist — for the project's final report
- Drafted the Implementation Plan section of the project proposal, covering the React/Vite frontend, Node/Express backend, PostgreSQL/Prisma ORM, Render deployment, and Cloudinary image storage
- Aligned the three-tier PlantUML architecture diagram in the proposal with the languages/frameworks table for consistency

---

## Tech Stack

**Frontend:**
- React 18, Vite
- Inline style objects for a consistent dark theme (`#2a2a2a`, `#CC0000`, `#181313`)

**Backend (integrated with, not authored):**
- Node.js, Express
- PostgreSQL, Prisma ORM
- Render (deployment), Cloudinary (image storage)

---

## Original Project

## Table of Contents
- Project Structure
- Getting Started
- Database Setup
- Data Lake Setup

## Project Structure
The project is a monorepo containing the frontend and backend of the YUBuy application

### Frontend

The frontend is a React application built with [Vite](https://vitejs.dev/).

**Code Structure:**
-   `frontend/src/assets`: Contains static assets like CSS and images.
-   `frontend/src/pages`: Contains React pages.
-   `frontend/src/components`: Contains reusable React components
-   `frontend/src/App.jsx`: The root React component.
-   `frontend/src/main.jsx`: The entry point of the application.
-   `frontend/src/test`: Contains test cases for frontend pages.

### Backend

The backend is a [Node.js](https://nodejs.org/) application using the [Express](https://expressjs.com/) framework.

**Code Structure:**
-   `backend/src/api`: Contains all the API modules. Each module is organized by feature.
    -   `*Controller.js`: Handles incoming requests, validates input, and calls the appropriate service.
    -   `*Router.js`: Defines the routes for the module.
    -   `*Service.js`: Contains the business logic for the module.
-   `backend/src/db`: Contains the database connection and initialization code.
-   `backend/tests`: Contains unit and integration tests for backend endpoints.

## Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) (v20 or higher) and [Prisma ORM](https://www.prisma.io) for connecting to the database

### Local Development

You can run the frontend and backend services locally concurrently.

#### Frontend
```bash
cd frontend
npm install
npm run build
```

### Backend
```bash
cd backend
npm install
# Make sure your .env file is configured
node src/server.js
```

## Backend Environment Variables

The following environment variables are required to run the backend application. These should be set in the `backend/.env` file.

| Variable | Description | 
| -------- | ----------- |
| `DATABASE_URL` | The connection string to your Prisma ORM. |
| `FRONTEND_URL` | The frontend URL that requests will be coming from |
| `SESSION_SECRET` | A session secret key (can be any value) |
| `AZURE_STORAGE_CONNECTION_STRING` | The connection string to your Azure storage account for ADLS |
| `EMAIL_USER` | yubuy.noreply@gmail.com |
| `GMAIL_CLIENT_ID` | Gmail client access ID for Oauth2 API |
| `GMAIL_CLIENT_SECRET` | The client secret for the Gmail access ID for Oauth2 API |
| `GMAIL_REFRESH_TOKEN` | The refresh token required for Oauth2 API |

## Frontend Environment Variables

The following environment variable is required to run the frontend application. This should be set in the `frontend/.env` file.

| Variable | Description | 
| -------- | ----------- |
| `VITE_API_BASE_URL` | http://localhost:8080 |

## Database Setup

This project uses a Prisma ORM for the database.

1. Get your connection string: [Quickstart: Prisma ORM with PostgreSQL | Prisma Documentation](https://www.prisma.io/docs/prisma-orm/quickstart/postgresql).
2. Set the `DATABASE_URL` in the `backend/.env` file.


### Initializing Tables

Once your `backend/.env` is configured with `DATABASE_URL`, run the following from the project root to create all the tables:

```bash
npx prisma generate
npx prisma migrate dev
```

## Data Lake Setup

This project uses an Azure ADLS Gen2 instance to host the data lake.
1. Create an Azure storage account.
    - **IMPORTANT:** When creating your storage account, in the advanced settings, enable "Hierarchical namespace".
2. Inside of the storage account, create a container named "raw".
3. Under `Access Keys`, retrieve your `Connection string (Key 1)` and paste it under `AZURE_STORAGE_CONNECTION_STRING` in the `backend/.env` file.
