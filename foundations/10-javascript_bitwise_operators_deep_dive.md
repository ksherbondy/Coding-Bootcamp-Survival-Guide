# Bitwise Operators in JavaScript: A Low-Level Practical Guide

## Overview

This lesson is a deep dive into JavaScript's four core bitwise operators:

```js
&   // AND
|   // OR
^   // XOR
~   // NOT
```

The goal is not just to memorize what they do. The goal is to understand what is happening at the bit level, why these operations are useful, where they appear in real systems, how JavaScript handles them internally, and how to recognize problems that are naturally solved with bit manipulation.

Bitwise operations show up heavily in embedded systems, operating systems, networking, binary file formats, graphics, game engines, compression, encryption and hashing, permissions, device registers, protocol parsing, databases, parsers, compilers, data packing, and algorithmic problems.

---

# 1. The Mental Model: Bits Are Tiny Switches

A bit can hold exactly two states:

```text
0 = off
1 = on
```

A group of bits can represent a number.

```text
8 4 2 1
-------
0 0 0 0 = 0
0 0 0 1 = 1
0 0 1 0 = 2
0 0 1 1 = 3
0 1 0 0 = 4
0 1 0 1 = 5
0 1 1 0 = 6
0 1 1 1 = 7
1 0 0 0 = 8
1 0 0 1 = 9
1 0 1 0 = 10
1 0 1 1 = 11
1 1 0 0 = 12
1 1 0 1 = 13
1 1 1 0 = 14
1 1 1 1 = 15
```

Each position represents a power of two:

```text
2^3  2^2  2^1  2^0
 8    4    2    1
```

So `1011` means `8 + 2 + 1 = 11`.

Bitwise operators work on these individual positions.

---

# 2. JavaScript-Specific Behavior: 32-Bit Integers

Normal JavaScript numbers are IEEE-754 double-precision floating-point values. But most JavaScript bitwise operators convert a `Number` into a signed 32-bit integer before operating on it.

Conceptually:

```text
JavaScript Number
      ↓
convert to signed 32-bit integer
      ↓
perform bitwise operation
      ↓
convert result back to JavaScript Number
```

The signed 32-bit range is:

```text
-2,147,483,648 through 2,147,483,647
```

So bitwise operators should not be blindly used with arbitrary large `Number` values.

JavaScript also supports bitwise operations on `BigInt`:

```js
const a = 0b1010n;
const b = 0b1100n;

console.log(a & b);
```

Do not mix `Number` and `BigInt` operands.

---

# 3. Bitwise AND: `&`

## Rule

AND only produces `1` when both bits are `1`.

```text
0 & 0 = 0
0 & 1 = 0
1 & 0 = 0
1 & 1 = 1
```

Example:

```text
  1010
& 1100
------
  1000
```

Mental model:

> Keep only the bits that exist in both values.

## Use case: checking a flag

```js
const READ    = 0b0001;
const WRITE   = 0b0010;
const EXECUTE = 0b0100;
const ADMIN   = 0b1000;

const permissions = 0b0101;

if (permissions & EXECUTE) {
  console.log("Execute permission enabled");
}
```

Binary:

```text
permissions  0101
EXECUTE      0100
             ----
             0100
```

This is called masking. `EXECUTE` is the mask.

## Use case: extracting part of a byte

```js
const value = 0b11010110;
const lowNibble = value & 0b00001111;

console.log(lowNibble); // 6
```

```text
11010110
00001111
--------
00000110
```

This is common in packet parsing, file formats, RGB values, CPU instructions, hardware registers, and binary protocols.

## Use case: testing odd/even

```js
if (number & 1) {
  console.log("odd");
} else {
  console.log("even");
}
```

Every odd binary number ends in `1`.

## Use case: checking multiple required flags

```js
const REQUIRED = READ | WRITE;

if ((permissions & REQUIRED) === REQUIRED) {
  console.log("Has both");
}
```

## Use case: power-of-two alignment

For power-of-two alignment, low bits can reveal whether a number is aligned.

```js
const alignedTo8 = (value & 7) === 0;
```

Any multiple of 8 has its bottom three bits clear.

This appears in memory allocators, buffers, SIMD, CPU data alignment, and block storage.

## Use case: color channel extraction

For a packed color value:

```js
const blue = color & 0xFF;
const green = (color >> 8) & 0xFF;
const red = (color >> 16) & 0xFF;
```

---

