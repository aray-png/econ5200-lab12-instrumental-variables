# Instrumental Variables: Weak Instruments, Compliers and the Exclusion Restriction

## Objective

I tested when an instrumental-variables estimate can be trusted, by simulating weak and slightly invalid instruments where I knew the true effect, and by replicating Card's (1995) estimate of the return to schooling.

## Methodology

- Simulated 2,000 people where the true effect of the treatment on the outcome was 1.0 and an unobserved confounder pushed OLS upward, then varied the strength of the instrument from very weak to strong.
- Repeated the simulation 500 times at each strength and compared the median OLS and 2SLS estimates, and how often a significant 2SLS estimate was far from the truth.
- Replicated Card's (1995) study, which uses growing up near a four-year college as an instrument for years of schooling, with controls for experience, race, living in the South and living in a metropolitan area.
- Added a small direct effect of the instrument on the outcome to the simulation, breaking the exclusion restriction on purpose, and measured how far it moved the 2SLS estimate.
- Ran the same kind of sensitivity check on Card's data, assuming growing up near a college raised wages directly by 0 to 3 percent.
- Wrote a two-stage least squares function from two OLS regressions and checked it against the `linearmodels` package.
- Used an `iv_tools.py` module that reports every 2SLS estimate together with its first-stage F.

## Key Findings

- **A weak instrument gives the confounded answer, not a noisy one.** At a median first-stage F of 0.5, the median 2SLS estimate was 2.038, almost the same as the median OLS estimate of 1.998, against a true effect of 1.0. Of the draws where 2SLS was significant, 98.1% were off by more than 0.5. A significant second stage did not mean a correct one.
- **The first stage was the only warning sign.** With one instrument, the median 2SLS estimate stayed close to the truth until the instrument became very weak (0.971 at a median F of 22.1), then slid toward OLS. The first-stage F showed the problem when the estimate alone did not.
- **Card's estimate is larger than OLS.** The first-stage F was 16.7, the OLS return to a year of schooling was 0.0740, and the 2SLS return was 0.1323. The IV estimate applies to men whose schooling changed because a college was nearby, not to everyone.
- **A small exclusion violation can erase Card's result.** In the simulation, the bias from a direct effect of the instrument equalled that effect divided by the first-stage strength. In Card's data, a direct effect of just 0.02 (growing up near a college raising wages by about 2% for some reason other than schooling) pulled the 2SLS estimate down to 0.0730, below OLS.
- **My two-stage function matched the package.** It reproduced the `linearmodels` estimate of 0.1323 and F of 16.7 exactly. With only experience and its square as controls, it gave a 2SLS estimate of 0.2587 and a first-stage F of 58.0.
