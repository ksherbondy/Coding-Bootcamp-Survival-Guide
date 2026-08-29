# 03. Reading Other People's Code

## How to enter an unfamiliar repository without reading every file

One of the biggest transitions from exercises to real development is this:

```text
In an exercise, you are shown the function.

In a project, you first have to find the function.
```

An unfamiliar repository can contain hundreds or thousands of files. Trying to read everything from the top is usually the wrong strategy.

The goal is to build a map.

---

# The Core Rule

Do not ask:

```text
How do I understand this entire repository?
```

Ask:

```text
What path through the repository explains the behavior I care about?
```

---

# 1. Start With the Shape

Before opening random files, inspect the structure.

Look for:

```text
package.json
src/
app/
server/
client/
routes/
controllers/
services/
models/
components/
tests/
config/
```

Directory names give clues about architecture.

A `structure.md` or `tree` output is extremely valuable because it lets you see the project before you enter it.

---

# 2. Read `package.json`

For Node/JavaScript projects, `package.json` is often the fastest orientation tool.

Look at:

```json
{
  "scripts": {},
  "dependencies": {},
  "devDependencies": {}
}
```

Ask:

```text
How is the app started?
How is it tested?
What framework is used?
What database library appears?
What build tool appears?
Is this frontend, backend, or both?
```

Scripts may reveal entry points:

```json
"dev": "node src/server.js"
```

Now you know where to begin.

---

# 3. Find the Entry Point

Common names include:

```text
server.js
index.js
main.js
app.js
main.jsx
index.tsx
```

The entry point usually reveals:

```text
what gets initialized
what major modules are imported
what starts first
```

Express:

```js
const app = express();
app.use(...);
app.listen(...);
```

React:

```jsx
createRoot(...).render(<App />);
```

---

# 4. Follow Imports, Not Filenames

Suppose you find:

```js
import userRouter from "./routes/users.js";
```

That import gives you the next meaningful location.

Then:

```js
router.get("/:id", getUser);
```

points to `getUser`.

Then:

```js
userService.findById(id);
```

points to the service.

You are following behavior:

```text
entry
→ route
→ controller
→ service
→ database
```

This is more useful than reading every utility in alphabetical order.

---

# 5. Trace One Feature

Pick one concrete behavior:

```text
login
load products
submit form
delete user
save comment
render dashboard
```

Trace only that feature.

```text
UI event
→ function
→ request
→ route
→ controller
→ service
→ database
→ response
→ state update
```

A large repository becomes a small path.

---

# 6. Separate Structure From Detail

On the first pass, ignore implementation details you do not need.

If you see:

```js
const result = validateAndNormalizeAndStoreUser(input);
```

your first conclusion may simply be:

```text
This is where user input is processed.
```

You do not need to open that function until your task depends on it.

This is like changing zoom levels on a map.

---

# 7. Build a Scratch Architecture Map

Write notes:

```text
server.js
  → mounts /api/users

routes/users.js
  → GET /:id calls getUser

controllers/users.js
  → reads req.params.id
  → calls userService

services/users.js
  → business rules
  → calls repository

repositories/users.js
  → SQL query
```

Externalizing the map reduces cognitive load.

---

# 8. Read Names as Claims

A function called:

```js
validateUser
```

claims that it validates a user.

Check whether behavior matches the name.

When names and behavior disagree, unfamiliar code becomes much harder to understand.

---

# 9. Find State

For frontend code ask:

```text
Where is state created?
Who owns it?
Who receives it?
Who changes it?
```

Example:

```jsx
const [cart, setCart] = useState([]);
```

Search for:

```text
cart
setCart
```

You can trace the life of that value.

---

# 10. Find Boundaries

Important boundaries include:

```text
browser ↔ server
server ↔ database
module ↔ module
component ↔ component
application ↔ external API
```

At boundaries ask:

```text
What format crosses?
Who validates it?
What assumptions are being made?
```

Bugs often live at boundaries because that is where two different parts must agree.

---

# 11. Search Strategically

Useful searches:

```text
function name
route path
error message
component name
database table
environment variable
API endpoint
```

If the UI literally says:

```text
"Failed to load profile"
```

search that exact text.

It may take you directly to the relevant code path.

---

# 12. Tests Can Be Documentation

A test such as:

```js
it("returns 404 when user does not exist", ...)
```

tells you intended behavior even if the implementation is complicated.

Tests often answer:

```text
What is this code supposed to guarantee?
```

---

# 13. Git History Can Explain Strange Code

Sometimes the current code only makes sense with history.

Useful tools:

```bash
git log
git blame
```

And on hosted repositories:

```text
pull request history
commit messages
issues
```

Use them when asking:

```text
Why was this added?
Was this a bug fix?
Is this temporary?
```

---

# 14. Do Not Chase Every Abstraction

It is okay to decide:

```text
I know this helper exists.
I do not need to understand it yet.
```

Understanding is task-scoped.

Professional developers constantly ignore irrelevant parts of large systems.

---

# Repository Reading Checklist

```text
1. What language/runtime is this?
2. What does package.json/config tell me?
3. How do I start it?
4. How do I test it?
5. What is the entry point?
6. What are the major folders?
7. What feature am I tracing?
8. What route/component/function starts it?
9. What imports does it follow?
10. Where does state/data change?
11. What boundaries are crossed?
12. What tests describe expected behavior?
```

---

# Practice

Take an unfamiliar project and answer only:

```text
How does one button click eventually change persistent data?
```

Do not read the whole repository.

Find the path.

That is the skill.
