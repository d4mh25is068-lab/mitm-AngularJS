# CampusFest – College Event Registration System

A modern, responsive college fest registration portal built with **HTML, CSS, AngularJS 1.8 and Supabase**.

## Features included

- Responsive hero page and event card grid
- Category filters: Technical, Cultural, Sports and Hackathon
- Search by event title, category, venue or guest speaker
- Seat availability gauge and sold-out state
- Student email/password sign-up and sign-in (Supabase Auth)
- RSVP form: name, USN, college email and phone number
- My Registrations drawer
- Cancellation before the event's cancellation deadline
- Supabase database with Row Level Security
- Server-side seat reservation function that locks the event row to avoid overbooking
- Demo mode with sample events before Supabase is configured

## Project files

- `index.html` – page structure and AngularJS templates
- `styles.css` – modern responsive design
- `app.js` – AngularJS controller and Supabase integration
- `supabase.sql` – database tables, policies, functions and sample events

## Step-by-step setup

### Step 1 – Create a Supabase project

1. Visit [https://supabase.com](https://supabase.com) and sign in.
2. Choose **New project**.
3. Enter a project name such as `campus-fest-portal`.
4. Create a strong database password and select a nearby region.
5. Wait for the project to finish creating.

### Step 2 – Create the database

1. Open your Supabase project.
2. Select **SQL Editor** from the left menu.
3. Click **New query**.
4. Open `supabase.sql` from this project folder and copy all its contents.
5. Paste it into the SQL Editor and click **Run**.
6. Check **Table Editor**. You should see `events` and `registrations` tables with sample events.

### Step 3 – Configure email authentication

1. In Supabase, open **Authentication → Providers → Email**.
2. Make sure Email provider is enabled.
3. For a classroom demo, you may turn off email confirmation in the Email provider settings. For a real deployment, keep email confirmation enabled and configure your email/redirect settings.
4. In **Authentication → URL Configuration**, add your local development URL to the allowed redirect URLs if prompted.

### Step 4 – Add your Supabase keys

1. Open **Project Settings → API** (or the API Keys page in the current dashboard).
2. Copy the Project URL.
3. Copy the **anon / publishable** key. Never use the `service_role` or secret key in browser JavaScript.
4. Open `app.js`.
5. Replace:
   - `YOUR_SUPABASE_PROJECT_URL` with your Project URL.
   - `YOUR_SUPABASE_ANON_KEY` with your anon/publishable key.
6. Save the file.

### Step 5 – Run the website

Recommended: use Visual Studio Code.

1. Extract the project ZIP.
2. Open the `campus_fest_portal` folder in VS Code.
3. Install the **Live Server** extension if it is not already installed.
4. Right-click `index.html` and select **Open with Live Server**.
5. The site opens in your browser. You can also use another local static web server.
6. Internet access is required because AngularJS, Supabase JS and Google Fonts are loaded from CDNs.

Do not rely on opening the HTML file directly with `file://`; use Live Server or a local web server.

## Step 6 – Test the system

1. Click **Create account** and sign up with an email and password.
2. Confirm the email if email confirmation is enabled.
3. Sign in.
4. Choose an event and click **Register**.
5. Fill in student name, USN, college email and phone number.
6. Click **Confirm my registration**.
7. Open **My registrations** to see the booking.
8. Click **Cancel** to release the seat (before the deadline).

To test duplicate prevention, try registering for the same event again with the same account. The database rejects a second active booking.

## Important implementation notes

- Each student must sign in before making an RSVP. The RSVP email field is collected for the requested form; Supabase Auth email identifies the account.
- A unique `(event_id, user_id)` constraint prevents duplicate rows for the same account and event.
- The SQL RPC locks the event row and updates the seat count inside a database transaction. This is safer than counting seats only in browser JavaScript.
- Students can read only their own registration rows due to Row Level Security.
- The sample event dates are November 20–22, 2026. Change them in the SQL sample data if your fest uses different dates.
- The sample `registered_count` values are demonstration data. Adjust them before using the portal for a real event.
- This starter project has a student-facing interface. Event creation/editing and admin permissions can be added as a separate admin module.
- The browser code uses the Supabase anon/publishable key, which is designed for client use with correct RLS policies. Never expose a service-role key.

## Troubleshooting

**Events do not load:** Confirm that the Project URL and anon/publishable key are correct and that the SQL script ran successfully.

**Sign-up says email confirmation required:** Check your email, confirm the account, then sign in. For a local classroom demo, you can disable email confirmation in Supabase Auth settings.

**Registration fails with permission or function error:** Re-run `supabase.sql` and confirm the RPC functions exist under Database → Functions. Confirm the user is signed in.

**Page looks unstyled:** Check that `styles.css` is in the same folder as `index.html`.

**Changes are not visible:** Refresh the page with Ctrl + Shift + R.

## Suggested enhancements for a later version

- Admin dashboard to create, edit and unpublish events
- Printable event pass with QR code
- CSV export of registrations
- Event banner image upload using Supabase Storage
- Email confirmation / reminder notifications
