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
