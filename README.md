# TuTime ASP.NET Core backend API

ASP.NET Core 8 Web API and EF Core data layer for the TuTime Flutter attendance app.

## Endpoints

- `POST /api/Auth/register`, `/api/Auth/login`, `/api/Auth/verify-otp`
- `GET/POST/PUT/DELETE /api/EmployeeApi` (write operations require Admin)
- `GET/POST /api/AttendanceApi`; `DELETE /api/AttendanceApi/{id}` (admin cancellation)
- `GET/PUT /api/Profile` (current user's profile)
- `GET/POST /api/ScheduleRequests`; `POST /api/ScheduleRequests/{id}/review` (Admin review)
- `GET /health` and `/swagger`

Attendance records retain GPS coordinates, GPS accuracy, distance from office, and overtime minutes. There is no radius block. The standard shift ends at 17:00 Vietnam time; check-outs after that are recorded as overtime.

## Employee registration and manual account creation

- Successful registration OTP verification automatically creates a linked employee profile, so the employee appears in the Admin employee list. A matching existing manual profile is reused, with its employee code and role preserved.
- Admin can still add employees manually. For a new or unverified account, supply an initial password of 8–128 characters; an optional username can be provided and otherwise defaults to the employee code. The response includes `loginUsername` and `accountCreated`.
- A manually created account can log in directly with its username or email and initial password, then confirm the email OTP to activate the account. A separate registration is unnecessary.
- Adding a matching verified account reuses it without changing its existing username or password. Conflicting names, duplicate emails and duplicate employee codes return a recoverable conflict.
- Updating an unverified employee's code or role preserves its account link. Its email must remain unchanged until the first successful OTP login. Retrying registration cannot overwrite an Admin-provisioned pending account.
- Profile name updates are synchronized with the linked employee. Employees can submit late-arrival/early-departure requests; Admin approves or rejects them, and matching attendance history shows the approved label.

## Local development

Set connection string and JWT/email configuration through environment variables or .NET user-secrets, then run:

```powershell
dotnet restore
dotnet run
```

Production settings belong in Render environment variables. Do not commit passwords, JWT keys, SMTP credentials, or Brevo API keys.

The deployed service is `https://ledoanvantu2-api.onrender.com`; API docs are available at `/swagger`.

## Tests

```powershell
dotnet test tests/LEDOANVANTU2.Api.Tests/LEDOANVANTU2.Api.Tests.csproj --verbosity minimal
```

The 16 backend cases use isolated SQLite databases with relational unique constraints to check OTP registration, automatic linking, code conflicts, manual account creation, activation and password preservation. They do not send network email or write to the deployed database.
