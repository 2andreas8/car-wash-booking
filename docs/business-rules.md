# Business Rules

Rules defined by the author, inspired by a real car wash. All of them are assumptions.

## Roles
- CLIENT: browses the catalog and free slots; creates, cancels and reschedules own appointments.
- OPERATOR: sees own schedule; marks appointments as COMPLETED / NO_SHOW.
- ADMIN: everything the others can do, plus manages the catalog, operators and working hours.

## Rules
- R1. Working hours: Mon–Fri 08:00–18:00, Saturday 08:00–15:00, Sunday closed.
- R2. Appointments start on the hour.
- R3. Occupied time = sum of service durations × vehicle multiplier,
  rounded up to a full hour (e.g. 72 min -> 120 min).
- R4. Total price = sum of service prices × vehicle multiplier (price is not rounded to the hour).
- R5. An appointment must end on the same day, before closing time.
  Services taking 14–24 hours are not bookable online in the MVP (bookable = false).
- R6. The bookable resource is the operator: one operator works on one car at a time.
- R7. No overlapping appointments for the same operator, even under concurrent requests.
- R8. If the client does not choose an operator, the system assigns a free one.
- R9. Statuses: CONFIRMED (automatic on creation), CANCELLED, COMPLETED, NO_SHOW.
- R10. A client may cancel at least 2 hours before the start; operators/admins at any time.
- R11. Rescheduling re-runs the availability check, with the same 2-hour limit.
- R12. Appointments in the past cannot be created, cancelled or rescheduled.
- R13. Clients must have an account (authenticated).

## Vehicle multipliers (price and duration)
Car x1.0 | Limousine x1.1 | SUV x1.2 | 4x4/Pickup x1.3

## Decisions
- Price/duration per vehicle type: a multiplier (not a service × vehicle table), for simplicity.
- Automatic confirmation, no manual approval flow.
- Duration rounded up to a full hour: simplifies slot logic, at the cost of the displayed
  duration sometimes being longer than the real one.

## Later
Email notifications, Redis, operator time off, multi-day appointments, frontend, deployment.