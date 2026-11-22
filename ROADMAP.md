# BloodLink - Phased Development Roadmap

This roadmap breaks BloodLink into small, reviewable phases. Project setup and
code generation will start only after the developer confirms the current phase.

BloodLink is a Flask-based blood request and donor matching portfolio demo.
It is not a medical eligibility system or an emergency notification service.

## Repository and Git workflow

- Repository: `D:\BloodLink`
- Work directly on the `main` branch; do not create a branch per phase.
- After each phase:
  1. Run the tests or checks applicable to that phase.
  2. Review the changed files and confirm no secrets are staged.
  3. Commit the completed phase on `main`.
  4. Push `main` to GitHub.
- Suggested commit format:
  - `feat: complete phase 1 foundation setup`
  - `feat: add database models and migrations`
  - `test: add authentication tests`
- The developer runs all Git and GitHub commands. The assistant provides
  guidance only and does not commit, push, or manage branches.
- Do not push unfinished or failing code unless explicitly requested for
  backup/debugging.

## Status legend

- `[ ]` Not started
- `[~]` In progress
- `[x]` Completed
- `[!]` Blocked

## Technology baseline

- Frontend: HTML, Tailwind CSS, Vanilla JavaScript
- Backend: Python, Flask, REST-style routes
- Database: local MySQL
- ORM and migrations: SQLAlchemy and Flask-Migrate
- Authentication: Flask server-side sessions
- Security: password hashing, CSRF protection, ownership/role authorization
- Testing: pytest
- Optional UI location tool: Leaflet.js
- Tools: Git, GitHub, VS Code

## Demo and privacy rules

- Use fake/demo personal information only.
- Do not expose exact donor locations or public phone numbers.
- Share donor contact details only after explicit donor consent.
- Never present matching as medical approval or compatibility confirmation.
- Eligibility and compatibility require qualified human/hospital review.
- BloodLink must not be presented as the sole option for emergencies.

---

## Phase 1 - Project foundation and local setup

Status: `[x]`

### Goal

A clean Python/Flask project structure should run locally and load configuration
from environment variables.

### Tasks

- [ ] Initialize the Git repository.
- [ ] Create the application package and standard folders.
- [ ] Add `requirements.txt` or an equivalent dependency manifest.
- [ ] Add `.env.example` and ensure `.env` is ignored.
- [ ] Configure Flask application factory pattern.
- [ ] Add MySQL configuration and local database connection setup.
- [ ] Configure SQLAlchemy and Flask-Migrate.
- [ ] Add `.gitignore`, `README.md`, and basic project documentation.
- [ ] Add a health check or basic home route.
- [ ] Confirm that Tailwind and static assets load cleanly.
- [ ] Add pytest configuration and a basic smoke test.

### Main files likely to change

- `app.py` or `wsgi.py`
- `config.py`
- `requirements.txt`
- `.env.example`
- `.gitignore`
- `README.md`
- `pyproject.toml` or `pytest.ini`
- `app/__init__.py`
- `app/routes/main.py`

### Verification

- Flask development server starts successfully.
- Database connection succeeds locally.
- Home/health route returns the expected response.
- pytest discovers and runs at least one smoke test.
- No secret or local environment file is committed.

### Git step

- Stay on `main`
- Commit: `feat: complete BloodLink project foundation`
- Push `main` to GitHub.

---

## Phase 2 - Database models and migrations

Status: `[ ]`

### Goal

Create a normalized database schema with clear relationships and constraints.

### Tasks

- [ ] Define the `User` model.
- [ ] Define roles and role-based authorization data.
- [ ] Define the `DonorProfile` model.
- [ ] Define the `BloodRequest` model.
- [ ] Define the `RequestResponse` model.
- [ ] Define status fields and allowed status values.
- [ ] Add timestamps and useful database constraints.
- [ ] Add model indexes for common lookups.
- [ ] Generate initial Flask-Migrate migration.
- [ ] Apply migration locally.
- [ ] Add model-level validation and representative test fixtures.

### Main data relationships

- One user can have one donor profile.
- One user can create multiple blood requests.
- One blood request can receive multiple donor responses.
- One donor response belongs to one user and one blood request.
- A donor should not be able to respond twice to the same request.

### Verification

- Migration creates all tables and constraints.
- Foreign keys behave as designed.
- Duplicate donor response is rejected.
- Invalid status values are prevented or validated.
- Model tests pass.

### Git step

- Stay on `main`
- Commit: `feat: add BloodLink models and migrations`
- Push `main` to GitHub.

---

## Phase 3 - Authentication and session security

Status: `[ ]`

### Goal

Users can safely register, log in, log out, and access protected resources.

### Tasks

- [ ] Create registration and login forms/routes.
- [ ] Hash passwords with a supported password hashing library.
- [ ] Add Flask secure session configuration.
- [ ] Implement logout and session invalidation.
- [ ] Add CSRF protection for state-changing requests.
- [ ] Add input validation and generic authentication errors.
- [ ] Add login rate limiting for the core demo.
- [ ] Add ownership and role authorization helpers.
- [ ] Prevent public registration of admin accounts.
- [ ] Provide a safe admin account creation mechanism for local setup.
- [ ] Add authentication and authorization tests.

