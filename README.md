# Artificial Anasazi: baseline + crowding

`AA3-Crowding.nlogox` adds scarcity-driven plot sharing to the baseline version of the Artificial Anasazi model (see the [`baseline`](../../tree/baseline) branch). With `crowding-limit` = 1 it reproduces the baseline.

## The baseline

The baseline is the NetLogo Models Library version (Stonedahl & Wilensky 2010) converted to NetLogo 7.0.4, with small fixes that make the model work as intended: the arable-uplands zone name (`uplands-fix?`), the setup base yield (`setup-yield-fix?`), the initial household count, one-tick-lag capacity reporting, and seed control with observation-only instruments. The fixes change results very little. Details are in the `baseline` README and in code comments marked `[BASELINE FIX]` and `[BASELINE]`.

## What this version adds

In the published model each plot belongs to one household (Janssen 2009, section 2.14). Here, when no empty plot is available anywhere, a relocating or newly formed household may join an occupied plot, up to `crowding-limit` households, if its share of the plot's output still meets the 800 kg nutrition need. Households therefore accept crowding only as land runs out. Code comments mark these changes `[CROWDING]`.

A plot worked by n households produces base-yield × n^alpha, so each household's share is base-yield × n^(alpha − 1). A plot can admit n households only if

    base-yield >= need × n^(1 − alpha)

| Control | Default | Meaning |
|---|---|---|
| `crowding-limit` | 1 | Maximum households per plot (slider 1 to 4). 1 is the published one-household-per-plot rule. |
| `intensification-return` | 0.5 | alpha, the exponent on labor (slider 0 to 1). alpha = 1 means no diminishing returns, and alpha < 1 means each added household yields less. |

Occupancy of a plot becomes a count instead of a 0/1 flag. A household releases its plot only if it actually holds one, which keeps the count correct when households die or relocate. Settlement placement still excludes farmed land, as in the published model.

<!-- MEDIA 1 (screenshot): interface with crowding-limit > 1 and map-view = occupancy, showing shared plots. -->
<!-- MEDIA 2 (GIF): a run with crowding-limit = 2 or 3. Add a monitor for mean-crowding first (the interface has none by default). -->

## Running it

1. Install NetLogo 7.0.4 from https://ccl.northwestern.edu/netlogo/.
2. Keep the `data/` folder next to `AA3-Crowding.nlogox`.
3. Open the model, press SETUP, then GO. Set `run-seed` first to repeat a run.

The baseline controls (`harvest-adjustment` 0.54, `harvest-variance` 0.4, `death-age` 38, `fertility-ends-age` 34, `fertility` 0.155, `uplands-fix?`, `setup-yield-fix?`) and their sources in Janssen (2009) are described in the `baseline` README.

## Reporters added for crowding

* `plot-capacity` (patch reporter): households a plot can feed, from the admission condition above.
* `potential-crowded`: crowding-adjusted carrying capacity. Equal to `potential` when `crowding-limit` = 1. `capc-series` holds it per year.
* `mean-crowding`, `max-crowding`: mean and maximum households per occupied plot. 1.0 means no crowding occurred.
* `peak1-crowded`, `peak2-crowded`, `crowded-peak-ratio`: the peak-window measures of Janssen (2009, section 4.13), applied to `capc-series`.
* `plot-share` (reporter with a plot argument): a household's share of that plot's output.

All baseline reporters (`fit-correlation`, `fit-l2`, `extinction-year`, the peak and ratio reporters) are also present.

## BehaviorSpace experiments

This branch includes none. The crowding experiments are on the [`combined`](../../tree/combined) branch, where they can be run with `soil-on?` OFF.

<!-- FINDINGS: hidden until the experiments are rerun. Verify against the analysis notebook, link the notebook or report, then uncomment.

## Findings from the accompanying analysis


* An earlier apparent improvement of 2.7 standard deviations came from excluding an extinct run and using the spread of seeds 1 to 10. On holdout seeds the best cell (`crowding-limit` 2, `intensification-return` 0.9, no soil depletion) gave a correlation of 0.835 against 0.844 for the baseline retuned to `harvest-adjustment` 0.56.
* The peak ratio stays near 1 for `intensification-return` below 1.
-->
<!-- MEDIA 3 (plot): potential vs potential-crowded over time for crowding-limit 1, 2 and 4. -->
<!-- MEDIA 4 (plot): heatmap of fit-correlation over crowding-limit x intensification-return (E3-crowding-sweep, run from the combined branch with soil-on? OFF). -->
<!-- MEDIA 5 (plot): holdout-seed comparison of the best crowding cell and the retuned baseline. -->

## More

Project page with all versions: https://well-noted.github.io/Artificial-Anasazi/

## License and credit

Original model copyright 2010 Uri Wilensky, licensed under Creative Commons Attribution-NonCommercial-ShareAlike 3.0. Modifications copyright 2026 Thomas E. Tuoti ([@well-noted](https://github.com/well-noted)), under the same license. The model also contains BSD-licensed code from the original Ascape implementation. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). The overview, citation and reference list are on the [`main`](../../tree/main) branch.
