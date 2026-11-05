# NOTICE

This project is a fork of **deltachat-desktop**
(<https://github.com/deltachat/deltachat-desktop>), based on the upstream
tag **v1.40.4**. It is published at
<https://github.com/SyntaxMusician/deltachat-desktop>.

## Copyright

- Copyright © 2019 and later, the **Delta Chat project / deltachat
  contributors** (see `CONTRIBUTORS.md` in this repository) — upstream
  deltachat-desktop.
- Local modifications of this fork (fetch-all-inbox on login, per-chat E2EE
  toggle, `DC_SYNC_LOG` log flushing) — the maintainers of this fork.

This fork is a derivative work of deltachat-desktop v1.40.4.

## License

This project is licensed under **GPL-3.0-or-later** — see the `LICENSE` file
(GPL-3.0) in this repository for the full license text.

## Bundled core dependency

The `deltachat-node` dependency bundled with this project is **not** the npm
package: it is the patched deltachat-core-rust **v1.121.0** distribution
<https://github.com/SyntaxMusician/deltachat-node-custom> (pinned to commit
`77f17f2` in `package-lock.json`). That distribution is licensed **MPL-2.0**
(Rust core) with **GPL-3.0-or-later** npm packaging metadata, and is built
from source during `npm install` (no prebuilt binaries).
