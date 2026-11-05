# Update core

In this fork the core is **not** updated from npm. The `deltachat-node`
dependency in `package.json` points at the patched core distribution
<https://github.com/SyntaxMusician/deltachat-node-custom>
(deltachat-core-rust v1.121.0 + the Node.js bindings of the historical
`deltachat-node@1.121.0` npm package + the fork's core patches — see its
README):

```json
"deltachat-node": "git+https://github.com/SyntaxMusician/deltachat-node-custom.git"
```

To update the core:

1. Bump the git ref/commit SHA of the `deltachat-node` dependency in
   `package.json` (e.g. to
   `git+https://github.com/SyntaxMusician/deltachat-node-custom.git#<sha>`).
2. Update the matching `resolved` URL (including the `#<sha>` suffix) of
   `node_modules/deltachat-node` in `package-lock.json`.
3. Run `npm install` — its install script rebuilds the Rust core and the
   native addon from source (requires Rust 1.65+, a C compiler and perl).

Note that `@deltachat/jsonrpc-client` (the JSON-RPC API types) stays a
regular npm dependency (`^1.121.0` in `package.json`) and is updated
separately via npm.
