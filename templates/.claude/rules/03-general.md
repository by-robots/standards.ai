# General Coding Conventions

- If code you write requires a comment to explain what it does, rewrite the code instead.
- Keep functions single-purpose. Split any function that does more than one distinct thing, or that cannot be summarised in a phrase without using "and".
- Before writing a new file, read a comparable existing one and match its structure, naming, and error-handling approach. If you deviate, say why in your summary.
- When an existing pattern looks wrong, say so and cite where it came from before following it. Match the codebase's conventions; do not inherit its defects.
- Change only what the task requires. Do not reformat, rename, or restructure code you are not otherwise touching.
- If the same error persists after two distinct fix attempts, stop and report what you tried. Do not attempt a third variation.
- Do not silently swallow exceptions or leave unhappy paths unhandled.
- Do not introduce abstraction unless it is used in more than one place.
- Prefer built-in and framework-provided operations over manual equivalents. If a single operation achieves what multiple steps do (e.g. upsert over separate create and update), use it.
- Identify edge cases before implementing. Raise them for discussion only when the correct handling is ambiguous; otherwise handle them and list them in your summary.
- Do not assume your knowledge of a library's API matches the version in use. When uncertain, look up the versioned documentation or read the installed dependency's source or type definitions rather than working from prior knowledge.
- Respect the project's linter and formatter configuration. Do not disable rules inline without explicit approval.
- In web applications, keep business logic out of controllers, handlers, and other entry points. Extract it into service objects, plain classes, or modules.
- Scope data access through the current user or equivalent context rather than querying top-level models directly.
- Prefer explicit null/nil checks over implicit truthiness. Do not rely on falsy coercion when the value could be `0`, `""`, or an empty collection.
- Use batched iteration for large collections rather than loading everything into memory at once.
