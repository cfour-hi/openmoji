# Agent notes

For setup, contribution rules, and the SVG styleguide, see `CONTRIBUTING.md` and `helpers/README.md`. This file only covers gotchas an agent runs into that aren't written down elsewhere.

- **Node version**: pinned in `.nvmrc` (currently v22.14.0). Agent shells don't auto-source it and `nvm use` doesn't persist across separate tool calls — chain it into every command: `source ~/.nvm/nvm.sh && nvm use && <command>`.

- **`libxmljs` can silently gut test coverage**: `test/utils/utils.js` requires it unconditionally, so if its native binary doesn't match the active Node's ABI/arch, `mocha --parallel` workers for `layer.js`, `color.js`, `skintone.js`, and `file-format.js` crash on import — with no failing-test report, just a lower "passing" count. If that count looks off, or you see a `dlopen`/"incompatible architecture" error, run `npm rebuild libxmljs` under the correct Node version before trusting `npm test`.

- **Unicode versions might be ahead of the `emojibase-data` dependency**: see the loader block and comments around `data/unicode-draft-overrides.json` in `helpers/generate-data-tables.js`.

- **Committing multi-step data changes**: `data/openmoji.json`/`.csv` are derived from all source CSVs merged together. When splitting related work into meaningful commits, regenerate them at each commit boundary, not just the last one, so every commit is independently buildable/testable.
