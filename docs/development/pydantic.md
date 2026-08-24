# Pydantic

## What it is

Pydantic is a Python tool that helps you check if data has the right shape and the right type of values.

Instead of just writing down data with no rules attached, you first describe what the data *should* look like — this description is called a "model." Then Pydantic checks any data you give it against that description.

For example, here we describe that a person should have a name (text) and an age (number):

```python
from pydantic import BaseModel

class Person(BaseModel):
    name: str
    age: int
```

`BaseModel` is what Pydantic gives you to build this description from. Writing `class Person(BaseModel):` is just saying: "Person is a data description, built using Pydantic."

```python
person = Person(name="Rahul", age=22)
```

If the data doesn't match what was described (say, `age` is given as text instead of a number), Pydantic immediately raises a **validation error** — it won't quietly accept incorrect data.

---

## Why it matters

You can't always assume the data you receive is correctly formatted. Pydantic lets you check data against the "model" you defined (like the `Person` example above — this means your own description of expected data, not an AI/LLM model) and catch problems — wrong types, missing fields, invalid values — before that data gets used further.

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
