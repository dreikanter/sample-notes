# Team standup notes — September 8

Brief notes from the Monday morning sync. Six people present, two async.

## Updates

**Platform team:** Deployment pipeline is back to green after last week's flakiness. The root cause was a race condition in the health-check polling interval—Mateus found it by reading the Kubernetes controller logs carefully rather than relying on dashboard alerts.

**Data team:** The warehouse migration is 80% complete. Remaining tables are the event-stream ones with heavy foreign key dependencies. Estimated completion by end of week.

**Product:** Reviewed mockups for the new notification preferences screen. Feedback was that the toggle density felt overwhelming. Going back to the designer with a request to group by frequency rather than by type.

**Infra:** Upgraded cluster nodes to the latest AMI. No incidents. Will run load tests Wednesday before declaring it stable.

## Blockers

- Lena is blocked on the auth refactor until the backend enum change is merged—PR has been open four days, needs a second reviewer.
- No test environment for the payment sandbox. Ticket created ([project tracker](https://linear.app/)), assigned to platform.

## Decisions

Agreed to move standups from 9:30 to 9:00 starting next Monday to free the late-morning block for focused work. Will revisit after four weeks.

## Action items

- [ ] Review Lena's auth PR today
- [ ] Schedule load test with Infra for Wednesday 2 p.m.
- [ ] Product to share revised notification mockups by Thursday
