# Diagnose model convergence and fit

Checks MCMC convergence diagnostics —
[R-hat](https://mc-stan.org/docs/reference-manual/analysis.html) and
[effective sample
size](https://mc-stan.org/docs/reference-manual/analysis.html) — against
user-specified thresholds, and reports any parameters that fail either
check. A per-parameter table is also returned, labelling each parameter
with the pathogen and time point it refers to, so that convergence
problems can be located in time and by pathogen.

## Usage

``` r
diagnose_model(fitted_model, rhat_threshold = 1.1, eff_sample_threshold = 100)
```

## Arguments

- fitted_model:

  A fitted model object of class `EpiStrainDynamics.fit`, as returned by
  [`fit_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/fit_model.md).

- rhat_threshold:

  R-hat threshold for convergence (default 1.1). Values above this
  threshold indicate chains have not mixed well.

- eff_sample_threshold:

  Effective sample size threshold (default 100). Values below this
  threshold indicate the posterior samples for a parameter are too
  autocorrelated to reliably estimate its distribution.

## Value

An object of class `EpiStrainDynamics.diagnostics` (a list), returned
invisibly, containing:

- convergence:

  Logical; `TRUE` if no parameter fails either threshold

- rhat_issues:

  Character vector of parameter names with R-hat above `rhat_threshold`

- eff_sample_issues:

  Character vector of parameter names with effective sample size below
  `eff_sample_threshold`

- max_rhat:

  The largest R-hat value across all parameters

- min_neff:

  The smallest effective sample size across all parameters

- summary:

  The full `rstan::summary()` table the above are derived from

- parameters:

  A data frame with one row per parameter and columns `parameter` (full
  Stan name), `parameter_group` (Stan name without indices), `pathogen`,
  `time_idx` (position on the observation-index scale, where 1 is the
  first observation; fractional for spline coefficients), `time` (on the
  time scale of the input data), `rhat`, `n_eff`, and the logical flags
  `rhat_issue` and `eff_sample_issue`. See Details.

- rhat_threshold, eff_sample_threshold:

  The thresholds the checks used

## Details

The `parameters` table identifies each parameter by the exact Stan name
before its brackets (`parameter_group`), and converts the bracketed
indices into a pathogen name and a time:

- `log_incidence` is the time-varying log-incidence trajectory. Each
  element is matched to the pathogen and the observation time it belongs
  to.

- `b_spline_coef`, in penalised spline models, holds the spline
  coefficients. A coefficient does not belong to a single time point, so
  it is placed at its Greville abscissa (the mean of the interior knots
  its basis function spans) and converted to the time scale of the data.
  Coefficients near the edges of the spline can fall before the first or
  after the last observation. Some of these have no weight on any
  observation, so only the smoothing prior informs them; a flag on one
  of them does not mean the fitted trajectory (`log_incidence`) has
  failed to converge.

- `noisy_incidence` is the pathogen-specific incidence with noise,
  present when the model includes noise in individual pathogens. It is
  matched to pathogen and time in the same way as the trajectory.

- `tau` is matched to a pathogen when each pathogen has its own
  smoothing parameter (`"independent"` smoothing, or a single-pathogen
  model). With `"shared"` smoothing one `tau` applies to all pathogens,
  so `pathogen` is `NA`. With `"correlated"` smoothing there is no
  `tau`; the covariance matrix `Sigma` is used instead.

- All other parameters (e.g. `phi`, `eta`, `Sigma`,
  `day_of_week_simplex`, `lp__`) have `NA` for `pathogen` and `time`.

## Examples

``` r
if (FALSE) { # interactive()
mod <- construct_model(
  pathogen_structure = single(
    data = sarscov2,
    case_timeseries = "cases",
    time = "date"
  ),
  method = random_walk()
)
fit <- fit_model(mod)
diag <- diagnose_model(fit)

# Parameters that failed either check, with pathogen and time attached
diag$parameters[diag$parameters$rhat_issue | diag$parameters$eff_sample_issue, ]
}
```