# 4. Bitwise OR: `|`

## Rule

OR produces `1` when either bit is `1`.

```text
0 | 0 = 0
0 | 1 = 1
1 | 0 = 1
1 | 1 = 1
```

```text
  1010
| 0101
------
  1111
```

Mental model:

> Turn on any bit requested by either side.

## Use case: enabling flags

```js
let permissions = READ;
permissions |= WRITE;
```

```text
0001
0010
----
0011
```

## Use case: combining configuration options

```js
const VERBOSE = 0b0001;
const CACHE   = 0b0010;
const DEBUG   = 0b0100;
const SAFE    = 0b1000;

const config = VERBOSE | CACHE | SAFE;
```

One integer now holds several independent Boolean options.

## Use case: hardware control registers

A register might encode:

```text
bit 0 = device enabled
bit 1 = interrupts enabled
bit 2 = turbo mode
bit 3 = reset
```

To enable device and interrupts while preserving other bits:

```text
register |= ENABLE | INTERRUPTS
```

## Use case: bit-set union

If two values represent feature sets, OR gives the union of both sets.

Applications include permissions, feature flags, CPU capability masks, game abilities, and protocol capability negotiation.

---

# 5. XOR: `^`

## Rule

XOR means exclusive OR. It produces `1` when the bits are different.

```text
0 ^ 0 = 0
0 ^ 1 = 1
1 ^ 0 = 1
1 ^ 1 = 0
```

Mental models:

> Same disappears. Different survives.

and

> Toggle selected bits.

## Useful identities

```text
x ^ 0 = x
x ^ x = 0
a ^ b = b ^ a
(a ^ b) ^ c = a ^ (b ^ c)
```

## Use case: find the unpaired integer

```js
function findUnique(numbers) {
  let result = 0;

  for (const number of numbers) {
    result ^= number;
  }

  return result;
}
```

For:

```js
[4, 1, 2, 1, 2]
```

pairs cancel, leaving `4`.

```text
Time:  O(n)
Space: O(1)
```

Important: this works only when the input obeys the required pattern.

## Use case: toggle a flag

```js
state ^= FLAG;
```

On becomes off. Off becomes on.

## Use case: finding changed bits

```text
old: 10110100
new: 10100110
XOR: 00010010
```

The `1` positions tell you exactly which bits changed.

This is useful in hardware state tracking, protocol state comparison, compact game state, device configuration, and binary diffs.

## Use case: parity

XOR is useful for parity calculations in communication and storage systems.

## Use case: RAID parity

If:

```text
A ^ B = P
```

then:

```text
P ^ B = A
```

because the repeated `B` cancels itself.

This property is useful for reconstructing missing data in XOR-based parity systems.

## Use case: cryptographic building block

XOR appears constantly in cryptography and hashing because it is reversible and cheap.

Important: XOR alone is not secure encryption.

## Use case: delta/difference masks

```js
const difference = before ^ after;
```

The result records which bits changed. Applying the same difference mask again returns the other state:

```text
before ^ difference = after
```

---

# 6. Bitwise NOT: `~`

NOT flips every bit.

```text
0 → 1
1 → 0
```

Conceptual 8-bit example:

```text
10110010
↓
01001101
```

JavaScript applies `~` to signed 32-bit integers.

Useful identity:

```text
~x = -(x + 1)
```

Examples:

```text
~0  = -1
~1  = -2
~5  = -6
~10 = -11
```

## Two's complement

Most modern computers represent signed integers using two's complement.

To get `-5` in an 8-bit example:

```text
+5      00000101
invert  11111010
add 1   11111011
```

Two's complement lets hardware use the same basic adder circuitry for positive and negative arithmetic.

## Use case: clearing a flag

```js
const WRITE = 0b0010;
let permissions = 0b0111;

permissions &= ~WRITE;
```

Conceptually:

```text
WRITE       0010
~WRITE      1101
permissions 0111
            ----
result      0101
```

---

# 7. The Four Core Flag Operations

These are worth memorizing:

```js
flags & FLAG       // check
flags |= FLAG      // enable
flags &= ~FLAG     // disable
flags ^= FLAG      // toggle
```

Mental model:

```text
&   CHECK
|   ENABLE
^   TOGGLE
& ~ DISABLE
```

---

# 8. Bit Masks as Tiny Sets

Suppose:

```text
bit 0 = fire
bit 1 = water
bit 2 = earth
bit 3 = air
```

