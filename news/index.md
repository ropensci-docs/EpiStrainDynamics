# Changelog

## EpiStrainDynamics 0.2.0

### Breaking changes

- [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)’s
  `unsubtyped_timeseries` argument is now `parent_timeseries`, since it
  takes the total count for the parent pathogen, covering both subtyped
  and unsubtyped cases; the previous name suggested it took only the
  unsubtyped cases
  ([\#80](https://github.com/ropensci/EpiStrainDynamics/issues/80)). Any
  code calling
  [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)
  with `unsubtyped_timeseries` will need to use `parent_timeseries`.
- The Stan model parameters have descriptive names
  ([\#82](https://github.com/ropensci/EpiStrainDynamics/issues/82)). `a`
  is now `log_incidence` in random walk models and `b_spline_coef` in
  p-spline models, where the trajectory `a_new` is now `log_incidence`;
  `c` and `c_new` are now `noisy_incidence`. Previously `a` meant the
  trajectory in one method and the spline coefficients in the other; now
  `log_incidence` is the latent log-incidence trend in every model. Code
  that reads the `stanfit` object in `fit$fit` directly (e.g. with
  `bayesplot` or `rstan::summary()`) will need to use the new names;
  [`incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/incidence.md),
  [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md),
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md)
  and
  [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  are unaffected.
- The `prop` column in `$measure` is now `prob_above`, since it is the
  posterior probability that the quantity is above a threshold, not a
  proportion
  ([\#79](https://github.com/ropensci/EpiStrainDynamics/issues/79)).
  [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md)
  and
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md)
  return it for thresholds of 0 and 1, giving the probability that the
  epidemic is growing; their documentation now says so.
  [`incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/incidence.md)
  and
  [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  no longer return it, as the probability that incidence or a proportion
  is above 0 is always 1.

### New features

- [`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
  now returns a `parameters` table giving each parameter’s R-hat and
  effective sample size, labelled with the pathogen and time it refers
  to, and a
  [`plot()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  method shows R-hat or effective sample size of the log-incidence
  trajectory over time by pathogen
  ([\#85](https://github.com/ropensci/EpiStrainDynamics/issues/85)).

### Bug fixes

- Fixed the Stan models failing to compile with newer versions of
  stanc3, which reject the old array syntax (e.g. `real[] t`,
  `int Y[n]`). This broke the R-universe build
  ([@jeroen](https://github.com/jeroen),
  [\#77](https://github.com/ropensci/EpiStrainDynamics/issues/77),
  [\#78](https://github.com/ropensci/EpiStrainDynamics/issues/78)).

### Minor improvements

- [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)
  now errors if `parent_timeseries` is smaller than the sum of
  `subtyped_timeseries` at any time point, since the subtyped cases are
  a subset of the parent total
  ([\#80](https://github.com/ropensci/EpiStrainDynamics/issues/80)).
  Previously such data was accepted and would silently underestimate the
  parent pathogen.

### Documentation

- Further corrected the `influenza` dataset documentation: `inf_A` is
  all influenza A positives, including those subtyped; counts are weekly
  rather than daily; the date range matches the data included (January
  2012 to February 2020); the `inf_*` columns are described as positive
  laboratory specimens; and `other` is described with how it is
  calculated
  ([\#80](https://github.com/ropensci/EpiStrainDynamics/issues/80)).
- The
  [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)
  documentation, vignette and README now describe `parent_timeseries` as
  the total for the parent pathogen
  ([\#80](https://github.com/ropensci/EpiStrainDynamics/issues/80)).

## EpiStrainDynamics 0.1.0 (2026-08-24)

### Breaking changes

- [`construct_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/construct_model.md)’s
  first two arguments are now `pathogen_structure` then `method`
  (previously `method` then `pathogen_structure`), to match the logical
  order in which a model is built: prepare the pathogen structure, then
  choose how to model it
  ([\#70](https://github.com/ropensci/EpiStrainDynamics/issues/70)). Any
  code calling
  [`construct_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/construct_model.md)
  positionally
  (e.g. `construct_model(my_method, my_pathogen_structure)`) will need
  to swap the order of these two arguments; calls using named arguments
  are unaffected.
- [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)’s
  `influenzaA_unsubtyped_timeseries` and
  `influenzaA_subtyped_timeseries` arguments are now
  `unsubtyped_timeseries` and `subtyped_timeseries`. The `influenzaA_`
  prefix implied the structure only applied to influenza A subtypes,
  when it applies to any pathogen with a combined, unsubtyped timeseries
  alongside partial subtype data — influenza A is just the most common
  example
  ([\#73](https://github.com/ropensci/EpiStrainDynamics/issues/73)). Any
  code calling
  [`subtyped()`](https://docs.ropensci.org/EpiStrainDynamics/reference/subtyped.md)
  with these named arguments will need to update the argument names.

### Bug fixes

- Fixed compilation failing on Windows ARM64 (`R CMD INSTALL` erroring
  with “unsupported option ‘-msse2’ for target ‘aarch64-w64-mingw32’”).
  The `-mfpmath=sse -msse2 -mstackrealign` compiler flags added to work
  around an unrelated Windows x86_64 CI memory issue (see below) are
  x86-specific and don’t exist on ARM64; they’re now only applied when
  compiling for x86_64.
- Fixed
  [`plot.incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  hardcoding “Modelled influenza cases” as its y-axis label regardless
  of the pathogen(s) actually being modelled; it now reads the
  disease-agnostic “Modelled cases”.
- Fixed the negative-binomial likelihood in the random-walk
  single-pathogen model when `dow_effect = TRUE`
  ([\#42](https://github.com/ropensci/EpiStrainDynamics/issues/42)).
- Corrected the
  [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  /
  [`plot()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  example in the vignette and fixed the underlying figure path so plots
  render correctly
  ([\#45](https://github.com/ropensci/EpiStrainDynamics/issues/45)).
- Fixed the
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md)
  equation failing to render on the pkgdown site by setting MathJax as
  the math renderer
  ([\#44](https://github.com/ropensci/EpiStrainDynamics/issues/44)).
- Fixed
  [`construct_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/construct_model.md)
  and `compute_multi_pathogen()` erroring on R versions before 4.4.0
  with “could not find function ‘%\|\|%’”, by explicitly importing
  `%||%` from `rlang` instead of relying on base R’s version, which only
  exists from R 4.4.0 (related to
  [\#43](https://github.com/ropensci/EpiStrainDynamics/issues/43)).
- Fixed intermittent R CMD check failures on Windows CI. The reported
  error (“Build process failed”) was misleading and had two distinct,
  unrelated causes hiding behind it, both only visible once compiler
  warning noise was suppressed (see below):
  - `rstan`’s own precompiled Windows binary DLL failed to load
    (“LoadLibrary failure”), most likely an ABI mismatch between the
    RSPM binary and the runner’s Rtools version. Fixed by reinstalling
    `rstan` from source on the Windows CI job specifically.
  - With that resolved, the package’s own compiled DLL then failed to
    load the same way, because `StanHeaders:::LdFlags()` unconditionally
    links against a *dynamic* `tbb`/`tbbmalloc`, which `RcppParallel`
    (from version 6.0.0) no longer reliably provides on Windows (having
    moved to static TBB linking), and `-Wl,-rpath` has no effect on
    Windows regardless. Fixed by dropping `StanHeaders:::LdFlags()` from
    `src/Makevars.win`’s `PKG_LIBS` and relying solely on
    [`RcppParallel::RcppParallelLibs()`](https://rdrr.io/pkg/RcppParallel/man/flags.html),
    matching what `rstan`’s own Windows build does. Also lowered
    optimisation from `-O2` to `-O1`, suppressed compiler warnings, and
    enlarged the Windows runner’s pagefile in `src/Makevars.win`/CI
    config; these weren’t the actual fix for either issue above, but
    remain in place as they reduce build time/noise and are otherwise
    harmless.

### Minor improvements

- [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  now returns the resolved pathogen names used as its numerator and
  denominator (`$numerator_combination` / `$denominator_combination`),
  and
  [`plot.proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  adds a subtitle reporting the denominator when it isn’t the default
  (all pathogens), since the denominator otherwise has no representation
  anywhere on the plot
  ([\#69](https://github.com/ropensci/EpiStrainDynamics/issues/69)).
- Capped the viridis palette used by
  [`plot()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  methods at `end = 0.9` so the palest yellow no longer makes lines hard
  to distinguish
  ([\#71](https://github.com/ropensci/EpiStrainDynamics/issues/71)).
- Plot functions now call `ggplot2` functions explicitly rather than
  importing the whole package
  ([\#35](https://github.com/ropensci/EpiStrainDynamics/issues/35)).
- Standardised quotation-mark style
  ([\#37](https://github.com/ropensci/EpiStrainDynamics/issues/37)).
- Reordered function arguments so those without defaults precede those
  with defaults
  ([\#38](https://github.com/ropensci/EpiStrainDynamics/issues/38)).
- Enabled parallel testing with `Config/testthat/parallel: true`
  ([\#39](https://github.com/ropensci/EpiStrainDynamics/issues/39)).
- Added tests for MCMC validation.
- Added a [`print()`](https://rdrr.io/r/base/print.html) method for
  `EpiStrainDynamics.model` objects
  ([\#47](https://github.com/ropensci/EpiStrainDynamics/issues/47)).
- Standardised error handling on
  [`cli::cli_abort()`](https://cli.r-lib.org/reference/cli_abort.html)
  ([\#49](https://github.com/ropensci/EpiStrainDynamics/issues/49)).
- [`plot()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  is no longer redefined as a generic
  ([\#48](https://github.com/ropensci/EpiStrainDynamics/issues/48)), and
  its S3 methods’ first argument was renamed from `df` to `x` to match
  the base generic’s signature, resolving an R CMD check S3
  generic/method consistency warning.
- Increased the minimum R version to 4.1.0, matching actual base pipe
  usage, and added it to the R-CMD-check.yaml test matrix
  ([\#43](https://github.com/ropensci/EpiStrainDynamics/issues/43)).
- Fixed all lintr-identified issues
  ([\#50](https://github.com/ropensci/EpiStrainDynamics/issues/50)):
  styling (line length, indentation, spacing) via `styler`, dropped
  explicit [`return()`](https://rdrr.io/r/base/function.html) on final
  statements, converted [`sapply()`](https://rdrr.io/r/base/lapply.html)
  to [`vapply()`](https://rdrr.io/r/base/lapply.html) for type-safe
  extraction, and removed genuine dead code flagged by unused-variable
  checks.

### Documentation

- Replaced `\dontrun{}` in examples with `@examplesIf interactive()` for
  slow examples and guards such as
  `@examplesIf rlang::is_installed("xts")` where a package is used only
  in an example
  ([\#36](https://github.com/ropensci/EpiStrainDynamics/issues/36)).
- Expanded documentation for
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md),
  including its `gi_dist` argument
  ([\#44](https://github.com/ropensci/EpiStrainDynamics/issues/44)).
- Expanded documentation for
  [`smoothing_structure()`](https://docs.ropensci.org/EpiStrainDynamics/reference/smoothing_structure.md)
  and
  [`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
  ([\#53](https://github.com/ropensci/EpiStrainDynamics/issues/53),
  [\#54](https://github.com/ropensci/EpiStrainDynamics/issues/54)).
- Expanded documentation for
  [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md),
  [`incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/incidence.md),
  and
  [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md)
  to match the level of detail in
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md),
  including symbol breakdowns for their adjustment formulas and, for
  [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md),
  its relationship to
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md)
  ([\#66](https://github.com/ropensci/EpiStrainDynamics/issues/66)).
- Added roxygen `[]` cross-links for referenced functions throughout the
  documentation
  ([\#55](https://github.com/ropensci/EpiStrainDynamics/issues/55)).
- Committed `precompile.R` for the vignette and included revision
  instructions in CONTRIBUTING.md
  ([\#46](https://github.com/ropensci/EpiStrainDynamics/issues/46)).
- Clarified contribution and maintenance guidelines, added installation-
  from-source instructions, and added citations to the README
  ([\#51](https://github.com/ropensci/EpiStrainDynamics/issues/51),
  [\#57](https://github.com/ropensci/EpiStrainDynamics/issues/57),
  [\#58](https://github.com/ropensci/EpiStrainDynamics/issues/58),
  [\#59](https://github.com/ropensci/EpiStrainDynamics/issues/59),
  [\#60](https://github.com/ropensci/EpiStrainDynamics/issues/60)).
- Corrected documentation of both `sarscov2` and `influenza` datasets
  ([\#56](https://github.com/ropensci/EpiStrainDynamics/issues/56)).
- Added explanations of “pathogen noise” and the day-of-week effect
  ([\#61](https://github.com/ropensci/EpiStrainDynamics/issues/61)), a
  link to the relevant timeseries class
  ([\#62](https://github.com/ropensci/EpiStrainDynamics/issues/62)), and
  a COVID-19 example
  ([\#63](https://github.com/ropensci/EpiStrainDynamics/issues/63)), to
  the vignette.
- Clarified the G1.6 srr standard statement to note that, although
  performance/scaling is evaluated in the vignettes, this does not
  constitute an explicit comparison against alternative implementations
  ([\#52](https://github.com/ropensci/EpiStrainDynamics/issues/52)).

## EpiStrainDynamics 0.0.1 (2026-03-27)

Initial version submitted to rOpenSci for peer review
(ropensci/software-review#763).

### New features

- Random-walk and p-spline models for inferring temporal trends of
  single and multiple (including subtyped) pathogens, fitted via Stan.
- Model construction and fitting via
  [`construct_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/construct_model.md)
  and
  [`fit_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/fit_model.md),
  with
  [`smoothing_structure()`](https://docs.ropensci.org/EpiStrainDynamics/reference/smoothing_structure.md)
  and
  [`dispersion_structure()`](https://docs.ropensci.org/EpiStrainDynamics/reference/dispersion_structure.md)
  helpers.
- Post-processing generics
  [`incidence()`](https://docs.ropensci.org/EpiStrainDynamics/reference/incidence.md),
  [`growth_rate()`](https://docs.ropensci.org/EpiStrainDynamics/reference/growth_rate.md),
  [`Rt()`](https://docs.ropensci.org/EpiStrainDynamics/reference/Rt.md),
  and
  [`proportion()`](https://docs.ropensci.org/EpiStrainDynamics/reference/proportion.md),
  each with a
  [`plot()`](https://docs.ropensci.org/EpiStrainDynamics/reference/plot.md)
  method.
- [`diagnose_model()`](https://docs.ropensci.org/EpiStrainDynamics/reference/diagnose_model.md)
  for MCMC convergence diagnostics.
- Bundled `influenza` and `sarscov2` example datasets.
- “Using EpiStrainDynamics” and “Algorithmic Scaling” vignettes.
