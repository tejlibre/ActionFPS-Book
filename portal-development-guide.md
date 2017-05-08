# Architecture

![](https://docs.google.com/drawings/d/1fGcN6fcKC_oCL1KtC_-pmWCT_1Da-ka0nijrzUuxiyw/pub?w=661&h=567)

# Repositories
* [ActionFPS @ GitHub](https://github.com/ScalaWilliam/ActionFPS)
* [ActionFPS/game-log-parser @ GitHub](https://github.com/ActionFPS/game-log-parser)
* [ActionFPS/binary-game-parser](https://github.com/ActionFPS/binary-game-parser)
* [ActionFPS/server-pinger @ GitHub](https://github.com/ActionFPS/server-pinger)

[View large image](https://docs.google.com/drawings/d/1fGcN6fcKC_oCL1KtC_-pmWCT_1Da-ka0nijrzUuxiyw/pub?w=1984&h=1701)

# Technology Choices

* __Scala__ for data processing and Play framework: solid, stable toolkit for dealing with complex data.
* __jsoup__ for rendering templates: dynamic, works well with HTML5 and XML.

# Project layout

My preference is to have everything in a single Git repository but I did some refactoring to move the **stable** bits out of the repository into their own libraries/modules/repositories so that the stable stuff does not get in the way any more.

* Portal: https://github.com/ScalaWilliam/ActionFPS
  * Game definition & game log parser. https://github.com/ActionFPS/game-log-parser
  * Server pinger. https://github.com/ActionFPS/server-pinger
* syslog ingester for AC: https://github.com/ScalaWilliam/syslog-ac
   Will try to make it generic though.

# Developing

Install SBT: http://www.scala-sbt.org/download.html

Use IntelliJ Community Edition: https://www.jetbrains.com/idea/download/. Simply import the directory as an 'SBT' project.

```
sbt web/run
```

This will load all the data and cache the computations in-memory, so you can work on the front-end.
If you wish to not cache all the data & would like to recompute it instead, use:

```
sbt 'web/run -Dfull.provider=normal'
```

# Running tests

```
sbt clean test it:test
```

# API endpoints

The API endpoints are intended to be stable.

We provide CSV where possible, and JSON otherwise.

**[cors]** means that you can call this API endpoint from a browser AJAX client.
Where CORS is not allowed, we expect you to use server-side access instead. The list of these definitions is specified in [application.conf](https://github.com/ScalaWilliam/ActionFPS/blob/master/web/conf/application.conf#L42) `play.filters.cors.pathPrefixes` section. 

## Recommended
* https://actionfps.com/player/by-email/?email=your-email@gmail.com
  **[cors]**: map an e-mail to player. Can be used in conjunction with Google+ Sign In.
* https://actionfps.com/players/?format=json
  **[cors]**: list all players.
* https://actionfps.com/all/games.ndjson[?since=2017-01...] (we just use string comparison)
* https://actionfps.com/all/games.json
* https://actionfps.com/server-updates/
* https://actionfps.com/new-games/

### Logs

We offer access to raw logs from ActionFPS. 

* Historical data: `curl -s 'https://actionfps.com/logs.tsv?from=2017-01-01T00:00:00Z&to=2099-04-10T11:02:03Z'` - get raw events for these time periods using the [tab-separated values (TSV)](https://en.wikipedia.org/wiki/Tab-separated_values) format.
* Live data from the end: `curl -s 'https://actionfps.com/logs'` - get live events using [EventSource standard](https://www.w3.org/TR/eventsource/).
* Live data from a specified time: `curl -H 'Last-Event-ID: 2017-04-30T01:02:03Z' -i https://actionfps.com/logs` - get live events from this last event ID. This is part of EventSource standard and is supported by EventSource clients such as [Scala Alpakka one](http://developer.lightbend.com/docs/alpakka/current/sse.html), [Node.js one](https://www.npmjs.com/package/eventsource), as well as the JavaScript one. With this, you can resume from a broken stream of data.

#### Filtering

We filter out some data for privacy reasons:
- IP Addresses are turned to `0.0.0.0`.
- Person-to-person messages are not displayed.

To have access to the true stream we use JWT.

## Other
* https://actionfps.com/game/?id=2015-04-04T14:09:12Z&format=json
   **[cors]**: retrieve a single game
* https://actionfps.com/clan/?id=woop&format=json
   **[cors]**: retrieve a single clan
* https://actionfps.com/all/games.tsv
* https://actionfps.com/all/games.csv
* https://actionfps.com/clanwars/?format=json
* https://actionfps.com/rankings/?format=json
* https://actionfps.com/clanwar/?id=2017-01-06T22:25:14Z&format=json
   **[cors]**: retrieve a single clanwar
* https://actionfps.com/players/?format=registrations-csv
* https://actionfps.com/players/?format=nicknames-csv
* https://actionfps.com/players/?format=json
* https://actionfps.com/player/?id=sanzo&format=json
* https://actionfps.com/playerranks/?format=json
* https://actionfps.com/hof/?format=json
* https://actionfps.com/servers/?format=json
  **[cors]**: retrieve the full server list
* https://actionfps.com/ladder/?format=json

# Dev endpoints

As defined in [web/app/controllers/Dev.scala#L27](https://github.com/ScalaWilliam/ActionFPS/blob/master/web/app/controllers/Dev.scala#L27).

So you can edit templates without having to have the true data.

# DevOps
Continuous Deployment: master --> <https://git.watch/> --> build & restart. Simple monolithic deployment.