Player A:

```text
0101 = {fire, earth}
```

Player B:

```text
0110 = {water, earth}
```

Intersection:

```text
A & B = 0100
```

Union:

```text
A | B = 0111
```

Symmetric difference:

```text
A ^ B = 0011
```

Bits can therefore act like an extremely compact fixed-size Set.

---

# 9. Real-World Use: Unix File Permissions

Unix permissions map naturally to bits:

```text
read    = 4 = 100
write   = 2 = 010
execute = 1 = 001
```

Therefore:

```text
7 = 111 = rwx
6 = 110 = rw-
5 = 101 = r-x
4 = 100 = r--
```

This is why:

```bash
chmod 755 file
```

means:

```text
owner: rwx
group: r-x
other: r-x
```

---

# 10. Real-World Use: Network Protocol Headers

Packet headers often pack many flags into a small field.

A byte could conceptually contain:

```text
bit 7 = ACK
bit 6 = SYN
bit 5 = FIN
bit 4 = RST
...
```

Masks answer questions like:

```text
Is SYN set?
Is ACK set?
Which flags changed?
Which capabilities are shared?
```

Protocols pack bits because bandwidth matters.

---

# 11. Real-World Use: Embedded Device Registers

A device register may look like:

```text
bit 0 = sensor enabled
bit 1 = heater enabled
bit 2 = interrupt enabled
bit 3 = error
bit 4 = calibration active
bit 5 = data ready
bit 6 = reserved
bit 7 = reset
```

Operations:

```text
check data ready     register & DATA_READY
enable interrupt     register |= INTERRUPT
disable heater       register &= ~HEATER
compare old/new      old ^ new
```

This is normal embedded programming.

---

# 12. Real-World Use: CPU Status Registers

Processors maintain status flags such as:

- zero,
- carry,
- overflow,
- sign,
- interrupt state.

Machine instructions test and modify individual bits. High-level languages usually hide these details, but they are fundamental to CPU execution.

---

# 13. Real-World Use: Binary File Formats

A file format might define one byte as:

```text
bits 0-2 = compression mode
bit 3    = encrypted
bits 4-7 = version
```

Extraction might look like:

```js
const compression = byte & 0b00000111;
const encrypted = (byte & 0b00001000) !== 0;
const version = (byte >> 4) & 0b1111;
```

Bit manipulation is fundamental to image, audio, archive, executable, and proprietary binary formats.

---

# 14. Real-World Use: UTF-8

UTF-8 uses recognizable leading-bit patterns.

```text
0xxxxxxx                        one-byte sequence
110xxxxx 10xxxxxx               two-byte sequence
1110xxxx 10xxxxxx 10xxxxxx      three-byte sequence
11110xxx 10xxxxxx 10xxxxxx ...  four-byte sequence
```

Parsers use masks to classify bytes and extract payload bits.

This is a direct example of bit operations underlying text processing.

---

# 15. Real-World Use: RGB / RGBA Pixels

A 32-bit value can hold four 8-bit channels.

Conceptually:

```text
AARRGGBB
```

You can extract channels with masks and shifts and construct colors with shifts plus OR.

Applications:

- Canvas,
- WebGL,
- image processing,
- game engines,
- codecs.

---

# 16. Real-World Use: Game Entity Flags

A game can encode traits:

```text
bit 0 = player
bit 1 = enemy
bit 2 = flying
bit 3 = visible
bit 4 = damageable
bit 5 = NPC
```

A system can test thousands of entities using masks.

This is especially useful in entity-component-system designs and performance-sensitive simulations.

---

# 17. Real-World Use: Collision Layers

Game engines often represent collision categories as masks.

```text
bit 0 = player
bit 1 = enemy
bit 2 = projectile
bit 3 = terrain
```

A projectile can declare that it collides with:

```text
enemy | terrain
```

Filtering then becomes a fast bit test.

---

# 18. Real-World Use: Feature and Capability Flags

Bits can represent:

```text
dark mode
beta feature
analytics
experimental renderer
offline mode
```

This is useful when flags are serialized, transmitted, stored very densely, or shared with low-level/native systems.

For ordinary application code, readable objects may still be better.

---

# 19. Real-World Use: Database Bit Fields

Some databases support bit and bitmask columns. One integer can represent many Boolean flags.

Advantages:

- compact storage,
- fast bit tests.

Disadvantages:

