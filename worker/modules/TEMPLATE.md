# HEADLINE

Headline - a very short quip about what your module does.

The above sentence is a good example for a headline, any detailed information goes into the next sections.


# DESCRIPTION

What your module does in detail. This is high-level documentation, so don't get too lost in implementation. You can also put sub-sections in here, if necessary.

This is so that other people can understand what your module's job is without having to delve into its code.


# JUSTIFICATION OF EXISTENCE

Why your module is necessary and shouldn't be kicked out of the production cycle and `git rm -rf`'d *right now*, like the waste of CPU time it is. Keep this kinda short too and don't repeat things from your description.

This is to aid finding obsoleted modules later.


# DEPENDENCIES

Which other modules and non-standard libraries you depend on and - most importantly - *why* you depend on them.

This is so that breaking changes can be identified before they happen.


# ENVIRONMENT

Which environment variables you require and - importantly again - *what* you do with them. You should make a list like this:

* `sample101url` (read): URL where important directives are read from.

* `sample101dir` (write): directory where data result data is put.

* `sample101dump` (read/write): file where the data dump of this module goes.

This is to document your module's in- and output. You *are* using environment variables instead of hard-coded paths, aren't you? Because otherwise it's `git rm -rf` again.
