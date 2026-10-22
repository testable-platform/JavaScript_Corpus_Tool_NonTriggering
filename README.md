# JS_V14_ROLLUP_BUN_MICRO

Part of the `javascript-combos` white-box test-repo corpus (GraniteMill /
`granite-mill`, domain: Community garden plots).

## Branch variables

| Variable | This branch |
| --- | --- |
| Branch | `JS_V14_ROLLUP_BUN_MICRO` |
| Node.js | 14.21.3 (family V14) |
| Bundler | Rollup |
| Package manager | bun |
| Bundled npm | 6.14.18 |
| Architecture | Microservices |
| Source root | `packages/shared/src` |

The application code under `src/` (or `packages/*/src/` for Microservices
branches) is byte-identical across all 576 branches of this corpus; only the
build tool, package manager, architecture layout, and each tool's real
status on this Node family vary.

## Supported tools

24 tool folders are wired on this corpus (one `Tool Triggering (Synthetic Data)/<dir>/` folder
each, covering the 103-metric white-box framework): the 23 tools of the tool list,
plus `eslint-scope`, the data-flow script behind the "ESLint (eslint-scope)" entry.
**9 of them run on Node 14; 15 do not.**

That is the measurement, not a defect. A tool that cannot run exits **3**,
not 0 -- a skip that looks like a pass is the failure mode this corpus
exists to expose. Every `Tool Triggering (Synthetic Data)/<dir>/trigger.yaml` records whether its status
here was actually invoked and observed by Claude Code (`measured: true`) or
is a real npm-registry `engines.node` claim not yet individually
invoke-verified on this exact family (`measured: false`) -- declared support
is a claim, invoking is the fact, and this corpus never blurs the two.

### Running here

