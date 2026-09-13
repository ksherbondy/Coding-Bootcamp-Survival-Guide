# Regex Cheat Sheet — What the Symbols Actually Do

This cheat sheet is written for **JavaScript / TypeScript regular expressions**.

The goal is not just to name each regex symbol, but to explain what it actually means when you read a pattern.

---

## 1. Anchors

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `^` | Match only at the **start** of the string or line | `^cat` | The text must start with `cat` |
| `$` | Match only at the **end** of the string or line | `cat$` | The text must end with `cat` |
| `\b` | Match a **word boundary** | `\bcat\b` | Match the word `cat`, but not `scatter` |
| `\B` | Match where there is **not** a word boundary | `\Bcat` | Match `cat` only when it is inside another word |

---

## 2. Character Matches

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `.` | Match **almost any single character** | `c.t` | Match `cat`, `cot`, `c9t`, etc. |
| `\d` | Match one digit: `0-9` | `\d` | Match one numeric digit |
| `\D` | Match one character that is **not** a digit | `\D` | Match letters, spaces, punctuation, etc. |
| `\w` | Match one "word" character: usually letter, digit, or `_` | `\w` | Match `a`, `Z`, `7`, `_` |
| `\W` | Match one character that is **not** a word character | `\W` | Match spaces, punctuation, symbols |
| `\s` | Match one whitespace character | `\s` | Match a space, tab, or newline |
| `\S` | Match one character that is **not** whitespace | `\S` | Match any visible/non-space character |

---

## 3. Character Sets

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `[abc]` | Match **one** character from the listed set | `[abc]` | Match `a`, `b`, or `c` |
| `[^abc]` | Match **one** character not in the listed set | `[^abc]` | Match anything except `a`, `b`, or `c` |
| `[a-z]` | Match one character in a range | `[a-z]` | Match one lowercase letter |
| `[A-Z]` | Match one uppercase letter | `[A-Z]` | Match one uppercase letter |
| `[0-9]` | Match one digit | `[0-9]` | Same idea as `\d` |
| `[a-zA-Z]` | Match one uppercase or lowercase letter | `[a-zA-Z]` | Match one English letter |
| `[a-z0-9_]` | Match one character from several ranges/items | `[a-z0-9_]` | Match lowercase letter, digit, or underscore |

Important:

```regex
[abc]
```

does **not** mean "match the string `abc`."

It means:

> Match one character, and that character may be `a`, `b`, or `c`.

---

## 4. Quantifiers

Quantifiers tell regex **how many times the thing immediately before them may repeat**.

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `*` | Repeat the previous thing **zero or more times** | `a*` | Match `""`, `a`, `aa`, `aaa`, etc. |
| `+` | Repeat the previous thing **one or more times** | `a+` | Match `a`, `aa`, `aaa`, etc. |
| `?` | The previous thing is **optional**: zero or one time | `a?` | Match `""` or `a` |
| `{3}` | Repeat exactly 3 times | `a{3}` | Match `aaa` |
| `{2,5}` | Repeat between 2 and 5 times | `a{2,5}` | Match `aa`, `aaa`, `aaaa`, `aaaaa` |
| `{2,}` | Repeat at least 2 times | `a{2,}` | Match `aa`, `aaa`, `aaaa`, etc. |
| `{,5}` | Not valid JavaScript regex syntax | — | Use `{0,5}` instead |

### Greedy vs lazy

By default, quantifiers like `*`, `+`, and `{m,n}` are **greedy**.

They try to match as much as possible.

Adding `?` after a quantifier makes it **lazy** or **non-greedy**.

| Regex | Meaning |
|---|---|
| `.*` | Match as much as possible |
| `.*?` | Match as little as possible |
| `.+` | Match one or more, greedily |
| `.+?` | Match one or more, lazily |

Example:

```regex
<.*>
```

Given:

```text
<b>hello</b>
```

it may match:

```text
<b>hello</b>
```

because `.*` grabs as much as possible.

But:

```regex
<.*?>
```

tries to stop at the first possible `>`.

---

## 5. Groups

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `(abc)` | Group characters together **and capture the match** | `(abc)+` | Repeat the whole `abc` group |
| `(?:abc)` | Group characters together **without capturing** | `(?:abc)+` | Repeat `abc`, but do not store the group |
| `(a\|b)` | Match one alternative and capture it | `(cat|dog)` | Match `cat` or `dog` |
| `(?:a\|b)` | Match one alternative without capture | `(?:cat|dog)` | Match `cat` or `dog`, no capture |

### Capture groups

If you run:

```js
const match = /(\d+)-(\d+)/.exec("12-34");
```

then:

```js
match[0] // "12-34"
match[1] // "12"
match[2] // "34"
```

`match[0]` is always the entire match.

The numbered entries after that come from capturing groups.

---

## 6. Alternation

| Regex | What it actually does | Example | Meaning |
|---|---|---|---|
| `\|` | Means **OR** | `cat|dog` | Match `cat` or `dog` |