### Verification

- Valid registration and login work.
- Wrong password is rejected.
- Protected pages redirect or return unauthorized responses.
- Logout invalidates the session.
- Missing or invalid CSRF tokens are rejected.
- Normal users cannot access admin routes.
- Admin signup is unavailable.

### Git step

- Stay on `main`
- Commit: `feat: implement secure authentication and authorization`
- Push `main` to GitHub.

---

## Phase 4 - Donor profile and availability

Status: `[ ]`

### Goal

Every normal user can maintain a donor profile and control whether they are
available for matching.

### Tasks

- [ ] Add create/edit donor profile route.
- [ ] Store blood group and approximate city/location.
- [ ] Add optional last donation date.
- [ ] Add availability on/off control.
- [ ] Add optional contact-sharing preference.
- [ ] Hide exact location and phone number from public output.
- [ ] Validate self-reported profile data.
- [ ] Add donor profile pages and mobile-responsive forms.
- [ ] Add create/edit/availability tests.

### Verification

- A logged-in user can create and update one profile.
- Availability state can be toggled.
- Donor location and phone details are not exposed publicly.
- Last donation date does not imply eligibility approval.
- Profile authorization rejects edits by another user.

### Git step

- Stay on `main`
- Commit: `feat: add donor profiles and availability controls`
- Push `main` to GitHub.

---

## Phase 5 - Blood request lifecycle

Status: `[ ]`

### Goal

Requesters can create, review, cancel, fulfill, and track blood requests.

### Tasks

- [ ] Add blood request creation form and route.
- [ ] Store blood group, units, hospital, city, approximate location,
  urgency, and optional notes.
- [ ] Add requester-owned request list/detail pages.
- [ ] Implement Open, Fulfilled, Cancelled, and Expired statuses.
- [ ] Add valid lifecycle transition checks.
- [ ] Add cancel request action.
- [ ] Add requester fulfill confirmation action.
- [ ] Prevent unauthorized status changes.
- [ ] Prevent new responses after cancellation, fulfillment, or expiry.
- [ ] Add request lifecycle tests.

### Status rules

- `Open`: may accept donor responses.
- `Fulfilled`: no longer accepts new responses.
- `Cancelled`: no longer accepts new responses.
- `Expired`: no longer accepts new responses.
- A donor response does not fulfill or complete a request.

### Verification

- Only the requester or authorized workflow can change status.
- A fulfilled/cancelled/expired request rejects new responses.
- Request details are owner-authorized.
- Status transitions cannot bypass lifecycle validation.

### Git step

- Stay on `main`
- Commit: `feat: implement blood request lifecycle`
- Push `main` to GitHub.

---

## Phase 6 - Nearby matching, accept, decline, and consent

Status: `[ ]`

### Goal

Available compatible-blood-group volunteers can discover relevant open requests,
respond, and explicitly consent to contact sharing.

### Tasks

- [ ] Implement matching service in a separate service module.
- [ ] Match only available donors.
- [ ] Match the request blood group by default.
- [ ] Use approximate distance from the hospital/request location.
- [ ] Exclude expired or inactive requests.
- [ ] Add relevant request dashboard for donors.
- [ ] Add accept and decline response actions.
- [ ] Record accept/decline timestamps and requester visibility.
- [ ] Add explicit consent control for sharing contact details.
- [ ] Reveal contact details to the requester only after donor consent.
- [ ] Prevent public and unauthorized contact lookup.
- [ ] Add matching and consent tests.

### Verification

- Only available donors receive potentially relevant matches.
- Blood group matching uses the default same-blood-group rule.
- A donor cannot respond twice to one request.
- Declined/accepted state behaves correctly.
- Contact details remain hidden until explicit consent.
- Request owner can see consented contact details.
- UI clearly states that acceptance is interest, not confirmed donation.

### Git step

- Stay on `main`
- Commit: `feat: add nearby matching and consented responses`
- Push `main` to GitHub.

---

## Phase 7 - Responsive interface and user dashboard

Status: `[ ]`

### Goal

Provide a clean, mobile-first experience for registration, requests, donor
actions, and responses.

### Tasks

- [ ] Add shared base layout and navigation.
- [ ] Create registration/login UI.
- [ ] Create requester dashboard.
- [ ] Create donor dashboard.
- [ ] Add open request list and detail pages.
- [ ] Add response status and consent controls.
- [ ] Add flash/error/empty/loading states.
- [ ] Make all core pages responsive.
- [ ] Add clear demo/disclaimer messaging.
- [ ] Add Leaflet location selection only if confirmed useful.
- [ ] Add basic JavaScript form/UX behavior.
- [ ] Test key pages at mobile and desktop widths.

### Verification

- Core flows work on mobile and desktop widths.
- Navigation reflects logged-in state and role.
- No exact donor phone/location is exposed in HTML or API output.
- Demo disclaimer is visible where relevant.
- There are no broken templates or assets.

### Git step

- Stay on `main`
- Commit: `feat: build responsive BloodLink dashboards`
- Push `main` to GitHub.

