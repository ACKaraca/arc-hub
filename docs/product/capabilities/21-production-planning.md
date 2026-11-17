# Production tracking: scheduling, resources and time

Planning tasks over time and people: Gantt scheduling, working calendars, bookings and capacity,
time logs and the comparison of estimates with actual effort.

Domain objects:

- **Working calendar**: working days, hours and holidays of an organization, project or person.
- **Baseline**: a saved copy of task dates for variance reporting.
- **Milestone**: a dated project event linked to deliveries or reviews.
- **Booking**: an allocation of a person or department to a project for a period.
- **Time log**: effort recorded by a user against a task and day.
- **Rate**: a cost per hour of a person or role.

## Scheduling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PLAN-001 | Gantt chart | Task bars per entity and step are moved and resized by dragging; dependencies are finish-to-start, start-to-start, finish-to-finish or start-to-finish with lag; milestones, today line, critical path highlighting and zoom levels are shown. | T2 | 4 |
| HB-PLAN-002 | Working calendars | Organizations and projects define working days, hours and holidays; users add leave to their own calendar. Durations are computed in working time of the assignee's calendar. | T2 | 4 |
| HB-PLAN-003 | Automatic scheduling | The scheduler places tasks respecting dependencies, assignee availability, calendars and priority. The proposal is deterministic for the same input and is applied only after the user reviews the list of date changes. | T2 | 4 |
| HB-PLAN-004 | Baselines | Users save baselines and see variance of current dates against a chosen baseline. | T2 | 4 |
| HB-PLAN-005 | Milestones | Milestones link to deliveries and reviews; tasks whose due dates move past a linked milestone trigger an alert to the producer role. | T2 | 4 |
| HB-PLAN-006 | Scenarios | Users copy the schedule into a scenario, change it freely, compare it with the live schedule and apply it as one change. | T3 | 5 |

## Resources

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PLAN-007 | Resource view | A timeline per person and department shows assigned tasks and bookings with utilization; overallocation is highlighted. | T2 | 4 |
| HB-PLAN-008 | Bookings | People and departments are booked to projects for a period at an allocation percentage; non-project bookings such as leave reduce capacity. | T2 | 4 |
| HB-PLAN-009 | Capacity planning | Demand from task estimates is compared with capacity from people and bookings per department and week, as table and chart. | T3 | 4 |

## Time and cost

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PLAN-010 | Time logs | Users log time per task and day in a weekly timesheet and submit it; approvers accept or return it, and approved weeks are locked. | T2 | 4 |
| HB-PLAN-011 | Estimate and actual | Estimates and logged time are totaled per task, entity, step and project, with a burn-down chart per milestone. | T2 | 4 |
| HB-PLAN-012 | Rates and labor cost | Rates per person or role turn time logs and bookings into labor cost per project in the project currency, exportable as CSV. | T3 | 4 |
| HB-PLAN-013 | Planning exports | Personal tasks export as an iCalendar feed and schedules as Microsoft Project XML and CSV. | T3 | 4 |
