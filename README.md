# TimeMaster

TimeMaster is a time-management + economy app where a user can:

- Visualize their free/occupied time per day in a draggable/board/calendar view  
- Start a timer to lock time for a task/project  
- Categorize tasks (billable / entertaining / custom)  
- Calculate money earned for billable tasks (based on hours × rate)  
- Allow the user to set rules / evaluations for entertaining / custom categories (fun score, money equivalent, etc.)  
- View reports/charts to see what tasks or habits are providing value (time, money, fun, etc.)

---

## Table of Contents

1. [Tech Stack](#tech-stack)  
2. [Data Model](#data-model)  
3. [API Endpoints](#api-endpoints)  
4. [Security & Best Practices](#security--best-practices)  
5. [Frontend Components](#frontend-components)  
6. [How to Run](#how-to-run)  
7. [Daily Task Board](#daily-task-board)  

---

## Tech Stack

- **Frontend**: Angular 20 (signals / zoneless features)  
- **Calendar UI**: FullCalendar for day/week view + drag/drop & resize  
- **Charts / Reports**: Chart.js or ngx-charts  
- **Backend**: ASP.NET Core Web API (C#)  
- **Authentication**: JWT (plus refresh tokens if stretch)  
- **ORM / DB Access**: Entity Framework Core (Code First)  
- **Database**: PostgreSQL (or SQL Server Express / LocalDB if preferred)  
- **Hosting / Deployment**: Docker, cloud VM or PaaS for backend; static host for frontend  

---

## Data Model

| Table | Key Fields |
|---|---|
| **Users** | Id (GUID), Email, PasswordHash, DisplayName, HourlyRateDefault, Timezone, Preferences |
| **Projects** | Id, OwnerId → Users, Name, Color, DefaultRate |
| **Categories** | Id, OwnerId, Name, Type *(Billable|Entertaining|Custom)*, ConfigJson |
| **Tasks / Events** | Id, OwnerId, ProjectId, CategoryId, Title, Notes, StartUtc, EndUtc, IsAllDay, LockedByTimer, Source, CreatedAt, UpdatedAt |
| **TimeEntries** | Id, TaskId, UserId, StartedAtUtc, EndedAtUtc, DurationSeconds, IsBillable, RateAtTime, MoneyCalculated, MetaJson |

---

## API Endpoints 
(REST)

- `POST /api/auth/register` — register new users  
- `POST /api/auth/login` — get JWT token  
- `POST /api/auth/refresh` — refresh token (if implemented)  
- `GET /api/users/me` — get current user profile  
- `PUT /api/users/me` — update profile (rate, settings)  
- `GET /api/tasks?date=YYYY-MM-DD` — list tasks/events for a specific day or range  
- `POST /api/tasks` — create a task/event  
- `PUT /api/tasks/{id}` — update (move / resize / change details)  
- `DELETE /api/tasks/{id}`  
- `POST /api/timer/start` — start timer for a task (or new task)  
- `POST /api/timer/stop` — stop timer, create TimeEntry + do money calc  
- `GET /api/timer/status` — current running timer(s)  
- `GET /api/reports/summary?from=...&to=...&groupBy=category|project`  
- `GET /api/reports/chart-data?...`  

---

## Security & Best Practices

- Use **HTTPS** everywhere.  
- Use JWT with expiration + refresh tokens. Store refresh tokens securely (HttpOnly cookies).  
- Always validate inputs; use DTOs to avoid over-binding models.  
- Use EF Core migrations for schema changes; review generated SQL.  
- Store dates/timestamps in UTC; convert on frontend for display.  
- Handle overlapping tasks / timers; prevent double booking.  
- Implement CORS policies, rate limiting for auth/timer endpoints.  
- Sanitize any user provided content; avoid XSS, etc.

---

## Frontend Components

- Login / Registration pages  
- App shell (navigation, guarding routes)  
- Calendar / Day-board view (FullCalendar)  
- Task/Event editor modal or page  
- Timer component (start / stop)  
- Projects & Categories management  
- Reports & Charts (filters, date ranges, category/proj grouping)  
- Settings (user preferences, rates, category scoring rules)  

---

## How to Run 
(Dev Setup)

```bash
# Backend
cd backend
dotnet new webapi -n TimeMaster.Api
# Add required packages:
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL   # if PostgreSQL
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
dotnet tool install --global dotnet-ef
dotnet ef migrations add InitialCreate
dotnet ef database update

# Frontend
cd frontend
ng new timemaster-frontend --routing --style=scss
npm install @fullcalendar/angular @fullcalendar/daygrid @fullcalendar/interaction chart.js
ng serve
```

## Daily Task Board

Here are 20 work days of tasks. Mark them off as you complete. Adjust if you need more time.
---

# Week 1 — Backend & Data Model
Day	Tasks
- **Day 1**	
- [x] Finalize project scope (MVP vs stretch)
- [x] Create Git repo & branches (main/dev)
- [ ] Setup ASP.NET Core + Angular skeleton projects
- [ ] Setup database (PostgreSQL or SQL Server Express)
- **Day 2**
- [ ] Define EF Core models (Users, Projects, Categories, Tasks, TimeEntries)
- [ ] Configure DB context & connection string
- [ ] Create & apply initial migration
- **Day 3**
- [ ] Implement authentication endpoints (register/login)
- [ ] Configure JWT in ASP.NET Core
- [ ] Add password hashing & basic security
- **Day 4**
- [ ] CRUD endpoints for Tasks & Categories
- [ ] DTOs + validation (DataAnnotations or FluentValidation)
- [ ] Swagger setup for API docs
- **Day 5**
- [ ] Timer start/stop logic in backend
- [ ] Money calculation logic for billable tasks
- [ ] Unit tests for timer & money logic
---

# Week 2 — Frontend Integration & Calendar
Day	Tasks
- **Day 6**	
- [ ] Angular login/register pages & auth service
- [ ] Route guards for protected routes
- **Day 7**
- [ ] Integrate FullCalendar, fetch tasks for a day
- [ ] Show events and edit modal on click
- **Day 8**
- [ ] Enable drag & resize on calendar & wire up update backend
- [ ] Ability to create new task via calendar interaction
- **Day 9**
- [ ] Timer UI component (start/stop) + lock tasks in calendar view
- [ ] Visual feedback for locked slots
- **Day 10**
- [ ] Projects & Categories screens in frontend (CRUD)
- [ ] Category scoring / rate settings UI
---

# Week 3 — Reports, Advanced Backend & Security
Day	Tasks
- **Day 11**	
- [ ] Backend report summary endpoints (group by category/project/date)
- [ ] Aggregation queries in EF Core
- **Day 12**
- [ ] Charts in frontend (time vs money vs score)
- [ ] Filters for date range / category / project
- **Day 13**
- [ ] (Optional) Add real-time features (SignalR) for updates across sessions
- [ ] Frontend subscription to real-time changes
- **Day 14**
- [ ] Add refresh token / better token storage
- [ ] Security hardening: CORS, input sanitization etc.
- **Day 15**
- [ ] Containerize backend & frontend (Docker)
- [ ] Deploy to staging or cloud environment to test production settings
---

# Week 4 — Polish, Testing, Docs & Launch
Day	Tasks
- **Day 16**	
- [ ] UI polish (styles, responsive layout, category color coding)
- [ ] Improve usability & UI feedback
- **Day 17**
- [ ] Export feature (CSV export of time entries)
- [ ] (Optional) Import tasks / bulk operations
- **Day 18**
- [ ] Add unit/integration tests backend; component / E2E tests frontend
- [ ] Profile backend for slow queries and optimize
- **Day 19**
- [ ] Write documentation (README, API docs, setup instructions)
- [ ] Prepare demo screenshots/video
- **Day 20**
- [ ] Final bug fixes, edge cases
- [ ] Final deployment & publish
- [ ] Reflect: lessons learned & next steps
License

MIT License — feel free to reuse or adapt as you like.

# What’s Next 
- Stretch Goals
- Multi-user / team mode so shared calendar / tasks
- Habit tracking
- Pomodoro mode
- Integration with external calendars (Google Calendar import/export)
- Mobile friendly/touch support
