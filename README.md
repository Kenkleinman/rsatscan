<!-- README.md is generated from README.Rmd. Please edit that file -->
rsatscan is a set of tools that function as wrappers for SaTScan, a stand-alone engine for Spatial and Temporal Scan statistics. It has no functions useful outside this context. In order to use the package, you probably need a fairly sophisticated understanding of what SaTScan does.

Included functions useful for most users include

-   write.???()
-   ss.options()
-   satscan()

The write.???() functions take data from data.frame objects and write them to the OS in SaTScan-readable formats.

The ss.options() function is used to set the parameters the SaTScan engine will use. It functions similarly to options() and par() in that there is a set of default parameter settings, and the user can change any or all of them, and reset the options as needed, or recover the defaults. Key parameters include the names of the input data files and the type of analysis and model to be used.

The satscan() function calls into the operating system to run SaTScan. An object is returned with all the output files that SaTScan made. By default, this is all possible outputs, but this can be controlled by changing the default parameter settings.

Three data sets distributed with SaTScan are also included with the package. An example of using the package, replicated from the vignettes, follows. See the vignette for additional explanatory text.

Begin by resetting the paremeter file:

``` r
library("rsatscan")
#> RSaTScan only does anything useful if you have SaTScan-- see http://www.satscan.org/ for free access.
invisible(ss.options(reset=TRUE))
```

Then, change the parameters. The parameters used in the SaTScan manual are replicated:

``` r
ss.options(list(CaseFile="NYCfever.cas", PrecisionCaseTimes=3))
ss.options(c("StartDate=2001/11/1","EndDate=2001/11/24"))
ss.options(list(CoordinatesFile="NYCfever.geo", AnalysisType=4, ModelType=2, TimeAggregationUnits=3))
ss.options(list(UseDistanceFromCenterOption="y", MaxSpatialSizeInDistanceFromCenter=3, NonCompactnessPenalty=0))
ss.options(list(MaxTemporalSizeInterpretation=1, MaxTemporalSize=7))
ss.options(list(ProspectiveStartDate="2001/11/24", ReportGiniClusters="n", LogRunToHistoryFile="n"))
```

Then, write the parameter file, the case file, and the geometry file to the OS.  These case and geometry files are included in the package and distributed with SaTScan.

``` r
td = tempdir()
write.ss.prm(td,"NYCfever")
write.cas(NYCfevercas,td,"NYCfever")
write.geo(NYCfevergeo,td,"NYCfever")
```

Then run SaTScan.

``` r
NYCfever = satscan(td,"NYCfever")
```

The `rsatscan` package provides a `summary` method for `satscan` objects.

``` 
summary(NYCfever)
#> Prospective Space-Time analysis 
#> scanning for clusters with high rates 
#> using the Space-Time Permutation model. 
#>  
#> Study period.......................: 2001/11/1 to 2001/11/24 
#> Number of locations................: 192 
#> Total number of cases..............: 194 
#> _______________________________________________________________________________________________ 
#>  
#> 
#> There were 3 clusters identified.
#> There were 0 clusters with p < .05.
```

SaTScan is available for free from satscan.org.
