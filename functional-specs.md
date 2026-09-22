# Functional Specifications — LiveList

**Version:** 1.0
**Status:** Validated, ready for technical specifications
**Scope:** v1 (MVP)

---

## 1. Product goal

LiveList lets a user keep track of the concerts they plan to attend, see
which concerts are circulating within their close circle, and centralize
event information scattered across multiple ticketing platforms (Shotgun,
Dice, Ticketmaster).

The product addresses two distinct needs:
- **A personal agenda** of concerts, populated via import or manual entry.
- **A shared catalog** within a space, allowing each member to discover
  concerts added by others and decide to join them.

---

## 2. Actors

| Actor | Description |
|---|---|
| Authenticated user | A person signed in via an external authentication provider. Has a profile and can join one or more spaces. |
| Space | A group a user joins via an invitation link/code. Defines the sharing scope for concerts. |

There is no admin role in v1: all members of a space have equal rights.

---

## 3. Functional modules

### 3.1 Authentication and user profile

- Authentication is handled via an **external provider** (Google, via an
  auth library such as Auth.js). No password management is built in-house.
- On first sign-in, a **username** is suggested based on the name provided
  by the auth provider; the user must confirm or change it before
  continuing.
- The username is **editable at any time** from the profile, subject to
  the following constraints:
  - Length: between 3 and 20 characters
  - Allowed characters: letters, digits, underscore (`_`), hyphen (`-`)
  - **Uniqueness** guaranteed across the entire application
- The avatar defaults to the picture provided by the auth provider. If no
  picture is available, an **initials-based avatar** is generated from the
  username (pattern: colored circle + first 2 letters).

### 3.2 Spaces

- A user joins a space via a **magic invitation link or code**, generated
  by an existing member of the space.
- There is no approval mechanism or admin role in v1: anyone holding the
  link/code can join, and all members have equal rights once inside.
- A user can belong to **multiple spaces** at the same time.
- A space defines the **sharing scope** for concerts: a concert visible
  within a space is only visible to that space's members.

> Not specified in v1, to be addressed if the need is confirmed: space
> name and image, ability to leave a space, revoking an invitation link.

### 3.3 Concert management

Two distinct logical entities structure this module:

- **An event (Event)**: the canonical, shared data for a concert (artist,
  venue, city, date, time). An event exists at the space level and is
  visible to all its members as soon as it is created by any one of them,
  whether imported or entered manually.
- **An attendance (Attendance)**: the link between a user and an event.
  Carries user-specific data: ticket status, personal notes, whether it
  is part of the user's personal agenda.

A user can therefore see an event circulating in their space without
being associated with it, and decide at any time to "pick" that event to
add it to their own agenda (creating an Attendance).

**Adding an event**, two methods:
- **Automatic import** from a ticketing URL (Shotgun, Dice, Ticketmaster):
  the system extracts artist, venue, date, and time.
  - If extraction is uncertain or partial, fields are pre-filled but
    **remain editable** before confirmation. Silently inserting uncertain
    data is never allowed.
- **Manual entry**: artist, venue, date, time, notes, ticket status.

**Editing and deletion**: a user can edit or delete the events and
attendances they created.

**Ticket status** (carried by the Attendance): possible values at a
minimum — "Interested," "Ticket purchased," "Imported from [platform]."
List extensible during the technical phase.

### 3.4 Duplicate detection

When an event is added (via import or manual entry) and a similar event
already exists within the same space, the system must **suggest a merge
rather than silently create a duplicate**.

- **Matching criteria**: date and time match, combined with text
  similarity on artist name and venue (text is normalized — case,
  accents, extra whitespace — before comparison).
- Merging is **never automatic**: it is suggested to the user adding the
  event, who can confirm the merge or still create a separate event.
- A merge must only affect the shared event data (Event); each user's
  attendance (Attendance) remains intact and independent.

### 3.5 Agenda view

- Displayed as a **vertical timeline showing only days that contain at
  least one concert** — no empty days are displayed.
- Concerts are **grouped by date**, with a bold, prominent date header
  (e.g., "SATURDAY, OCT 12").
- Each concert is represented as a card showing: artist name, venue and
  city, ticket status badge, avatars of space members also associated
  with this event, and a direct link to the ticketing page if available.

### 3.6 Filters and search

Three filterable views:

| View | Content |
|---|---|
| My Schedule | Events the user has an active Attendance for |
| Group Schedule | All events visible across the user's space(s) |
| Past Shows | Events whose date has passed, **computed on the fly** (no separately stored status field) |

A **quick search** by artist or venue name is available, applied to the
currently selected view.

---

## 4. Consolidated business rules

1. Space membership is granted via an invitation link/code; no manual
   approval is required in v1.
2. An event is data shared at the space level; an attendance is data
   specific to a user.
3. Duplicate merging is assisted but never automatic.
4. No automatically extracted data (URL import) is inserted without the
   user being able to review it first.
5. A concert's "past" status is a computed value, never stored data.
6. The username is unique across the whole application, not just within
   a space.

---

## 5. Out of scope (v1)

Explicitly excluded to prevent scope creep during development:

- In-app ticket purchasing or a built-in ticketing system
- Messaging or chat between users
- Algorithmic concert recommendations
- Differentiated roles/permissions within a space
- Notifications (push or email)

---

## 6. Open points for technical specifications

These items are functionally settled but involve technical decisions to
be documented in the detailed technical specifications (DTS):

- Exact modeling of the text-similarity algorithm used for duplicate
  detection (threshold, method: Levenshtein or other)
- Choice of authentication provider and associated library
- Strategy for generating and (optionally) expiring space invitation
  links/codes
- UI distinction between "space catalog" and "who's attending what"
  within the Group Schedule view

---

*This document should evolve alongside the data model and the detailed
technical specifications. Any significant functional change after
development has started must be reflected here before being implemented.*
