# Structured Output

## What it is

Structured output means getting a response back from an LLM in a fixed, predictable format — instead of a free-text sentence.

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

## What a Schema is

A schema is just a description of what fields you expect back, and what type each field should be.

Think of it like filling out a form with labeled boxes — one box says "put a number here," another says "put text here." A schema is that form, written in a way a computer can read.

| Term | Meaning |
|---|---|
| `object` | The output is a set of fields |
| `properties` | Defines what fields exist |
| `string` / `number` | The data type of a field |
| `required` | Fields that must be present |
| `null` | No value available |
| `additionalProperties: false` | No extra/unexpected fields allowed |

Example — a schema saying "give me a `total` (a number) and a `vendor` (text)":

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

## Example: Getting structured output from Gemini

Now let's see this actually happen in code, step by step, using Google's Gemini model.

**Step 1 — Connect to the model**

```python
import google.generativeai as genai

model = genai.GenerativeModel("gemini-pro")
```

- `import google.generativeai as genai` loads the tool we need to talk to Gemini.
- `genai.GenerativeModel("gemini-pro")` picks which Gemini model to use, and saves it as `model` so we can use it in the next steps.

**Step 2 — Define the schema**

```python
schema = {
    "type": "object",
    "properties": {
        "total": {"type": "number"},
        "vendor": {"type": "string"}
    }
}
```

This is the exact same kind of schema we just covered above — it tells Gemini what fields we want back, and what type each one should be.

**Step 3 — Ask Gemini for a structured response**

```python
response = model.generate_content(
    "Extract the total and vendor from this invoice.",
    generation_config={
        "response_mime_type": "application/json",
        "response_schema": schema
    }
)
```

- The first part is a normal plain-English instruction, same as you'd type into a chat.
- `response_mime_type: "application/json"` tells Gemini: "give me the answer as JSON, not as a sentence."
- `response_schema: schema` tells Gemini: "and make sure that JSON follows exactly the shape I defined in Step 2."

**Step 4 — Read the result**

```python
print(response.text)
```

Output:

```json
{
  "total": 42.5,
  "vendor": "Acme"
}
```

Instead of a sentence, we get back a JSON object that matches the schema exactly — with a `total` as a number and a `vendor` as text, nothing extra, nothing missing.

---

!!! warning "Key point to remember"
    A schema defines the **shape** of the output — not whether the *content* is correct or unambiguous.

    Example: if a source value is ambiguous (like a date that could be read two different ways), the schema can force the output into a certain format, but it cannot decide which interpretation is the right one. That requires separate logic or explicit rules, not the schema alone.

!!! tip "One-line takeaway"
    A schema controls the format of the output, not the correctness of what goes into it.
