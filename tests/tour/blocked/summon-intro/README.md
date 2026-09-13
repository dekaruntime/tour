# Summon intro — blocked draft

Stable ID reserved: `summon-intro`. Intended placement: after
`result-vs-exception` and its diagnostic, before `try-diagnostic`.

**blocked-on-harness:** the native runner copies only the lesson source to
`project/lesson.ds`; it does not ship a sibling `.mjs`. Summon checks the vendored
module's exports at compile time, so publishing this as a single-source lesson
would fail. This nested directory is deliberately outside `manifest.json` and
the runner's top-level source scan. It must not be published as a runnable page.

**blocked-on-syntax:** dsc 0.51.1 requires a colon before a summon signature's
return type (`expected ':'`). This lane forbids colon return types. The draft
uses the requested colon-free spelling, which is not yet accepted for summon.
Do not add it to the manifest until that syntax is supported (or the content
rule is explicitly changed), as well as sibling-module delivery being available.

The example keeps one DekaScript source and one small vendored module. `double`
is declared total; `decode` can raise a native URIError, represented by JsError.
The compiler checks local exports and arity, but `total` is an author contract,
not proof that arbitrary JavaScript cannot fail. No fetch or package workflow is
needed to understand this boundary.

Validation with released dsc 0.51.1 + deka 0.50.0: the exact draft is rejected at
the first summon return type. A scratch copy using the compiler's required
signature punctuation, alongside `vendor.mjs`, runs and prints `6`, `Deka`, and
`could not decode`, one per line. This does **not** count as a passing lesson;
the draft is excluded from the native runner total.
