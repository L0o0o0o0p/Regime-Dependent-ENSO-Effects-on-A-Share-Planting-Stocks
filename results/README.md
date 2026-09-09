# Results

Running the notebook writes three machine-readable tables to `results/tables/`:

- `historical_horizon_results.csv` — 5-year and 15-year horizon regressions.
- `event_window_results.csv` — exploratory 5-day regressions after each event date.
- `regime_shift_results.csv` — pre-event beta, beta change, implied post-event beta, p-values, and sample sizes.

Generated tables are kept in version control so the headline results can be reviewed without rerunning the notebook. They should be regenerated whenever the data or methodology changes.
