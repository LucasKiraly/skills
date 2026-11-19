---
name: python-docstrings
description: Document Python modules, classes, functions, and methods with concise Google-style docstrings.
metadata:
  author: Lucas Kiraly
  updated: "2026-11-19"
---

# Python Docstrings

Document Python modules, classes, functions, and methods with concise Google-style docstrings.

## Instructions

1. Determine the target scope from the request.

   * If a module or file is requested, document the module following the order below.

   * If a specific class, function, method, property, or symbol is requested, document only that target.

   * If only a symbol name is given and matches multiple places in the source tree, list the matches and ask which one to document before editing.

2. Read the complete containing module before writing documentation.

   * Inspect public behavior, dependencies, inheritance, decorators, and related types.

   * Read local dependencies only when needed to understand the public contract.

   * Do not infer behavior from names or type annotations alone.

3. Preserve complete existing docstrings.

   A docstring is complete when it has a useful summary and applicable sections for parameters, returns, yields, raises, or attributes. Update only missing or incomplete documentation.

4. When documenting a module, apply documentation in this order:

   1. Module docstring.

   2. Public classes, dataclasses, Pydantic models, configuration classes, and enums.

   3. Constructors and their parameters.

   4. Public methods and functions.

   5. Private methods only when their behavior is complex, non-obvious, or important for maintenance.

   Skip trivial private methods, simple dunder methods (`__str__`, `__repr__`, `__post_init__`), and pass-through properties.

## Module docstring format

Document every module right after the copyright header and before imports, using a one-line summary followed by an optional context paragraph when needed.

```
"""Load and validate application configuration.

Read configuration files and expose validated settings used at startup.
"""

```

## Class docstring format

Document every public class with a one-line summary followed by optional context about its public behavior, usage, or caveats.

```
class FrameProcessor:
    """Process frames in a pipeline.

    Receive frames, process them, and pass them to the next or
    previous processor.
    """

```

When a class exposes event handlers, list their names without backticks and include an `Example:` block only when it clarifies how to register a handler.

```
class FrameProcessor:
    """Process frames and emit lifecycle events.

    Event handlers available:

    - on_before_process_frame: Called before processing a frame.
    - on_after_process_frame: Called after processing a frame.

    Example:

        @processor.event_handler("on_before_process_frame")
        async def on_before_process_frame(processor, frame):
            ...
    """

```

## Constructor format

Document every explicit `__init__` written in the class and always describe its parameters.

```
def __init__(self, *, api_key: str, sample_rate: int = 22050, **kwargs):
    """Initialize the speech service.

    Args:
        api_key: API key used for authentication.
        sample_rate: Audio sample rate in Hz. Defaults to 22050.
        **kwargs: Additional arguments passed to the parent service.
    """

```

Do not add an `__init__` method or docstring when the constructor is generated automatically (such as in dataclasses or Pydantic models). Document fields under `Attributes:` in the class docstring instead.

## Method format

Document every public method or function with a one-line summary, followed by context and sections only when useful. Never begin the summary with `Returns`.

```
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

Use a hanging indent when a description exceeds 72 characters. Indent continuation lines two or four spaces further than the parameter name, and match the file's existing convention.

```
    Args:
        destination: Directory where downloaded files are stored.
            Create it before calling this method when it does not exist.
        *filters: Predicates to apply to each row.
        **options: Backend-specific query options.

```

### Returns

Use `Returns:` when a returned value has non-obvious semantics, shape, or sentinel values. Omit it for `None` and obvious results that need no explanation.

```
    Returns:
        Parsed configuration for the current environment.

```

### Yields

Use `Yields:` instead of `Returns:` for generators.

```
def iter_batches(records, size):
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

```
    Raises:
        FileNotFoundError: If the configuration file does not exist.
        ConfigError: If the file contains invalid settings.

```

## Overridden methods

Retain inherited documentation for an `@override` method when it preserves the public contract. Add or update the docstring when the override changes behavior, accepted inputs, returned values, side effects, exceptions, or timing.

```
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

```
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

```
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

```
@property
def timeout(self) -> float:
    """The request timeout, in seconds."""

```

## Docstring style rules

* Use triple double quotes (`"""`).

* Keep every line at 72 characters or fewer.

* End the summary line with a period.

* Put a blank line between the summary and subsequent sections.

* Use concise, professional language.

* Use imperative mood for functions and methods (e.g., `Fetch rows.`).

* Use a noun phrase for properties, attributes, and modules.

* Explain purpose and observable behavior before implementation details.

* Use backticks for code references, parameters, and literals.

* Do not restate obvious behavior or duplicate type annotations.

* Add usage examples only when they clarify non-obvious behavior.

## Checklist

Before finishing, verify that:

* Identify the target scope and read the containing module.

* Ensure the module docstring is present when the target scope is a module.

* Document applicable classes, public methods, and functions concisely.

* Document private methods only when their behavior is non-obvious.

* Include `Args:`, `Returns:`, `Yields:`, `Raises:`, and `Attributes:` sections where useful.

* Document fields or values under `Attributes:` for dataclasses, config classes, Pydantic models, and enums.

* Leave existing complete docstrings untouched.

* Keep wording concise, use imperative mood for actions, and match the implementation.

* Keep every produced docstring line at 72 characters or fewer.
