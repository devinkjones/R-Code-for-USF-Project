## USGS StreamStats in R

## For the extraction of site, watershed, and drainage variables given a GPS location using the USGS StreamStats site. 

## Original RCode (package: streamstats; version: 0.0.1.9011; see below) was modified to directly access StreamStats (communication between R and StreamStats wasn't working, so had to modify RCode with URL tools)

+ Package: streamstats
+ Type: Package
+ Title: R bindings to the USGS Streamstats API
+ Version: 0.0.1.9011
+ Authors@R: c(
    person("Mark", "Hagemann", email = "mark.hagemann@gmail.com",
           role = c("aut", "cre")))
Description: Functions for delineating, visualizing, computing statistics about
    watersheds.
License: MIT
LazyData: TRUE
RoxygenNote: 5.0.1
Suggests: testthat, knitr, rmarkdown
Imports: magrittr, jsonlite, httr, maps, maptools, assertthat, dplyr,
        leaflet, rgdal
VignetteBuilder: knitr
Author: Mark Hagemann [aut, cre]
Maintainer: Mark Hagemann <mark.hagemann@gmail.com>
Built: R 3.4.1; ; 2017-12-07 15:36:32 UTC; unix
RemoteType: github
RemoteHost: https://api.github.com
RemoteRepo: streamstats
RemoteUsername: markwh
RemoteRef: master
RemoteSha: da29c899202f5af60200a8fae20b1bc70fc3b2d7
GithubRepo: streamstats
GithubUsername: markwh
GithubRef: master
GithubSHA1: da29c899202f5af60200a8fae20b1bc70fc3b2d7

