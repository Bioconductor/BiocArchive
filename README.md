# BiocArchive <img src="inst/resources/BiocArchive.png" align="right" width="120" />

# Introduction

`BiocArchive` is a package dedicated to preserving reproducibility with
older Bioconductor versions. It works for older Bioconductor releases,
for example version `3.14`. Note that users must have the proper `3.14`
setup to be able to install packages from the archive. This means that
users should be running R version `4.1`.

It is highly recommended that users run docker containers with the
appropriate R version installation and install the package via GitHub or
via source.

# Installation

Currently, it is available via GitHub.

``` r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("Bioconductor/BiocArchive")
```

# Load the package

``` r
library(BiocArchive)
```

# Last release version

The `lastBuilt` helper function finds the last built date for the
supplied Bioconductor version. Other functions rely on this date to
install the appropriate packages.

``` r
lastBuilt(version = "3.14")
#>         3.14
#> "2022-04-13"
```

# Archived installations

To install Bioconductor packages from previous releases, we strongly
recommend using Bioconductor Docker containers, where possible. Such
containers will have `BiocManager` installed. `BiocArchive` allows
versioned installations of [CRAN](https://cran.r-project.org/) packages
from either the Posit Public Package Manager (`P3M`) or the CRAN
[archive](https://cran.r-project.org/src/contrib/Archive/).

## Docker installation

To download the Docker container, one can run the docker command:

    docker pull bioconductor/bioconductor_docker:RELEASE_3_14

For more information, see the Docker for Bioconductor page:
<https://www.bioconductor.org/help/docker/>

## Bioconductor installations

Installations of Bioconductor packages are handled by `BiocManager` and
will work as normal within a legacy container or local installation.

``` r
install("DESeq2", version = "3.14", dry.run = TRUE)
#>                                              CRAN
#> "https://packagemanager.posit.co/cran/2022-04-13"

install("MultiAssayExperiment", version = "3.14", dry.run = TRUE)
#>                                              CRAN
#> "https://packagemanager.posit.co/cran/2022-04-13"
```

**Note**. The `dry.run` argument returns the `CRAN` repository location.
The default is to install `CRAN` packages from the P3M snapshot
repository.

# P3M installations

The [Posit Public Package
Manager](https://packagemanager.posit.co/client/) (`P3M`) allows
installations of packages from their respective snapshot repositories.
To enable installation from these repositories, users must either set
their `getOption("BiocArchive.snapshot")` option or the `snapshot`
argument to `P3M`. By default, the package uses `P3M` snapshots tied to
the last build date of the Bioconductor version.

``` r
install("DESeq2", version = "3.14", dry.run = TRUE, snapshot = "P3M")
#>                                              CRAN
#> "https://packagemanager.posit.co/cran/2022-04-13"

install(
    "MultiAssayExperiment", version = "3.14", dry.run = TRUE, snapshot = "P3M"
)
#>                                              CRAN
#> "https://packagemanager.posit.co/cran/2022-04-13"
```

# CRAN installations from the source archive

Packages on CRAN have a history of versions at a particular URL
location:

<https://cran.r-project.org/src/contrib/Archive>

A CRAN package from the archive can be installed (from source) using
`CRANinstall`:

``` r
CRANinstall("dplyr", "3.14", dry.run = TRUE)
#> https://cran.r-project.org/src/contrib/Archive/dplyr/dplyr_1.0.8.tar.gz
#>
#> The downloaded source packages are in
#>         '/tmp/Rtmp9tHSf3/downloaded_packages'
```

The function will attempt to satisfy all dependencies from the CRAN
archive.

# Repository URLs

To see the list of active repositories based on option configurations,
use the `repositories()` function:

``` r
repositories(version = "3.14")
```

**Note**. The R version must coincide with the Bioconductor version
sought.

# Package validity

To check whether all packages are within the valid time interval of the
Bioconductor release, the `valid()` function will compare package
versions with those in the `P3M` repository.

``` r
valid(version = "3.14")
```

**Note**. The R version must coincide with the Bioconductor version
sought.

# Session Information

``` r
sessionInfo()
#> R version 4.5.1 Patched (2025-06-14 r88325)
#> Platform: x86_64-pc-linux-gnu
#> Running under: Ubuntu 24.04.2 LTS
#>
#> Matrix products: default
#> BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/openblas-pthread/libopenblasp-r0.3.26.so;  LAPACK version 3.12.0
#>
#> locale:
#>  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C
#>  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8
#>  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8
#>  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C
#>  [9] LC_ADDRESS=C               LC_TELEPHONE=C
#> [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C
#>
#> time zone: America/New_York
#> tzcode source: system (glibc)
#>
#> attached base packages:
#> [1] stats     graphics  grDevices utils     datasets  methods   base
#>
#> other attached packages:
#> [1] BiocArchive_0.99.19 nvimcom_0.9-167     colorout_1.3-2
#>
#> loaded via a namespace (and not attached):
#>  [1] vctrs_0.6.5       httr_1.4.7        cli_3.6.5
#>  [4] knitr_1.50        rlang_1.1.6       xfun_0.52
#>  [7] processx_3.8.6    generics_0.1.4    promises_1.3.3
#> [10] jsonlite_2.0.0    glue_1.8.0        htmltools_0.5.8.1
#> [13] ps_1.9.1          chromote_0.5.1    rmarkdown_2.29
#> [16] tibble_3.3.0      evaluate_1.0.4    fastmap_1.2.0
#> [19] yaml_2.3.10       lifecycle_1.0.4   memoise_2.0.1
#> [22] compiler_4.5.1    rvest_1.0.4       codetools_0.2-20
#> [25] pkgconfig_2.0.3   timechange_0.3.0  websocket_1.4.4
#> [28] Rcpp_1.1.0        later_1.4.2       digest_0.6.37
#> [31] R6_2.6.1          pillar_1.11.0     curl_6.4.0
#> [34] magrittr_2.0.3    tools_4.5.1       lubridate_1.9.4
#> [37] cachem_1.1.0      xml2_1.3.8
```
