# Testfoss demo agent guide

## Scope and source pin

This branch is `testfoss-demo`, based on upstream commit `c26e390544b6388f86e50387cd4565759b4da0df` (3Dmol.js 2.5.5). The public fork is `https://github.com/testfoss/3Dmol.js`; upstream is `https://github.com/3dmol/3Dmol.js`.

Only append the README Testfoss section and add `AGENTS.md`, `durable-memory/**`, `testfoss/**`, and `.github/workflows/testfoss.yml`. Never modify upstream source, examples (especially `examples/module.html`), tests, lockfiles, or other workflows. Do not commit `build/`, `node_modules/`, screenshots, or browser results. Do not open upstream issues or pull requests for this demo.

## Reproduce

Use Node 24 (24.15 or newer is required by lmscript 0.2.463); the historical lab build used Node 22. From the repository root:

```sh
./testfoss/run.sh package
(cd testfoss && npm ci && npx playwright install chromium)
./testfoss/run.sh browser
```

`package` deliberately uses `npm ci --ignore-scripts` followed by `npm run build:dev`, producing the local development UMD and ES-module bundles. The browser runner serves the repository on loopback port 8112 and runs `testfoss/browser.lms` through `lmwebtest playwright`. Browser dependencies are locked under `testfoss/`: lmwebtest commit `2920befd355eacc654697820d690db074e94dce3`, `@lmctl-ai/lmctl` 0.2.463, and Playwright 1.62.1. Every browser CI path must invoke lmwebtest.

`testfoss/module.html` is a complete byte-for-byte copy of the lab's locally modified `examples/module.html`, not a patch to the upstream example. Its one-level-deep location is required: `../build/3Dmol.es6.js`, `../tests/auto/data/4hhb.bcif.gz`, and `../tests/test_structs/multi.pdb` resolve to the local build and pristine upstream fixtures. Keep those relative paths unchanged.

## Limitations

The lab host could not complete ordinary `npm ci` or start upstream Jest because `canvas@2.11.2` had no Node 22 prebuilt binary and native Cairo/Pango development files were unavailable. Nineteen Jest suites failed during environment setup with zero tests executed; this is a host dependency limitation, not a browser-test pass or a source failure. The required development build and real browser demo passed after ignoring install scripts. CI follows that explicit route and does not claim native Jest coverage.

Headless Chromium uses software WebGL for this browser scenario. A stock log reports that `SurfaceWorker` is unavailable and synchronous surface generation is used. See `durable-memory/status.md` for historical versus fork-local verification.
