# Get reference table from package data

The first time, the function will read from disk, the second time from
the environment. In the case of a necessary update the new data will be
saved to the environment for the current session. You can use this table
to look at the reference tables and if necessary extract respective
classification codes by hand. In general we would recommend the function
`ct_commodity_lookup` for this purpose. It uses the present function in
the backend.

## Usage

``` r
ct_get_ref_table(dataset_id, update = FALSE, verbose = FALSE)
```

## Arguments

- dataset_id:

  The dataset ID, which is either partner, reporter or a valid
  classification scheme.

- update:

  If TRUE, downloads possibly updated reference tables from the UN.
  Default: FALSE.

- verbose:

  If TRUE, sends status updates to the console. If FALSE, runs functions
  quietly. Default: FALSE.

## Value

a tidy dataset with a reference table

## Details

The function allows you to query most possible input parameters that are
listed by the Comtrade API. The following dataset_ids are permitted:

- Datasets that contain codes for the `commodity_code` argument. The
  name is the same as you would provide under
  `commodity_classification`.

  - 'HS' This is probably the most common classification for goods.

  - 'B4'

  - 'B5'

  - 'EB02'

  - 'EB10'

  - 'EB10S'

  - 'EB'

  - 'S1'

  - 'S2'

  - 'S3'

  - 'S4'

  - 'SS'

- 'reporter'

- 'partner'

- 'mode_of_transport'

- 'customs_code'

- 'flow_direction'

- 'frequency'

- 'mode_of_supply'

- 'units_of_quantity'

- 'available_variables'

## Examples

``` r
if (FALSE) { # interactive()
## get HS commodity table
ct_get_ref_table("HS")

## get reporter table
ct_get_ref_table("reporter")
}
```
