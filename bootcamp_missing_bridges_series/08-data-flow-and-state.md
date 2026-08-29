# 08. Data Flow and State

## Where did this value come from, who owns it, and who can change it?

Many frontend and backend bugs are not syntax problems.

They are state problems.

A powerful debugging question is:

```text
What is the life story of this value?
```

---

# 1. What Is State?

State is information that can influence future behavior.

Examples:

```text
logged-in user
shopping cart
selected tab
database record
form input
server cache
game score
```

State can change over time.

---

# 2. Ownership

Ask:

```text
Who owns this value?
```

React example:

```jsx
const [count, setCount] = useState(0);
```

The component containing that state owns it.

A child may receive:

```jsx
<Counter value={count} />
```

but receiving a value is not the same as owning the source of truth.

---

# 3. Source of Truth

If the same information is independently stored in multiple places, those copies can disagree.

Example:

```text
cart items
cart total
```

If both are stored and one changes without the other, state becomes inconsistent.

Often better:

```js
const total = cart.reduce(...);
```

Derive what can be derived from the true source.

---

# 4. Data Flow

A common React model:

```text
state lives above
      ↓ props
children read
      ↓ events/callbacks
owner updates state
```

This direction makes behavior easier to predict.

---

# 5. Props Are Inputs

```jsx
function Greeting({ name }) {
  return <h1>Hello {name}</h1>;
}
```

`name` is input supplied by the parent.

The child reads it.

Ownership remains elsewhere.

---

# 6. State Updates Represent Events

Instead of thinking:

```text
setCount changes a variable
```

think:

```text
something happened,
so the application requests a state transition
```

Example:

```js
setCount(count + 1);
```

represents:

```text
increment event
→ new state should reflect one more
```

---

# 7. Derived State

If a value can be computed from existing state, storing a second independent copy may be unnecessary.

Example:

```js
const completed = todos.filter(todo => todo.done);
```

rather than always maintaining both:

```text
todos
completedTodos
```

The more independent sources of truth you create, the more synchronization work you create.

---

# 8. Server State vs Client State

Client/UI state:

```text
open modal
selected tab
temporary form draft
```

Server state:

```text
users
orders
products
comments
```

Server state often involves:

```text
fetching
caching
refreshing
invalidating
synchronizing
```

Different categories of state have different lifecycles.

---

# 9. Persistence

Ask:

```text
Should this survive a refresh?
Should this survive closing the browser?
Should this be shared with other users/devices?
```

Possible storage layers:

```text
component state
URL
sessionStorage
localStorage
server memory
database
```

Choosing storage is partly choosing lifetime and ownership.

---

# 10. State Machines

Sometimes unrelated booleans allow impossible combinations.

Example:

```js
isLoading
isSuccess
isError
```

Potentially:

```text
true
true
true
```

A state machine style:

```js
status = "idle" | "loading" | "success" | "error"
```

makes invalid combinations harder to represent.

---

# 11. Trace State Changes

Suppose:

```text
cart shows quantity 2
checkout sends quantity 1
```

Trace:

```text
Where is quantity created?
Where is it stored?
Which component reads it?
Which function updates it?
What request body uses it?
```

Debug the value, not just the screen.

---

# 12. Mutation vs Replacement

Mutation:

```js
user.name = "Ada";
```

Replacement:

```js
user = {
  ...user,
  name: "Ada"
};
```

Frameworks such as React often rely on replacement patterns because object identity changes can help signal new state.

---

# 13. Events as State Transitions

A useful way to model an application:

```text
current state
   +
event
   ↓
next state
```

Example:

```text
cart = []
+
ADD_ITEM
↓
cart = [item]
```

This framing scales from small components to reducers and state machines.

---

# 14. State at System Boundaries

When state crosses a boundary:

```text
browser → server
server → database
server → external API
```

it often changes representation.

Example:

```text
JavaScript object
→ JSON
→ request body
→ parsed server object
→ SQL parameters
```

Never assume a value remains the same object or reference across systems.

---

# Data Flow Checklist

```text
VALUE / STATE:

WHERE CREATED:

SOURCE OF TRUTH:

OWNER:

WHO READS IT:

WHO WRITES IT:

HOW IT CHANGES:

IS IT DERIVED?:

SHOULD IT PERSIST?:

WHERE IS IT PERSISTED?:

WHAT BOUNDARIES DOES IT CROSS?:

WHAT STATES ARE VALID?:

WHAT STATES SHOULD BE IMPOSSIBLE?:
```

---

# Final Mental Model

For any important value, be able to answer:

```text
Where did it come from?
Who owns it?
Who can change it?
When does it change?
Where does it go next?
```

If you can answer those questions, many state bugs stop being mysterious.