| Tool | Role | Pin | Why |
| --- | --- | --- | --- |
| `Lizard` | primary | lizard (pip) | measured directly: pip-installed and run for real against this domain's src/ (lizard 1.24.0). Found policy.js's evaluatePolicy as the highest-CCN function at 18 -- corrects an earlier assumption that dataflow.js's tally loop (CCN 17) was highest; Node-independent by construction, so this holds on every family. _(measured: real invocation confirmed by Claude Code)_ |
| `jscpd` | primary | jscpd==4.3.0 | installs and runs (12's `??` wall is gone), but see the ignore-list fix below. _(measured: real invocation confirmed by Claude Code)_ |
| `npm audit + npm ls` | primary | n/a | npm's own bundled audit/ls -- ships with npm itself on every Node family this corpus supports. _(declared active from npm registry data; not yet individually invoke-verified on this family)_ |
| `npm ls` | primary | n/a | npm's own bundled `npm ls` -- ships with npm itself on every Node family this corpus supports (npm 6.14.18 on this branch). _(declared active from npm registry data; not yet individually invoke-verified on this family)_ |
| `nyc + mocha` | primary | nyc==15.1.0 | clean on every family once the architecture-aware test glob (bug #1) and the Microservices require() path (bug #4) were both fixed. _(measured: real invocation confirmed by Claude Code)_ |
| `monocart-coverage-reports` | alternative | monocart-coverage-reports==2.13.0 | monocart-coverage-reports 2.13.0 declares Node 14 compatibility (npm registry engines.node range). _(declared active from npm registry data; not yet individually invoke-verified on this family)_ |
| `diff-cover` | primary | diff-cover (pip) | the diff-cover CLI itself was invoked for real (pip install + --help) and works; its coverage-delta output depends on nyc's cobertura report, which needs a full npm install this session's verification bridge could not complete (slow mounted filesystem + shared disk quota, not a tool defect -- see javascript-repos-build-contract.md). Node-independent by construction. _(measured: real invocation confirmed by Claude Code)_ |
| `ESLint (eslint-scope)` | primary | eslint-scope==7.2.2 | clean on every family tested (12 through 26) -- the dataflow script is hand-written without `?.`/`??` specifically so it never depends on the same syntax features that break other tools on the oldest families. _(measured: real invocation confirmed by Claude Code)_ |
| `PyDriller` | primary | pydriller (pip) | PyDriller is a Python package invoked externally via a small wrapper script -- Node-version-independent by construction. _(declared active from npm registry data; not yet individually invoke-verified on this family)_ |

### Dark here

| Tool | Role | Status | Why |
| --- | --- | --- | --- |
| `debtmap` | alternative | not installed | debtmap is a Rust CLI (`cargo install debtmap`; version 0.24.1 on crates.io), not present on this host. The Clean corpus's debtmap README records it built from crates.io and run for real there. |
| `eslint-plugin-sonarjs` | primary | skipped | Runs through eslint itself, whose own status on Node 14 governs this plugin: eslint itself now runs (12's `?.` wall is gone on 14), but `ERR_UNKNOWN_BUILTIN_MODULE: node:path/posix` inside eslint-plugin-sonarjs blocks it -- a specific unresolvable `node:`-prefixed submodule path at this Node 14 patch, distinct from the N12 syntax crash. |
| `cccc` | alternative | not installed | CCCC (C and C++ Code Counter) is a native program, not installable via npm/pip on this host. It parses C, C++, Ada and Java, not JavaScript, so on this branch's JavaScript it is not expected to produce meaningful metrics (see Tool Clean (Synthetic Data)/cccc/README.md). Version 3.2.0 is the Debian package's upstream version; it was not confirmed against the upstream project. |
| `Dolos` | alternative | not installed | dolos's native tree-sitter step needs a C++ toolchain -- already documented as absent on this host by the C#/Python sibling corpora's own precedent; installs with --ignore-scripts, which skips the native build |
| `eslint` | primary | skipped | eslint itself now runs (12's `?.` wall is gone on 14), but `ERR_UNKNOWN_BUILTIN_MODULE: node:path/posix` inside eslint-plugin-sonarjs blocks it -- a specific unresolvable `node:`-prefixed submodule path at this Node 14 patch, distinct from the N12 syntax crash. |
| `oxlint` | alternative | skipped | same ERR_UNKNOWN_FILE_EXTENSION as N12 -- confirmed not Node-version-specific. |
| `eslint-plugin-security` | primary | skipped | Runs through eslint itself, whose own status on Node 14 governs this plugin: eslint itself now runs (12's `?.` wall is gone on 14), but `ERR_UNKNOWN_BUILTIN_MODULE: node:path/posix` inside eslint-plugin-sonarjs blocks it -- a specific unresolvable `node:`-prefixed submodule path at this Node 14 patch, distinct from the N12 syntax crash. |
| `OpenGrep` | alternative | not installed | standalone binary, not installable via npm/pip on this host -- same category as Dolos's native step and the Python corpus's own Trivy/OpenGrep entries |
| `Trivy` | alternative | not installed | standalone binary, not installable via npm/pip on this host |
| `Mocha` | primary | skipped | mocha 12.0.1 declares Node ^20.19.0 || >=22.12.0 (npm registry engines.node range); Node 14.21.3 does not satisfy it. This branch's own `npm test` runs mocha@10.8.2. |
| `StrykerJS + Mocha` | primary | skipped | the inquirer `??` crash is gone on 14, but `@stryker-mutator/mocha-runner` still resolves to an old release (6.4.2, Node 14's engines ceiling) whose internal file-collection bridge (`lib-wrapper.js`) is incompatible with the mocha release that resolves alongside it (`TypeError: testFileNames.forEach is not a function`) -- confirmed by direct comparison against the working 16/18 pairing, not assumed. |
| `gutcheck` | alternative | skipped | gutcheck has no release compatible with Node 14 (npm registry engines.node range, prereleases excluded). |
| `CodeQL` | alternative | not installed | standalone binary (CodeQL CLI release bundle), not installable via npm/pip on this host; the Clean corpus's CodeQL README records GitHub release downloads returning 403 in the build environment. |
| `knip` | alternative | skipped | knip has no release compatible with Node 14 (npm registry engines.node range, prereleases excluded). |
| `Git-Spark` | alternative | skipped | Git-Spark has no release compatible with Node 14 (npm registry engines.node range, prereleases excluded). |

## Build

```
npm install    # or yarn / pnpm / bun, per this branch's packageManager field
npm run build
```

## Run

```
node packages/shared/src/index.js
```

## Test

```
npm test              # mocha packages/shared/tests/**/*.test.js
npm run coverage      # nyc + mocha
```


## Planted fixtures

| Fixture | File(s) | Planted for |
| --- | --- | --- |
| Duplication | [`packages/shared/src/http-errors.js`](packages/shared/src/http-errors.js) + [`packages/shared/src/http-errors-legacy.js`](packages/shared/src/http-errors-legacy.js) | jscpd, Dolos |

## Workspace layout

```
javascript-combos/  (JS_V14_ROLLUP_BUN_MICRO)
|-- .github/
|-- packages/
|-- scripts/
|-- .eslintrc.json
|-- .gitignore
|-- .jscpd.json
|-- .nvmrc
|-- .nycrc.json
|-- .oxlintrc.json
|-- LICENSE
|-- README.md
|-- bunfig.toml
|-- dataset.json
|-- package.json
|-- rollup.config.mjs
|-- stryker.conf.json
```

## History

This branch's real commit history starts from the corpus's scaffolding
generation, followed by the six genuine scaffolding-bug fixes found by
Claude Code's actual local install/test runs across all nine Node families
(see `javascript-repos-build-contract.md`) -- every commit here is real,
authored by the accounts that actually did the work; nothing is
back-filled or fabricated.

## Machine-readable

[`dataset.json`](dataset.json) carries every branch variable and the full
tool-status breakdown, including the reason each dark tool is dark and
whether that status was actually measured. It is the answer key: a run is
correct when what the tool platform reports matches what `dataset.json`
says should happen, including the tools that are supposed to be dark.

See `javascript-repos-build-contract.md` in the Testable (Tools) project for
the full 103-metric roster, the repair notes, and the live pin-resolution
method (npm registry `engines.node` ranges, prereleases excluded).

<!-- tools-non-triggering -->
## Tools non triggering (Synthetic Data)

One of five per-branch data folders in the original combined `testable-platform/javascript-corpus` repo -- this repo carries only this one.

Clean makes each tool run and report nothing wrong. Invalid makes it run and
report something. This folder holds data with nothing in it for any tool to
catch -- and, where no program would reach the tool at all, nothing for it to
start on. It is the negative control that tells *correctly detected nothing*
apart from *the scan never ran*.

`Tools non triggering (Synthetic Data)/` holds 23 tool-named folders, matching the names in Clean
and Invalid so the data sets line up name-for-name:

* **11 tools read source**, so they get their own minimal program, for node14 -- one class or one function, no branching, no
  duplication, no dependency, no dead export, no magic number.
* **12 tools cannot be answered by a program** -- they read a lockfile,
  a coverage report, compiled bytecode or the commit history -- so they carry
  the subject matter as a plain record instead, with the reason stated in that
  folder's own README.

**No manifest, no lockfile, no tool configuration, no runner and no
`trigger.yaml` anywhere in it**, so the folder adds no discovered project and
no task to a run.

See `Tools non triggering (Synthetic Data)/README.md` for the per-tool table, the mechanism each tool is
inert by, and what was measured.
