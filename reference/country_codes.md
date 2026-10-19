# Country codes

A full dataset of all reporter and partner codes available in the UN
Comtrade database.

## Usage

``` r
country_codes
```

## Format

`country_codes` A dataframe with 312 rows and eight columns:

- id:

  Unique country code.

- country:

  Name of the country (in English).

- iso_3:

  The country's ISO 3 code.

- iso_2:

  The country's ISO 2 code.

- note:

  Notes about the country.

- entry_year:

  The country's entry into the international system or 1900 (whichever
  is largest).

- exit_year:

  The country's exit from the international system, if applicable.

- group:

  Indicates whether the entity is a group of countries. For example,
  ASEAN or the European Union.

- reporter:

  Indicates whether the country is a reporter in the UN Comtrade
  database.

- partner:

  Indicates whether the country can be reported on by others in the UN
  Comtrade database. Not all partners are reporters. For example, the
  World cannot report its trade values.

## Source

<https://comtradeapi.un.org/files/v1/app/reference/Reporters.json> and
<https://comtradeapi.un.org/files/v1/app/reference/partnerAreas.json>
