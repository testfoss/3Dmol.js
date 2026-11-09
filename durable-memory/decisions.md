# Testfoss decisions

## Copy the complete demo, not the upstream modification

Use source pin `c26e390544b6388f86e50387cd4565759b4da0df`. Copy the entire locally modified lab `examples/module.html` to new `testfoss/module.html`. Both paths are one directory below root, so the complete file's `../build` and `../tests` references work without rewriting. Upstream `examples/module.html` stays byte-for-byte pristine.

## Keep data local and avoid generated source

Build `3Dmol.es6.js` with the pristine development webpack command. The demo reads existing 4HHB BinaryCIF and multi-model PDB fixtures directly from upstream test data. Generated bundles are CI/local state only.

## lmwebtest owns browser assertions

`testfoss/browser.lms` uses `_lmcode_ cli="lmwebtest playwright"` and `run_lmcode`. It checks HEM atom selection, surface opacity 0.35 to 0 to 0.35, two animation frames, play/stop state, next-frame status, local requests, and page/console errors. Screenshots are disposable supporting evidence.

## Make Cairo an explicit host boundary

Use `npm ci --ignore-scripts` for the browser build because the lab host lacked Cairo/Pango and could not build canvas. Do not hide this limitation or report zero executed Jest tests as success. Native Jest remains a separate task for a host with canvas build prerequisites.
