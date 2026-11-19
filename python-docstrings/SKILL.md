---
name: python-docstrings
description: Document Python modules, classes, functions, and methods with concise Google-style docstrings.
metadata:
  author: Lucas Kiraly
  updated: "2026-11-19"
---

# Python Docstrings

Follow these instructions to write and maintain concise, consistent Google-style docstrings across Python codebases.

## Instructions

1. Determine the target scope from the request.

   - If a module or file is requested, document the module following the order below.
   - If a specific class, function, method, property, or symbol is requested, document only that target.
   - If only a symbol name is given and matches multiple places in the source tree, list the matches and ask which one to document before editing.

2. Read the complete containing module before writing documentation.

   - Inspect public behavior, dependencies, inheritance, decorators, and related types.
   - Read local dependencies only when needed to understand the public contract.
   - Do not infer behavior from names or type annotations alone.
   - Base documentation on actual implementation and observable behavior.

3. Preserve existing complete and accurate docstrings.

   A docstring is complete when it has a useful summary and all applicable sections for parameters, returns, yields, raises, or attributes. Update only missing, incomplete, or inaccurate documentation.

4. When documenting a module, apply documentation in this order:

   1. Module docstring.
   2. Public classes, dataclasses, Pydantic models, configuration classes, and enums.
   3. Constructors and their parameters.
   4. Public methods and functions.
   5. Private methods and functions only when their behavior is complex, non-obvious, or important for maintenance.

   Skip trivial private callables, simple dunder methods (`__str__`, `__repr__`, `__post_init__`), and pass-through properties.

## Module docstring format

Document every module right after the copyright header and before imports, using a one-line summary followed by an optional context paragraph when needed.

```python
"""Application configuration loading and validation.

This module reads configuration files and exposes validated settings used at startup.
"""
```

## Class docstring format

Document every public class with a one-line summary followed by optional context about its public behavior, usage, or caveats.

```python
class RetryPolicy:
    """Define retry behavior for failed operations.

    Set the maximum number of attempts and the delay between retries.
    """
```

When a class exposes event handlers, list their names using backticks and include an `Example:` block only when it clarifies how to register a handler.

```python
class FrameProcessor:
    """Process frames and emit lifecycle events.

    Event handlers available:

    - `on_before_process_frame`: Called before processing a frame.
    - `on_after_process_frame`: Called after processing a frame.

    Example:

        @processor.event_handler("on_before_process_frame")
        async def on_before_process_frame(
            processor: FrameProcessor,
            frame: Frame,
        ) -> None:
            ...
    """
```

## Constructor format

Document every explicit `__init__` defined in the class and always describe its parameters.

```python
def __init__(
    self,
    *,
    api_key: str,
    sample_rate: int = 22_050,
    **kwargs: object,
) -> None:
    """Initialize the speech service.

    Args:
        api_key: API key used for authentication.
        sample_rate: Audio sample rate in Hz. Defaults to 22050.
        **kwargs: Additional arguments passed to the parent service.
    """
```

Do not add an `__init__` method or docstring when the constructor is generated automatically, such as in dataclasses or Pydantic models. Document fields under `Attributes:` in the class docstring instead.

## Method format

Document every public method or function with a one-line summary, followed by context and sections only when useful. Never begin the summary with `Returns`.

```python
async def put(
    self,
    item: tuple[Frame, FrameDirection, FrameCallback],
) -> None:
    """Put an item into the priority queue.

    Give system frames higher priority than other frames.

    Args:
        item: Item to enqueue.
    """
```

### Args

List each parameter by its exact signature name, followed by a colon and description. Include `*args` and `**kwargs` with those exact names.

Use a hanging indent for continuation lines and match the file's existing indentation convention. Keep descriptions concise and readable.

```python
def list_users(
    page: int,
    page_size: int,
    *filters: str,
    **options: object,
) -> list[dict[str, object]]:
    """List users with optional filters and query options.

    Args:
        page: Page number to retrieve.
        page_size: Maximum number of users per page.
        *filters: Additional conditions to filter users.
        **options: Additional query options, such as ordering.
    """
```

