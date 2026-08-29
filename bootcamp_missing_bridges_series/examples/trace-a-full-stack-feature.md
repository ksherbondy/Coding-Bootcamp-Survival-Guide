# Example: Trace a Full-Stack Feature

## Feature

```text
User changes a profile display name and clicks Save.
```

## Trace

```text
1. User edits form input.
2. Frontend state receives the new text.
3. User clicks Save.
4. Click/submit handler runs.
5. Frontend validates basic form requirements.
6. Frontend sends PUT /api/profile.
7. Request body contains JSON.
8. Express receives request.
9. JSON middleware parses request body.
10. Authentication middleware identifies user.
11. Route handler validates the new display name.
12. Service applies business rules.
13. Database UPDATE runs.
14. Database returns the updated record.
15. Server responds 200 with JSON.
16. Frontend awaits the response.
17. Frontend parses JSON.
18. Frontend state is updated.
19. Component renders the new display name.
```

## Debugging Questions

```text
What if step 4 never happens?

What if step 6 sends the wrong URL?

What if step 9 was never configured?

What if step 10 finds no user?

What if step 13 fails?

What if step 15 returns 500?

What if step 17 receives a non-JSON body?

What if step 18 updates the wrong state?
```

The architecture of a real project may differ.

The lesson is the tracing method:

```text
start with the user-visible behavior
follow data and control flow
cross one boundary at a time
find the first point where reality differs from expectation
```
