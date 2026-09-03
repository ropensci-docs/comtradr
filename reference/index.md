# Package index

## Get data

Workhorse functions of the package

- [`ct_get_data()`](https://docs.ropensci.org/comtradr/reference/ct_get_data.md)
  : Get trade data from the UN Comtrade API
- [`ct_get_bulk()`](https://docs.ropensci.org/comtradr/reference/ct_get_bulk.md)
  : Get trade data from the UN Comtrade API

## Internal functions

These functions are called internally by `ct_get_data` and set the
comtrade keys.

- [`get_primary_comtrade_key()`](https://docs.ropensci.org/comtradr/reference/get_primary_comtrade_key.md)
  : get_primary_comtrade_key
- [`set_primary_comtrade_key()`](https://docs.ropensci.org/comtradr/reference/set_primary_comtrade_key.md)
  : Set your primary Comtrade API key in the environment variable

## Access package data

These functions allow you to access internal reference tables, such as
possible ISO codes and commodity codes.

- [`country_codes`](https://docs.ropensci.org/comtradr/reference/country_codes.md)
  : Country codes
- [`ct_commodity_lookup()`](https://docs.ropensci.org/comtradr/reference/ct_commodity_lookup.md)
  : UN Comtrade commodities database query
- [`ct_get_ref_table()`](https://docs.ropensci.org/comtradr/reference/ct_get_ref_table.md)
  : Get reference table from package data

## Migrating cache

This function migrates your cache (comtradr version 1.0.0 and earlier)
to the CRAN compliant place.

- [`ct_migrate_cache()`](https://docs.ropensci.org/comtradr/reference/ct_migrate_cache.md)
  : Migrate cache to new location

## Deprecated functions

These functions were part of the previous iteration of the package and
are now deprecated.

- [`ct_commodity_db_type()`](https://docs.ropensci.org/comtradr/reference/ct_commodity_db_type.md)
  **\[superseded\]** : ct_commodity_db_type
- [`ct_country_lookup()`](https://docs.ropensci.org/comtradr/reference/ct_country_lookup.md)
  **\[superseded\]** : ct_country_lookup
- [`ct_get_remaining_hourly_queries()`](https://docs.ropensci.org/comtradr/reference/ct_get_remaining_hourly_queries.md)
  **\[superseded\]** : ct_get_remaining_hourly_queries
- [`ct_get_reset_time()`](https://docs.ropensci.org/comtradr/reference/ct_get_reset_time.md)
  **\[superseded\]** : ct_get_reset_time
- [`ct_pretty_cols`](https://docs.ropensci.org/comtradr/reference/ct_pretty_cols.md)
  : ct_pretty_cols
- [`ct_register_token()`](https://docs.ropensci.org/comtradr/reference/ct_register_token.md)
  **\[superseded\]** : ct_register_token
- [`ct_search()`](https://docs.ropensci.org/comtradr/reference/ct_search.md)
  **\[superseded\]** : ct_search
- [`ct_update_databases()`](https://docs.ropensci.org/comtradr/reference/ct_update_databases.md)
  **\[superseded\]** : ct_update_databases
- [`ct_use_pretty_cols()`](https://docs.ropensci.org/comtradr/reference/ct_use_pretty_cols.md)
  **\[superseded\]** : ct_use_pretty_cols
