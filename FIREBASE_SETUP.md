# Firebase Setup Guide — SafeHer Network

## Step 1 — Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click **Add project**
3. Enter project name: `safeher-network` (or any name you like)
4. Disable Google Analytics (not needed for MVP)
5. Click **Create project**

## Step 2 — Enable Firestore

1. In your project, click **Firestore Database** in the left sidebar
2. Click **Create database**
3. Choose **Start in test mode** (for development — allows all reads/writes)
4. Select a region close to you (e.g. `asia-south1` for India)
5. Click **Enable**

## Step 3 — Create Required Index

Firestore needs a composite index for the authority query.

1. Go to **Firestore → Indexes** tab
2. Click **Add index**
3. Collection ID: `alerts`
4. Fields:
   - `reportedToAuthority` (Ascending)
   - `timestamp` (Descending)
5. Click **Create**

> Alternatively, run the backend once and click the auto-generated index link in the error message.

## Step 4 — Get Admin SDK Credentials (Backend)

1. Go to **Project Settings → Service accounts**
2. Click **Generate new private key**
3. Save the downloaded JSON file as `serviceAccountKey.json`
4. Place it inside `backend/` folder

⚠️  NEVER commit serviceAccountKey.json to git. It's already in .gitignore.

## Step 5 — Get Web App Config (Frontend)

1. Go to **Project Settings → General**
2. Scroll to **Your apps** → click **Add app** → choose **Web** (</>)
3. Register app name: `safeher-web`
4. Copy the `firebaseConfig` object values
5. Create `frontend/.env` from `frontend/.env.example`:

```env
VITE_FIREBASE_API_KEY=AIza...
VITE_FIREBASE_AUTH_DOMAIN=safeher-network.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=safeher-network
VITE_FIREBASE_STORAGE_BUCKET=safeher-network.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
VITE_FIREBASE_APP_ID=1:123456789:web:abc123
VITE_API_BASE=http://localhost:5000
```

## Step 6 — Firestore Security Rules (Production)

Replace the default test-mode rules with these for production:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /alerts/{alertId} {
      allow read, write: if true; // Replace with proper auth in production
    }
  }
}
```

## Firestore Data Structure

Collection: `alerts`
Document fields:
```json
{
  "userId": "user_abc12",
  "triggeredBy": "button | voice",
  "status": "pending | accepted | escalated",
  "riskLevel": "HIGH | LOW",
  "timestamp": "2024-01-01T20:30:00.000Z",
  "acceptedBy": null,
  "acceptedAt": null,
  "escalatedAt": null,
  "reportedToAuthority": false,
  "reportedAt": null
}
```
