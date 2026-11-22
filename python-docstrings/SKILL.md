---
name: python-docstrings
description: Writes and updates concise Google-style docstrings in Python code. Use when documenting or improving docstrings.
---

# Python Docstrings

Docstrings follow Google style and are concise and consistent.

Each docstring tells the caller what they need to use the code without reading it. Implementation details belong in code comments, not docstrings.

A docstring is added or updated only when it is missing, incomplete, or no longer matches the code. All other docstrings stay untouched.

## Style

- The summary is a single line that ends with a period.
- When the summary alone is not enough, a paragraph after a blank line adds the missing context, such as special behavior or restrictions.
- Functions and methods use the descriptive mood (`Fetches rows.`), not the imperative (`Fetch rows.`).
- Modules, classes, properties, and attributes use a noun phrase (`The request timeout, in seconds.`).
- Descriptions in `Args:`, `Returns:`, `Yields:`, and `Attributes:` are full phrases with articles (`The page number to retrieve.`).
- Code references, parameters, and literals appear in backticks.
- Descriptions do not repeat type annotations.
- An `Example:` block appears only when usage is not obvious from the summary and signature.

## Modules

A module docstring sits after the copyright header and before imports. It gives an overall description of the module.

```python
"""Order checkout and shipping.

Charging an order requires network access to the payment gateway.
"""
```

## Classes

A class docstring describes what the class represents or does. Public attributes are listed under `Attributes:`.

```python
class PriceCache:
    """Cache of product prices, refreshed periodically from the catalog.

    Instances are not thread-safe. Each thread should use its own instance.
    """
```

A class that exposes event handlers lists their names in backticks, with an `Example:` showing how to register one.

```python
class Checkout:
    """Checkout flow that charges an order and emits lifecycle events.

    Event handlers available:

    - `on_before_charge`: Called before the payment is charged.
    - `on_after_charge`: Called after the payment is charged.

    Example:

        @checkout.event_handler("on_after_charge")
        async def on_after_charge(checkout: Checkout, order: Order) -> None:
            print(f"Charged order {order.id}")
    """
```

## Data models

Dataclasses, Pydantic models, and similar classes declare their fields instead of defining `__init__`. Their fields are described under `Attributes:` in the class docstring.

The generated constructor needs no docstring.

```python
class ShippingOptions(BaseModel):
    """Shipping preferences for an order.

    Attributes:
        carrier: The name of the shipping carrier.
        express: Whether the order is delivered by express shipping.
        max_days: The maximum number of delivery days. Must be positive.
    """

    carrier: str
    express: bool = False
    max_days: int = Field(gt=0)
```

## Enums

An enum docstring describes its purpose, and `Attributes:` describes each public member.

```python
class OrderStatus(Enum):
    """Lifecycle state of an order.

    Attributes:
        PENDING: An order awaiting payment.
        PAID: An order paid and awaiting shipment.
        SHIPPED: An order handed to the carrier.
    """

    PENDING = 1
    PAID = 2
    SHIPPED = 3
```

## Functions

This section covers functions and methods.

A function docstring has a summary, followed by the sections below that apply to it.

```python
def charge_order(order: Order, gateway: PaymentGateway) -> None:
    """Charges the payment for an order.

    The call blocks until the gateway responds. The order status is updated to `PAID` only when the payment is accepted.

    Args:
        order: The order to charge.
        gateway: The gateway that processes the payment.

    Raises:
        PaymentDeclinedError: If the gateway declines the payment.
    """
```

```python
def close(self) -> None:
    """Closes the connection to the payment gateway."""
```

### Args

A function with parameters has `Args:`, listing each parameter by its exact signature name. `*args` and `**kwargs` keep those exact names.

```python
def search_products(
    query: str,
    *categories: str,
    **filters: object,
) -> list[Product]:
    """Searches the catalog for products matching a query.

    Results are ordered by relevance and limited to 100 products.

    Args:
        query: The text to search for.
        *categories: The categories to restrict the search to.
        **filters: The additional filters to apply, such as `max_price` or `in_stock`.

    Returns:
        The matching products, or an empty list if there are none.
    """
```

### Returns

A function that returns a value has `Returns:`, describing the value, including a possible `None`.

```python
def find_order(order_id: str) -> Order | None:
    """Looks up an order by ID.

    Args:
        order_id: The ID assigned when the order was placed.

    Returns:
        The matching order, or `None` if no order has that ID.
    """
```

### Yields

A generator has `Yields:` instead of `Returns:`, describing each yielded object.

```python
def read_orders(path: Path) -> Iterator[Order]:
    """Reads orders from a CSV file.

    Orders are read lazily, so the file can be larger than the available memory.

    Args:
        path: The path of the CSV file.

    Yields:
        An order for each valid row. Rows with missing fields are skipped.
    """
```

### Raises

A function that raises exceptions callers can reasonably expect to handle has `Raises:`, listing each one with the condition that triggers it.

Exceptions that only signal incorrect use, such as an invalid argument type, are not listed.

```python
def save_catalog(catalog: Catalog, path: Path) -> None:
    """Saves the product catalog to a file.

    An existing file at `path` is overwritten.

    Args:
        catalog: The catalog to save.
        path: The path of the destination file.

    Raises:
        PermissionError: If the file at `path` cannot be written.
    """
```

## Constructors

An explicit `__init__` always has a docstring, with `Args:` for its parameters.

Other simple dunder methods, such as `__str__`, `__repr__`, and `__post_init__`, need no docstring.

```python
def __init__(self, *, token: str, timeout: float = 10.0, **client_options: object) -> None:
    """Initializes the payment gateway client.

    Args:
        token: The token used to authenticate with the gateway.
        timeout: The maximum time to wait for a response, in seconds.
        **client_options: The additional options passed to the underlying HTTP client.
    """
```

## Overridden methods

An `@override` method that preserves the inherited contract has no docstring, so tools show the base documentation.

When the override refines the contract or adds details, such as side effects, its docstring describes what differs from the base.

```python
class EmailNotifier(Notifier):
    @override
    def send(self, message: str) -> None:
        self._client.deliver(message)


class SmsNotifier(Notifier):
    @override
    def send(self, message: str) -> None:
        """Sends a message by SMS.

        Messages longer than 160 characters are truncated.
        """
        self._client.deliver(message[:160])
```

## Properties

A property docstring is a noun phrase describing its value. Pass-through properties that only expose a private attribute need no docstring.

```python
@property
def total_price(self) -> Decimal:
    """The total price of the order, including tax."""
```
