# Testfoss status

## Exact pins and commands

- Source: `c26e390544b6388f86e50387cd4565759b4da0df` (`c26e390`, 2.5.5), branch `testfoss-demo`.
- Node: 24 for fork commands/Actions (24.15+ for lmscript); the lab used Node 22. Playwright: 1.62.1.
- lmwebtest: `2920befd355eacc654697820d690db074e94dce3`; lmscript: `@lmctl-ai/lmctl` 0.2.463.
- Build: `npm ci --ignore-scripts`, then `npm run build:dev`.
- Browser: serve root on `127.0.0.1:8112`; open `/testfoss/module.html`; run `lmscript testfoss/browser.lms` with local lmwebtest on PATH.

## Recorded lab evidence

At the same source pin, the isolated molecular lab built the development UMD/ES-module bundles after `npm ci --ignore-scripts`. The complete modified module example displayed local 4HHB with 172 selected HEM atoms in sticks/spheres and a VDW surface. Surface opacity followed 0.35, 0, 0.35 through hide/show. The local multi-PDB model reported two frames; animation play and stop worked, and Next frame displayed Frame 2 / 2. Both fixture requests returned 200, with zero browser errors/warnings. These are historical lab results, not a completed fork Actions claim.

Ordinary `npm ci` failed on that host because canvas 2.11.2 lacked a Node 22 prebuilt binary and Cairo/Pango development metadata was absent. Upstream Jest then failed environment setup for all 19 suites and executed zero tests. Do not describe this as a test-suite pass. The browser demo is the verified route.

## Fork verification

Shell/JSON/YAML and inline module-JavaScript syntax checks passed, as did real lmwebtest schema discovery plus `lmscript --check browser.lms`. The copied HTML is byte-identical to the lab source and upstream `examples/module.html` remains pristine. The new scenario passed 15/15 checks through `lmwebtest playwright` against the existing same-pin lab build, with a loopback mapping for `/testfoss/module.html` and installed Chrome 157 selected by `TESTFOSS_CHROME_EXECUTABLE`. It verified 172 HEM atoms, surface opacity 0.35 to 0 to 0.35, two-frame play/stop and Next frame, local BinaryCIF/PDB requests, and zero page/console errors.

GitHub Actions run 42645110796 subsequently passed the development build and the corrected lmwebtest HEM, surface, and animation scenario. Generated `build/`, `node_modules/`, and `testfoss/artifacts/` content is excluded from commits.
