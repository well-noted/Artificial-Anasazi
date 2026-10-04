# Artificial Anasazi: baseline + soil + crowding, each switchable

`AA4-Combined.nlogox` contains both modifications to the baseline Artificial Anasazi model (see the [`baseline`](../../tree/baseline) branch), each behind its own switch.

| `soil-on?` | `crowding-on?` | Model |
|---|---|---|
| OFF | OFF | The baseline |
| ON | OFF | Baseline + dynamic soil (the [`soil`](../../tree/soil) branch) |
| OFF | ON | Baseline + crowding (the [`crowding`](../../tree/crowding) branch) |
| ON | ON | Both mechanisms |

The soil and crowding sliders have no effect while their switch is OFF. In the code, `soil-on?` OFF skips the soil update and makes the setup quality draw use `harvest-variance`, as the baseline does. `crowding-on?` OFF makes `crowd-cap`, the effective crowding limit, return 1.

<!-- MEDIA 1 (screenshot): interface with both switches visible, showing the soil and crowding sliders. -->

## The baseline

The baseline is the NetLogo Models Library version (Stonedahl & Wilensky 2010) converted to NetLogo 7.0.4, with small fixes that make the model work as intended: the arable-uplands zone name (`uplands-fix?`), the setup base yield (`setup-yield-fix?`), the initial household count, one-tick-lag capacity reporting, and seed control with observation-only instruments. The fixes change results very little. Details are in the `baseline` README and in code comments marked `[BASELINE FIX]` and `[BASELINE]`.

## Soil

Each cultivated plot loses `depletion-rate` of its relative quality per year, and every plot regrows toward its setup value following Janssen (2010, eq. 4). Controls: `depletion-rate`, `recovery-rate`, `degradation-factor` (Janssen's lambda: 0 is logistic regrowth, higher values mimic erosion) and `soil-heterogeneity` (standard deviation of the setup quality draw). A "soil quality" view is added to `map-view`. Code comments mark this `[SOIL]`. The `soil` README has the details.

## Crowding

When no empty plot is available anywhere, a relocating or newly formed household may join an occupied plot, up to `crowding-limit` households, if its share, base-yield × n^(alpha − 1), still meets the nutrition need. `intensification-return` is alpha. Code comments mark this `[CROWDING]`. The `crowding` README has the details.

With both mechanisms ON, depletion scales with the number of households working a plot.

<!-- MEDIA 2 (GIF): toggling soil-on? and crowding-on? during a run, with population and available-sites plots visible. -->

## Running it

1. Install NetLogo 7.0.4 from https://ccl.northwestern.edu/netlogo/.
2. Keep the `data/` folder next to `AA4-Combined.nlogox`.
3. Open the model, press SETUP, then GO. Set `run-seed` first to repeat a run.

Both switches default to ON. To compare against the baseline, set both OFF.

The baseline controls (`harvest-adjustment` 0.54, `harvest-variance` 0.4, `death-age` 38, `fertility-ends-age` 34, `fertility` 0.155, `uplands-fix?`, `setup-yield-fix?`) and their sources in Janssen (2009) are described in the `baseline` README.

## Reporters

All reporters from the other branches are present: the baseline instruments (`fit-correlation`, `fit-l2`, `extinction-year`, `sim-series`, `hist-series`, `cap-series`, and the peak and ratio reporters), the soil reporters (`mean-farmed-quality`, `dead-cells`, `soil-series`, `soil-1130`, `soil-1260`) and the crowding reporters (`potential-crowded`, `capc-series`, `mean-crowding`, `max-crowding`, `peak1-crowded`, `peak2-crowded`, `crowded-peak-ratio`).

## BehaviorSpace experiments

`N3-limit1`, `E3-crowding-sweep`, `E3-holdout-controls`, `E3-holdout-cell1`, `E3-holdout-cell2`, `E3-holdout-cell3` and `E3-holdout-retuned`. They leave the two switches at their interface values, so set the switches before running them. To reproduce the crowding results without soil, set `soil-on?` OFF.

<!-- FINDINGS: hidden until the experiments are rerun. Verify against the analysis notebook, link the notebook or report, then uncomment.

## Findings from the accompanying analysis


See the `baseline`, `soil` and `crowding` READMEs. In the accompanying analysis, neither mechanism improved fit over its no-mechanism control.
-->
<!-- MEDIA 3 (plot): population trajectories for the four switch combinations against the archaeological series, same seeds. -->
<!-- MEDIA 4 (plot): fit-correlation and extinction rate for the four switch combinations across 30 seeds. -->

## More

Project page with all versions: https://well-noted.github.io/Artificial-Anasazi/

## License and credit

Original model copyright 2010 Uri Wilensky, licensed under Creative Commons Attribution-NonCommercial-ShareAlike 3.0. Modifications copyright 2026 Thomas E. Tuoti ([@well-noted](https://github.com/well-noted)), under the same license. The model also contains BSD-licensed code from the original Ascape implementation. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). The overview, citation and reference list are on the [`main`](../../tree/main) branch.
