# Artificial Anasazi: baseline + dynamic soil

`AA2-Soil.nlogox` adds dynamic soil quality to the baseline version of the Artificial Anasazi model (see the [`baseline`](../../tree/baseline) branch). With soil dynamics off, it reproduces the baseline.

## The baseline

The baseline is the NetLogo Models Library version (Stonedahl & Wilensky 2010) converted to NetLogo 7.0.4, with small fixes that make the model work as intended: the arable-uplands zone name (`uplands-fix?`), the setup base yield (`setup-yield-fix?`), the initial household count, one-tick-lag capacity reporting, and seed control with observation-only instruments. The fixes change results very little. Details are in the `baseline` README and in code comments marked `[BASELINE FIX]` and `[BASELINE]`.

## What this version adds

Soil quality becomes a state variable. Each cultivated plot loses `depletion-rate` of its relative quality per year, and every plot regrows toward its setup value following Janssen (2010, eq. 4). Code comments mark these changes `[SOIL]`.

| Control | Default | Meaning |
|---|---|---|
| `depletion-rate` | 0 | Loss of relative soil quality per year of cultivation (slider 0 to 0.02). |
| `recovery-rate` | 0 | Maximum annual regrowth, as a fraction of the setup quality. |
| `degradation-factor` | 1 | Janssen's lambda. 0 gives logistic regrowth, so soil always recovers. Higher values suppress regrowth at low quality, which mimics erosion. |
| `soil-heterogeneity` | 0.4 | Standard deviation of the setup quality draw. The published model uses `harvest-variance` for this. |

`recovery-rate` is rescaled internally so that it means the maximum annual regrowth at every `degradation-factor`. Without that, raising `degradation-factor` would also slow recovery overall.

With `depletion-rate` = `recovery-rate` = 0 and `soil-heterogeneity` = `harvest-variance`, results match the baseline exactly. The `map-view` chooser gains a "soil quality" view.

<!-- MEDIA 1 (screenshot): interface with map-view = soil quality late in a run with depletion-rate > 0, showing depleted cells. -->
<!-- MEDIA 2 (GIF): a run with depletion-rate > 0 and recovery-rate > 0, map-view = soil quality. -->

## Running it

1. Install NetLogo 7.0.4 from https://ccl.northwestern.edu/netlogo/.
2. Keep the `data/` folder next to `AA2-Soil.nlogox`.
3. Open the model, press SETUP, then GO. Set `run-seed` first to repeat a run.

The baseline controls (`harvest-adjustment` 0.54, `harvest-variance` 0.4, `death-age` 38, `fertility-ends-age` 34, `fertility` 0.155, `uplands-fix?`, `setup-yield-fix?`) and their sources in Janssen (2009) are described in the `baseline` README.

## Reporters added for soil

* `mean-farmed-quality`: mean relative soil quality over cells that have been cultivated (1 if none).
* `dead-cells`: cells whose quality has reached zero. Quality zero is absorbing, so these cannot recover at any `degradation-factor`.
* `soil-series`: mean farmed quality per year. `soil-1130` and `soil-1260` read it at those years.

All baseline reporters (`fit-correlation`, `fit-l2`, `extinction-year`, the peak and ratio reporters) are also present.

## BehaviorSpace experiments

`N2-control` (no soil dynamics, 30 seeds), `E2-soil-sweep` and `E2-lowr-sweep`.

<!-- FINDINGS: hidden until the experiments are rerun. Verify against the analysis notebook, link the notebook or report, then uncomment.

## Findings from the accompanying analysis


* No combination of `depletion-rate`, `recovery-rate` and `degradation-factor` in the sweeps fit better than the no-soil control.
* Households die out before soil degrades: mean farmed quality is about 0.96 to 0.99 at extinction.
* Slow depletion lowers second-peak capacity (ratio 0.87 in surviving runs), which is the wrong direction for the two-peak asymmetry described by Janssen (2009, section 4.13).
* 22% of low-recovery runs reach zero households between 1280 and 1350, but every such parameter cell also has early extinctions.
-->
<!-- MEDIA 3 (plot): soil-series (mean farmed quality) over time for several depletion-rate and recovery-rate settings (E2-soil-sweep). -->
<!-- MEDIA 4 (plot): heatmap of fit-correlation over depletion-rate x recovery-rate, with the N2-control level marked. -->
<!-- MEDIA 5 (plot): population against the archaeological series for the no-soil control and one soil setting. -->

## More

Project page with all versions: https://well-noted.github.io/Artificial-Anasazi/

## License and credit

Original model copyright 2010 Uri Wilensky, licensed under Creative Commons Attribution-NonCommercial-ShareAlike 3.0. Modifications copyright 2026 Thomas E. Tuoti ([@well-noted](https://github.com/well-noted)), under the same license. The model also contains BSD-licensed code from the original Ascape implementation. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). The overview, citation and reference list are on the [`main`](../../tree/main) branch.

Soil dynamics follow Janssen, M. A. (2010). Population aggregation in ancient arid environments. *Ecology and Society* 15(2): 19.
