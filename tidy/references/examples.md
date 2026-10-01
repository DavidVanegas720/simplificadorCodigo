# Before / After Examples

## 1. Nested ifs → early returns

Before (JavaScript):
```js
function getDiscount(user) {
  if (user) {
    if (user.isActive) {
      if (user.orders > 10) {
        return 0.2;
      } else {
        return 0.1;
      }
    } else {
      return 0;
    }
  } else {
    return 0;
  }
}
```

After:
```js
function getDiscount(user) {
  if (!user?.isActive) return 0;
  return user.orders > 10 ? 0.2 : 0.1;
}
```

## 2. Giant function → small functions

Before (Python):
```python
def process_order(order):
    if not order["items"]:
        raise ValueError("Empty order")
    for item in order["items"]:
        if item["qty"] <= 0:
            raise ValueError("Invalid quantity")
    total = 0
    for item in order["items"]:
        total += item["price"] * item["qty"]
    if order["coupon"] == "SAVE10":
        total *= 0.9
    db.save({"id": order["id"], "total": total})
    send_email(order["email"], f"Your total is {total}")
```

After:
```python
def process_order(order):
    validate(order)
    total = apply_coupon(subtotal(order["items"]), order["coupon"])
    db.save({"id": order["id"], "total": total})
    send_email(order["email"], f"Your total is {total}")

def validate(order):
    if not order["items"]:
        raise ValueError("Empty order")
    if any(item["qty"] <= 0 for item in order["items"]):
        raise ValueError("Invalid quantity")

def subtotal(items):
    return sum(item["price"] * item["qty"] for item in items)

def apply_coupon(total, coupon):
    return total * 0.9 if coupon == "SAVE10" else total
```

## 3. Cryptic names → descriptive names

Before:
```js
const d = getD(u);
const r = d.filter(x => x.s === 1);
```

After:
```js
const orders = getOrders(user);
const paidOrders = orders.filter(order => order.status === STATUS_PAID);
```

## 4. Unnecessary abstraction → direct code

Before (Python):
```python
class GreetingFactory:
    def __init__(self, config=None):
        self.config = config or {}

    def create(self, name):
        return GreetingBuilder().with_name(name).build()

class GreetingBuilder:
    def with_name(self, name):
        self.name = name
        return self

    def build(self):
        return f"Hello, {self.name}"
```

After:
```python
def greet(name):
    return f"Hello, {name}"
```

## 5. Manual loop → standard function

Before (JavaScript):
```js
const names = [];
for (let i = 0; i < users.length; i++) {
  if (users[i].active) {
    names.push(users[i].name);
  }
}
```

After:
```js
const names = users.filter(u => u.active).map(u => u.name);
```

## 6. AI noise → clean code

Before (Python):
```python
import os
import json  # unused

def add(a, b):
    # Check that inputs are not None
    if a is None or b is None:
        raise ValueError("Inputs cannot be None")
    # Add the two numbers
    result = a + b
    print(f"DEBUG: result is {result}")
    # Return the result
    return result
```

After:
```python
def add(a, b):
    return a + b
```

## 7. When NOT to simplify

Before:
```python
def is_valid(user):
    has_email = "@" in user.email
    is_adult = user.age >= 18
    return has_email and is_adult
```

Don't collapse it into a dense one-liner: the named variables document the rules.
This code is already tidy — leave it.
