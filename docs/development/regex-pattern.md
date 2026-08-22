# Regex

## What Regex is

Regex is a way of searching for something in a piece of text.

You write a rule that describes what you're looking for, and then you check the text against that rule.

- If the text has what you're looking for, the rule finds a match.
- If the text doesn't have what you're looking for, the rule doesn't match — and that usually means the rule itself needs to be changed, not that the text is "wrong."

In other words: regex isn't about checking whether something is a valid value. It's about searching for a specific pattern of characters in what was given to you. If you don't find it, you go back and fix your search rule.

---

## A few basics

| Symbol | Meaning |
|---|---|
| `\d` | Any single digit (0-9) |
| `\d{4}` | Exactly 4 digits in a row |
| `^` | Start of the text |
| `$` | End of the text |
| `-` | A literal hyphen character |

Example:

```
^\d{4}-\d{2}-\d{2}$
```

This reads as: *look for text that starts with 4 digits, followed by a hyphen, 2 digits, a hyphen, and 2 digits, then ends* -- e.g. `2026-08-08`.

---

## What you need to understand

- Regex is used to search for a specific format inside a piece of text — it's not checking whether something is "correct." If the rule doesn't find what it's looking for, the rule needs to be adjusted, not the data.