- less obvious than named columns,
- harder to query and maintain in some systems.

Use bit fields when flags are truly the natural domain representation, not simply because bit tricks look efficient.

---

# 20. Real-World Use: Bloom Filters

Bloom filters use a bit array plus several hash functions.

Insert:

> Hash an item to several positions and turn those bits on.

Query:

> Check whether all corresponding bits are on.

They can answer:

> definitely not present

or

> possibly present

They are used in databases, caches, storage systems, and distributed systems.

---

# 21. Real-World Use: Bitmaps / Bitsets

Tracking one million yes/no states requires one million bits if represented ideally.

```text
1,000,000 bits / 8 = 125,000 bytes ≈ 122 KB
```

Bitsets are useful for:

- database indexes,
- search engines,
- schedulers,
- memory allocators,
- operating systems,
- large simulations.

---

# 22. Real-World Use: Memory Allocation Maps

An allocator can track blocks with:

```text
0 = free
1 = occupied
```

A long bit string becomes a compact allocation map.

Masks can inspect, reserve, and release blocks.

---

# 23. Real-World Use: CPU Instruction Encoding

A hypothetical instruction might be:

```text
OOOO RRRR IIIIIIII
```

where:

```text
OOOO     opcode
RRRR     register
IIIIIIII immediate value
```

Masks and shifts decode each field.

This is how assemblers, disassemblers, emulators, and virtual machines work with machine instructions.

---

# 24. Real-World Use: Emulators

If you build an emulator such as CHIP-8, Game Boy, NES, or another CPU/VM, bit operations appear everywhere:

- decode opcodes,
- extract register indices,
- manipulate status flags,
- combine bytes,
- inspect pixels,
- emulate hardware state.

Bitwise operators become normal vocabulary.

---

# 25. Real-World Use: Compression

Compression formats often pack symbols into fewer than 8 bits.

If a symbol only requires 3 bits, storing a full byte wastes 5 bits.

Compression code uses masks and shifts to:

- pack symbols,
- extract fields,
- combine partial bytes,
- track arbitrary bit positions.

---

# 26. Real-World Use: Hash Functions

Hash algorithms frequently combine:

- XOR,
- AND,
- NOT,
- shifts,
- rotations,
- addition.

These operations mix bits so small input changes affect many output bits.

---

# 27. Real-World Use: CRC Error Detection

Cyclic redundancy checks use XOR-based polynomial arithmetic.

CRCs are common in:

- Ethernet,
- disks,
- compressed files,
- embedded buses,
- serial communication.

XOR plays a role analogous to subtraction in the binary polynomial arithmetic CRCs use.

---

# 28. Real-World Use: Schedules

A seven-bit integer can represent a weekly schedule:

```text
bit 0 = Monday
bit 1 = Tuesday
...
bit 6 = Sunday
```

Monday, Wednesday, Friday might be:

```text
0010101
```

Operations:

```text
schedule & WEDNESDAY      check Wednesday
A & B                     shared available days
A | B                     combined available days
A ^ B                     days that differ
```

---

# 29. Real-World Use: Chess Bitboards

A chess board has 64 squares. A 64-bit integer can represent one property across the whole board.

One bitboard can represent all white pawns. Another can represent occupied squares. Another can represent legal attack squares.

AND, OR, XOR, NOT, and shifts can manipulate entire board states at once.

In JavaScript, `BigInt` is useful for 64-bit bitboards.

---

# 30. Real-World Use: Sudoku and Constraint Solving

A Sudoku cell can encode candidates 1-9 as nine bits:

```text
bit 0 = candidate 1
bit 1 = candidate 2
...
bit 8 = candidate 9
```

AND can find shared valid candidates.

NOT + AND can remove candidates.

Counting set bits tells you how many candidates remain.

This is bits acting as a mathematical set.

---

# 31. Real-World Use: Access Control

A role may encode:

```text
VIEW
EDIT
DELETE
CREATE
ADMIN
EXPORT
IMPORT
AUDIT
```

Eight permissions fit in one byte.

```js
role & DELETE;
role |= EXPORT;
role &= ~ADMIN;
role ^= AUDIT;
```

---

# 32. Real-World Use: Capability Negotiation

Suppose two systems advertise supported capabilities.

```text
client = 10110100
server = 01110110
```

Common capabilities:

```text
client & server
```

Combined capabilities:

```text
client | server
```

Differences:

```text
client ^ server
```

