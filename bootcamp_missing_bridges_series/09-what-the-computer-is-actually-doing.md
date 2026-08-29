# 09. What the Computer Is Actually Doing

## A bootcamp-level guide to the machinery beneath the abstraction

You do not need to become a CPU architect to write web applications.

But understanding a few lower-level ideas makes high-level behavior easier to reason about.

---

# 1. Programs Are Instructions and Data

At a high level:

```text
program
=
instructions
+
data
```

The CPU executes instructions.

Memory holds working data and code.

Storage keeps data persistently.

---

# 2. CPU

A CPU performs lower-level operations such as:

```text
load
store
add
compare
branch
shift
call
return
```

High-level operations eventually become work the processor can execute.

Example:

```js
x = x + 1;
```

conceptually requires:

```text
obtain x
add 1
store result
```

The exact generated machine instructions depend on the runtime and optimization state, but the high-level expression still becomes real computation.

---

# 3. Memory

RAM is fast temporary working memory.

Running programs need memory for things such as:

```text
variables
objects
arrays
function execution state
buffers
code
runtime metadata
```

High-level languages manage much of this automatically.

---

# 4. Addresses and References

Memory locations have addresses.

Lower-level languages can expose addresses through pointers.

JavaScript hides raw pointers, but reference behavior still matters.

```js
const a = { value: 1 };
const b = a;

b.value = 5;

console.log(a.value); // 5
```

`a` and `b` refer to the same object.

You did not create an independent copy.

---

# 5. Primitive Values vs Objects in JavaScript

Primitive example:

```js
let a = 5;
let b = a;

b = 10;
```

`a` stays `5`.

Object example:

```js
let a = { n: 5 };
let b = a;

b.n = 10;
```

Both names observe the same object.

This distinction is fundamental to mutation and data flow.

---

# 6. Stack and Heap as a Simplified Mental Model

A useful simplification:

```text
STACK
active function calls
execution bookkeeping

HEAP
dynamically managed objects/data
```

Real JavaScript engines are much more sophisticated than this diagram.

Still, the model helps explain:

```text
recursive call depth
object allocation
garbage collection
memory pressure
```

---

# 7. Call Stack

Each active function requires execution information.

Conceptually:

```text
return location
parameters
local bindings
temporary values
```

Deep recursion means many active calls.

That is why sufficiently deep recursion can exhaust call-stack capacity.

---

# 8. Heap Allocation and Space Complexity

Objects, arrays, closures, and runtime structures consume memory.

```js
const users = new Array(1_000_000);
```

has a very different memory footprint from:

```js
const users = [];
```

Space complexity eventually becomes physical memory consumption.

---

# 9. Garbage Collection

JavaScript normally reclaims memory automatically.

Simplified rule:

```text
if allocated data is no longer reachable,
the runtime may eventually reclaim it
```

You do not normally call `free()` manually.

But memory leaks are still possible when code unintentionally keeps references alive.

---

# 10. Bits, Bytes, and Representation

Everything eventually needs representation:

```text
text
numbers
images
audio
network packets
files
```

are encoded as bits and bytes.

A character is not necessarily one byte.

UTF-8, for example, uses variable-length encodings.

---

# 11. Numbers Are Representations Too

JavaScript `Number` uses IEEE-754 double-precision floating-point semantics.

This is why:

```js
0.1 + 0.2 === 0.3
```

is false.

Many decimal fractions cannot be represented exactly in binary floating point.

---

# 12. Bitwise Operations

Binary operators manipulate bit representations.

Example odd check:

```js
5 & 1
```

Binary:

```text
0101
0001
----
0001
```

The low bit is set, so 5 is odd.

Important JavaScript caveat:

> Traditional bitwise operators coerce `Number` values into 32-bit integer representations.

So they are not a general replacement for arithmetic on every JavaScript number.

---

# 13. Processes

A process is a running program instance with its own execution environment and resources.

Your browser and Node server are separate processes.

They do not normally share ordinary JavaScript application memory.

They communicate using mechanisms such as:

```text
network sockets
IPC
files
```

---

# 14. Threads

Threads are execution paths within a process.

JavaScript application code often feels single-threaded because one JavaScript call stack executes at a time within a context.

But runtimes can use other threads internally for:

```text
I/O support
garbage collection
compilation
worker pools
browser rendering
```

Do not confuse:

```text
my JavaScript call stack
```

with:

```text
the entire computer only doing one thing
```

---

# 15. I/O Is Expensive Relative to Simple CPU Work

Network, disk, and user input involve waiting.

That is one reason asynchronous systems exist.

Instead of making the main execution path sit idle:

```text
start I/O
do other work
continue when result is ready
```

---

# 16. Files Are Operating-System Resources

Applications ask the operating system to perform operations such as:

```text
open
read
write
close
```

The language/runtime gives you higher-level APIs around those system capabilities.

---

# 17. Network Sockets

A socket is an operating-system abstraction used for communication.

Your high-level:

```js
fetch(url)
```

eventually depends on layers beneath it:

```text
browser API
HTTP handling
TLS when applicable
socket
operating system network stack
network
```

Abstraction is useful.

Understanding that layers exist helps when debugging.

---

# 18. Language vs Runtime

JavaScript the language defines things such as:

```text
syntax
functions
objects
operators
control flow
```

A runtime such as a browser or Node provides the execution environment and additional APIs.

Browser:

```js
document
window
fetch
```

Node:

```js
process
Buffer
filesystem APIs
```

That is why not every JavaScript environment exposes the same globals.

---

# 19. Expand One Layer When Something Feels Magical

```text
array method
→ iteration-like work

function call
→ execution state + return

object
→ allocated runtime-managed memory

fetch
→ runtime + network I/O

database query
→ driver + process/network/file work

file read
→ runtime + operating system
```

You do not need every transistor-level detail.

You need enough lower layers to know where the behavior comes from.

---

# Final Mental Model

Whenever an abstraction feels magical:

```text
expand it one layer
```

Then ask:

```text
Who actually performs this work?
Where does the data live?
What representation is being used?
What resource is being consumed?
What boundary is being crossed?
```
