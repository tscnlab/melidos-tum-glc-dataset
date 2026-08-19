# MeLiDos TUM GLC dataset

This repository contains the Munich, Germany site data collected by the
Technical University of Munich (TUM) for the MeLiDos field study. It is a GLC
Schema 3.0.2 Data Package derived from the
[original MeLiDos dataset](https://github.com/MeLiDosProject/HildenEtAl_Dataset_2025).

The package entry point is `datapackage.json`. Core metadata are stored in
`data/`; participant-level CSV files referenced by `data/datasets.json` are
organized under `data/files/` by sensor, longitudinal-report, questionnaire,
and study-timing resource.

The source dataset is:

> Hilden, S., Lee, S., Zauner, J., & Spitschan, M. (2025). *Personal light
> exposure dataset for Munich, Germany* (Version 1.0.1) [Data set].
> https://doi.org/10.5281/zenodo.16893901

The refactoring converts the source repository's processed tabular RData
resources to UTF-8 CSV, separates records by participant, preserves R factor
ordering as numeric value/label metadata, and supplies GLC 3.0.2 metadata.

## Important source-processing notes

- All light streams were restricted to the participant trial intervals and
  implicit gaps were expanded by the source processing pipeline. Wrist light
  streams were aligned to 10-second timestamps.
- For TUM_S009 wrist data, the source script identifies clusters of nighttime
  melanopic EDI values above 4,000 lx as implausible and replaces those MEDI
  values with missing values.
- For TUM_S010, the source chronotype script shifts a workday bedtime entered
  before noon forward by 12 hours because it identifies an AM/PM entry error.
- For the source MCTQ overview calculation, `alarm_f` was forced to `FALSE`.
  The source script states that participants who used an alarm on free days
  must be excluded from formal analyses of affected chronotype measures.
- The source device inventory and raw ActLumus exports identify head-device
  serial number `3333`; the study-date workbook records `333`. The package
  normalizes that workbook entry to the source-supported serial `3333`.
- `MS`, the ActLumus field immediately following `DATE/TIME`, is represented as
  the timestamp millisecond component. Its values are zero in the audited TUM
  data.
- All audited TUM ActLumus `EVENT` values are integers; metadata represents
  this field as an integer device event-marker code.

## Calibration-date note

The source metadata workbook supplies device delivery dates but no separately
identified calibration dates. These delivery dates are used provisionally as
calibration-date proxies and require confirmation against calibration records.

## Validation

The package is validated against GLC Schema 3.0.2 with validator 0.5.2.
Validation also runs automatically through
`.github/workflows/validate-glc-dataset.yml`.

## License

The dataset is licensed under the Creative Commons Attribution 4.0
International License (CC BY 4.0). See `LICENSE`.
