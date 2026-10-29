# TuTime ASP.NET Core backend API

ASP.NET Core 8 Web API and EF Core data layer for the TuTime Flutter attendance app.

## Endpoints

- `POST /api/Auth/register`, `/api/Auth/login`, `/api/Auth/verify-otp`
- `GET/POST/PUT/DELETE /api/EmployeeApi` (write operations require Admin)
- `GET/POST /api/AttendanceApi`; `DELETE /api/AttendanceApi/{id}` (admin cancellation)
- `GET /health` and `/swagger`

Attendance records retain GPS coordinates, GPS accuracy, distance from office, and overtime minutes. There is no radius block. The standard shift ends at 17:00 Vietnam time; check-outs after that are recorded as overtime.

## Local development

Set connection string and JWT/email configuration through environment variables or .NET user-secrets, then run:

```powershell
dotnet restore
dotnet run
```

Production settings belong in Render environment variables. Do not commit passwords, JWT keys, SMTP credentials, or Brevo API keys.

The deployed service is `https://ledoanvantu2-api.onrender.com`; API docs are available at `/swagger`.
