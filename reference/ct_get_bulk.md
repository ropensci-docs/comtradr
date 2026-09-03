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
ct_get_bulk(
  type = "goods",
  frequency = "A",
  commodity_classification = "HS",
  reporter = "all_countries",
  start_date = NULL,
  end_date = NULL,
  tidy_cols = TRUE,
  verbose = FALSE,
  primary_token = get_primary_comtrade_key(),
  update = FALSE,
  requests_per_second = 10/60,
  cache = FALSE,
  download_bulk_files = TRUE
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
  `c('HS','H0','H1','H2','H3','H4','H5','H6')`and
  `c(S1','S2','S3','S4','SS','B4','B5')`; for services:
  `c('EB02','EB10','EB10S','EB')`. Default: 'HS'.

- reporter:

  Reporter ISO3 code(s), `everything` or `all_countries`. See
  [`comtradr::country_codes`](https://docs.ropensci.org/comtradr/reference/country_codes.md)
  or `comtradr::ct_get_ref_table('reporter')` for possible values.
  `all_countries` returns all countries without aggregates `everything`
  returns all possible parameters. Default: 'all_countries'.

- start_date:

  The start date of the query. Format: `yyyy` for yearly, `yyyy-mm` for
  monthly.

- end_date:

  The end date of the query. Format: `yyyy` for yearly, `yyyy-mm` for
  monthly. Max: 12 years after start date for annual data, one year for
  monthly data.

- tidy_cols:

  If TRUE, returns tidy column names. If FALSE, returns raw column
  names. Default: TRUE.

- verbose:

  If TRUE, sends status updates to the console. If FALSE, runs functions
  quietly. Default: FALSE.

- primary_token:

  Your primary UN Comtrade API token. Default: stored token from
  [`comtradr::set_primary_comtrade_key`](https://docs.ropensci.org/comtradr/reference/set_primary_comtrade_key.md).

- update:

  If TRUE, downloads possibly updated reference tables from the UN.
  Default: FALSE.

- requests_per_second:

  Rate of requests per second executed, usually specified as a fraction,
  e.g. 10/60 for 10 requests per minute, see `req_throttle()` for
  details.

- cache:

  A logical value to determine, whether requests should be cached or
  not. If set to True, `tools::R_user_dir(which = 'cache')` is used to
  determine the location of the cache. Use the .Renviron file to set the
  R_USER_CACHE_DIR in order to change this location. Default: False.

- download_bulk_files:

  If TRUE downloads all files that are returned from the Comtrade API as
  a list for the specified parameters. This can result in large writing
  and reading operations from your file system.

## Value

A data.frame with trade data or, if `download_bulk_files = FALSE`, a
data.frame listing the available bulk files for the given parameters.

## Details

The UN Comtrade database provides a repository of official international
trade statistics and relevant analytical tables. It contains annual
trade statistics starting from 1988 and monthly trade statistics since
2000 for goods data

Parameters that accept `everything` will query all possible values. For
example, setting `commodity_code = 'everything'` will retrieve data for
all commodity codes. This can be useful for broad queries but may result
in large datasets.
