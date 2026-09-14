[README.md](https://github.com/user-attachments/files/32214563/README.md)
# Mobile Oil Change Express — mileage & oil change tracker

A single-page web app that logs odometer readings and warns you when an oil
change is coming up or overdue.

## Running it

No build step — it's plain HTML/CSS/JS. Two options:

1. **Locally**: open `index.html` directly in a browser. (The "Install" banner
   and service worker won't activate from a `file://` URL — see below.)
2. **Hosted** (recommended): upload the folder to any static host — GitHub
   Pages, Netlify, Vercel, or your own server. Once it's served over `https://`,
   you can add it to your phone's home screen like a native app (Safari:
   Share → Add to Home Screen; Chrome: menu → Install app).

## Data storage

Everything is stored in the browser's `localStorage`, on-device only —
nothing is sent to a server. That means:
- Your data stays private, but it's tied to that one browser/device.
- Clearing browser data or switching phones will lose your history, unless
  you're syncing it yourself (see "Next steps" below).

## Notifications

The app requests notification permission on load and will show an alert
(in-app banner, plus a browser notification if permitted) once you're within
500 miles of your set interval, or overdue. Browser notifications only fire
while the app is open in a tab or running as an installed PWA in the
background on some platforms — they can't wake up from being fully closed
the way a native app's push notifications can.

## In-car automation (CarPlay / Android Auto)

Neither Apple's CarPlay framework nor Android Auto exposes a "phone just
connected" event to web pages — that's only available to native apps with
the right entitlements. Until the native app exists, the in-app "In-car
reminders" section walks through a workaround using iOS Shortcuts or Android
Tasker to auto-open this app when your phone connects to your car.

## Path to a native app

When you're ready to build native iOS/Android apps, the pieces that carry
over directly are the data model (mileage readings + interval + last-change
mileage) and the alert logic (the `render()` function's threshold checks).
What a native app adds on top:

- **CarPlay entitlement (iOS)**: requires applying to Apple's CarPlay
  program — approval is scoped to app categories like navigation, audio, EV
  charging, and parking, so worth checking current eligibility before
  building around it.
- **Android Auto**: more open — apps can use the Android for Cars App
  Library to add a simple in-car screen without special approval.
- **Real push notifications**: needs a backend (even a small one) to send
  pushes via APNs (iOS) or FCM (Android) so reminders arrive even when the
  app isn't open.
- **Cross-device sync**: if you want mileage history to follow you across
  phone/tablet/car, that also needs a backend or a sync service (iCloud,
  Firebase, etc.) instead of local-only storage.

## Files

- `index.html` — the app
- `manifest.json` — PWA metadata for home-screen install
- `icon.svg` — app icon
- `sw.js` — service worker (offline caching)
