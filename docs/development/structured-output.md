# Structured Output

## What it is

Structured output means getting a response back in a fixed, predictable format — instead of a free-text sentence.

Free text:

> "The total is 42.50 and the vendor is Acme."

Structured output:

```json
{
  "total": 42.5,
  "vendor": "Acme"
}
```

---

## Why it matters

Structured output makes a response:

- Predictable
- Easy to validate
- Easy to pass into another system or database
- Independent of how something is phrased

---

## How the format is defined: Schema

A schema describes exactly what the output must contain.

| Term | Meaning |
|---|---|
| `object` | The output is a set of fields |
| `properties` | Defines what fields exist |
| `string` / `number` | The data type of a field |
| `required` | Fields that must be present |
| `null` | No value available |
| `additionalProperties: false` | No extra/unexpected fields allowed |

Example:

```json
{
  "type": "object",
  "properties": {
    "total": { "type": "number" },
    "vendor": { "type": "string" }
  },
  "required": ["total", "vendor"],
  "additionalProperties": false
}
```

---

!!! warning "Key point to remember"
    A schema defines the **shape** of the output — not whether the *content* is correct or unambiguous.

    Example: if a source value is ambiguous (like a date that could be read two different ways), the schema can force the output into a certain format, but it cannot decide which interpretation is the right one. That requires separate logic or explicit rules, not the schema alone.

!!! tip "One-line takeaway"
    A schema controls the format of the output, not the correctness of what goes into it.
