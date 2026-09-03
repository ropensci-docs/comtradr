# Querying bulk files from the API

``` r

library(comtradr)
library(dplyr)
```

### Why use bulk files? 📦📦📦

When you are on the free API tier, you can get relatively large amounts
of data already. I explained this
[here](https://docs.ropensci.org/comtradr/articles/large_data.html).
There is essentially two reasons why you still want to use bulk-files.

1.  you want to get A LOT more data, in like hundreds of millions of
    rows
2.  you want to get consistent HS codes across years.

While a) is certainly a possibility, b) is a lot more likely and a very
common scenario. I will focus on explaining b) in the following.

#### Accounting for HS Code changes 🏗️👷

Let us assume, you want to compare the amount of imported shrimp from
other countries into the EU. You could use HS code
[3003](https://www.trade-tariff.service.gov.uk/headings/0306) for this
purpose and query the data for this across the last 30 years. In the
free tier API this would look similar to something like this:

``` r

hs0 <- comtradr::ct_get_data(
  reporter = c("DEU","FRA"), # only some examples here,
  commodity_classification = 'HS',
  commodity_code = '0306',
  start_date = 1990, # only one year here
  end_date = 1990)

hs5 <- comtradr::ct_get_data(
  reporter = c("DEU","FRA"), # only some examples here,
  commodity_classification = 'HS',
  commodity_code = '0306',
  start_date = 2020, # only one year here
  end_date = 2020)
```

However, as you might not know, the HS code definitions change over the
years. That makes this operation a lot more tricky, because if you
compare the two HS code descriptions for your code, you will see that
they are different!

``` r

print(unique(c(hs0$cmd_desc,hs5$cmd_desc)))
#> [1] "Crustaceans, in shell or not, live, fresh, chilled, frozen, dried, salted or in brine; crustaceans, in shell, cooked by steaming or boiling in water, chilled or not, frozen, dried, salted or in brine"                                                     
#> [2] "Crustaceans; in shell or not, live, fresh, chilled, frozen, dried, salted or in brine; smoked, cooked or not before or during smoking; in shell, steamed or boiled, whether or not chilled, frozen, dried, salted or in brine; edible flours, meals, pellets"
```

To a degree then, you are comparing Apples with Oranges. Or in this
case, Crustaceans that are smoked with the ones that are not.

Hence, you are not really only seeing actual changes in trading values,
but these might change simply because of changes in definition.

With the free API tier, you would now have to engage in lengthy
comparisons between different HS codes, e.g. using the
[concordance](https://github.com/insongkim/concordance)-package. The
package is quite amazing, but the exercise of matching HS codes can be
cumbersome, as there are multiple many-to-many-matches.

#### Getting started with bulk files 🏃

The bulk tier API offers a solution. You can specify which HS
Classification scheme you want to use across all the years you are
interested in. The above code becomes:

``` r

hs0_all <- comtradr::ct_get_bulk(
  reporter = c("DEU","FRA"), # only some examples here,
  commodity_classification = 'H0',
  frequency = 'A',
  verbose = T,
  start_date = 1990, # only one year here
  end_date = 2020)
```

Essentially, the bulk file facility only allows you to list years/month,
reporters and a classification scheme you want to query. You will then
receive one file per period for each reporter. There is multiple
requests involved, which the `ct_get_bulk` function will handle for you.

Be careful though, you cannot query individual commodity codes, hence
the datasets you are querying become very large very quickly. The above
query will download more than 3GB of data, which once it is unpacked
becomes even larger.

The bulk function will download this data, unpack it on your computer,
read it in and then delete the files again. This is a costly operation
and you want to think about how much data you will actually need and
after first reading it in save it in a appropriately compressed file for
future use instead of querying it every time you need it (see
e.g. [fst](https://www.fstpackage.org/)).

#### Formatting bulk files 📝

The bulk files come with less information, in that there is no
descriptive variables included. E.g. partner countries are only included
by code, not by name. Since in most cases we want the description for
plotting and further analysis these need to be merged on. This is very
straight forward with the reference tables included in the package.

``` r

reporter <- comtradr::ct_get_ref_table("reporter") |>
  dplyr::transmute(
    reporter_code = as.character(id),
    reporter_iso = iso_3,
    reporter_desc = country
  )

partner <- comtradr::ct_get_ref_table("partner") |>
  dplyr::transmute(
    partner_code = as.character(id),
    partner_iso = iso_3,
    partner_desc = country
  )

flow <- comtradr::ct_get_ref_table("flow_direction") |>
  dplyr::transmute(flow_code = as.character(id), flow_desc = tolower(text))

clean_data <- hs0_all |>
  dplyr::filter(customs_code == "C00" &
                  mot_code == "0" & partner2code == "0")  |>
  dplyr::left_join(reporter) |>
  dplyr::left_join(partner) |>
  dplyr::left_join(flow) |>
  dplyr::transmute(
    frequency = freq_code,
    reporter_desc,
    reporter_iso,
    partner_desc,
    partner_iso,
    classification_code,
    cmd_code,
    flow_desc = tolower(flow_desc),
    time = period,
    customs_code,
    mot_code,
    mos_code,
    partner2code,
    primary_value
  ) 
#> Joining with `by = join_by(reporter_code)`
#> Joining with `by = join_by(partner_code)`
#> Joining with `by = join_by(flow_code)`
```
