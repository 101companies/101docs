# TODO

Things that need to be done in and around 101worker.


## Add more TODOs to this list

Kevin, Martin, Ralf et alii pls.


## Tests

Modules and libraries in 101worker need more tests. These may be in the form of unit tests or [functional tests with 101test](https://github.com/101companies/101test).

See also [module contracts](https://github.com/101companies/101worker#module-contracts), which talks about those tests.

The following modules and libraries have unit tests:

* [modules](https://github.com/101companies/101worker/tree/master/modules)
    * [pull101repo](https://github.com/101companies/101worker/tree/master/modules/pull101repo)
* [libraries](https://github.com/101companies/101worker/tree/master/libraries)
    * [meta101](https://github.com/101companies/101worker/tree/master/libraries/meta101)
    * [kludge101](https://github.com/101companies/101worker/tree/master/libraries/kludge101)
    * [incremental101](https://github.com/101companies/101worker/tree/master/libraries/incremental101)

The following modules have functional tests:

* [pull101repo](https://github.com/101companies/101worker/tree/master/modules/pull101repo)
* [rules101meta](https://github.com/101companies/101worker/tree/master/modules/rules101meta)
* [matches101meta](https://github.com/101companies/101worker/tree/master/modules/matches101meta)


## Documentation

Virtually all 101worker modules and libraries lack documentation, especially on this 101docs since it didn't exist way back when. This probably requires yanking back the original developers.

See also [module contracts](https://github.com/101companies/101worker#module-contracts), which talks about how to document a module.

The following ones are documented.

* [modules](https://github.com/101companies/101worker/tree/master/modules)
    * [pull101repo](https://github.com/101companies/101worker/tree/master/modules/pull101repo)
* [libraries](https://github.com/101companies/101worker/tree/master/libraries)
    * [meta101](https://github.com/101companies/101worker/tree/master/libraries/meta101)
    * [kludge101](https://github.com/101companies/101worker/tree/master/libraries/kludge101)
    * [incremental101](https://github.com/101companies/101worker/tree/master/libraries/incremental101)

The rest bitterly need it.


## Fix Broken Modules

A bunch of modules don't exit successfully and need to be fixed:

* [refineTokens](https://github.com/101companies/101worker/tree/master/modules/refineTokens)
* [resolve101meta](https://github.com/101companies/101worker/tree/master/modules/resolve101meta)
* [index101meta](https://github.com/101companies/101worker/tree/master/modules/index101meta)
* [repo2charts](https://github.com/101companies/101worker/tree/master/modules/repo2charts)
* [parseMegaL](https://github.com/101companies/101worker/tree/master/modules/parseMegaL)
* [webDeployMegaModels](https://github.com/101companies/101worker/tree/master/modules/webDeployMegaModels)
* [integrate](https://github.com/101companies/101worker/tree/master/modules/integrate)


## Refactor Modules

Modules need to stop using the deprecated [101meta](https://github.com/101companies/101worker/tree/master/libraries/101meta) and [Makefile.vars](https://github.com/101companies/101worker/tree/master/modules/Makefile.vars) and instead use [meta101](https://github.com/101companies/101worker/tree/master/libraries/meta101) and [environment variables](https://github.com/101companies/101worker/tree/master/configs/env). This will make them incremental and testable.

Here's a table, of which modules have been refactored already and which ones need to be.


Module                                                                                                                   | Status
-------------------------------------------------------------------------------------------------------------------------|:--------:
[pull101repo](https://github.com/101companies/101worker/tree/master/modules/pull101repo)                                 | ok
[pull](https://github.com/101companies/101worker/tree/master/modules/pull)                                               | n/a
[pullLibraries](https://github.com/101companies/101worker/tree/master/modules/pullLibraries)                             | n/a
[build](https://github.com/101companies/101worker/tree/master/modules/build)                                             | n/a
[gatherGeshi](https://github.com/101companies/101worker/tree/master/modules/gatherGeshi)                                 | n/a
[rules101meta](https://github.com/101companies/101worker/tree/master/modules/rules101meta)                               | ok
[matches101meta](https://github.com/101companies/101worker/tree/master/modules/matches101meta)                           | ok
[metrics101meta](https://github.com/101companies/101worker/tree/master/modules/metrics101meta)                           | ok
[validate101meta](https://github.com/101companies/101worker/tree/master/modules/validate101meta)                         | ok
[extract101meta](https://github.com/101companies/101worker/tree/master/modules/extract101meta)                           | ok
[fragmentMetrics101meta](https://github.com/101companies/101worker/tree/master/modules/fragmentMetrics101meta)           | ok
[refineTokens](https://github.com/101companies/101worker/tree/master/modules/refineTokens)                               | **not ok**
[predicates101meta](https://github.com/101companies/101worker/tree/master/modules/predicates101meta)                     | ok
[fragments101meta](https://github.com/101companies/101worker/tree/master/modules/fragments101meta)                       | ok
[summary101meta](https://github.com/101companies/101worker/tree/master/modules/summary101meta)                           | ok
[wiki2json](https://github.com/101companies/101worker/tree/master/modules/wiki2json)                                     | **not ok**
[themesExtractor](https://github.com/101companies/101worker/tree/master/modules/themesExtractor)                         | **not ok**
[languageExtractor](https://github.com/101companies/101worker/tree/master/modules/languageExtractor)                     | **not ok**
[vocabulary](https://github.com/101companies/101worker/tree/master/modules/vocabulary)                                   | **not ok**
[wiki2tagclouds](https://github.com/101companies/101worker/tree/master/modules/wiki2tagclouds)                           | **not ok**
[moretagclouds](https://github.com/101companies/101worker/tree/master/modules/moretagclouds)                             | **not ok**
[summarizeModuleDescriptions](https://github.com/101companies/101worker/tree/master/modules/summarizeModuleDescriptions) | **not ok**
[members](https://github.com/101companies/101worker/tree/master/modules/members)                                         | **not ok**
[resolve101meta](https://github.com/101companies/101worker/tree/master/modules/resolve101meta)                           | **not ok**
[index101meta](https://github.com/101companies/101worker/tree/master/modules/index101meta)                               | **not ok**
[repo2charts](https://github.com/101companies/101worker/tree/master/modules/repo2charts)                                 | **not ok**
[parseMegaL](https://github.com/101companies/101worker/tree/master/modules/parseMegaL)                                   | **not ok**
[webDeployMegaModels](https://github.com/101companies/101worker/tree/master/modules/webDeployMegaModels)                 | **not ok**
[validateModuleDescriptions](https://github.com/101companies/101worker/tree/master/modules/validateModuleDescriptions)   | **not ok**
[integrate](https://github.com/101companies/101worker/tree/master/modules/integrate)                                     | **not ok**


## Delete Unused Stuff

There's a lot of code in 101worker that isn't actually used anymore. Those things should all be deleted so that they don't confuse everyone. Git keeps a history anyway, so we can get them back if necessary (as if).

Especially the [attic](https://github.com/101companies/101worker/tree/master/attic) contains old stuff that could probably all be deleted, but there's also a lot of unused [modules](https://github.com/101companies/101worker/tree/master/modules), maybe unused [libraries](https://github.com/101companies/101worker/tree/master/libraries), mysterious [configs](https://github.com/101companies/101worker/tree/master/configs) and [examples](https://github.com/101companies/101worker/tree/master/api%20examples/simpleMetrics) for an API that doesn't appear to actually exist.
