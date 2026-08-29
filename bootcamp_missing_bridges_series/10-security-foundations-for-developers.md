# 10. Security Foundations for Developers

## Security starts with assumptions about trust

Security is not a final feature added after the application works.

It begins with a rule:

> Never assume external input is safe simply because your own frontend produced it.

Anything crossing a trust boundary should be treated deliberately.

---

# 1. Trust Boundaries

Examples:

```text
browser → server
server → database
server → external API
uploaded file → application
URL parameter → route
form input → server
```

At each boundary ask:

```text
Who controls this value?
What assumptions am I making?
What happens if that assumption is false?
```

---

# 2. Validation

Validation asks:

```text
Is this input acceptable?
```

Rules may include:

```text
type
length
range
format
allowed values
required fields
relationships between fields
```

Frontend validation improves user experience.

Server-side validation protects the system.

Do not rely on the browser to enforce your server's rules.

---

# 3. Sanitization Is Different

Validation asks:

```text
Should I accept this value?
```

Sanitization/escaping asks:

```text
How should this value be transformed or represented safely
in a particular output context?
```

They are related but not interchangeable ideas.

---

# 4. Authentication vs Authorization

Authentication:

```text
Who are you?
```

Authorization:

```text
What are you allowed to do?
```

A user can be logged in and still not be allowed to:

```text
delete another user's account
read admin reports
change another user's order
```

---

# 5. Hashing vs Encryption

Hashing:

```text
one-way transformation
```

Used in appropriate password-storage schemes with purpose-built password-hashing algorithms.

Encryption:

```text
reversible with the correct key
```

Used when the protected data must later be recovered.

Do not store plaintext passwords.

Do not invent your own password algorithm.

---

# 6. Secrets

Examples:

```text
API keys
database passwords
private tokens
signing secrets
```

Do not hard-code secrets into public source code.

Also remember:

> Anything shipped to browser JavaScript can ultimately be inspected by the user.

A frontend environment variable is not automatically private.

---

# 7. SQL Injection

Dangerous pattern:

```js
const query =
  `SELECT * FROM users WHERE name = '${name}'`;
```

Untrusted data is being inserted directly into query syntax.

Prefer parameterized queries/prepared statements.

Core principle:

```text
data should stay data
```

User input should not become executable query structure.

---

# 8. Cross-Site Scripting (XSS)

XSS can occur when untrusted content becomes executable browser content.

Frameworks often escape output by default in common rendering paths.

Developers can bypass those protections.

Be careful whenever you intentionally render raw HTML or otherwise reinterpret untrusted text as markup/code.

Again:

```text
data should stay data
```

---

# 9. CSRF

Cross-Site Request Forgery abuses an authenticated browser context to trigger an unwanted action.

The key beginner lesson:

> A request arriving with credentials does not necessarily prove the user intentionally initiated that action.

Mitigations depend on architecture and may involve:

```text
SameSite cookie configuration
CSRF tokens
origin checks
appropriate auth design
```

---

# 10. CORS

CORS is a browser mechanism controlling whether frontend code from one origin may read certain responses from another origin.

CORS is not:

```text
authentication
authorization
general API protection
```

A non-browser client is not restricted by browser CORS enforcement in the same way.

---

# 11. Least Privilege

Give users and systems only the permissions they need.

Examples:

```text
database account is not root
service token has narrow scope
ordinary user lacks admin permission
process cannot write everywhere
```

Less privilege means less damage when something goes wrong.

---

# 12. Error Messages

Detailed errors are useful during development.

Production responses should avoid exposing unnecessary internal information such as:

```text
stack traces
credentials
secret values
internal paths
raw database internals
```

Log what developers need in an appropriate place.

Return only what the client should know.

---

# 13. Dependency Risk

Third-party packages become part of your application's trust and attack surface.

Good habits include:

```text
lockfiles
dependency audits
version awareness
minimal dependencies
trusted packages
removing unused packages
```

A package can save time, but it also becomes code you depend on.

---

# 14. File Uploads

Uploaded files are untrusted input.

Consider:

```text
size
declared type
actual content
filename
storage location
execution risk
path traversal
malware scanning when appropriate
```

Do not trust a file merely because its extension says `.jpg`.

---

# 15. Rate and Abuse Thinking

Even valid operations can become harmful when repeated.

Ask:

```text
What if this endpoint is called 10,000 times?
What if login is brute-forced?
What if password reset is spammed?
What if expensive search is automated?
```

Security includes abuse resistance.

---

# 16. Security Is Adversarial Edge-Case Thinking

Problem solving normally asks:

```text
What could go wrong accidentally?
```

Security adds:

```text
What could someone intentionally make go wrong?
```

That is a powerful shift in mindset.

---

# Security Feature Review

```text
INPUTS:
Where does outside data enter?

VALIDATION:
What rules are enforced?

IDENTITY:
How do we know who is acting?

AUTHORIZATION:
What are they allowed to do?

SECRETS:
Are sensitive values exposed?

DATABASE:
Are queries parameterized?

OUTPUT:
Could untrusted data become executable content?

ERRORS:
Do responses leak internals?

DEPENDENCIES:
What third-party code is trusted?

RATE / ABUSE:
What happens under repeated use?

LOGGING:
Could logs expose sensitive information?
```

---

# Final Mental Model

Replace:

```text
"The user would never do that."
```

with:

```text
"What guarantee prevents that behavior
from becoming dangerous?"
```

Good security does not depend on good intentions.
