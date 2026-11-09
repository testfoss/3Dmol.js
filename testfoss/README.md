# 3Dmol.js Testfoss demo

Source pin: `c26e390544b6388f86e50387cd4565759b4da0df` (2.5.5). The demo is additive and uses local molecular data only.

`module.html` is the complete locally modified lab example copied to a new location. Its `../build/3Dmol.es6.js`, `../tests/auto/data/4hhb.bcif.gz`, and `../tests/test_structs/multi.pdb` paths work because `testfoss/` has the same depth as `examples/`. Upstream `examples/module.html` is untouched.

## Development build

From the repository root:

```sh
./testfoss/run.sh package
```

This runs `npm ci --ignore-scripts` and `npm run build:dev`. Install scripts are deliberately skipped because the lab host lacked the Cairo/Pango prerequisites for canvas; see the limitation below.

## HEM, surface, and animation browser check

```sh
(cd testfoss && npm ci && npx playwright install chromium)
./testfoss/run.sh browser
```

Open <http://127.0.0.1:8112/testfoss/module.html>. The runner starts a loopback-only server and invokes `lmwebtest playwright` through `browser.lms`. Set `TESTFOSS_CHROME_EXECUTABLE=/path/to/chrome` to use an installed Chrome instead of Playwright-managed Chromium. Assertions cover:

- 172 local HEM atoms with stick/sphere styling and a visible VDW surface;
- surface opacity 0.35 to 0 to 0.35 through hide/show;
- two local animation frames, play/stop state, and Next frame reaching Frame 2 / 2;
- local BinaryCIF/PDB requests and no page or console errors.

Screenshots are disposable files under `testfoss/artifacts/`.

**Cairo host limitation:** ordinary `npm ci` could not build canvas 2.11.2 on the Node 22 lab host because native Cairo/Pango development files were unavailable. Upstream Jest failed setup in 19 suites and executed zero tests; no native Jest pass is claimed. The development build and real browser demo passed after ignoring install scripts. Synchronous surface generation is expected when `SurfaceWorker` is unavailable.

Syntax/equality checks without rebuilding:

```sh
bash -n testfoss/run.sh
node -e 'JSON.parse(require("fs").readFileSync("testfoss/package.json", "utf8"))'
(cd testfoss && npm ci && npm run check)
```

See `../durable-memory/status.md` for pins and verification provenance.
