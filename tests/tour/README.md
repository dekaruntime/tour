# Tour lessons

Canonical DekaScript for deka.gg. The website owns prose; this directory owns
the samples. Match by `id` in `manifest.json`, never by display name.

```
bun tests/tour/run.mjs
```

`run.mjs` compiles every lesson with the local compiler binary: `DSC` or
`DEKA_NATIVE` env var, else `target/release/dsc` or `target/release/cli`.
A language change that breaks a lesson fails here.

Every `.dsx` lesson must `export fn Page` (RFD 24 §16): a `.dsx` lesson is
`app/page.dsx`, and the website harness imports the `Page` export. `run.mjs`
asserts this contract and fails a `.dsx` lesson that ends in a bare JSX
expression instead.
