# Transition from old comtradr

## Transitioning from the old API to the new API 🔄

With the update of the Comtrade API by the United Nations, this package
has undergone a comprehensive rewrite. Most functions that were
available have been deprecated and there are breaking changes also in
the names of arguments and possible parameter values.

With the below examples, we hope to make the transition a little easier.
Most of the design principles of the package have remained similar.

The most important changes can be summarized in that the package: -
extensively checks parameters for validity before submitting them -
allows iso3 standardized country codes as inputs - queries the new
parameters the UN added, such as a mode of transport.

You will see that most other things have stayed more or less the same
and the transition will be a breeze! 💨

### The basics 📊

``` r

#### Previously
q <- ct_search(reporters = "USA", 
               partners = c("Germany", "France", "Japan", "Mexico"), 
               trade_direction = "imports")

#### Now
q <- ct_get_data(reporter = "USA", 
               partner = c("DEU", "FRA", "JPN", "MEX"), 
               flow_direction = "import",
               start_date = 2020,
               end_date = 2023)
```

### The time parameter

``` r

#### Previously
# Get all monthly data for a single year (API max of 12 months per call).
q <- ct_search(reporters = "USA", 
               partners = c("Germany", "France", "Japan", "Mexico"), 
               trade_direction = "imports", 
               start_date = 2012, 
               end_date = 2012, 
               freq = "monthly")

# monthly data for a specific span of months (API max of five months per call).
q <- ct_search(reporters = "USA", 
               partners = c("Germany", "France", "Japan", "Mexico"), 
               trade_direction = "imports", 
               start_date = "2012-03", 
               end_date = "2012-07", 
               freq = "monthly")


#### Now
# Get all monthly data for a single year (API max of 12 months per call).
q <- ct_get_data(reporter = "USA", 
               partner = c("DEU", "FRA", "JPN", "MEX"), 
               flow_direction = "import",
               start_date = 2012, 
               end_date = 2012, 
               frequency = "M"
               )

# monthly data for a specific span of months (API max of five months per call).
q <- ct_get_data(reporter = "USA", 
               partner = c("DEU", "FRA", "JPN", "MEX"), 
               flow_direction = "import",
               start_date = "2012-03", 
               end_date = "2012-07", 
               frequency = "M"
               )
```

### Country Names 🌍

#### Previously

Countries passed to parameters `reporters` and `partners` must be
spelled as they appear in the Comtrade country reference table. Function
`ct_country_lookup` allows us to query the country reference table.

``` r

ct_country_lookup("korea", "reporter")
ct_country_lookup("bolivia", "partner")
q <- ct_search(reporters = "Rep. of Korea", 
               partners = "Bolivia (Plurinational State of)", 
               trade_direction = "all")
```

#### Now

No need to specify the Comtrade country name, just use iso3 codes, as
you can extract them from a myriad of other packages, e.g. 
`countrycodes`, `rnaturalearth` or `giscoR`.

``` r

asia <- countrycode::codelist |> 
  dplyr::filter(un.region.name == "Asia") |> 
  dplyr::pull(iso3c)

q <- ct_get_data(reporter = asia, 
               partner = c("DEU", "FRA", "JPN", "MEX"), 
               flow_direction = "import",
               start_date = 2012, 
               end_date = 2012, 
               frequency = "M"
               )
```

### Searching for commodity codes 🚢📦

#### Previously == Now

This has not changed!

Search trade related to specific commodities (say, tomatoes). We can
query the Comtrade commodity reference table to see all of the different
commodity descriptions available for tomatoes.

``` r

ct_commodity_lookup("tomato")
```

If we want to search for shipment data on all of the commodity
descriptions listed, then we can simply adjust the parameters for
`ct_commodity_lookup` so that it will return only the codes, which can
then be passed along to `ct_search`.

``` r

tomato_codes <- ct_commodity_lookup("tomato", 
                                    return_code = TRUE, 
                                    return_char = TRUE)
```

### API search metadata 📑

``` r

#### Previously
# The url of the API call.
attributes(q)$url
# The date-time of the API call.
attributes(q)$time_stamp
# The total duration of the API call, in seconds.
attributes(q)$req_duration
```

``` r

#### Now
# The url of the API call.
attributes(q)$url
```

    ## [1] "https://comtradeapi.un.org/data/v1/get/C/A/HS?cmdCode=TOTAL&flowCode=M&partnerCode=280%2C276&reporterCode=842%2C841&period=2012&motCode=0&partner2Code=0&customsCode=C00&includeDesc=TRUE"

``` r

# The date-time of the API call.
attributes(q)$time
```

    ## [1] "2023-12-23 11:47:51 UTC"

``` r

# The total duration of the API call, in seconds is not returned anymore!
```

### Package Data 📦

`comtradr` ships with a few different package data objects, and
functions for interacting with and using the package data.

#### Previously

The package data was mostly stored in different databases and had to be
queried and updated separately. See below:

``` r

ct_update_databases()

ct_commodity_db_type()
```

#### Now

All package data can be referenced from one function, which
automatically includes the `update` possibility.

``` r

## to get the parameter values for the mode_of_transport argument
ct_get_ref_table("mode_of_transport")

## to get the parameter values for the partner argument
ct_get_ref_table("partner")

## to update the parameter reference for the partner argument
ct_get_ref_table("partner", update = T)

## to get any commodity classification scheme, just pass in the code
## you would use in commodity_classification
ct_get_ref_table("HS")
```

### “Polished” Column Headers 🎨

Previously there were polished column names, that were handy for
plotting, because they were human readable. This is no longer an
included functionality.

``` r

# Apply polished column headers
q <- ct_use_pretty_cols(q)
```
