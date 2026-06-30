# Project Brief: World Clock

| | |
|---|---|
| **Document** | Project brief |
| **Repository location** | `process/brief/brief.md` |
| **Version** | v1.0.0 (first release) |
| **Status** | Draft for approval |
| **Date** | 2026-06-29 |

> **Purpose of this document.** The brief fixes *what* we are building and *why*, the boundaries of
> the first release, and how we will know it is done. It is **binding**: once approved it is not
> reopened casually. Keep it stable — running status lives in `process/milestone-plan.md`, not here.
> Approval of this brief closes Milestone 1.
>
> This is the **v1.0.0** brief written at version start (`sfk-version`). It is deliberately
> interview-light; the section-by-section depth is added when Milestone 1 is worked
> (`sfk-next-milestone`).

---

## 1. Overview

World Clock is a small, offline web application that shows the current local time across several
user-chosen cities at a glance, updating live without manual refresh. It is for anyone who works
across time zones and wants an always-correct, clearly readable view of "what time is it there right
now" for the places they care about. Its single sentence of value: **accurate local times for many
places, side by side, always live, fully offline.**

## 2. Problem statement

Checking the time in other places is fiddly and error-prone: people do mental offset arithmetic, get
daylight-saving transitions wrong, or rely on tools that need a network connection and clutter the
screen with extras. The status quo either requires connectivity, hides the time behind navigation, or
silently drifts out of date. World Clock keeps a handful of chosen cities visible, correct, and live,
with no network dependency.

## 3. Users

A single user on their own machine, running the app locally in a browser. No accounts, no
multi-user state, no server. The user works across or travels between time zones and wants a quick,
glanceable reference. The app must work with no internet connection.

## 4. Goals (first release)

1. Display the current local time for a set of cities chosen from a **curated city list**, all
   visible at once.
2. Keep every displayed time **live** — it advances on its own, with no manual refresh, and stays
   accurate to the system clock.
3. Let the user **add and remove** cities from the curated list to build their own view.
4. Offer a **12-hour / 24-hour** display toggle that applies to all shown clocks.
5. Run **fully offline**, computing every time locally from the system clock against a bundled
   time-zone database.

## 5. Domain model / core structure

The central concept is a **clock**: a chosen city bound to an IANA time zone (e.g. *Tokyo →
`Asia/Tokyo`*). The user's view is an ordered set of clocks. Each clock derives its displayed time by
applying its zone's current rules (including daylight-saving offset) to the current system instant.
The **curated city list** is a bundled, fixed catalogue of well-known cities, each pre-mapped to its
IANA zone; the user picks clocks from this catalogue rather than entering zones freely.

## 6. Core approach / algorithm

A single local "tick" reads the current system instant; for each clock, the app converts that instant
into the city's local wall-clock time using the bundled time-zone database (which encodes UTC offsets
and daylight-saving rules per zone), and renders it in the chosen 12/24-hour format. The view
re-renders on a regular interval so seconds advance smoothly. All computation is local and
deterministic given the system clock — there is no network call at any point.

## 7. Secondary approach, if any

Not applicable for the first release.

## 8. Technical direction

A **local web application**: it runs in the browser with no backend and no runtime network access.
The bundled IANA time-zone data is the only data source. Exact language, framework, build tooling and
the precise time-zone data source are **directional here and made binding in Milestone 3
(Architecture)**, consistent with the layered dependency rule in the root `CLAUDE.md`
(`core → domain → services → interface`, `storage` beneath `services`). Hard constraints: must work
fully offline; no external services at runtime.

## 9. Out of scope (first release)

- **Free time-zone search** over the full IANA database — the first release uses a curated city list
  only.
- **Date and weekday display** alongside the time.
- **UTC-offset or daylight-saving indicators** shown per clock.
- **Persisting** the user's chosen cities, order, or settings between launches — the app starts from
  a default set each load.
- **Reordering** clocks by the user.
- Accounts, syncing, sharing, multi-user state, or any server component.
- Alarms, timers, stopwatch, or meeting-time / overlap planning.

## 10. Future roadmap (post-release, not committed)

- Free search and selection across the full IANA time-zone database.
- Show date and weekday per clock.
- UTC-offset and daylight-saving indicators.
- Persist chosen cities, order, and settings across launches.
- User reordering of clocks.
- Meeting/overlap planner across selected zones.

> None of these are deferred for a structural reason — all remain compatible with the offline
> constraint. Each will require its own version brief (see `process/README.md`, "How versions
> evolve").

## 11. Success criteria

The release is done when the user can:

1. Open the app with no internet connection and see the current local time for a default set of
   cities, all at once.
2. Watch each displayed time advance on its own, staying accurate to the system clock, without ever
   refreshing.
3. Add a city from the curated list and see its live clock appear; remove a city and see it disappear.
4. Toggle between 12-hour and 24-hour display and see every clock update its format accordingly.
5. Confirm the times shown are correct for each city, including correct daylight-saving behaviour for
   the current date.

## 12. Risks and open questions

| Risk / question | Notes / mitigation |
|-----------------|--------------------|
| Time-zone data correctness (especially DST) | Use an authoritative bundled IANA tz database; pin its version; cover DST behaviour in the test strategy. Source finalised in Milestone 3. |
| No persistence in v1.0.0 means the view resets each launch | Accepted for first release; a sensible default city set keeps it useful. Persistence is on the roadmap. |
| Curated city list coverage | Curate a broad, well-known set; gaps are acceptable for v1.0.0 since full search is roadmapped. Final list decided in Requirements (Milestone 2). |
| Live-update accuracy / drift | Re-render on a regular interval read from the system clock; verify against expected wall-clock in tests. |

## 13. Repository strategy

A **single repository**, already initialised, with the kit's `process/` documentation and tickets
living beside the application code so they stay honest and update in the same commits. Intended
top-level layout:

```
/process     – all project documentation (this brief, requirements, architecture, test strategy…)
               and tickets (process/tickets/)
/<code>      – the application source, subdivided by the layers in the dependency rule
               (core / domain / services / interface, with storage beneath services)
```

---

*Approval of this brief closes Milestone 1.*
