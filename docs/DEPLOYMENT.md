# Deployment Guide

## Prerequisites

- Firebase CLI installed and authenticated (`firebase login`)
- Correct project selected (`firebase use dev` or `firebase use default`)
- SMTP secrets set (see [ENV_SETUP.md](./ENV_SETUP.md))

## Deployment Commands

### Full Deployment

```bash
# Deploy everything: hosting, functions, and Firestore rules
firebase deploy
```

### Selective Deployment

```bash
# Frontend only (React app)
npm run build                          # Build production bundle
firebase deploy --only hosting

# Cloud Functions only
firebase deploy --only functions

# Firestore security rules only
firebase deploy --only firestore:rules

# Specific function(s) only
firebase deploy --only functions:sendInvoiceEmail,functions:sendAgreementEmail
```

### Build for Dev Environment

```bash
npm run build:dev    # Uses .env.development via env-cmd
```

## Deployment Checklist

### Before Deploying to Production

- [ ] Verify correct project: `firebase use` (should show `trial-bench--crm`)
- [ ] SMTP secrets are set for production project
- [ ] Build the production bundle: `npm run build`
- [ ] Test locally if possible: `firebase serve --only hosting`
- [ ] Review Firestore rules changes

### After Deployment

- [ ] Verify the app loads at the Firebase Hosting URL
- [ ] Test login flow
- [ ] Test email sending (OTP, invoice, agreement)
- [ ] Check Cloud Functions logs: `firebase functions:log`

## Firebase Hosting Config

From `firebase.json`:

```json
{
  "hosting": {
    "public": "build",
    "rewrites": [
      { "source": "**", "destination": "/index.html" }
    ]
  }
}
```

All routes are rewritten to `index.html` (SPA routing).

## Managing Secrets

```bash
# Set a secret
firebase functions:secrets:set SMTP_USER

# List all secrets
firebase functions:secrets:access SMTP_USER

# Secrets are project-scoped — set them for each project (dev/prod)
firebase use dev
firebase functions:secrets:set SMTP_USER   # Dev value

firebase use default
firebase functions:secrets:set SMTP_USER   # Prod value
```

## Monitoring

```bash
# View Cloud Functions logs
firebase functions:log

# View logs for a specific function
firebase functions:log --only sendInvoiceEmail
```
