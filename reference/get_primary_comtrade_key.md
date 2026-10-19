# get_primary_comtrade_key

If you would like your Comtrade API key to persist in between sessions,
use
[`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
to add the env variable COMTRADE_PRIMARY to your environment file.

## Usage

``` r
get_primary_comtrade_key()
```

## Value

Gets your primary comtrade key from the environment var COMTRADE_PRIMARY

## Examples

``` r
if (FALSE) { # interactive()
## get API key
get_primary_comtrade_key()
}
```
