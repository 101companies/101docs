# testAllExplorerEntities

`testAllExplorerEntities` - recursively walks over all pages of [101explorer](https://github.com/101companies/101docs/tree/master/explorer) and summarizes all encountered errors in JSON and HTML output.

# DESCRIPTION

Upon running this module it will recursively access all pages of 101explorer and analyse them for errors.
A machine readable JSON listing of all encountered errors along with descriptions will be created at `${worker101dir}/modules/testAllExplorerEntities/results.json`.
Afterwards this data will be rendered to a human readable and nicely formatted HTML document at `{views101dir}/testAllExplorerEntities.html`.

This test module will access the explorer root site with URL `/`.
It will then access all pages that this site links to.
This process is continued recursively until no sites remain.
This means all user visible errors will be reported.
But faulty pages that are not linked to from anywhere, are invisible to this type of check.

All error checking happens server side, that is the network connection is not tested.

Development currently happens on [101worker](https://github.com/101companies/101worker) branch `testAllExplorerEntities`, but is ready to be merged into `master`.

## Running

Like any other module this one is started with each normal worker cycle.

If you want to explicitly run this module do:

    RUNONLY=testAllExplorerEntities make production.run

The module will use all available cores during execution.
The currently tested runtime on a local machine is about 6 Minutes (4x 3.4 GHz), but expected to be faster on the actual server.
Therefore we deemed that this module can be included in the normal worker cycle,.

## Errors types

The following error types will be recognized:

- __Python Exceptions__: Should the attempt to generate a single page of the explorer fail with a thrown python exception, this exception along with description will be logged.
- __Dead Links__: Should the explorer link to another explorer site which it returns a 404, this error is logged as a `ResourceNotFoundException`.
  Note that it is perfectly fine behaviour for invalid links to return 404, but the explorer should not link to 404 pages.
- __Duplicate Links to differernt Resources__: Should the explorer use the same link to address multiple different entities, a `ResourceAlreadyAssignedError` will be logged.
  For Example: The top-level Root-Entity has the URL `/`, if further down a link to another entity is found, with exactly that url, it is assumed that the link generation is faulty, instead of that one url points to two different entities. 
- __Schema Validation Errors__: The explorer includes schema validation functionality.
  That is for each generated webpage, it is checked if it matches the corresponding [Schema] (https://github.com/101companies/101worker/tree/master/schemas).
  Any errors in that validation process will be logged as `ValidationError`.
- __Link/Page inconsistencies__: If a page reports `name` or `classifier` values that differ from the ones that were used to link to the page a `WrongNameError` or `WrongClassifierError` is logged.

## Output format

The HTML output is designed to be human readable, and features descriptions for all custom error types.
It comes with practical CSS (aslong as somethings red, there are errors) and JavaScript (ability to collapse all similiar errors to save screen space).

The JSON output is designed to be machine readable:

- The `errors_descriptions` dictionary contains all known error descriptions.
- The `{entity}_errors` dictionaries track all errors of a certain entity type.
  Entities are: `root`, `namespace`, `member`, `folder`, `file`, `fragment`.
  Errors per entity are then listed for each encounterd error type.

## Implementation

For ease of programming, the actual error checking code is implemented as a Django test under `{worker101dir}/services/explorer/testAllExplorerEntities.py`.
This test will also write the JSON output.
It is run from the "normal" module script `{worker101dir}/modules/testAllExplorerEntities/program.py`.
This script also generates the HTML output based on the JSON.

As the execution of the Django test takes quite a bit of time, it is only run if the environment variable `TEST_ALL_EXPLORER_ENTITIES` is set by the module script.
So normally executing all explorer tests will not run this, are to not slow down TDD.

# JUSTIFICATION OF EXISTENCE

This module serves as a big regression test for 101explorer.

It serves as a help for developers, to be sure that the explorer features no publically visisble errors in 101explorer.

# DEPENDENCIES

This module obviously depends on the availablity of 101explorer.
However it will start its own separate test instance, so 101explorer does not have to be running.
This module may break if the JSON output of 101explorer pages is changed in a big way.

Aside from that it uses the python packages `django.test` to query a page, and `jinja2` to generate the HTML output from a template.

# ENVIRONMENT

- `worker101dir` writes JSON output in the module directory to `{worker101dir}/modules/testAllExplorerEntities/results.json`.
- `views101dir` writer HTML output to `{views101dir}/testAllExplorerEntities.html`.

