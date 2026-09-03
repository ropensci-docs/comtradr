# Set your primary Comtrade API key in the environment variable

If you would like your Comtrade API key to persist in between sessions,
use
[`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
to add the env variable COMTRADE_PRIMARY to your environment file.

## Usage

``` r
set_primary_comtrade_key(key = NULL)
```

## Arguments

- key:

  Provide your primary comtrade key

## Value

Saves your comtrade primary key in the environment.

## Examples

``` r
if (FALSE) { # interactive()
## set API key
set_primary_comtrade_key("xxxxxc678ca4dbxxxxxxxx8285r3")
}
```