---

## Phase 8 - Admin review and audit basics

Status: `[ ]`

### Goal

Authorized admins can review platform activity and suspicious requests without
public role escalation.

### Tasks

- [ ] Add role-based admin route protection.
- [ ] Create basic admin dashboard.
- [ ] Show platform counts and request statistics.
- [ ] Add request review and flag/suspicious status workflow.
- [ ] Create audit log model for important actions.
- [ ] Record actor, action, target type, and timestamp.
- [ ] Avoid unnecessary sensitive data in audit records.
- [ ] Add audit entries for auth and status changes.
- [ ] Add admin authorization and audit tests.

### Verification

- Normal users cannot access admin routes.
- Admin users can review basic statistics and requests.
- Flagged requests cannot be marked verified through the UI unintentionally.
- Audit entries are recorded for important actions.
- Audit records do not contain passwords or unnecessary personal information.

### Git step

- Stay on `main`
- Commit: `feat: add admin dashboard and audit logging`
- Push `main` to GitHub.

---

## Phase 9 - Automated tests, API docs, and hardening

Status: `[ ]`

### Goal

The core application should be testable, documented, and hardened before
deployment.

### Tasks

- [ ] Add route, model, service, authorization, and validation tests.
- [ ] Test success and failure cases for core flows.
- [ ] Add test database setup and fixtures.
- [ ] Add CSRF, session, and rate-limit tests.
- [ ] Add API documentation for REST-style endpoints.
- [ ] Document setup and environment variables.
- [ ] Add request validation and consistent error responses.
- [ ] Run migration upgrade/downgrade checks.
- [ ] Review security and privacy exposure.
- [ ] Add a test script or Makefile-style command if useful.

### Verification

- Full pytest suite passes.
- Core authorization paths are tested.
- Database migrations can be applied cleanly.
- API docs match the implemented endpoints.
- README can guide a fresh local setup.

### Git step

- Stay on `main`
- Commit: `test: add core tests and API documentation`
- Push `main` to GitHub.

---

## Phase 10 - Advanced features and staged rollout

Status: `[ ]`

Implement these features only after the core application is stable and tested.
Complete and verify one advanced subphase at a time, committing each on `main`.

### 10A - Request expiry

- [ ] Add expiry timestamp to request creation.
- [ ] Add an expiry service or scheduled task.
- [ ] Show expired state clearly.
- [ ] Prevent expired request responses.

### 10B - In-app notifications

- [ ] Add notification model.
- [ ] Notify matched/request-accepted users.
- [ ] Add notification read status.
- [ ] Add notification list page.

### 10C - Optional Telegram notifications

- [ ] Confirm opt-in and consent requirements.
- [ ] Add Telegram linking and secure token handling.
- [ ] Send notifications without relying on Telegram for emergencies.
- [ ] Document delivery limitations and failures.

### 10D - Radius escalation

- [ ] Define initial search radius.
- [ ] Escalate only when appropriate and request status remains open.
- [ ] Document and test radius behavior.

### 10E - Analytics

- [ ] Extend admin dashboard metrics.
- [ ] Use aggregated/anonymized information where possible.
- [ ] Avoid exposing unnecessary personal data.

---

## Phase 11 - Demo deployment

Status: `[ ]`

### Goal

Deploy a portfolio demo using free-tier or low-cost services, with clear
limitations and fake demo data.

### Tasks

- [ ] Select current hosting based on available free-tier terms.
- [ ] Select a hosted MySQL or MySQL-compatible database.
- [ ] Configure production environment variables.
- [ ] Configure secure session and CSRF settings.
- [ ] Disable debug mode.
- [ ] Run database migrations on deployment.
- [ ] Add demo seed data clearly marked as fake.
- [ ] Verify the deployed health route.
- [ ] Document cold starts/server sleep and database limits.
- [ ] Document notification reliability limitations.
- [ ] Confirm whether a payment card is required.
- [ ] Add the deployed demo URL to the README.

### Hosting checklist

- Re-check provider free-tier availability before selecting it.
- Document payment-card and trial requirements if applicable.
- Do not claim guaranteed uptime or emergency delivery.
- Keep any real personal information out of demo seed data.

### Git step

- Stay on `main`
- Commit: `deploy: prepare BloodLink demo deployment`
- Push `main` to GitHub.

---

## Progress tracker

| Phase | Name | Status |
|---|---|---|
| 1 | Project foundation | `[x]` |
| 2 | Models and migrations | `[ ]` |
| 3 | Authentication and security | `[ ]` |
| 4 | Donor profile | `[ ]` |
| 5 | Request lifecycle | `[ ]` |
| 6 | Matching and consent | `[ ]` |
| 7 | Responsive interface | `[ ]` |
| 8 | Admin and audit | `[ ]` |
| 9 | Tests and hardening | `[ ]` |
| 10 | Advanced features | `[ ]` |
| 11 | Demo deployment | `[ ]` |

## Next step

Confirm **Phase 1 - Project foundation and local setup**. During that phase, we
will inspect the local MySQL setup, initialize the repository, create the
Flask structure, and verify that the project starts before any feature code is
added.
