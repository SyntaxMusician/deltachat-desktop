# Delta Chat Desktop (fork) <a id="deltachat-desktop"></a>

This is a fork of [deltachat/deltachat-desktop](https://github.com/deltachat/deltachat-desktop),
based on the upstream tag **v1.40.4**, published at
<https://github.com/SyntaxMusician/deltachat-desktop>.

It is the **Desktop Application for [delta.chat](https://delta.chat)** —
an email-based messenger that works with any email account.

<center><img src="README_ASSETS/screenshot.png"/></center>

## Differences from upstream deltachat-desktop <a id="differences"></a>

On top of upstream v1.40.4 this fork adds:

1. **Show all existing inbox messages on first login** — on login/startup the
   app resets the `fetch_existing_msgs` / `fetched_existing_msgs` settings
   (`src/main/deltachat/controller.ts`, `src/renderer/ScreenController.tsx`),
   so the whole existing inbox is imported into chats instead of only the
   newest messages. The heavy lifting is done by the patched core
   (see below).
2. **Per-chat end-to-end encryption toggle** — a "Prefer End-To-End
   Encryption" switch in the group profile
   (`src/renderer/components/dialogs/ViewGroup.tsx`) and in the contact
   profile for 1:1 chats (`src/renderer/components/dialogs/ViewProfile.tsx`).
   It calls the `set_chat_e2ee_enabled` JSON-RPC method and reads the
   `e2eeDisabled` field of `get_full_chat_by_id` — both provided by the
   patched core.
3. (minor) `DC_SYNC_LOG=1` env var for synchronous log flushing that survives
   hard crashes (`src/main/log-handler.ts`).

Additionally, the upstream CI/IDE/Transifex configs (`.github/`, `.vscode/`,
`.tx/`) were removed, and the `deltachat-node` dependency does **not** come
from npm — it points at the patched core distribution
<https://github.com/SyntaxMusician/deltachat-node-custom>
(`"deltachat-node": "git+https://github.com/SyntaxMusician/deltachat-node-custom.git"`
in `package.json`, pinned to commit `77f17f2` in `package-lock.json`).
`deltachat-node-custom` is deltachat-core-rust v1.121.0 plus the Node.js
bindings of the historical `deltachat-node@1.121.0` npm package (no prebuilt
binaries) plus the two core patches above (fetch all inbox messages, per-chat
E2EE toggle exposed over JSON-RPC).

## Building from source <a id="source"></a>

Prerequisites:

- **Node.js >= 16** (see `engines` in `package.json`)
- **Rust 1.65+**, a **C compiler** and **perl** — `npm install` runs the
  install script of `deltachat-node-custom`, which builds the Rust core and
  the native addon from source (vendored OpenSSL and sqlcipher); no prebuilt
  core binaries are downloaded.

```sh
# Get the code
$ git clone https://github.com/SyntaxMusician/deltachat-desktop.git
$ cd deltachat-desktop

# Install dependencies (this also builds the patched core from source;
# the core sources come from deltachat-node-custom automatically via npm,
# no manual checkout needed)
$ npm install

# Build the app (backend + frontend; only needed on the first time or if the code was changed)
$ npm run build

# Start the application (dev run)
$ npm start
```

To build installers use `npm run pack:linux` (AppImage/deb), `npm run pack:win`
(nsis/portable) or `npm run pack:mac` (dmg/mas) — see the `scripts` section in
`package.json` for all variants.

To update the core in this fork, see [docs/UPDATE_CORE.md](docs/UPDATE_CORE.md).

## Documentation <a id="docs"></a>

For users: [TROUBLESHOOTING](./docs/TROUBLESHOOTING.md),
[CLI flags](./docs/CLI_FLAGS.md), [Keybindings](./docs/KEYBINDINGS.md),
[custom Themes](./docs/THEMES.md).

For developers: [Logging](./docs/LOGGING.md),
[DEVELOPMENT](./docs/DEVELOPMENT.md), [Styling Guidelines](./docs/STYLES.md),
[Update core](./docs/UPDATE_CORE.md), [E2E testing](./docs/E2E-TESTING.md),
[Release workflow](./docs/RELEASE_WORKFLOW.md).

## License <a id="license"></a>

Licensed under `GPL-3.0-or-later`, see the [LICENSE](./LICENSE) file for
details and [NOTICE.md](./NOTICE.md) for attribution.

> Copyright © 2019 `DeltaChat` contributors (upstream), plus the local
> modifications of this fork.

> This program is free software: you can redistribute it and/or modify
> it under the terms of the GNU General Public License as published by
> the Free Software Foundation, either version 3 of the License, or
> (at your option) any later version.

> This program is distributed in the hope that it will be useful,
> but WITHOUT ANY WARRANTY; without even the implied warranty of
> MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
> GNU General Public License for more details.

> You should have received a copy of the GNU General Public License
> along with this program. If not, see <http://www.gnu.org/licenses/>.