Grouping is often important:

```regex
^(cat|dog)$
```

means:

> Match a string that is exactly `cat` or exactly `dog`.

Without grouping:

```regex
^cat|dog$
```

means something different:

> Starts with `cat` OR ends with `dog`.

---

## 7. Escaping Special Characters

Some characters have special regex meanings.

To match the actual character itself, escape it with `\`.

| You want to match | Regex |
|---|---|
| literal `.` | `\.` |
| literal `*` | `\*` |
| literal `+` | `\+` |
| literal `?` | `\?` |
| literal `(` | `\(` |
| literal `)` | `\)` |
| literal `[` | `\[` |
| literal `]` | `\]` |
| literal `{` | `\{` |
| literal `}` | `\}` |
| literal `\` | `\\` |
| literal `^` | `\^` |
| literal `$` | `\$` |
| literal `|` | `\|` |

Example:

```regex
\.
```

means:

> Match an actual period character.

But:

```regex
.
```

means:

> Match almost any single character.

---

## 8. Lookarounds

Lookarounds check what is nearby **without consuming it as part of the match**.

| Regex | What it actually does | Example |
|---|---|---|
| `(?=abc)` | Positive lookahead: make sure `abc` comes next | `foo(?=bar)` |
| `(?!abc)` | Negative lookahead: make sure `abc` does not come next | `foo(?!bar)` |
| `(?<=abc)` | Positive lookbehind: make sure `abc` came before | `(?<=\$)\d+` |
| `(?<!abc)` | Negative lookbehind: make sure `abc` did not come before | `(?<!\$)\d+` |

Example:

```regex
foo(?=bar)
```

matches `foo` only when it is followed by `bar`.

Given:

```text
foobar
```

the match is only:

```text
foo
```

The `bar` was checked, but not consumed.

---

## 9. Backreferences

Backreferences let you match the same text that a previous capture group matched.

| Regex | What it does |
|---|---|
| `\1` | Match the exact same text captured by group 1 |
| `\2` | Match the exact same text captured by group 2 |

Example:

```regex
(\w+)\s+\1
```

Matches repeated words such as:

```text
hello hello
```

because group 1 captured `hello`, and `\1` requires the same text again.

---

## 10. Flags

Flags go after the closing `/` in a JavaScript regex.

```js
/pattern/gi
```

| Flag | What it actually does |
|---|---|
| `g` | Global: find all matches instead of stopping after the first |
| `i` | Ignore case |
| `m` | Multiline: `^` and `$` work per line, not just for the whole string |
| `s` | Dot-all: `.` can also match newline characters |
| `u` | Unicode-aware matching |
| `y` | Sticky: match only from the regex engine's current position |
| `d` | Include match indices in the result |

Example:

```js
/cat/gi
```

means:

> Find every occurrence of `cat`, ignoring capitalization.

So it can match:

```text
cat
Cat
CAT
cAt
```

---

# Reading Regex in English

The best way to understand regex is to read it from left to right as a sentence.

---

## Example 1 — Exactly one letter

```regex
^[a-z]$
```

### Read it in English

> Start of string.  
> Match exactly one lowercase letter from `a` through `z`.  
> End of string.

Matches:

```text
a
m
z
```

Does not match:

```text
ab
7
A
```

With the `i` flag:

```regex
^[a-z]$/i
```

it also matches uppercase letters.

---

## Example 2 — One or more digits

```regex
^\d+$
```

### Read it in English

> Start of string.  
> Match one or more digits.  
> End of string.

Matches:

```text
1
42
123456
```

Does not match:

```text
12a
abc
12 34
```

---

## Example 3 — Optional minus sign

```regex
^-?\d+$
```

### Read it in English

> Start of string.  
> Optionally match one minus sign.  
> Match one or more digits.  
> End of string.

Matches:

```text
42
-42
0
-100
```

Does not match:

```text
+42
--42
4.2
```

---

## Example 4 — A simple file extension

```regex
^.+\.txt$
```

### Read it in English

> Start of string.  
> Match one or more characters.  
> Match a literal period.  
> Match the letters `txt`.  
> End of string.

Matches:

```text
notes.txt
hello.txt
report.final.txt
```

Does not match:

```text
notes.md
txt
notes.txt.bak
```

---

## Example 5 — A 24-hour minute section

```regex
^[0-5]\d$
```

### Read it in English

> Start of string.  
> Match one digit from `0` through `5`.  
> Match any digit from `0` through `9`.  
> End of string.

Matches:

```text
00
09
15
30
59
```

Does not match:

```text
60
99
5
```

---

## Example 6 — A 24-hour time

```regex
^(?:[01]?\d|2[0-3]):[0-5]\d$
```

### Break it down

```regex
^
(?:[01]?\d|2[0-3])
:
[0-5]\d
$
```

### Read it in English

> Start of string.  
> Match either:
> - an optional `0` or `1`, followed by any digit,  
> OR
> - the digit `2`, followed by a digit from `0` through `3`.
>
> Then match a literal colon.  
> Then match a digit from `0` through `5`.  
> Then match any digit.  
> End of string.

Matches:

```text
1:00
01:00
00:00
13:45
23:59
```

Does not match:

```text
24:00
13:60
13:5
```

---

## Example 7 — Markdown heading

From SensibleMD:

```regex
^(#{1,6})\s*(.*?)\s*#*\s*$
```

### Break it down

| Piece | Meaning |
|---|---|
| `^` | Start of line |
| `(#{1,6})` | Capture 1 to 6 `#` characters |
| `\s*` | Allow zero or more whitespace characters |
| `(.*?)` | Capture as little text as possible |
| `\s*` | Allow whitespace |
| `#*` | Allow zero or more trailing `#` characters |
| `\s*` | Allow trailing whitespace |
| `$` | End of line |

### Read it in English

> Start at the beginning of the line.  
> Capture between one and six `#` characters.  
> Allow any amount of whitespace.  
> Capture the heading text, taking as little as necessary.  
> Allow whitespace after the heading text.  
> Allow optional trailing `#` characters.  
> Allow trailing whitespace.  
> End at the end of the line.

Example:

```text
### Section Title ###
```

Capture groups:

```js
heading[0] // "### Section Title ###"
heading[1] // "###"
heading[2] // "Section Title"
```

Then this code:

```js
const level = heading[1].length;
```

turns:

```text
"#"      -> 1
"##"     -> 2
"###"    -> 3
"######" -> 6
```

into the Markdown heading level.

---

## Example 8 — Empty Markdown link text

```regex
\[\s*\]\([^)]*\)
```

### Break it down

| Piece | Meaning |
|---|---|
| `\[` | Literal `[` |
| `\s*` | Zero or more whitespace characters |
| `\]` | Literal `]` |
| `\(` | Literal `(` |
| `[^)]*` | Zero or more characters that are not `)` |
| `\)` | Literal `)` |

### Read it in English

> Match a literal opening square bracket.  
> Allow only whitespace inside the brackets.  
> Match the closing square bracket.  
> Match an opening parenthesis.  
> Match zero or more characters until the closing parenthesis.  
> Match the closing parenthesis.

Matches:

```md
[](https://example.com)
[   ](page.md)
```

This is useful for finding links with no accessible link text.

---

## Example 9 — Filename-looking image alt text

```regex
^(image|img|screenshot)[_ -]?\d*\.(png|jpe?g|gif|webp)$
```

### Break it down

| Piece | Meaning |
|---|---|
| `^` | Start of string |
| `(image|img|screenshot)` | Match `image`, `img`, or `screenshot` |
| `[_ -]?` | Optionally match `_`, space, or `-` |
| `\d*` | Match zero or more digits |
| `\.` | Match a literal period |
| `(png|jpe?g|gif|webp)` | Match one allowed image extension |
| `$` | End of string |

### Read it in English

> Start of string.  
> Match `image`, `img`, or `screenshot`.  
> Optionally match an underscore, space, or hyphen.  
> Match zero or more digits.  
> Match a literal period.  
> Match `png`, `jpg`, `jpeg`, `gif`, or `webp`.  
> End of string.

Matches:

```text
image.png
image1.png
image_12.jpg
screenshot-4.webp
img.jpeg
```

This is useful for detecting alt text that looks like a filename instead of a description.

---

# A Useful Mental Model

When reading regex, ask four questions:

1. **Where am I allowed to match?**
   - `^`
   - `$`
   - `\b`

2. **What character or group am I matching?**
   - `\d`
   - `\w`
   - `.`
   - `[a-z]`
   - `(abc)`

3. **How many times may it appear?**
   - `*`
   - `+`
   - `?`
   - `{1,6}`

4. **Are there alternatives or conditions?**
   - `|`
   - `(?:...)`
   - `(?=...)`
   - `(?!...)`

If you can answer those four questions, even a complicated regex becomes much easier to decode.

---

# Very Short Memory Guide

```text
^        start
$        end
.        almost any one character

\d       digit
\D       not digit
\w       word character
\W       not word character
\s       whitespace
\S       not whitespace

[...]    one character from this set
[^...]   one character NOT from this set

*        zero or more
+        one or more
?        zero or one
{n}      exactly n
{m,n}    between m and n

(...)    capture this group
(?:...)  group without capturing
|        OR

.*       as much as possible
.*?      as little as possible

\        escape the next special character
```

---

# JavaScript Reminder

Regex literal:

```js
const regex = /^\d+$/;
```

Test whether a string matches:

```js
regex.test("123");
```

Get the actual match:

```js
regex.exec("123");
```

Find all matches:

```js
text.matchAll(/pattern/g);
```

Create a regex dynamically:

```js
const regex = new RegExp("^" + value + "$");
```

When using `new RegExp()`, remember that backslashes live inside a JavaScript string, so they often need to be doubled:

```js
const regex = new RegExp("\\d+");
```

instead of:

```js
const regex = /\d+/;
```
