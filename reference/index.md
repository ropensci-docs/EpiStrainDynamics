# Package index

## Data

Datasets incorporated in the package for demonstration.

- [`influenza`](https://docs.ropensci.org/EpiStrainDynamics/reference/influenza.md)
  : World Health Organisation Global Influenza Programme for Australia
- [`sarscov2`](https://docs.ropensci.org/EpiStrainDynamics/reference/sarscov2.md)
  : United Kingdom Health Security Agency SARS-CoV-2 case data

## Construct model functions

Functions for constructing model object.

- [`p_spline()`](https://docs.ropensci.org/EpiStrainDynamics/reference/p_spline.md)
  : Specify p_spline method
- [`random_walk()`](https://docs.ropensci.org/EpiStrainDynamics/reference/random_walk.md)
  : Specify random walk method
- [`single()`](https://docs.ropensci.org/EpiStrainDynamics/reference/single.md)
  : Single pathogen structure
- [`multiple()`](https://docs.ropensci.org/EpiStrainDynamics/reference/multiple.md)
  : Multiple pathogen structure
- [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)
  : Subtyped pathogen structure
- [`smoothing_structure()`](https://docs.ropensci.org/EpiStrainDynamics/reference/smoothing_structure.md)
  : Create Smoothing Structure Specification with Priors
- [`dispersion_structure()`](https://docs.ropensci.org/EpiStrainDynamics/reference/dispersion_structure.md)
  : Create Dispersion Structure Specification
- [`construct_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/construct_model.md)
  : Construct model
- [`print(`*`<EpiStrainDynamics.model>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/print.EpiStrainDynamics.model.md)
  : Print method for constructed EpiStrainDynamics models

## Fit stan model

- [`fit_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/fit_model.md)
  : Generic Method for fitting model
- [`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
  : Diagnose model convergence and fit
- [`plot(`*`<EpiStrainDynamics.diagnostics>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.EpiStrainDynamics.diagnostics.md)
  : Plot convergence diagnostics over time

## Post-modelling metrics calculations

Functions to calculate and plot metrics from fitted models.

- [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md)
  : Generic Method for Rt Analysis
- [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md)
  : Generic Method for Growth Rate Analysis
- [`incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/incidence.md)
  : Generic Method for Incidence Analysis
- [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  : Generic Method for proportion Analysis
- [`plot(`*`<incidence>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  [`plot(`*`<growth_rate>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  [`plot(`*`<Rt>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  [`plot(`*`<proportion>`*`)`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  : Plot metrics calculation outputs
