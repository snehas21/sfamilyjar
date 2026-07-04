# sfamilyjar

A family swear jar tracker for Daddy, Samridh, and Sulagna. Each swear costs 25&cent;.

Static web app (`index.html`, no build step, no backend) — open it in a browser and go. It's also an installable PWA: add it to your phone's home screen for an app-like icon and offline use.

- Log a swear for any family member (adds 25&cent; to the jar)
- Undo the most recent swear for a family member, or delete any specific entry from the activity log
- View swear counts and totals by timeframe: this month, last month, since start of year, and all time
- Rename family members by clicking their name

By default, data is stored in the browser's `localStorage`, so it stays on whichever device/browser you use the app in. Set up Firebase (below) to sync data live across every family member's phone instead.

## Syncing across devices with Firebase

The app checks `firebase-config.js` on load. If it still has placeholder values, the app runs in local-only mode. Once you fill it in with a real project's config, every device that loads the page syncs through Firebase Realtime Database instead.

1. Go to the [Firebase console](https://console.firebase.google.com/) and create a new project (any name, e.g. "sfamilyjar"). Google Analytics is not needed.
2. In the project, go to **Build > Realtime Database > Create Database**. Pick any location and start in locked mode (rules are set in step 4).
3. Go to **Build > Authentication > Get started > Sign-in method**, and enable **Anonymous**.
4. Still in Authentication/Database settings, go to **Realtime Database > Rules** and paste:
   ```json
   {
     "rules": {
       ".read": "auth != null",
       ".write": "auth != null"
     }
   }
   ```
   This requires a signed-in (anonymous) client to read or write — it isn't per-user security, just a basic bar against random internet scanners, appropriate for a low-stakes family app.
5. Go to **Project settings** (gear icon) **> General**, scroll to "Your apps", click the web icon (`</>`), and register an app (any nickname). It will show a `firebaseConfig` object.
6. Copy those values into `firebase-config.js` in this repo, replacing the `REPLACE_WITH_...` placeholders.

Once deployed with real values, the header will show "☁️ Synced across devices" instead of "📴 Saved on this device only". Any swears already logged locally are copied into Firebase automatically the first time it connects, so you won't lose existing data.
