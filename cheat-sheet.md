# JavaScript Higher-Order Function "Toolbox"

| If your goal is to...	| Use this Method | Works With | Chainable | Think of it as... |
| :--- | :--- | :--- | :--- | :--- |
| "Keep some" |	`.filter()` |	Array |	Yes (Array) |	***The Sieve:*** Use to count or remove noise. |
| "Change all" |	`.map()` |	Array |	Yes (Array) |	***The Transformer:*** Changes the "shape" of items. |
| "Find one" |	`.find()` |	Array |	No (Item) |	***The Search Party:*** Returns the first match or undefined. |
| "Check all" |	`.every()` |	Array |	No (Boolean) |	***The Safety Inspector:*** Returns true only if all pass. |
| "Check if any" |	`.some()` |	Array |	No (Boolean) |	***The Red Flag:*** Returns true if at least one matches. |
| "Squash to one" |	`.reduce()` |	Array |	No (Value) |	***The Snowball:*** Sums up numbers or builds one object. |
| "Find where" |	`.findIndex()` |	Array |	No (Number) |	***The GPS:*** Tells you the index (i) of the first match. |
| "Do something" |	`.forEach()` |	Array |	No (Undefined) |	***The Worker:*** Good for console.log, bad for logic. |
| "Make Array" |	`.split()` |	String |	Yes (Array) |	***The Bridge:*** Turns a string into a list of "scraps." |
| "Make String" |	`.join()` |	Array |	Yes (String) |	***The Glue:*** Turns an array back into a single string. |
| "Get Piece" |	`.slice()` |	Str / Arr |	Yes (S/A) |	***The Knife:*** Cuts out a section without changing original. |
## How to Pick Your Tool (The Decision Tree)

Do I want a new array back?

- Yes, a smaller one → `.filter()`

- Yes, the same size but different data → `.map()`

- No, I just want one result → `.find()`, `.reduce()`, or `.every()`

Does the order matter?

- Almost always yes for `.map()` and `.filter()`. If you need the index, remember that these methods hand it to you as the second argument: `list.map((item, index) => ...)`

Should I stop early?

- If you find what you need, `.find()`, `.some()`, and `.every()` will "short-circuit" (stop looping) to save time. `.map()` and `.filter()` always look at every single item.

Understanding the "Chainable" Column

- If it says YES: You can immediately hit `.` and add another method from the Array section.

- If it says NO: The "conveyor belt" has stopped. Whatever is in your hand now is a single Value (a String, Number, or Boolean). You can only use methods that work for that specific type.

Decision Tree Update: "What am I holding?"

Before you add a `.` to a chain, check your cheat sheet:

- Am I holding an Array? I can use everything in the top section.

- Am I holding a String? I must use `.split()` or `.slice()` to get back into "Array mode."

- Am I holding a Boolean/Number? The chain is over. Use this for if statements or final returns.