# Get trade data from the UN Comtrade API

This function queries the UN Comtrade API to retrieve international
trade data. It allows for detailed specification of the query, including
the type of data (goods or services), frequency (annual or monthly),
commodity classification, flow direction, and more. By providing
`everything` for certain parameters, you can query all possible values.
The function is opinionated in that it already verifies certain
parameters for you and is more than a pure wrapper around the API.

## Usage

``` r
ct_get_data(
  type = "goods",
  frequency = "A",
  commodity_classification = "HS",
  commodity_code = "TOTAL",
  flow_direction = c("Import", "Export", "Re-export", "Re-import"),
  reporter = "all_countries",
  partner = "World",
  start_date = NULL,
  end_date = NULL,
  process = TRUE,
  tidy_cols = TRUE,
  verbose = FALSE,
  primary_token = get_primary_comtrade_key(),
  mode_of_transport = "TOTAL modes of transport",
  partner_2 = "World",
  customs_code = "C00",
  update = FALSE,
  requests_per_second = 10/60,
  extra_params = NULL,
  cache = FALSE
)
```

## Arguments

- type:

  The type of returned trade data. Possible values: 'goods' for trade in
  goods, 'services' for trade in services. Default: 'goods'.

- frequency:

  The frequency of returned trade data. Possible values: 'A' for annual
  data, 'M' for monthly data. Default: 'A'.

- commodity_classification:

  The trade classification scheme. Possible values for goods:
  `c('HS','S1','S2','S3','S4','SS','B4','B5')`; for services:
  `c('EB02','EB10','EB10S','EB')`. Default: 'HS'.

- commodity_code:

  The commodity code(s) or `everything` for all possible codes. See
  `comtradr::ct_get_ref_table('HS')` for possible values. Default:
  'TOTAL' (sum of all commodities).

- flow_direction:

  The direction of trade flows or `everything`. Possible values can be
  found in `ct_get_ref_table('flow_direction')`. These are implemented
  case-insensitive, 'import' and 'Import' are equivalent. Default:
  c('import','export','re-export','re-import').

- reporter:

  Reporter ISO3 code(s), `everything` or `all_countries`. See
  [`comtradr::country_codes`](https://docs.ropensci.org/comtradr/reference/country_codes.md)
  or `comtradr::ct_get_ref_table('reporter')` for possible values.
  `all_countries` returns all countries without aggregates `everything`
  returns all possible parameters. Default: 'all_countries'.

- partner:

  Partner ISO3 code(s), `everything` or `all_countries`. See
  [`comtradr::country_codes`](https://docs.ropensci.org/comtradr/reference/country_codes.md)
  for possible values. `all_countries` returns all countries without
  aggregates `everything` returns all possible parameters, incl.
  aggregates like World. Default: 'World' (all partners as an
  aggregate).

- start_date:

  The start date of the query. Format: `yyyy` for yearly, `yyyy-mm` for
  monthly.

- end_date:

  The end date of the query. Format: `yyyy` for yearly, `yyyy-mm` for
  monthly. Max: 12 years after start date for annual data, one year for
  monthly data.

- process:

  If TRUE, returns a data.frame with results. If FALSE, returns the raw
  httr2 request. Default: TRUE.

- tidy_cols:

  If TRUE, returns tidy column names. If FALSE, returns raw column
  names. Default: TRUE.

- verbose:

  If TRUE, sends status updates to the console. If FALSE, runs functions
  quietly. Default: FALSE.

- primary_token:

  Your primary UN Comtrade API token. Default: stored token from
  [`comtradr::set_primary_comtrade_key`](https://docs.ropensci.org/comtradr/reference/set_primary_comtrade_key.md).

- mode_of_transport:

  Text code of mode of transport or `everything` for all possible
  parameters. See `ct_get_ref_table(dataset_id = 'mode_of_transport')`
  for possible values. Default: 'TOTAL modes of transport' (TOTAL).

- partner_2:

  Partner 2 ISO3 code(s), `everything` or `all_countries`. See
  [`comtradr::country_codes`](https://docs.ropensci.org/comtradr/reference/country_codes.md)
  for possible values. `all_countries` returns all countries without
  aggregates `everything` returns all possible parameters, incl.
  aggregates like World. Default: 'World' (all partners as an
  aggregate).

- customs_code:

  Customs Code ID or `everything` for all possible parameters. See
  `ct_get_ref_table(dataset_id = 'customs_code')` for possible values.
  Default: 'C00' (TOTAL).

- update:

  If TRUE, downloads possibly updated reference tables from the UN.
  Default: FALSE.

- requests_per_second:

  Rate of requests per second executed, usually specified as a fraction,
  e.g. 10/60 for 10 requests per minute, see `req_throttle()` for
  details.

- extra_params:

  Additional parameters to the API, passed as query parameters without
  checking. Please provide a named list to this parameter. Default:
  NULL.

- cache:

  A logical value to determine, whether requests should be cached or
  not. If set to True, `tools::R_user_dir(which = 'cache')` is used to
  determine the location of the cache. Use the .Renviron file to set the
  R_USER_CACHE_DIR in order to change this location. Default: False.

## Value

A data.frame with trade data or, if `process = F`, a httr2 response
object.

## Details

The UN Comtrade database provides a repository of official international
trade statistics and relevant analytical tables. It contains annual
trade statistics starting from 1988 and monthly trade statistics since
2000 for goods data

Parameters that accept `everything` will query all possible values. For
example, setting `commodity_code = 'everything'` will retrieve data for
all commodity codes. This can be useful for broad queries but may result
in large datasets.

The Comtrade API rejects requests whose URL is too long. If long lists
of partner or reporter codes (e.g. `partner = 'all_countries'`) would
exceed this limit, the query is automatically split into several
requests and the results are combined into a single data.frame. This is
not possible with `process = FALSE`, because a single response object is
returned.

## Examples

``` r
if (FALSE) { # interactive()
# Query goods data for China's trade with Argentina and Germany in 2019
ct_get_data(
  type = "goods",
  commodity_classification = "HS",
  commodity_code = "TOTAL",
  reporter = "CHN",
  partner = c("ARG", "DEU"),
  start_date = "2019",
  end_date = "2019",
  flow_direction = "Import",
  partner_2 = "World",
  verbose = TRUE
)

# Query all commodity codes for China's imports from Germany in 2019
ct_get_data(
  commodity_code = "everything",
  reporter = "CHN",
  partner = "DEU",
  start_date = "2019",
  end_date = "2019",
  flow_direction = "Import"
)

# Query all commodity codes for China's imports from Germany
# from January to June of 2019
ct_get_data(
  commodity_code = "everything",
  reporter = "CHN",
  partner = "DEU",
  start_date = "2019",
  end_date = "2019",
  flow_direction = "import"
)
}
```
