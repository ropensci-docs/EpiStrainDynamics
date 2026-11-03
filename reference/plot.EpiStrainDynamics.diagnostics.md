# Plot convergence diagnostics over time

Plots the R-hat or effective sample size of the log-incidence trajectory
(`log_incidence`) against time, with one line per pathogen, so that
convergence problems can be located in time and by pathogen. The
threshold used by
[`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
is shown as a dashed line.

## Usage

``` r
# S3 method for class 'EpiStrainDynamics.diagnostics'
plot(x, metric = c("rhat", "n_eff"), xlab = "Time", ...)
```

## Arguments

- x:

  Output of
  [`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md),
  of class `EpiStrainDynamics.diagnostics`

- metric:

  The diagnostic to plot: `"rhat"` (default) or `"n_eff"`

- xlab:

  Time label for x axis, defaults to "Time"

- ...:

  Not used

## Value

ggplot2 plot output

## Details

R-hat is shown between 1 and 1.2, so values far above the threshold run
off the top of the plot. Effective sample size is shown on a log scale,
covering the range of the values and the threshold, so that values near
the threshold stay readable even when others are in the thousands.

Only the trajectory is plotted. Other parameters, including the spline
coefficients of p-spline models, are listed in the `parameters` table of
the
[`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
output.

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
plot(diag)
plot(diag, metric = "n_eff")
}
```
