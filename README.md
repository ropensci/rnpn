
# rnpn

[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](http://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/)
[![cran
checks](https://cranchecks.info/badges/worst/rnpn)](https://cranchecks.info/pkgs/rnpn)
[![codecov.io](https://codecov.io/github/ropensci/rnpn/coverage.svg?branch=master)](https://codecov.io/github/ropensci/rnpn?branch=master)
[![R build
status](https://github.com/usa-npn/rnpn//workflows/R-CMD-check/badge.svg)](https://github.com/usa-npn/rnpn/actions)

`rnpn` is an R client for interacting with the USA National Phenology
Network data web services. These services include access to a rich set
of observer-contributed, point-based phenology records as well as
geospatial data products including gridded phenological model and
climatological data.

Documentation is available for the National Phenology Network [API
documentation](https://docs.google.com/document/d/1yNjupricKOAXn6tY1sI7-EwkcfwdGUZ7lxYv7fcPjO8/edit?hl=en_US),
which describes the full set of REST services this package wraps.

There is no need for an API key to grab data from the National Phenology
Network but users are required to self identify, on an honor system,
against requests that may draw upon larger datasets. For functions that
require it, simply populate the request\_source parameter with your name
or the name of your institution.

## Installation

CRAN version

``` r
install.packages("rnpn")
```

Development version:

``` r
install.packages("devtools")
library('devtools')
devtools::install_github("usa-npn/rnpn")
```

``` r
library('rnpn')
```

This package has dependencies on both curl and gdal. Some Linux based
systems may require additional system dependencies for the package to
install correctly. For example, on Ubuntu:

``` r
sudo apt install libcurl4-openssl-dev
sudo apt install libproj-dev libgdal-dev
```

## The Basics

Many of the functions to search for data require knowing the internal
unique identifiers of some of the database entities to filter the data
down efficiently. For example, if you want to search by species, then
you must know the internal identifier of the species. To get a list of
all available species use the following:

``` r
species_list <- npn_species()
```

Similarly, for phenophases:

``` r
phenophases <- npn_phenophases()
```

### Getting Observational Data

There are four main functions for accessing observational data, at
various levels of aggregation. At the most basic level you can download
the raw status and intensity data.

``` r
some_data <- npn_download_status_data(request_source='Your Name or Org Here',years=c(2015),species_id=c(35),states=c('AZ','IL'))
```

Note that through this API, data can only be filtered chronologically by
full calendar years. You can specify any number of years in each API
call. Also note that request\_source is a required parameter and should
be populated with your name or the name of the organization you
represent. All other parameters are optional but it is highly
recommended that you filter your data search further.

### Getting Geospatial Data

This package wraps around standard WCS endpoints to facilitate the
transfer of raster data. Generally, this package does not focus on
interacting with WMS services, although they are available. To get a
list of all available data layers, use the following:

``` r
layers <- npn_get_layer_details()
```

You can then use the name of the layers to select and download
geospatial data as a raster.

``` r
npn_download_geospatial(coverage_id = 'si-x:lilac_leaf_ncep_historic',date='2016-12-31',format='geotiff',output_path='./six-test-raster.tiff')
```

If you’re looking for a grid value at a specific latitude/longitude,
that is also possible.

``` r
point_value <- npn_get_point_data('si-x:lilac_leaf_ncep_historic',date='2016-12-31',lat=38.5,long=-110.7)
```

## What’s Next

Please read and review the vignettes for this package to get further
information about the full scope of functionality available.

## Meta

  - Please [report any issues or
    bugs](https://github.com/ropensci/rnpn/issues).
  - License: MIT
  - Get citation information for `rnpn` in R doing `citation(package =
    'rnpn')`
  - Please note that this package is released with a [Contributor Code
    of Conduct](https://ropensci.org/code-of-conduct/). By contributing
    to this project, you agree to abide by its terms.

[![image](http://ropensci.org/public_images/github_footer.png)](https://ropensci.org/)
