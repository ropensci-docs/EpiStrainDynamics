# World Health Organisation Global Influenza Programme for Australia

Influenza-like illness data were retrieved from the World Health
Organization’s Global Influenza Programme for Australia, and subset to
the weeks starting January 2, 2012 to February 24, 2020, before the
COVID-19 pandemic disrupted influenza transmission and surveillance. The
data described: (1) the weekly number of cases of influenza-like
illness; and (2) the weekly number of laboratory specimens tested, and
of those positive for influenza by type and subtype. The specimen counts
come from laboratory surveillance separate from the influenza-like
illness counts, and are not a subset of them. We grouped the specimens
into: influenza A (all influenza A positives, whether or not subtyped),
of which some were subtyped as influenza A H3N2 or influenza A H1N1
(influenza A H1N1, influenza A H1N1pdm09); influenza B (influenza B
Yamagata, influenza B Victoria, influenza B lineage not determined); and
other (specimens tested that were not positive for influenza).

## Usage

``` r
influenza
```

## Format

### `influenza`

A data frame with 426 rows and 7 columns:

- ili:

  Integer, weekly number of cases of influenza-like illness

- week:

  Date, start of the week, from 2 January 2012 to 24 February 2020

- inf_A:

  Integer, weekly number of specimens positive for influenza A,
  including those subsequently subtyped as H3N2 or H1N1

- inf_B:

  Integer, weekly number of specimens positive for influenza B

- inf_H3N2:

  Integer, weekly number of specimens positive for influenza A subtype
  H3N2

- inf_H1N1:

  Integer, weekly number of specimens positive for influenza A subtype
  H1N1

- other:

  Integer, weekly number of specimens tested that were not positive for
  influenza, calculated as the number of specimens tested minus the
  number positive for influenza A or B

## Source

<https://www.who.int/teams/global-influenza-programme/surveillance-and-monitoring/influenza-surveillance-outputs>
