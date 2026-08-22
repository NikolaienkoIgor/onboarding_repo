# Regex / Pattern

## What Regex is

Regex (regular expression) is a way of writing a search rule that describes the **shape** a piece of text should follow, instead of an exact fixed value.

A few basics:

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

This reads as: *the text must start with 4 digits, followed by a hyphen, 2 digits, a hyphen, and 2 digits, then end* -- e.g. `2026-08-08`.

Regex is used whenever you need to check or enforce that a value follows a specific format (dates, codes, IDs, emails, etc.) without listing every possible valid value one by one.

---

## What `pattern` is

`pattern` is a keyword used in schema definitions to attach a regex rule to a field, so that field is expected to match that shape.

```json
{
  "type": "string",
  "pattern": "^\\d{4}-\\d{2}-\\d{2}$"
}
```

---

## What you need to understand

- `pattern` describes the shape a value must take, using regex.
- Adding `pattern` to a schema does not automatically mean it is strictly enforced everywhere. Enforcement depends entirely on what is actually checking the value against the schema.
- A pattern can technically be present in a schema without actually being applied or checked at all, depending on the tool/process involved.

---

!!! warning "Key point to remember"
    Never assume "it's in the schema" means "it's guaranteed." Always confirm whether the pattern is actually being enforced before relying on it, or validate the value separately after you receive it.

!!! tip "One-line takeaway"
    Regex defines the shape of a valid value. `pattern` is how that shape gets attached to a schema field -- but its presence alone doesn't guarantee enforcement.
