# Omarchy Kids AI Tutor

Home for the [Omarchy Kids](https://omarchy.org) AI Tutor and the assessment
plugins it will orchestrate. **This repository is not itself an Omarchy
plugin.** The tutor will live here; each skill check is a separate Omarchy
shell plugin under `plugins/`.

Target ages for the first release: **5–12**.

## Layout

| Path | What it is |
| --- | --- |
| `plugins/math` | Kids Math — first suggested plugin for kid-login onboarding |
| *(later)* tutor overlay | The AI Tutor that runs an initial aptitude check across plugins |

Kids Math (`omarchykids.math`) is not the tutor. It is a standalone overlay
the tutor will summon, alongside later siblings (reading, typing, and so on),
using a shared JSON assessment contract. The marketplace listing lives in
[mikeg0/omarchy-kids-math](https://github.com/mikeg0/omarchy-kids-math).

## Install a sub-plugin

Omarchy’s `plugin add` clones a git *root* that contains `manifest.json`, so
each listed plugin has its own repository:

```sh
omarchy plugin add https://github.com/mikeg0/omarchy-kids-math.git --enable
```

From this monorepo checkout (no inner symlinks — Omarchy rejects those):

```sh
cp -a plugins/math ~/.config/omarchy/plugins/omarchykids.math
omarchy plugin validate ~/.config/omarchy/plugins/omarchykids.math
omarchy-shell shell rescanPlugins
omarchy plugin enable omarchykids.math
```

Plugins run unsandboxed inside `omarchy-shell`. Read the QML before enabling.

## Shared assessment contract

Each skill plugin should accept a summon payload (`mode`, `learnerId`,
`level`, …) and expose the result only over shell IPC (`lastResult`), with
at least:

- `pluginId`, `schemaVersion`, `mode`, `status`
- `recommendedLevel`
- `skillScores`
- `items`

See [plugins/math/README.md](plugins/math/README.md) for the first complete
implementation. The tutor will stitch those snapshots into one aptitude
picture.

## Develop

```sh
node plugins/math/tests/math-engine-test.js
```

## License

MIT. See [LICENSE](LICENSE).
