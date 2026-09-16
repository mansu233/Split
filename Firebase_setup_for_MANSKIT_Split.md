# Firebase setup for MANSKIT Split

The app is configured for the existing Firebase project `manskit-23d9f` and now supports email/password accounts, password reset emails, sign-out, and private Realtime Database backups.

## One-time Firebase Console setup

1. Open **Firebase Console → Authentication → Sign-in method** and enable **Email/Password**.
2. Open **Realtime Database → Rules** and replace the rules with the contents of [`firebase-database.rules.json`](firebase-database.rules.json). Publish the rules.
3. In **Authentication → Settings → Authorized domains**, add the domain where this PWA is hosted if it is not already listed.
4. Host the files over HTTPS (or use `localhost` during development). Firebase Auth will retain the signed-in session in the browser.

## How cloud backup works

A user first creates an account or signs in with an email address and password. **Forgot Password?** sends a Firebase password-reset email. After signing in, the user enters a Sync Id such as `goa-trip-2026`, saves it, and can back up or restore the complete Split data set.

Backups are stored under:

```text
manskit_split_backups/{authenticated-user-id}/{sync-id}
```

The database rules only allow an authenticated user to read or write their own user-id branch. The Sync Id is a label within that private branch; it is not a shared secret. The existing local browser storage and JSON export/import remain available when offline.

## Important note

The Firebase web configuration is intentionally present in the client-side app; Firebase web API keys are identifiers, not passwords. The protection comes from Firebase Authentication and the Realtime Database rules. Do not publish the database with open `true` read/write rules.