Networking and hardware protocols use this pattern frequently.

---

# 33. Real-World Use: Interrupt Masks

Hardware may support several interrupt sources:

```text
bit 0 = timer
bit 1 = network
bit 2 = keyboard
bit 3 = sensor
```

A mask determines which interrupts are enabled.

OR enables them.

AND checks them.

NOT + AND disables them.

---

# 34. Real-World Use: File Attributes

Attributes may include:

```text
read-only
hidden
system
archive
compressed
encrypted
```

A file can have several attributes simultaneously in one packed field.

---

# 35. Node.js Buffer and Typed Arrays

Bit manipulation becomes especially useful when JavaScript touches binary data.

Typed arrays include:

```js
Uint8Array
Int8Array
Uint16Array
Int16Array
Uint32Array
Int32Array
BigUint64Array
BigInt64Array
```

Example:

```js
const bytes = new Uint8Array([0b10110100]);

const low = bytes[0] & 0x0F;
const high = (bytes[0] >> 4) & 0x0F;
```

Node's `Buffer` is commonly used for files, TCP packets, binary messages, and cryptographic data.

```js
const buffer = Buffer.from([0b11010110]);
const byte = buffer[0];
const lowNibble = byte & 0x0F;
```

---

# 36. Useful Bit Tricks

## Test whether a positive integer is a power of two

```js
function isPowerOfTwo(n) {
  return n > 0 && (n & (n - 1)) === 0;
}
```

Why?

```text
8     = 1000
8 - 1 = 0111
AND   = 0000
```

A power of two has exactly one set bit.

## Clear the lowest set bit

```js
n &= n - 1;
```

Example:

```text
n     = 10110000
n - 1 = 10101111
AND   = 10100000
```

## Count set bits

```js
function countBits(n) {
  let count = 0;

  while (n !== 0) {
    n &= n - 1;
    count++;
  }

  return count;
}
```

This is Brian Kernighan's bit-counting algorithm.

## Check whether two values share any flags

```js
if (flagsA & flagsB) {
  console.log("At least one shared flag");
}
```

## Complement a bounded set

If only eight bits matter:

```js
const ALL = 0xFF;
const disabled = (~enabled) & ALL;
```

The final AND removes the unwanted upper bits created by JavaScript's 32-bit NOT operation.

---

# 37. Why Flags Use Powers of Two

Flags are normally assigned one bit each:

```text
00000001 = 1
00000010 = 2
00000100 = 4
00001000 = 8
00010000 = 16
00100000 = 32
01000000 = 64
10000000 = 128
```

Each contains exactly one `1`, so they do not overlap.

A common definition style is:

```js
const FLAG_A = 1 << 0;
const FLAG_B = 1 << 1;
const FLAG_C = 1 << 2;
const FLAG_D = 1 << 3;
```

---

# 38. Related Operators: Bit Shifts

Real bit manipulation commonly uses:

```js
<<   // left shift
>>   // signed right shift
>>>  // unsigned right shift
```

Example:

```text
0001 << 3 = 1000
```

Shifts let you position fields within an integer.

---

# 39. Extracting Multi-Bit Fields

Suppose a byte is arranged as:

```text
AA BBB CCC
```

To extract `BBB`, shift it down and mask it:

```js
const type = (byte >> 3) & 0b111;
```

This pattern is everywhere in binary formats and protocols.

---

# 40. Packing Multiple Fields into One Integer

Suppose red, green, and blue each use eight bits:

```js
const rgb =
  (red << 16) |
  (green << 8) |
  blue;
```

Extract them later:

```js
const redOut   = (rgb >> 16) & 0xFF;
const greenOut = (rgb >> 8) & 0xFF;
const blueOut  = rgb & 0xFF;
```

Useful mental model:

```text
shift + OR  = pack
shift + AND = unpack
```

---

# 41. Bitwise Masking and Powers of Two

For non-negative integers, when the divisor is a power of two:

```js
n & 7
```

extracts the remainder modulo 8 because `7` is `0b111`.

This can also be used in power-of-two circular buffers:

```js
index = (index + 1) & 255;
```

for a 256-entry buffer.

And in low-level hash tables with power-of-two capacities:

```js
const bucket = hash & (capacity - 1);
```

Do not automatically replace readable modulo arithmetic with bit tricks unless the constraints and performance justify it.

---

# 42. Hardware Logic Gates

Bitwise operators correspond directly to Boolean logic gates.

