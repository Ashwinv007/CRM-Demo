# Environment Setup Guide

## Prerequisites

- **Node.js** v18+ and **npm** v9+
- **Firebase CLI**: `npm install -g firebase-tools`
- A Firebase project (dev or prod)
- Git access to the repository

## Quick Start

```bash
# 1. Clone the repository
git clone <repo-url> && cd trial_bench

# 2. Install frontend dependencies
npm install

# 3. Install Cloud Functions dependencies
cd functions && npm install && cd ..

# 4. Authenticate with Firebase
firebase login

# 5. Select the target project
firebase use dev       # For development
# firebase use default # For production

# 6. Set up SMTP secrets (required for email features)
firebase functions:secrets:set SMTP_USER    # Your Zoho email
firebase functions:secrets:set SMTP_PASS    # Your Zoho App Password
firebase functions:secrets:set SMTP_HOST    # smtp.zoho.in
firebase functions:secrets:set SMTP_FROM    # "Trial Bench" <your-email@zoho.in>

# 7. Start the development server
npm start
```

## Environment Files

| File | Purpose | Used By |
|---|---|---|
| `.env.development` | Dev Firebase project config + asset paths | `npm start` |
| `.env.production` | Prod Firebase project config + asset paths | `npm run build` |

All client-side env vars use the `REACT_APP_` prefix (CRA convention).

Cloud Functions use **Firebase Secrets** (not `.env` files). See the SMTP secrets setup in step 6 above.

## Firebase Project Aliases

Defined in `.firebaserc`:

| Alias | Project ID | Usage |
|---|---|---|
| `default` | `trial-bench--crm` | Production |
| `dev` | `trial-bench-dev-10946` | Development |

Switch with:
```bash
firebase use dev        # Development
firebase use default    # Production
firebase use            # Check current project
```

## Project Structure

```
trial_bench/
├── .env.development        # Dev Firebase config
├── .env.production         # Prod Firebase config
├── .firebaserc             # Firebase project aliases
├── firebase.json           # Firebase hosting/functions/firestore config
├── firestore.rules         # Firestore security rules
├── firestore.indexes.json  # Firestore composite indexes
├── functions/              # Cloud Functions (Node.js)
│   ├── index.js            # All Cloud Function definitions
│   └── package.json        # Functions dependencies
├── public/                 # Static assets (PDFs, images)
├── src/
│   ├── App.js              # Root component with routing
│   ├── auth/               # Auth guards & permission hooks
│   ├── components/         # UI components (pages, modals, dashboard)
│   ├── firebase/config.js  # Firebase SDK initialization
│   ├── pages/              # Page wrapper components
│   ├── store/              # Context providers (Auth + Data)
│   └── utils/              # Utility functions (logging)
├── docs/                   # Project documentation
└── package.json            # Frontend dependencies & scripts
```

## Available Scripts

| Command | Description |
|---|---|
| `npm start` | Dev server (uses `.env.development`) |
| `npm run build` | Production build (uses `.env.production`) |
| `npm run build:dev` | Build with dev config (uses `env-cmd`) |
| `npm test` | Run tests |
| `firebase deploy` | Deploy everything (hosting + functions + rules) |
| `firebase deploy --only hosting` | Deploy frontend only |
| `firebase deploy --only functions` | Deploy Cloud Functions only |
| `firebase deploy --only firestore:rules` | Deploy Firestore rules only |

## Key Technologies

| Technology | Version | Purpose |
|---|---|---|
| React | 19.x | UI framework |
| Material UI (MUI) | 7.x | Component library |
| Firebase | 12.x | Backend (Auth, Firestore, Hosting, Functions) |
| Day.js | 1.x | Date manipulation |
| Recharts | 3.x | Charts for expense reports |
| pdf-lib | 1.x | Invoice/agreement PDF generation |
| Nodemailer | — | Email sending (in Cloud Functions) |
| xlsx | 0.18 | Excel export for expenses |