### Returns

Use `Returns:` when a returned value has non-obvious semantics, shape, or sentinel values. Omit it for `None` and obvious results that need no explanation.

```python
def load_config(environment: str) -> dict[str, str]:
    """Load configuration for the specified environment.

    Args:
        environment: Name of the target environment.

    Returns:
        Parsed configuration for the current environment.
    """
```

### Yields

Use `Yields:` instead of `Returns:` for generators.

```python
def iter_batches(records: list[str], size: int) -> Iterator[list[str]]:
    """Yield records in batches of a fixed size.

    Args:
        records: Records to group.
        size: Maximum number of records per batch.

    Yields:
        Lists of records, with a shorter final batch when needed.
    """
```

### Raises

Use `Raises:` for exceptions callers can reasonably expect during normal use. Describe the condition that triggers each exception and use the same hanging-indent rule as `Args:` for long descriptions.

```python
def validate_age(age: int) -> None:
    """Validate a person's age.

    Args:
        age: Age to validate.

    Raises:
        ValueError: If the age is negative.
    """
```

## Overridden methods

Do not add a docstring solely to repeat inherited documentation when an `@override` method preserves the public contract.

Add or update the docstring when the override changes behavior, accepted inputs, returned values, side effects, exceptions, or timing.

```python
@override
def connect(self, timeout: float) -> Connection:
    """Connect with a bounded wait time.

    Args:
        timeout: Maximum time to wait for the connection.

    Returns:
        An active connection.
    """
```

## Dataclass, configuration, and Pydantic model format

Document dataclasses, configuration classes, and Pydantic `BaseModel` classes as data contracts. Use `Attributes:` to document fields, meaningful defaults, validation constraints, aliases, and serialization behavior that affect callers.

```python
class ServiceSettings(BaseModel):
    """Connection settings for a service.

    Attributes:
        api_url: Base URL of the service.
        timeout: Request timeout in seconds. Defaults to 30.
        retry_count: Maximum retries. Must be nonnegative.
    """

    api_url: HttpUrl
    timeout: int = 30
    retry_count: int = Field(ge=0)
```

## Enum format

Document an enum's purpose and each public member under `Attributes:`.

```python
class Status(Enum):
    """Processing state of a task.

    Attributes:
        READY: Ready to accept work.
        RUNNING: Processing work.
    """

    READY = 1
    RUNNING = 2
```

## Property format

Document a property's value or contract with a noun phrase. Do not use `Returns:`.

```python
@property
def timeout(self) -> float:
    """The request timeout, in seconds."""
```

## Docstring style rules

- Use triple double quotes (`"""`).
- Keep docstrings concise, readable, and well-formatted.
- End the summary line with a period.
- Put a blank line between the summary and subsequent sections.
- Use concise, professional language.
- Use imperative mood for functions and methods (e.g., `Fetch rows.`).
- Use a noun phrase for properties, attributes, and modules.
- Explain purpose and observable behavior before implementation details.
- Use backticks for code references, parameters, and literals.
- Do not restate obvious behavior or duplicate type annotations.
- Add usage examples only when they clarify non-obvious behavior.

## Checklist

Before finishing, verify that:

- The target scope has been identified and the containing module has been read.
- The module docstring is present when the target scope is a module.
- Applicable classes, public methods, and functions are documented.
- Private callables are documented only when their behavior is non-obvious.
- Applicable `Args:`, `Returns:`, `Yields:`, `Raises:`, and `Attributes:` sections are included.
- Dataclasses, configuration classes, Pydantic models, and enums document their fields or members under `Attributes:`.
- Existing complete and accurate docstrings remain untouched.
- Wording is concise, uses imperative mood for actions, and accurately reflects the implementation.
- Docstrings are concise, readable, and consistently formatted.
