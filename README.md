betfaiR
=======

**coming soon** (hopefully)

This package isn't complete and only has a little functionality at the moment, primarily the easier things, still unable to use the package to place a bet.  I will definitely add this functionality in time, but when I am unsure.

#### Installation

```R
# install devtools
devtools::install_github("durtal/betfaiR")
```

#### Help

There are help pages available [here](http://durtal.github.io/betfaiR/), and issues can be filed [here](https://github.com/durtal/betfaiR/issues).  Help with the package would be welcome.

#### Usage

The primary function in **betfaiR** is `betfair`, which takes three arguments, your username, your password, and an application key (see [here](https://api.developer.betfair.com/services/webapps/docs/display/1smk3cen4v3lu3yomq5qye0ni/Application+Keys) on how to get a key).

```R
bf <- betfair(usr = "USERNAME", pwd = "PASSWORD", key = "APP_KEY")
```

The `betfair` function returns an environment with various methods, you can inspect you session token (required by Betfair for all methods) via the following:

```R
bf$session()
## Session Token: "some_session_token"
```

There is a login in method returned into the `bf` environment, allowing users to login in again if something went wrong initially, this function stores the new details in an hidden object within the environment, and updates the session token.

```R
bf$login(usr = "USERNAME2", pwd = "PASSWORD", key = "APP_KEY")
```

The view the available methods, simply print the environment `bf`

```R
bf
## < betfaiR API>
## Methods available:
##    $competitions(filter = marketFilter())
##    $countries(filter = marketFilter())
##    $events(filter = marketFilter())
##    $eventTypes(filter = marketFilter())
##    $login(usr, pwd, key)
##    $marketBook(marketIds = list(), priceProjection = NULL, orderProjection = NULL, matchProjection = NULL)
##    $marketCatalogue(filter = marketFilter(), marketProjection = NULL, sort = NULL, maxResults = 1, keepRules = FALSE)
##    $marketTypes(filter = marketFilter())
##    $session()
##    $venues(filter = marketFilter())
```

So, if you are familiar with Betfair's API, you will realise the available methods in the package might need a little bit of work.  Each of the methods has its own help page, so to view the help page of the events method, `?events`.

`marketBook` can be used, but because the data returned is often heavily nested, it might not parse the response from betfair as well as other methods.  The response from `marketBook` does include the raw response if required, as an element in a list.

Some of the available methods have a `filter` parameter, which can be used to filter data, this is then added into the body of the request sent to Betfair.  The `marketFilter` function helps you build a filter object, providing all the available parameters that can be filtered by (this hasn't been tested exhaustively but should work).  For example to find horse racing events:

```R
racing <- bf$events(filter = marketFilter(eventTypeIds = 7))
```

The `racing` object is now a dataframe of horse racing events, providing data that includes the event Id, event name, countryCode, timezone, venue, date, and the number of markets.

### PASSWORD and KEY

The appendix **_API key best practices_** in the [api packages vignette](https://cran.r-project.org/web/packages/httr/vignettes/api-packages.html) from the [httr](https://github.com/hadley/httr) package provides some pertinent advice for the storing of you PASSWORD and KEY.  

The advice is to store your PASSWORD and KEY, or any other variables that should be kept safe, in an environment variable, this is to prevent accidentally sharing your credentials, either by sharing an .Rhistory file, or sharing a workspace.

### example

The plot below is data collected from betfair using the `marketCatalogue` and `marketBook` methods.  The `marketBook` method is still incomplete, it returns the json object as a list, so is unparsed.  Data was collected at minute intervals from when the game kicked off, via a simple for loop and then plotted.

![](manchester-derby.jpeg)
