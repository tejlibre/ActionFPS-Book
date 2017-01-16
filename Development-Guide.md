# Architecture

![](https://docs.google.com/drawings/d/1fGcN6fcKC_oCL1KtC_-pmWCT_1Da-ka0nijrzUuxiyw/pub?w=661&h=567)

[View large image](https://docs.google.com/drawings/d/1fGcN6fcKC_oCL1KtC_-pmWCT_1Da-ka0nijrzUuxiyw/pub?w=1984&h=1701)

# Technology Choices

* __Scala__ for data processing and Play framework: solid, stable toolkit for dealing with complex data.
* __jsoup__ for rendering templates: dynamic, works well with HTML5 and XML.

# Project layout

My preference is to have everything in a single Git repository but I did some refactoring to move the **stable** bits out of the repository into their own libraries/modules/repositories so that the stable stuff does not get in the way any more.

* Portal: https://github.com/ScalaWilliam/ActionFPS
  * Game definition & game log parser. https://github.com/ActionFPS/game-log-parser
  * Server pinger. https://github.com/ActionFPS/server-pinger
* syslog ingester for AC: https://github.com/ScalaWilliam/syslog-ac <br/> Will try to make it generic though.

# Developing

Install SBT: http://www.scala-sbt.org/download.html

Use IntelliJ Community Edition: https://www.jetbrains.com/idea/download/. Simply import the directory as an 'SBT' project.

```
sbt web/run
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
* https://actionfps.com/player/by-email/?email=your-email@gmail.com <br/> **[cors]**: map an e-mail to player. Can be used in conjunction with Google+ Sign In.
* https://actionfps.com/players/?format=json <br/> **[cors]**: list all players.
* https://actionfps.com/all/games.ndjson
* https://actionfps.com/all/games.json
* https://actionfps.com/server-updates/
* https://actionfps.com/new-games/
* https://actionfps.com/inters/
* https://actionfps.com/clans/?format=csv <br/>**[cors]** list all clans (in CSV)
* https://actionfps.com/clans/?format=json <br/> **[cors]**: list all clans, e.g. to create a challenge or something.

## Other
* https://actionfps.com/game/?id=2015-04-04T14:09:12Z&format=json <br/> **[cors]**: retrieve a single game
* https://actionfps.com/clan/?id=woop&format=json <br/> **[cors]**: retrieve a single clan
* https://actionfps.com/all/games.tsv
* https://actionfps.com/all/games.csv
* https://actionfps.com/clanwars/?format=json
* https://actionfps.com/rankings/?format=json
* https://actionfps.com/clanwar/?id=2017-01-06T22:25:14Z&format=json <br/> **[cors]**: retrieve a single clanwar
* https://actionfps.com/players/?format=registrations-csv
* https://actionfps.com/players/?format=nicknames-csv
* https://actionfps.com/players/?format=json
* https://actionfps.com/player/?id=sanzo&format=json
* https://actionfps.com/playerranks/?format=json
* https://actionfps.com/hof/?format=json
* https://actionfps.com/servers/?format=json <br/> **[cors]**: retrieve the full server list
* https://actionfps.com/ladder/?format=json

# Dev endpoints

As defined in [web/app/controllers/Dev.scala#L27](https://github.com/ScalaWilliam/ActionFPS/blob/master/web/app/controllers/Dev.scala#L27).

So you can edit templates without having to have the true data.

* https://actionfps.com/dev/live-template/
* https://actionfps.com/dev/clanwars/
* https://actionfps.com/dev/sig.svg
* https://actionfps.com/dev/sig/
* https://actionfps.com/dev/player/

# Debugging parsing issues etc

First stage of sanity is to use the 'dev-app' package:

```$xslt
$ sbt show dev-app/stage
...
[info] .../dev-app/target/universal/stage
$ .../dev-app/target/universal/stage
$ ls .../dev-app/target/universal/stage/bin
dev-app     dev-app.bat game-parser
$ 3999.log | ./dev-app/target/universal/stage/bin/game-parser
```

## DevOps
Continuous Deployment: master --> <https://git.watch/> --> build & restart. Simple monolithic deployment.
