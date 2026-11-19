# Meridian

A responsive world-clock dashboard built with React, TypeScript, Vite, and Supabase. All clocks use the browser's `Intl.DateTimeFormat` and IANA time zones, including daylight-saving transitions and fractional-hour offsets.

## Included

- A live home clock with a custom geographic globe, local date, UTC offset, and fullscreen analog/digital modes.
- 452 searchable locations spanning 248 countries and territories. Add cities, change the home location, sort saved clocks, and switch between 12- and 24-hour time.
- A clickable multi-city timeline with previous/next-day navigation. The timeline represents real instants, including 23- and 25-hour daylight-saving days.
- City-specific alarms with once/daily/weekday/weekend schedules, enable switches, names, five-minute snooze, and a sound preference.
- A lap stopwatch with pause/resume, reset, and CSV export.
- A countdown timer with custom durations and six presets. Both timing instruments use timestamps rather than assuming an interval fires exactly on time.
- Light and dark appearance, keyboard-accessible dialogs, responsive layouts, loading states, and actionable errors.

## Persistent data

| Table | Purpose |
| --- | --- |
| `meridian_cities` | Country, city, IANA identifier, geographic coordinates, flag code, and catalog order |
| `meridian_workspaces` | Browser workspace, home location, saved cities, display format, theme, sound, and sort preferences |
| `meridian_alarms` | Workspace-owned reminders, local target time, city, recurrence, enabled state, and next UTC firing instant |

The API routes `/api/cities`, `/api/workspace`, and `/api/alarms` read and write Supabase. The frontend refreshes its data after each database mutation. A random browser capability identifies the workspace without requiring a login. Reminder names are encrypted with AES-256-GCM before storage. Stopwatch and timer state are device-local and survive reloads.

## Run

- `npm run dev` — local UI and real API handlers using the provided environment configuration.
- `npm run build` — TypeScript validation and production build.
- `npm run preview` — preview the production UI with the same API handlers.
- `npm run lint` — static checks.
- `python scripts/browser-check.py` — real-database browser smoke test after building, using the installed Playwright Chromium. It needs no running development server.

Vercel serves the production `api/` handlers. The shared Supabase client stays server-side. When the host provides only a publishable database key, request-scoped headers are used with the database policies; no elevated key is synthesized or shipped to the browser.

## Browser behavior

Keep a Meridian tab open for alarms and timer alerts. A user gesture is required by browser audio policies. Browsers and operating systems can suspend sleeping or background tabs, so this is not a replacement for an operating-system alarm. Closing the browser does not schedule a system notification. Clearing site storage removes access to that browser's saved workspace.

## Assets

The globe is rendered from Natural Earth's public-domain land geometry and rotates to the selected home location. Country flags are bundled SVGs from FlagCDN. DM Sans is bundled locally under the SIL Open Font License. The app requires no third-party API keys.
