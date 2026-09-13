# Tour lessons

Canonical DekaScript for deka.gg. The website owns prose; this directory owns
the samples. Match by `id` in `manifest.json`, never by display name.

```
bun tests/tour/run.mjs
```

`run.mjs` compiles every lesson with the local compiler binary: `DSC` or
`DEKA_NATIVE` env var, else `target/release/dsc` or `target/release/cli`.
A language change that breaks a lesson fails here.

Lessons import `@deka/io`, and dsc is a compiler with no package manager, so
seed the shared package cache once with a deka CLI (`run.mjs` fills
`.cache/deka-packages/io/` on a cache miss):

```
DEKA_NATIVE=/path/to/deka bun tests/tour/run.mjs   # once, to seed
DSC=/path/to/dsc bun tests/tour/run.mjs            # every run after
```

To seed without a deka CLI, lay out `.cache/deka-packages/io/` by hand:
`ds_modules/@deka/io/` from the `io` tarball on the deka.gg stdlib CDN plus a
`deka.lock`. Once seeded, the cache is reused and the suite runs offline.

Every `.dsx` lesson must `export fn Page` (RFD 24 §16): a `.dsx` lesson is
`app/page.dsx`, and the website harness imports the `Page` export. `run.mjs`
asserts this contract and fails a `.dsx` lesson that ends in a bare JSX
expression instead.

## New language surface (dsc 0.51.1)

With release binaries, use deka 0.50.0 as the native CLI and select its compiler
with `DEKA_DSC` (distinct from the runner's direct-compiler `DSC` variable):

```sh
DEKA_DSC=/absolute/path/to/dsc-0.51.1 \
DEKA_NATIVE=/absolute/path/to/deka-0.50.0 \
bun tests/tour/run.mjs
```

The manifest order places indexing proofs after the first indexing sample,
Exceptions and Result conversion after errors-as-values, ternary after match
expressions, and Option erasure after Result/Option. Existing IDs and their
relative order are preserved. Diagnostic companions verify that unproven
indexing, implicit Exception-to-Result coercion, and nested Option are rejected.

The runner checks compile outcomes and diagnostic substrings, not stdout.
The five new runnable lessons include expected output comments; their output
was also checked with `deka run` using the same pinned compiler and io package.

[Summon intro](blocked/summon-intro/README.md) is an excluded draft, blocked on
sibling-module delivery. dsc#188 (0.52.0) unblocks its colon-free summon syntax.
It is not a manifest entry or a published runnable page.
