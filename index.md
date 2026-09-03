# comtradr

Please [report](https://github.com/ropensci/comtradr/issues) issues,
comments, or feature requests. We are very much looking for feedback on
the usability of the new functions.

Please note that this package is released with a [Contributor Code of
Conduct](https://rOpenSci.org/code-of-conduct/). By contributing to this
project, you agree to abide by its terms.

For information on citation of this package, use `citation("comtradr")`

## Installation 🛠️

You can install the package with:

``` r

install.packages("comtradr")
```

To install the dev version from github, use:

``` r

# install.packages("devtools")
devtools::install_github("ropensci/comtradr@dev")
```

## Usage

### Authentication 🔐

**Do not be discouraged by the complicated access to the token - you can
do it! 💪**

As stated above, you need an API token, see the FAQ of Comtrade for
details on how to obtain it:

➡️ <https://uncomtrade.org/docs/api-subscription-keys/>

You need to follow the detailed explanations, which include screenshots,
in the Wiki of Comtrade to the letter. ☝️ I am not writing them out
here, because they might be updated regularly. However, once you are
signed up, select the `comtrade - v1` product, which is the free API.

#### Storing the API key

If you are in an interactive session, you can call the following
function to save your API token to the environment file for the current
session.

``` r

library(comtradr)

set_primary_comtrade_key()
```

If you are not in an interactive session, you can register the token
once in your session using the following base-r function.

``` r

Sys.setenv('COMTRADE_PRIMARY' = 'xxxxxxxxxxxxxxxxx')
```

If you would like to set the comtrade key permanently, we recommend
editing the project `.Renviron` file, where you need to add a line with
`COMTRADE_PRIMARY = xxxx-your-key-xxxx`.

ℹ️ Do not forget the line break after the last entry. This is the
easiest by taking advantage of the great `usethis` package.

``` r

usethis::edit_r_environ(scope = 'project')
```

### Example 1 ⛴️

Now we can get to actually request some data. Let us query the total
trade between China and Germany and Argentina, as reported by China.

``` r


# Country names passed to the API query function must be spelled in ISO3 format. 
# For details see: https://en.wikipedia.org/wiki/ISO_3166-1_alpha-3 

# You can request a maximum interval of twelve years from the API
example1 <- comtradr::ct_get_data(
  reporter = 'CHN',
  partner = c('ARG', 'DEU'),
  start_date = 2010,
  end_date = 2012
)

# Inspect the return data
str(example1)
```

### Example 2 ⛴️

Return all exports related to Wine from Argentina to all other
countries, for years 2007 through 2011.

``` r

library(comtradr)

# Fetch all shrimp related commodity codes from the Comtrade commodities DB.
# This vector of codes will get passed to the API query.
wine_codes <- ct_commodity_lookup("wine", return_code = TRUE, return_char = TRUE)

# API query.
example2 <- ct_get_data(
  reporter =  "ARG",
  flow_direction = "export",
  partner = "all_countries",
  start_date = 2007,
  end_date = 2011,
  commodity_code = wine_codes
)

# Inspect the output
str(example2)
```

### Bulk Download Example 📦

To download bulk files, use the function `ct_get_bulk`. Usage is
documented in the package vignettes, see
[here](https://docs.ropensci.org/comtradr/articles/bulk_files.html) for
an example.

Attention, this downloads large files (often more than one Gigabyte in
size) and requires a premium key.

``` r

hs0_all <- comtradr::ct_get_bulk(
  reporter = c("DEU"), # only some examples here,
  commodity_classification = 'H0',
  frequency = 'A',
  verbose = T,
  start_date = 2020, # only one year here
  end_date = 2020)
```

## Querying Metadata

The Comtrade API returns many variables. To understand what each
variable means, you can query the variable metadata:

``` r

# Get all available variables with descriptions for the main trade dataset
trade_variables <- ct_get_ref_table("available_variables")
```

You can also query each possible reference table for the arguments that
can be passed to the `ct_get_data` function like this:

``` r

# Get the description for different possible values for the frequency code
frequency <- ct_get_ref_table("frequency")

# You can merge this back to our example
example2 <- example2 |> 
  dplyr::left_join(frequency, by = c("freq_code" = "id"))
```

## Data availability

See [here for an
overview](https://uncomtrade.org/docs/why-are-some-converted-datasets-not-accessible-in-the-ui/)
of available commodity classifications.
