# Pydantic

## What it is

Pydantic is a Python library used to define and validate structured data using plain Python classes instead of raw dictionaries.

```python
from pydantic import BaseModel

class Person(BaseModel):
    name: str
    age: int
```

```python
person = Person(name="Rahul", age=22)
```

If the data doesn't match the defined model, Pydantic raises a **validation error** instead of silently accepting bad data.

---

## Why it matters

You cannot always assume incoming data is correctly formatted. Pydantic gives you a way to check data against a model and catch mismatches — wrong types, missing fields, invalid values — before that data is used further.

---

## Important distinction

Pydantic is **not** the same thing as structured output:

- Structured output is the overall idea of getting data back in a fixed format.
- A schema is one way of describing that format.
- Pydantic is a separate tool that can define and validate data models, and can also generate schemas — but it is its own concept, not interchangeable with the other two.

---

## What Pydantic can and can't do

**Can:**

- Enforce that a value matches a pattern, type, or range
- Check that required fields exist and are the right type
- Reject malformed data before it moves further downstream

**Can't:**

- Resolve ambiguity in the *meaning* of a value. Example: a date that could correctly be read two different ways is still a "valid" date either way — Pydantic can confirm it's valid, but it cannot tell you which interpretation was intended.

---

!!! tip "One-line takeaway"
    Pydantic checks that data is structurally valid — it does not tell you if an ambiguous value was interpreted correctly.
