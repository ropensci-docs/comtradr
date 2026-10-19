# Migrate cache to new location

Comtradr versions previous to version 1.0.1 have used a cache location
that was not CRAN compliant. You can migrate any remaining files to the
new cache location using this function. It will delete the old cache.

## Usage

``` r
ct_migrate_cache()
```

## Value

Nothing

## Examples

``` r
if (FALSE) { # interactive()
ct_migrate_cache()
}
```
