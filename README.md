# eu-sovereign-stack-map

**This repository is archived and is now data only. The site it used to serve has moved to
[https://sovereigntymonitor.eu/map/](https://sovereigntymonitor.eu/map/).**

Nodes across the European technology stack, each with its dependencies named and tied to a dated public source.

## What is still here, and why

`data/stack.json` is the published dataset, under CC BY 4.0. The board on
sovereigntymonitor.eu fetches this file directly, so it is live rather than a copy, and the commit
history is the record of what was published and when. That record is the point: the publication
grades every claim as verified or reported, and a reader who wants to check a figure needs the file
it came from.

`CONTRIBUTING.md` documents the scoring rubric and the sourcing rules that produced this data.
The current version of both lives on the [method page](https://sovereigntymonitor.eu/method/).

## What changed

The map and the ledger were built as standalone sites in 2026 for three reasons: to learn the
subject, to have something concrete to show, and to be open source. The first two worked. The
third did not: across both repositories there were no stars, no forks, no issues and no pull
requests, and a fortnight of traffic before the move totalled one human visitor.

Keeping two separate origins alive had a real cost. The duplicated chrome, the collisions between
two stylesheets that did not know about each other, and three different dark-mode mechanisms
across three surfaces were all consequences of the split rather than of the design. Consolidating
onto one platform removed that class of problem entirely.

Corrections are still welcome, with a primary source. The route is the publication rather than a
pull request: see [sovereigntymonitor.eu](https://sovereigntymonitor.eu/).
