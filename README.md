# Chime — TODO App with Alarms

> 👨‍💻 **Author:** Raj — WhiteHat Developer

A task manager where urgent tasks **ring an alarm** at the exact date and time you set.
Chime ships in two versions built from the same design:

| | 🌐 Web app | 📱 iOS / Android app |
|---|---|---|
| File | `todo-alarm.html` | `chime-mobile/App.js` |
| Tech | Single HTML file (no install) | Expo / React Native |
| Alarm rings when… | The browser tab is **open** | **Anytime** — even app closed, phone locked |
| Storage | Browser persistent storage | On-device (AsyncStorage) |
| Setup time | 0 minutes | ~5 minutes |

## Features (both versions)

- **Title + description** for every task
- **Date & time** pickers
- **Urgent — ring alarm** toggle: sound + alert at the scheduled moment
- **Repeat**: Daily (choose any of Mon, Tue, Wed, Thu, Fri, Sat, Sun), Weekly, Monthly, Yearly
- **End repeat**: repeat forever, or stop after a chosen date (inclusive of that day)
- Completing a recurring task rolls it forward to the next occurrence; it only truly
  completes once the series passes its end date
- Live countdown chips ("in 2h 15m"), red pulsing "overdue" state, delete & complete
- Edge cases handled: Jan 31 → Feb 28 for monthly, Feb 29 → Feb 28 for yearly,
  day-picker start dates snap to the first selected day

---

## 🌐 Web version (`todo-alarm.html`)

### Run it
Open `todo-alarm.html` in any modern browser (Chrome, Edge, Safari, Firefox).
That's it — no server, no install.

### Using alarms
1. Add a task, pick a date & time, switch on **Urgent — ring alarm**
2. Keep the tab open. At the set time a full-screen alert appears with a repeating
   beep and **Stop** / **Snooze 5 min** buttons (phones also vibrate)
3. Browsers require one tap or click on the page before they allow sound —
   adding your first task counts

### Limitations
- Alarms only fire while the tab is open (a browser security restriction —
  this is exactly what the mobile version solves)
- On phones, you can "Add to Home Screen" for an app-like feel

---

## 📱 iOS / Android version (`chime-mobile/`)

One React Native codebase that runs on **both** iPhone and Android. Alarms are
**local scheduled notifications**, so they ring with sound even when the app is
closed and the phone is locked.

### Prerequisites
1. **Node.js LTS** on your computer (Windows/Mac/Linux — no Mac required)
2. **Expo Go** app on your phone (App Store / Play Store)
3. Phone and computer on the same Wi-Fi

### Setup

```bash
# 1. Create a fresh Expo project
npx create-expo-app@latest chime --template blank
cd chime

# 2. Install dependencies (expo install picks compatible versions)
npx expo install expo-notifications expo-device @react-native-async-storage/async-storage @react-native-community/datetimepicker

# 3. Replace the generated App.js with chime-mobile/App.js from this package

# 4. Start the dev server
npx expo start
```

Open **Expo Go** on your phone, scan the QR code from the terminal, and allow
notifications when prompted.

### Testing an alarm
Add a task 1–2 minutes in the future with **Urgent — ring alarm** on, then lock
your phone. The notification fires with sound at the set time.

> **Notes**
> - The iOS **Simulator** does not deliver notifications — always test on a real device.
> - If notification behavior seems limited inside Expo Go (varies by Expo SDK,
>   especially on Android), make a development build: `npx expo run:ios` (needs a Mac)
>   or a free cloud build: `npx eas build --profile development --platform ios`.

### How mobile alarms work
- Each alarmed task schedules its next occurrences (up to 12) as system notifications
- iOS allows ~64 pending notifications per app, so schedules refresh every time the
  app opens — seamless in everyday use
- Completing or deleting a task cancels its pending notifications; end-repeat is
  enforced when occurrences are generated

### Publishing to the stores (optional, later)
1. **App Store**: enroll in the Apple Developer Program ($99/yr), then
   `npx eas build --platform ios` and `npx eas submit --platform ios`
   (Expo's cloud build — no Mac needed)
2. **Play Store**: Google Play developer account ($25 one-time), then the same
   two commands with `--platform android`

---

## Recurrence rules reference

| Repeat | Behavior |
|---|---|
| Daily | Fires on the days of week you select (any mix of Mon–Sun) |
| Weekly | Same weekday every week |
| Monthly | Same date every month; short months clamp (Jan 31 → Feb 28) |
| Yearly | Same date every year; Feb 29 → Feb 28 in non-leap years |
| End repeat | Optional cutoff date, inclusive — a daily task ending Aug 15 still fires on Aug 15 |

If the app/page was closed past several occurrences, the task skips ahead to the
next **future** occurrence rather than firing repeatedly.

---

## 👨‍💻 Author

**Raj** · WhiteHat Developer

Built with ❤️ — web (HTML/CSS/JS) and mobile (React Native / Expo).

<!-- Add your links here:
- GitHub: https://github.com/your-username
- LinkedIn: https://linkedin.com/in/your-profile
- Medium: https://medium.com/@your-handle
-->
