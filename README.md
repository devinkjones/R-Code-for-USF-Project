## USGS StreamStats in R

## For the extraction of site, watershed, and drainage variables given a GPS location using the USGS StreamStats site. 

## Original RCode (package: streamstats; version: 0.0.1.9011; see below) was modified to directly access StreamStats (communication between R and StreamStats wasn't working, so had to modify RCode with URL tools)

Package: streamstats
Type: Package
Title: R bindings to the USGS Streamstats API
Version: 0.0.1.9011
Authors@R: c(
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

## Libraries used for streamstats extraction

## install.packages prior to loading libraries
library(XML)
library(RCurl)
library(streamstats)
library(urltools) 
library(utils)
library(xml2)
library(htmltools)
library(htmlwidgets)
library(httpcode)
library(httpuv)
library(httr)
library(raster)
library(rgdal)
library(rgeos)
library(RgoogleMaps)

## Convert GPS to State 

## Can do it this way, or merge StateFIPSCode in File 1 to File 2 with StateFIPSCode, StateName, StateAbbreviation

## Would suggest merging (left_join) with StateFIPS code document, as the GPS coordinate style doesn't handle points near state boundaries well <-- returns lots of 'NA' and you have to clean the data

## Merge (left_join) is cleaner and doesn't add/miss any GPS locations

library(sp)
library(maps)
library(maptools)

latlong2state <- function(pointsDF) {
## Prepare SpatialPolygons object with one SpatialPolygon
## per state (plus DC, minus HI & AK)
  states <- map('state', fill=TRUE, col="transparent", plot=FALSE)
  IDs <- sapply(strsplit(states$names, ":"), function(x) x[1])
  states_sp <- map2SpatialPolygons(states, IDs=IDs, proj4string=CRS("+proj=longlat +datum=WGS84"))

## Convert pointsDF to a SpatialPoints object 
  pointsSP <- SpatialPoints(pointsDF, proj4string=CRS("+proj=longlat +datum=WGS84"))

## Use 'over' to get _indices_ of the Polygons object containing each point 
  indices <- over(pointsSP, states_sp)

## Return the state names of the Polygons object containing each point
  stateNames <- sapply(states_sp@polygons, function(x) x@ID)
  stateNames[indices]
}

## Match state names with abbreviation
state.abb[match(stateNames[indices], tolower(state.name))]

## OR --> Merge (left_join) --> RECOMMENDED
library(maps)
library(dplyr)

## Load in Excel file that contains columns with 'StateFIPSCode', 'State Name', 'State Abbreviation'
library(readxl)
StateFIPS <- read_excel("~/Desktop/StateFIPS.xlsx")
View(StateFIPS)

## Merge both data sets

## "SiteData" contains all site longtitude, site latitude, state FIPS, county FIPS, monitoring location identifier

## "StateFIPS" contains state FIPS, state name, state abbreviation

## Had to use 'by = c()' because State FIPS codes were labeled differently in c1 (SiteData) than c2 (StateFIPS)
left_join(SiteData, StateFIPS, by = c("StateCode" = "FIPS Code"))