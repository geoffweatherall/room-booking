# Room Booking — Business Functionality

This document is the authoritative record of the business rules and functionality provided by the Room Booking system. It is written for a non-technical audience and deliberately excludes technical implementation detail.

Going forward, this file is the source of truth for what the system does. New functionality is added here first, in the same bullet style, before any corresponding code or test changes are made. Each commit to this file is intended to double as a release note for stakeholders — a summary of what changed for users, and why.

## Accounts and Sign-In

- Anyone can create their own account (self-service sign-up) by providing a name, email address, and password — no administrator approval is required.
- A new account must be verified before it can be used: after signing up, the user receives a confirmation code by email which they must enter before they can sign in.
- Passwords must be at least 10 characters long and include a lowercase letter and a number. This is deliberately loose (there's no requirement for uppercase letters or a special character) because this is a demo system rather than a real business, and the policy is set to match the demo account below.
- Since this is a demo system, anyone can sign in immediately using a shared, publicly-known demo account instead of creating their own — no sign-up needed. Its email and password are shown directly on the home page for signed-out visitors, next to a sign-in form ready to submit with those details already filled in. The password is randomly generated (not a fixed, guessable word — an earlier one turned out to appear on a public list of compromised passwords) but stays easy to type: lowercase letters and digits only. This applies in every deployment, including a "production" one, since making the system easy to try is the whole point.
- Users who forget their password can reset it themselves without contacting support: they request a reset code by email, then enter that code along with a new password to regain access.
- Requesting a password reset never reveals whether a given email address actually has an account — the same response is shown either way, to protect user privacy.
- Every part of the system other than the home page, sign-in, sign-up, and password-reset screens requires the user to be signed in.
- If someone tries to open a page without being signed in, they are taken to the sign-in screen and automatically returned to the page they originally wanted once they've signed in.
- There are currently no distinct user roles — every signed-in person has identical access to every part of the system. There is no separate "administrator" account type.
- There is currently no self-service way for a user to close or delete their own account.

## People

- Everyone who uses the system — whether they have their own login or not — is represented as a "Person," and a Person's name is required.
- Signing up for an account automatically creates a matching Person record, using the name given at sign-up.
- The system also supports adding people who do not have their own account or login (for example, a visiting guest), and they can then be booked as the organiser or an attendee of a meeting on their behalf. This is no longer possible directly through the web app, though — it currently has no page for adding such a person, a gap to close before this is needed in practice.

## Rooms

- A room is defined by a name and a capacity — the maximum total number of people, including the organiser, that the room can hold for a meeting.
- A room's capacity must be at least 2 people.
- Any signed-in user can add a new room to the system.
- Once a room has been created, its details cannot currently be edited or removed.

## Business Hours

- The business day runs from 08:00 to 17:00. This is the one definition of "business hours" used throughout this document and the system — for example, it is the window shown on the daily room-availability view described below.

## Bookings

- A booking reserves a specific room for a specific block of time, on behalf of an organiser, with an optional list of other attendees.
- Every booking must have a subject; it cannot be left blank.
- A room can only be booked for one meeting at a time: the system will not allow a new booking whose time overlaps with an existing booking for the same room. A meeting is allowed to start the instant an earlier meeting in the same room ends.
- The total number of people at a meeting — the organiser plus all attendees — cannot exceed the room's capacity.
- A meeting's start and finish times must fall on a five-minute boundary (for example 10:15 or 10:20, but not 10:13), keeping scheduling consistent across the system.
- There is no limit on how far in advance, or how far in the past, a meeting can be booked.
- Any signed-in user can create a booking and may organise it on behalf of any person in the system, not only themselves.
- If a booking or room can't be saved because it breaks one or more rules, every problem is reported back at once, so it can be corrected in a single attempt rather than discovered one issue at a time.
- Once a booking has been made there is currently no way to cancel or change it.
- The system currently assumes all users share the same time zone; meeting times are not converted or adjusted between time zones.

## Calendar Views

- A daily room-availability view shows every room for a chosen day (defaulting to today) as a row spanning business hours, with each existing booking shown as a blocked-out segment across the portion of the day it occupies, labelled with as much of the booking's subject as fits.
- A person calendar view shows the meetings a chosen person is organising or attending, as a wall calendar covering the current work week and the five following work weeks (Monday to Friday only). Each day lists its meetings in time order, showing the start and end time, the subject, and the room. The person is chosen from a searchable list — typing any part of a name narrows it down to matching people, which keeps this usable as the number of people grows.
- Once signed in, both views can be reached from the home page and from the main navigation menu ("Availability" and "Calendar"), and each has its own URL reflecting the day or person being viewed, so it can be shared with another signed-in user to show them the same view. Both places default the calendar view to the signed-in user's own calendar.
- These calendar views replace the earlier flat lists of every person, every room, and every booking, which have all been removed — rooms and bookings are now browsed through the daily room-availability view rather than as undifferentiated lists. Forms to add a room or a booking remain available, reached from that view.
- Signed-in users also see two agenda lists on the home page: the meetings they're organising or attending "Today" and "Tomorrow", each sorted by start time and showing the subject, time, and room.
- Signing in doesn't always mean there's a Person to show a calendar for — the demo account and the e2e test account are both examples of a sign-in with no matching Person. Rather than guessing (which used to mean showing a different, unrelated person's private calendar and bookings), the home page shows a clear "your account hasn't been set up properly" message instead, and the navigation menu's "Calendar" item is disabled.
- Signed-out visitors to the home page see none of the above — since every part of the system requires sign-in, there's nothing to show them yet. Instead the home page offers two ways in: an embedded sign-in form pre-filled with the demo account's details (see Accounts and Sign-In) ready to submit, and a "sign up for your own account" section that spells out the three steps involved before showing the sign-up button.

## Notifications

- Users receive an automated email containing a verification code when they sign up, and again if they request a password reset.
- There are currently no email or in-app notifications for bookings — no booking confirmations, no reminders before a meeting, and no cancellation notices.

## Design and User Experience

- The application's visual design follows Google's Material Design language, for a clean, consistent, and familiar look and feel.
- Screens that list information show a clear loading indicator while data is being fetched, and a similar indicator while data is being refreshed in the background.
- Save buttons disable themselves and show a progress indicator while a submission is in progress, preventing accidental duplicate submissions.
- When something goes wrong — whether a connection problem or a broken business rule — the user sees a clear, dismissible message explaining what happened or what needs to be fixed.
- Navigation is a vertical menu down the left of the page (Home, Calendar, Availability, then Sign in/Sign up or Sign out, then About and Feedback), always visible on wider screens; on a narrow screen it collapses behind a menu button that opens the same menu as a flyout. Once signed in, the signed-in user's name and a settings shortcut appear at the bottom of the menu.

## Administration

- There is currently no administrator dashboard, no user or room management beyond what any signed-in user can already do, and no reporting or usage metrics.
- A data-reset function exists that erases all rooms, people, and bookings. It is intended for testing purposes but is not currently restricted to administrators or disabled in customer-facing settings — this is a known gap to close before wider release.

## Not Yet Implemented

The following functionality has been discussed but is not yet built. It is recorded here as a starting point for future enhancements to this document:

- Recurring bookings (for example, a weekly standup meeting).
- Editing or cancelling an existing booking.
- A "find a room" search that suggests the smallest available room for a given list of attendees and time window.
- User roles and administrator permissions.
- Closing/deleting one's own account.
- Signing up or signing in using an existing Google account.
- Usage metrics and reporting.
- A custom brand theme, including a dark mode, in place of the current default Material Design styling.
- An Android app offering the same functionality as the web application.
