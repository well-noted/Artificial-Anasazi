# Artificial Anasazi: baseline fixes with soil and crowding variants

Modified versions of the Artificial Anasazi agent-based model of the Long House Valley (Arizona, 800 to 1350 CE), for NetLogo 7.0.4. The model is the NetLogo Models Library version (Stonedahl & Wilensky 2010), which adapts Marco Janssen's NetLogo replication (Janssen 2009) of the original Ascape model (Dean et al. 2000; Axtell et al. 2002).

This repository has one branch per version. Each branch contains a single model file, the `data/` folder the model reads, and its own README.

| Branch | Model file | What it is |
|---|---|---|
| [`baseline`](../../tree/baseline) | `AA1-Baseline.nlogox` | The Models Library model plus small fixes that make it work as intended. The starting point for the other branches. |
| [`soil`](../../tree/soil) | `AA2-Soil.nlogox` | Baseline plus dynamic soil quality (depletion and recovery). |
| [`crowding`](../../tree/crowding) | `AA3-Crowding.nlogox` | Baseline plus scarcity-driven plot sharing with diminishing returns. |
| [`combined`](../../tree/combined) | `AA4-Combined.nlogox` | Baseline, soil and crowding, each behind its own switch. |

Project page: https://well-noted.github.io/Artificial-Anasazi/

To get one version:

```
git clone --branch soil https://github.com/well-noted/Artificial-Anasazi.git
```

The `soil`, `crowding` and `combined` branches start from `baseline`, so comparing them on GitHub (for example `baseline...soil`) shows exactly what each modification changes.

<!-- MEDIA 1 (overview image or GIF): the model interface mid-run, with the zones map, the population plot (simulated vs. archaeological households) and the available-sites plot visible. -->

## The baseline

All versions are built from one baseline. The fixes change results very little. They give modifications a reference point that does what the model's documentation says it does.

| Baseline change | Control | What it does |
|---|---|---|
| Uplands zone name | `uplands-fix?` (default ON) | The map loader names the arable-uplands zone "Uplands", but `calculate-yield` tested for "Upland", so those patches never received a PDSI or hydro value. OFF reproduces the published behavior. |
| Setup base yield | `setup-yield-fix?` (default OFF) | The published setup computes base yield without `harvest-adjustment`. ON applies it at setup. |
| Initial household count | none | `total-households` was never assigned in setup, so the first recorded value was 0. |
| Carrying capacity reporting | none | The published `potential` lags one tick. The baseline re-measures it each tick and keeps the published value as `potential-lagged`. |
| Seed control and instruments | `run-seed` | Reproducible runs, per-year series, extinction year, fit measures against the archaeological series, and peak-window reporters (Janssen 2009, section 4.13). Observation only. |

In the code, comments marked `[BASELINE FIX]` and `[BASELINE]` identify these changes, and `[SOIL]` and `[CROWDING]` identify the other two modifications.

## License and credit

The original model is copyright 2010 Uri Wilensky and licensed under Creative Commons Attribution-NonCommercial-ShareAlike 3.0. These modifications are copyright 2026 Thomas E. Tuoti ([@well-noted](https://github.com/well-noted)) and are released under the same license, as that license requires. The model also contains code under a BSD license from the original Ascape implementation. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).

The NonCommercial term applies to this repository. For commercial use of the Models Library model, contact netlogo-commercial-admin@ccl.northwestern.edu.

## How to cite

If you use these versions, please cite the original model and NetLogo, and this repository:

* Stonedahl, F. and Wilensky, U. (2010). NetLogo Artificial Anasazi model. http://ccl.northwestern.edu/netlogo/models/ArtificialAnasazi. Center for Connected Learning and Computer-Based Modeling, Northwestern University, Evanston, IL.
* Wilensky, U. (1999). NetLogo. http://ccl.northwestern.edu/netlogo/. Center for Connected Learning and Computer-Based Modeling, Northwestern University, Evanston, IL.
* Tuoti, T. E. (2026). Artificial Anasazi: baseline fixes with soil and crowding variants. https://github.com/well-noted/Artificial-Anasazi

## References

* Axtell, R. L., Epstein, J. M., Dean, J. S., Gumerman, G. J., Swedlund, A. C., Harburger, J., Chakravarty, S., Hammond, R., Parker, J., and Parker, M. (2002). Population growth and collapse in a multi-agent model of the Kayenta Anasazi in Long House Valley. *Proceedings of the National Academy of Sciences* 99(3): 7275-7279.
* Dean, J. S., Gumerman, G. J., Epstein, J. M., Axtell, R. L., Swedlund, A. C., Parker, M. T., and McCarroll, S. (2000). Understanding Anasazi culture change through agent-based modeling. In *Dynamics in Human and Primate Societies*, Kohler and Gumerman (eds.), Oxford University Press.
* Janssen, M. A. (2009). Understanding Artificial Anasazi. *Journal of Artificial Societies and Social Simulation* 12(4): 13. https://www.jasss.org/12/4/13.html
* Janssen, M. A. (2010). Population aggregation in ancient arid environments. *Ecology and Society* 15(2): 19.
