# Room Booking — Business Functionality

This document is the authoritative record of the business rules and functionality provided by the Room Booking system. It is written for a non-technical audience and deliberately excludes technical implementation detail.

Going forward, this file is the source of truth for what the system does. New functionality is added here first, in the same bullet style, before any corresponding code or test changes are made. Each commit to this file is intended to double as a release note for stakeholders — a summary of what changed for users, and why.

## Accounts and Sign-In

- Anyone can create their own account (self-service sign-up) by providing a name, email address, and password — no administrator approval is required.
- A new account must be verified before it can be used: after signing up, the user receives a confirmation code by email which they must enter before they can sign in.
- Passwords must be at least 8 characters long and include a mix of uppercase letters, lowercase letters, a number, and a special character.
- Users who forget their password can reset it themselves without contacting support: they request a reset code by email, then enter that code along with a new password to regain access.
- Requesting a password reset never reveals whether a given email address actually has an account — the same response is shown either way, to protect user privacy.
- Every part of the system other than the home page, sign-in, sign-up, and password-reset screens requires the user to be signed in.
- If someone tries to open a page without being signed in, they are taken to the sign-in screen and automatically returned to the page they originally wanted once they've signed in.
- There are currently no distinct user roles — every signed-in person has identical access to every part of the system. There is no separate "administrator" account type.
- There is currently no self-service way for a user to close or delete their own account.

## People

- Everyone who uses the system — whether they have their own login or not — is represented as a "Person," and a Person's name is required.
- Signing up for an account automatically creates a matching Person record, using the name given at sign-up.
- The system also supports adding people who do not have their own account or login (for example, a visiting guest). Any signed-in user can add such a person, and they can then be booked as the organiser or an attendee of a meeting on their behalf.

## Rooms

- A room is defined by a name and a capacity — the maximum total number of people, including the organiser, that the room can hold for a meeting.
- A room's capacity must be at least 2 people.
- Any signed-in user can add a new room to the system.
- Once a room has been created, its details cannot currently be edited or removed.

## Bookings

- A booking reserves a specific room for a specific block of time, on behalf of an organiser, with an optional list of other attendees.
- A room can only be booked for one meeting at a time: the system will not allow a new booking whose time overlaps with an existing booking for the same room. A meeting is allowed to start the instant an earlier meeting in the same room ends.
- The total number of people at a meeting — the organiser plus all attendees — cannot exceed the room's capacity.
- A meeting's start and finish times must fall on a five-minute boundary (for example 10:15 or 10:20, but not 10:13), keeping scheduling consistent across the system.
- There is no limit on how far in advance, or how far in the past, a meeting can be booked.
- Any signed-in user can create a booking and may organise it on behalf of any person in the system, not only themselves.
- If a booking or room can't be saved because it breaks one or more rules, every problem is reported back at once, so it can be corrected in a single attempt rather than discovered one issue at a time.
- Once a booking has been made there is currently no way to cancel or change it.
- A booking does not currently have a title, subject, or description — only a room, organiser, attendees, and time.
- The system currently assumes all users share the same time zone; meeting times are not converted or adjusted between time zones.

## Notifications

- Users receive an automated email containing a verification code when they sign up, and again if they request a password reset.
- There are currently no email or in-app notifications for bookings — no booking confirmations, no reminders before a meeting, and no cancellation notices.

## Design and User Experience

- The application's visual design follows Google's Material Design language, for a clean, consistent, and familiar look and feel.
- Screens that list information show a clear loading indicator while data is being fetched, and a similar indicator while data is being refreshed in the background.
- Save buttons disable themselves and show a progress indicator while a submission is in progress, preventing accidental duplicate submissions.
- When something goes wrong — whether a connection problem or a broken business rule — the user sees a clear, dismissible message explaining what happened or what needs to be fixed.

## Administration

- There is currently no administrator dashboard, no user or room management beyond what any signed-in user can already do, and no reporting or usage metrics.
- A data-reset function exists that erases all rooms, people, and bookings. It is intended for testing purposes but is not currently restricted to administrators or disabled in customer-facing settings — this is a known gap to close before wider release.

## Not Yet Implemented

The following functionality has been discussed but is not yet built. It is recorded here as a starting point for future enhancements to this document:

- Recurring bookings (for example, a weekly standup meeting).
- Editing or cancelling an existing booking.
- A calendar view of bookings, per person and per room.
- A "find a room" search that suggests the smallest available room for a given list of attendees and time window.
- User roles and administrator permissions.
- Closing/deleting one's own account.
- Signing up or signing in using an existing Google account.
- Usage metrics and reporting.
- A custom brand theme, including a dark mode, in place of the current default Material Design styling.
- An Android app offering the same functionality as the web application.
