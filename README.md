# acs

## ⛔️ Package Archived ⛔️

It's been a number of years since this package was updated, and it's been mostly hobbling along without support. A number of the features have become unreliable due to changes in the way the Census serves up data via the API. Newer and more well-maintained packages have sprung up to meet the ever-growing needs of R census enthusiasts.

Earlier this year, the package failed a CRAN check. Given the availability of alternative tools, CRAN was allowed to archive it and the package officially went into archive status on `2025-02-12`.

## Description

Provides a general toolkit for downloading, managing, analyzing, and
presenting data from the U.S. Census, including SF1 (Decennial
short-form), SF3 (Decennial long-form), and the American Community
Survey (ACS).  Confidence intervals provided with ACS data are
converted to standard errors to be bundled with estimates in complex
acs objects.  Package provides new methods to conduct standard
operations on acs objects and present/plot data in statistically
appropriate ways.  

## Maintainer

Ezra Haber Glenn <eglenn@mit.edu>

## Current version

In March, 2016, acs version 2.0 was released, considered a substantial
update over the previous version 1.2 due to (1) a major expansion in
the number of datasets available and (2) a modification to the
acs.fetch and acs.look options, which now require a user to explicitly
specify "endyear=" for *all* calls.  

The package now provides full support for *all ACS, SF1, and SF3 data*
currently available via the Census API, including ACS data from
2005-2014 and Decennial data from 1990, 2000, and 2010.
 
You can track development of the `acs` package at
http://eglenn.scripts.mit.edu/citystate/.


## Installation/Upgrading 
### Installation

To install the updated version, simply fire up an R session and type:

```R
install.packages("acs", clean=T)
```

### Upgrading

If you've previously installed the package, you can upgrade with:

```R
update.packages("acs", clean=T)
```
### api.keys migration

To use the package to download data via the American Community Survey
application program interface (API), users need to request an API key
from the Census.  See
http://www.census.gov/developers/tos/key_request.html.

The package includes a function, api.key.install, to allow users to save their key in the package data directory, where it can be found and used automatically for future sessions:  

```R
> # do this once, you never need to do it again
> api.key.install(key="592bc14cnotarealkey686552b17fda3c89dd389")
```

If a user has previously installed a key, it may be lost during the
update process.  If the "clean" option has been set as part of the
update, the package configure scripts will attempt to migrate the key
to a new location.  Failing this, the install script will suggest that
users run api.key.migrate() after installation, which might resolve
the issue.  
```R
> api.key.migrate()
```

At worst, if both migration methods fail, you can simply re-run
api.key.install() with your original key and be good to go.

## Use

The package includes a number of functions with advanced options, to
allow users to work with data from the Census in any number of
different ways.  That said, the general workflow is fairly simple:

 + install and load the package, and (optionally) install an API key;

 + create a geo.set using the geo.make() function;

 + optionally, use the acs.lookup() function to explore the variables
   you may want to download;

 + use the acs.fetch() function to download data for your new
   geography; and then
 
 + use the existing functions in the package to work with your data.

To learn more, consult the following:

 + the printed manual pages for the package;

 + ["Working with acs.R" (June 2013), Ezra Haber Glenn](http://papers.ssrn.com/sol3/papers.cfm?abstract_id=2552524);

 + [The CityState webpage] (http://eglenn.scripts.mit.edu/citystate/ "CityState")


## Changes since previous version

 + endyear is now required: under the old package, acs.fetch and
   acs.lookup would default to endyear=2011 when no endyear was
   provided.  This seemed smart at the time -- 2011 was the most
   recent data available -- but it is becoming increasingly absurd.
   One solution would have been to change the default to be whatever
   data is most recent, but that would have the unintended result of
   making the same script run differently from one year to the next:
   bad mojo.  So the new preferred "version 1.3 solution" is to
   *require* users to explicitly indicate the endyear that they want
   to fetch each time.

 + New ACS Data: the package now provides on-board support for all
   endyears and spans currently available through the API, including:
     
    + American Community Survey 5-Year Data (2005-2009 through 2010-2014)
   
    + American Community Survey 3 Year Data (2013, 2012)

    + American Community Survey 1 Year Data (2014, 2013, 2012, 2011)

   See <http://www.census.gov/data/developers/data-sets.html> for more
   info, including guidance about which geographies are provided for
   each dataset.

 + Decennial Census Data: the package now includes the ability to
   download Decennial Data from the SF1 and SF3, using the same
   acs.fetch() function used for ACS data.  Data includes:

    + SF1/Short-Form (1990, 2000, 2010)

    + SF3/Long-Form (1990, 2000)

 + Other improvements/updates/changes:

   + CPI tables: the CPI tables used for currency.year() and
     currency.convert() have been updated to include data up
     through 2015.
 
   + acs.fetching with saved acs.lookup results: the results of
     acs.lookup can still be saved and passed to acs.fetch via the
     "variable=" option, with a slight change: under v. 1.2, the
     passed acs.lookup results would overrule any explicit endyear or
     span; with v. 1.3, the opposite is true (the endyear and span in
     the acs.lookup results are ignored by acs.fetch).  This may seem
     insignificant, but it will eventually be important, when users
     want to fetch data from years that are more recent than the
     version of the package, and need to use old lookup results to do
     so.

   + divide.acs fixes: the package includes a more robust divide.acs()
     function, which handles zero denominators better and takes full
     advantage of the potential for reduced standard errors when
     dividing proportions.

## Additional notes, details, issues

 + treatment of SF1/SF3 data: When fetched via acs.fetch(), this data
   is downloaded and converted to acs-class objects. (Note: standard
   errors for Decennial data will always be zero, which is technically
   not correct for SF3 survey data, but no margins of error are
   reported by the API.) See
   <http://www.census.gov/data/developers/data-sets/decennial-census-data.html>
   for more info.

 + 1990 table names and numbers: Census support for the 1990 data has
   been a bit inconsistent -- the variable lookup tables were not in
   the same format as others, and far less descriptive information has
   been provided about table and variable names.  This can make it
   tricky to find and fetch data, but if you know what you want, you
   can probably find it; looking in the files in package's extdata
   directory might help give you a sense of what the variable codes
   and table numbers look like.