```text
AND  → &
OR   → |
XOR  → ^
NOT  → ~ conceptually
```

The relationships you use in software are also implemented as digital logic in hardware.

---

# 43. Half Adder: Where AND and XOR Become Arithmetic

A half adder adds two individual bits.

Inputs:

```text
A
B
```

Outputs:

```text
SUM
CARRY
```

The sum bit is:

```text
A XOR B
```

The carry bit is:

```text
A AND B
```

Truth table:

```text
A B | SUM CARRY
----|----------
0 0 |  0    0
0 1 |  1    0
1 0 |  1    0
1 1 |  0    1
```

This means the same operations you are learning are fundamental pieces of binary addition inside computers.

Full adders extend this by including a carry-in bit and combining XOR, AND, and OR logic. Chaining full adders allows processors to add multi-bit integers.

---

# 44. Branchless Techniques

Low-level code sometimes uses bitwise operations to avoid branches.

This can matter in specialized performance-sensitive code, but modern CPUs and JIT compilers are sophisticated.

Do not convert readable branches into obscure bit hacks without measurement.

Rule:

> Measure first. Optimize second.

---

# 45. Serialization and Bandwidth

Instead of transmitting several Boolean properties separately:

```json
{
  "enabled": true,
  "compressed": false,
  "encrypted": true,
  "priority": true
}
```

a binary protocol may encode them in one byte:

```text
00001101
```

At large scale or on constrained links, compact representation matters.

---

# 46. Sensor Data

A sensor may return a 16-bit field:

```text
SSSMMMMMMMMMMMMM
```

where:

```text
SSS = status
MMM... = measurement
```

JavaScript example:

```js
const status = (value >> 13) & 0b111;
const measurement = value & 0x1FFF;
```

This exact style of encoding is common in embedded sensors.

---

# 47. CAN Bus, Automotive, and Industrial Systems

Compact messages can encode things like:

- door open,
- brake pressed,
- ABS active,
- engine warning,
- lights on,
- traction control state.

Industrial buses and automotive systems frequently pack state into bits because bandwidth and deterministic message sizes matter.

---

# 48. IoT Systems

Tiny devices care about memory, bandwidth, power, and CPU time.

Packing Boolean state into bits reduces transmitted and stored data.

That makes bit masks common in firmware and IoT protocols.

---

# 49. Transfer to C and Rust

The concepts transfer almost directly.

C:

```c
flags |= FLAG;
flags &= ~FLAG;
flags ^= FLAG;
if (flags & FLAG) { }
```

Rust:

```rust
flags |= FLAG;
flags &= !FLAG;
flags ^= FLAG;

if flags & FLAG != 0 {
}
```

Rust uses `!` for bitwise NOT rather than JavaScript's `~`.

---

# 50. When NOT to Use Bitwise Operators

Do not use bit manipulation just because it is clever.

Avoid it when:

- readability suffers,
- the domain naturally needs arbitrary-sized sets,
- values exceed numeric assumptions,
- objects or named booleans are clearer,
- performance and memory are irrelevant,
- future maintainers would struggle to understand the encoding.

This may be better application code:

```js
user.permissions.admin
```

than:

```js
user.permissions & 0x80
```

unless the compact representation serves a real purpose.

---

# 51. How to Recognize a Bitwise Problem

Ask these questions.

### Do I have many independent Boolean states?

Bit masks may fit.

### Am I parsing or constructing binary data?

Masks and shifts probably matter.

### Do I need to know which bits changed?

Use XOR.

### Do I need only common features?

Use AND.

### Do I need the union of features?

Use OR.

### Do I need to toggle state?

Use XOR.

### Do I need to disable selected flags?

Use NOT + AND.

### Am I dealing with powers of two?

There may be a useful bitwise identity.

### Am I working with embedded systems, protocols, graphics, codecs, binary files, or hardware?

Expect bit manipulation.

---

# 52. Practical Cheat Sheet

```js
flags & FLAG       // check flag
flags |= FLAG      // enable flag
flags &= ~FLAG     // disable flag
flags ^= FLAG      // toggle flag

A & B              // intersection / common bits
A | B              // union / combined bits
A ^ B              // differences / changed bits
~A                 // invert all bits

n & 1              // odd/even test
n & (n - 1)        // useful power-of-two/set-bit operation
n & 0xFF           // low byte
(n >> shift) & mask // extract field
(value & mask) << shift // position field
```

