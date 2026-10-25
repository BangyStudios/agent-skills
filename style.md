When writing or editing PyTorch model code, also read `~/.agents/style_pytorch.md`.

## Naming

- `snake_case` hierarchical trees (`root_child_leaf`): each token narrows the meaning, so pruning the tail keeps the most meaning. Reuse stable roots across modules, one canonical tree per concept (no synonyms), plural only for collections. No vague roots (`data`, `info`, `obj`, bare `value`).
- Functions: `verb_domain_subdomain_detail`, a single verb root (`get`, `set`, `update`, `delete`, `load`, `save`, `pull`, `push`, `run`, `start`, `solve`, `add`, `init`, `insert`, `is`); no stacked verbs (`get_or_create_entity` -> `resolve_entity`).
- Variables: standalone ones put the kind first (`id_plant`, `time_start`); ones mirroring a structure follow its nesting (`plant_id` for the id in a plant dict, `penalty_value`, never `value_penalty`). Keep the direction consistent across the codebase.
- Predicates and boolean locals start with `is_` (not `check_`/`has_`).
- Aggregates and bounds trail the metric (`penalty_max`, `time_end`). Prefixes: `n_` counts, `d_` dimensions, `len_` lengths, `p_` probabilities.
- Time-dependent functions end in `_datetime` and take that parameter (`get_constraint_value_datetime`).
- Raw input and its parsed form share a root (`months_str` -> `months`). Don't invent qualifiers (`_text`, `_copy`); rebind the same name when transforming.
- Abbreviate only when already standard in the codebase (`tz`). Alias colliding imports into the tree (`from datetime import time as datetime_time`).
- No leading underscores unless necessary. A constant used by one class is a lowercase class attribute (`Command.types`), not module-level ALL_CAPS.

## Legibility over performance

- Write what a reader understands on first pass, even if slower, longer or less idiomatic. Don't optimize (caching, vectorizing, clever structures) unless asked or measured.
- One step per line: name intermediates instead of chaining or nesting. Prefer plain loops with early `continue`/`return`; no tricks (walrus, bit hacks, ternary returns, long chained comparisons).
- Minimal abstraction and nesting; no unused helpers. A helper or named variable is fine when it states intent; otherwise repeat small logic.
- Short single-purpose functions that read top to bottom in the order the work happens.
- Explicit conditions (`if n_items == 0`) over intent-hiding truthiness; name magic numbers.

## Conventions

- Annotate every parameter and return (`-> None` included) with `X | None` and builtin generics.
- Call everything with keyword arguments; parameter names match the caller's variables and config keys.
- Reduce with explicit loops: typed list (`values: list[float] = list()`), append in a `for`, `if not values: return None`, `return min(values)`. No filtering comprehensions or `any()`; plain map comprehensions are fine.
- Name a predicate's result before returning it.
- Read config with `.get(key)`; validate with `raise ValueError(...)` naming the key in quotes and the offending values.
- Mutators return `None`; constructors store arguments as given.
- Order definitions by first use: dependencies above callers, a shared one above its topmost caller; `__init__` first, most-dependent method last. Re-sort when editing.
- Comments state contracts only (bounds, expected failures, who handles errors: `# Closed floor`); no banners or restating code. Tag overcomplicated AI-written code `# AI Generated`, and never use tagged code as a style reference.
