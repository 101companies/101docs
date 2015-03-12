# HEADLINE

pull101repo - clone, pull, slice and dice the virtual 101repo.


# DESCRIPTION

This module takes care of synchronizing the virtual 101repo from Github to the local machine. It clones or pulls the root 101repo and all other dependent repositories. It then symlinks those dependencies into the 101repo folder so that the file system looks like one big repository. These symlinks are also maintained if a contribution is removed.

The initial 101diff is also generated from this module. It simply asks `git diff --name-status` for this and joins the results for all repositories together.

Since later modules require it, it will also write the list of Github dependencies it got to a file.


# JUSTIFICATION OF EXISTENCE

101repo needs to be present on the local machine for any 101worker modules to get any work done. Incrementality also requires an initial 101diff to be generated from the changes in 101repo.


# DEPENDENCIES

None - this module should be the first one that runs.


# ENVIRONMENT

* `repo101dir` (write): directory where 101repo is written to.

* `gitdeps101dir` (write): directory where the other contributions from Github should go.

* `repo101url` (read): where to get 101repo from. In production, this is https://github.com/101companies/101repo.

* `gitdeps101url` (read): where to get the list of Github contributions from. In production, this is http://101companies.org/pullRepo.json.

* `pullRepo101dump` (write): the contents of a request to `gitdeps101url` is written here for other modules to use.