---

# 53. Practice Exercise: Permissions

```js
const READ    = 0b0001;
const WRITE   = 0b0010;
const DELETE  = 0b0100;
const ADMIN   = 0b1000;

let permissions = READ | WRITE;
```

Manually work out:

```js
permissions |= ADMIN;
permissions ^= WRITE;
permissions &= ~READ;
permissions |= DELETE;
```

Determine:

- final binary value,
- final decimal value,
- enabled permissions.

---

# 54. Practice Exercise: Hardware Register

```text
bit 0 = motor
bit 1 = warning light
bit 2 = cooling fan
bit 3 = emergency shutdown
```

Start:

```text
0101
```

Answer:

1. Is the motor running?
2. Is the warning light enabled?
3. Is cooling enabled?
4. Turn warning on.
5. Turn cooling off.
6. Toggle motor.
7. Compare the original and final states using XOR.

---

# 55. Practice Exercise: Packet Header

Given:

```text
10110110
```

Interpret:

```text
bit 7 = encrypted
bit 6 = compressed
bit 5 = acknowledged
bit 4 = priority
bits 0-3 = packet type
```

Use masks to extract every field.

---

# 56. Practice Exercise: Changed Device State

Previous:

```text
10110010
```

Current:

```text
10010110
```

Use XOR to determine exactly which flags changed.

Then use AND to determine which changed bits are currently enabled.

---

# 57. Practice Exercise: Capability Negotiation

Client capabilities:

```text
10110100
```

Server capabilities:

```text
11100110
```

Find:

- capabilities supported by both,
- capabilities supported by either,
- capabilities that differ.

---

# 58. Practice Exercise: Find the Lonely Number

```js
[15, 22, 8, 15, 4, 22, 4]
```

Solve using:

- `Map`,
- counting logic,
- XOR.

Compare:

- time complexity,
- extra memory,
- assumptions required by each solution.

---

# 59. Practice Exercise: Eight-Sensor Status Byte

Define:

```text
bit 0 = temperature alarm
bit 1 = pressure alarm
bit 2 = humidity alarm
bit 3 = door open
bit 4 = fan running
bit 5 = pump running
bit 6 = network connected
bit 7 = emergency stop
```

Write operations to:

- check temperature alarm,
- enable fan,
- disable pump,
- toggle network status,
- check whether either pressure or humidity alarm is active,
- determine which bits changed between two readings.

---

# 60. Suggested Lesson Progression

## Part 1 — Binary

Convert decimal 0-15 to binary manually.

## Part 2 — Truth Tables

Build truth tables for:

```text
AND
OR
XOR
NOT
```

## Part 3 — Manual Operations

Perform operations on four-bit values by hand.

## Part 4 — Flags

Represent four Boolean states in one integer.

## Part 5 — Real Binary Data

Parse a fake hardware register or network packet byte.

## Part 6 — Algorithms

Explore:

- odd/even,
- unique integer,
- power of two,
- set-bit counting.

## Part 7 — Systems

Connect the concepts to:

- CPU registers,
- protocols,
- file formats,
- memory,
- embedded devices.

## Part 8 — JavaScript Binary APIs

Use:

```js
Buffer
Uint8Array
Uint32Array
BigInt
```

---

# 61. Final Mental Models

## AND `&`

> What do these two values have in common?

## OR `|`

> Combine everything enabled in either value.

## XOR `^`

> What is different?

or

> Toggle these selected bits.

## NOT `~`

> Flip every bit.

For flags:

```text
CHECK    &
ENABLE   |
TOGGLE   ^
DISABLE  & ~
```

---

# 62. The Bigger Picture

Bitwise operators are not mainly clever coding-challenge tricks. They expose one of the simplest computational layers beneath software.

At the hardware level, computers operate on electrical states that we model as `0` and `1`.

Logic gates combine those states.

Processors build larger operations from those gates.

Instruction sets encode fields and flags in binary.

Operating systems use masks and permissions.

Protocols pack information into bits.

File formats store fields in bit ranges.

Applications build abstractions on top of all of this.

So when you write:

```js
flags & MASK
```

you are using essentially the same Boolean relationship that exists down at the digital-circuit level.

That makes bitwise operations an important bridge between high-level programming and the machine underneath it.

They are especially valuable for anyone interested in embedded development, systems programming, networking, compilers, operating systems, emulation, performance work, binary formats, and computer architecture.
