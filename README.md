# Artificial Anasazi: baseline

`AA1-Baseline.nlogox` is the NetLogo Models Library version of Artificial Anasazi (Stonedahl & Wilensky 2010) converted to NetLogo 7.0.4, with small fixes that make the model work as intended. The `soil`, `crowding` and `combined` branches of this repository are built from it.

The model simulates household-level farming and settlement in the Long House Valley, Arizona, from 800 to 1350 CE, and compares simulated household numbers with the archaeological record. It follows Janssen's (2009) NetLogo replication of the original Ascape model (Dean et al. 2000; Axtell et al. 2002).

<!-- MEDIA 1 (screenshot): full interface after SETUP, map-view = zones, historic-view? on. -->

## Running it

1. Install NetLogo 7.0.4 from https://ccl.northwestern.edu/netlogo/.
2. Keep the `data/` folder next to `AA1-Baseline.nlogox`. The model reads its map, settlement, water and climate files from `data/`.
3. Open the model, press SETUP, then GO. The run stops after 1350.

Set `run-seed` before SETUP to repeat a run exactly.

<!-- MEDIA 2 (GIF): one run from 800 to 1350 with map-view = occupancy, showing households spreading and the collapse after 1300. -->

## What the baseline changes

| Change | Control | Effect |
|---|---|---|
| Uplands zone name | `uplands-fix?` (default ON) | The map loader names the arable-uplands zone "Uplands", but `calculate-yield` tested for "Upland", so those patches never received a PDSI or hydro value and sat in the middle yield class (547 kg; Janssen 2009, Table 3). OFF reproduces the published behavior. |
| Setup base yield | `setup-yield-fix?` (default OFF) | The published setup computes base yield without `harvest-adjustment` (Janssen 2009, section 2.11 defines it as y × q × Ha). ON applies it at setup. The first tick corrects the value in either case, so only initial placement differs. |
| Initial household count | none | `total-households` was never assigned in setup, so the first recorded value was 0. |
| Carrying capacity reporting | none | The published `potential` is read before the year's harvest is recomputed, so it lags one tick. `potential` is now measured on the current tick (Janssen 2009, section 3.3) and the published value is kept as `potential-lagged`. |
| Seed control and instruments | `run-seed` | Described below. |

Code comments mark these as `[BASELINE FIX]` (corrects behavior) and `[BASELINE]` (seeding, measurement, reporting).

## Controls

| Control | Default | Meaning |
|---|---|---|
| `harvest-adjustment` | 0.54 | Fraction of the harvest kept (Ha in Janssen 2009). 0.54 is the lower end of the range he explored (section 4.4) and the carrying-capacity optimum in his Table 4. His replicated-model optimum is 0.56 (section 4.11). |
| `harvest-variance` | 0.4 | Spatial and annual variability of harvest (Table 4). |
| `death-age` | 38 | Maximum household age (Table 4, column 1). |
| `fertility-ends-age` | 34 | Maximum age at which a household can fission (Table 4, column 1). |
| `fertility` | 0.155 | Annual fission probability, p_f in Janssen's Table 2 (Table 4, column 1). |
| `uplands-fix?`, `setup-yield-fix?` | ON, OFF | See above. |
| `historic-view?` | ON | Show archaeological settlements. |
| `map-view` | zones | zones, water sources, yield or occupancy. |
| `run-seed` | 1 | Random seed, applied right after `clear-all`. |

The published defaults were harvest-adjustment 1.0, harvest-variance 0.1, death-age 30, fertility-ends-age 30 and fission probability 0.125 (Janssen 2009, Table 2). Axtell et al.'s (2002) calibration, as reported in section 3.4, used harvest-variance 0.4 and harvest-adjustment 0.6.

## Reporters

For BehaviorSpace and monitors. None of them draws from the main random number stream.

* `sim-series`, `hist-series`, `cap-series`: simulated households, archaeological households and carrying capacity for each year, stamped with the model's `year` after its end-of-year increment (the first tick simulates 800 and is stamped 801).
* `fit-correlation`: Pearson correlation between `sim-series` and `hist-series`. Not in Janssen (2009).
* `fit-l2`: root mean square difference between the same series, in households. Janssen's L² metric (section 4.6) is the sum of squared differences. This is its root-mean-square form.
* `extinct?`, `extinction-year`: whether and when the population reaches zero.
* `uplands-farmers`: households farming the arable-uplands zone.
* `peak1-capacity`, `peak2-capacity`, `peak1-population`, `peak2-population`, `peak1-historical`, `peak2-historical`: the maximum within each window, 1030 to 1130 and 1180 to 1260 (Janssen 2009, section 4.13).
* `capacity-peak-ratio`, `population-peak-ratio`: second peak divided by first. Janssen reports no ratio. These formalize his statement that capacity is the same at both peaks.

<!-- MEDIA 3 (screenshot): the "available sites vs carrying capacity" plot and the population plot from a single run, showing the two peaks. -->

## BehaviorSpace experiments

Included under Tools > BehaviorSpace: `E1-uplands`, `E1-replicates`, `E1-setup-yield`, `E1-ofat-harvest-adjustment` and `E1-ofat-harvest-variance`.

<!-- FINDINGS: hidden until the experiments are rerun. Verify against the analysis notebook, link the notebook or report, then uncomment.

## Findings from the accompanying analysis


* With `uplands-fix?` ON, arable-uplands plot-years per run rise from 13 to 86. Population is unchanged (paired difference in correlation 0.003 ± 0.014), because the population sits at carrying capacity and the fix adds about 1.5 farmable patches.
* About 2% of baseline runs go extinct early (4 of 200 seeds).
* `harvest-adjustment` 0.56 fits better than 0.54 (correlation +0.019, RMS error −6.3 households) with no new mechanism.
* Carrying capacity is identical at the two peaks for every `harvest-adjustment` and `harvest-variance` value (ratio exactly 1), as Janssen (2009, section 4.13) describes. The archaeological peak ratio with these windows is 1.22 (204/167).
-->
<!-- MEDIA 4 (plot): simulated household trajectories for 30 seeds against the archaeological series (E1-replicates). -->
<!-- MEDIA 5 (plot): uplands-farmers over time with uplands-fix? ON vs OFF (E1-uplands). -->
<!-- MEDIA 6 (plot): fit-correlation and fit-l2 against harvest-adjustment (E1-ofat-harvest-adjustment). -->

## More

Project page with all versions: https://well-noted.github.io/Artificial-Anasazi/

## License and credit

Original model copyright 2010 Uri Wilensky, licensed under Creative Commons Attribution-NonCommercial-ShareAlike 3.0. Modifications copyright 2026 Thomas E. Tuoti ([@well-noted](https://github.com/well-noted)), under the same license. The model also contains BSD-licensed code from the original Ascape implementation. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). The overview, citation and reference list are on the [`main`](../../tree/main) branch.
