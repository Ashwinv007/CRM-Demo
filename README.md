> **Note:** This repository demonstrates a real-world CRM implementation.
> Client branding appears only in generated documents (invoices/agreements) to reflect authentic business workflows.
> No sensitive data, credentials, or proprietary client systems are exposed.

# Trial Bench

A role-based CRM web application for managing leads, members, agreements, invoices, and expenses — built with React and Firebase.

## Tech Stack

- **Frontend:** React 19, React Router v7, MUI v7, Recharts, pdf-lib
- **Backend:** Firebase (Firestore, Auth, Cloud Functions v2)
- **Email:** Nodemailer via Zoho SMTP (Cloud Function)
- **Deployment:** Firebase Hosting

## Features

- **Dashboard** — KPI overview with charts and recent activity
- **Leads** — Add, edit, and track prospective clients through the pipeline
- **Members** — Active member profiles with agreement and invoice history
- **Past Members** — Archived records for exited members
- **Agreements** — Generate and manage PDF membership agreements
- **Invoices** — Create and download PDF invoices
- **Expenses** — Track expenses by category with report exports
- **Logs** — Immutable audit trail of user actions
- **Settings** — Manage roles, users, and email templates
- **Role-Based Access Control** — Granular per-route and per-collection permissions enforced on both client and Firestore rules

## Project Structure

```
trial_bench/
├── src/
│   ├── pages/          # Route-level page components
│   ├── components/     # Shared UI components
│   ├── store/          # React context (Auth, Data)
│   ├── auth/           # ProtectedRoute guard
│   ├── firebase/       # Firebase config & initialization
│   └── utils/          # Helpers (PDF generation, formatters, etc.)
├── functions/          # Firebase Cloud Functions (Node 22)
│   └── index.js        # OTP, scheduled jobs, Firestore triggers
├── scripts/            # One-off migration & maintenance scripts
├── firestore.rules     # Role-based Firestore security rules
└── firestore.indexes.json
```

## Getting Started

### Prerequisites

- Node.js 18+
- Firebase CLI: `npm install -g firebase-tools`

### Install dependencies

```bash
# Frontend
npm install

# Cloud Functions
cd functions && npm install
```

### Environment

Create a `.env` file in the project root with your Firebase project config:

```
REACT_APP_FIREBASE_API_KEY=...
REACT_APP_FIREBASE_AUTH_DOMAIN=...
REACT_APP_FIREBASE_PROJECT_ID=...
REACT_APP_FIREBASE_STORAGE_BUCKET=...
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=...
REACT_APP_FIREBASE_APP_ID=...
```

Update the SMTP credentials in `functions/index.js` before deploying.

## Available Scripts

### Frontend

| Command | Description |
|---|---|
| `npm start` | Start dev server at `http://localhost:3000` |
| `npm run build` | Production build to `build/` |
| `npm run build:dev` | Build using `.env.development` |
| `npm test` | Run tests in watch mode |

### Cloud Functions

| Command | Description |
|---|---|
| `npm run serve` | Start local emulator |
| `npm run deploy` | Deploy functions to Firebase |
| `npm run logs` | Tail function logs |

## Deployment

```bash
# Deploy everything
firebase deploy

# Deploy only functions
firebase deploy --only functions

# Deploy only hosting
firebase deploy --only hosting
```

## Permissions

Permissions are stored per role in Firestore (`/roles/{roleId}`) and checked on both the client (via `ProtectedRoute`) and server (via Firestore rules and Cloud Function helpers).

Key permissions: `leads:view`, `leads:add`, `leads:edit`, `leads:delete`, `members:view`, `members:add`, `members:edit`, `members:delete`, `agreements:view/add/edit/delete`, `invoices:view/add/edit/delete`, `expenses:view/add/edit/delete`, `logs:view`, `settings:manage_roles`, `settings:manage_users`, `settings:manage_templates`. Admins with the `all` permission bypass all checks.
