drone - Divide and Reclassify on Near Error
===========================================

    Depends: dbacl

A simple wrapper for [`dbacl`][1]
to divide a collection of preclassified samples
into subclasses until a perfect classification
can be obtained.

[1]: http://www.lbreyer.com/dbacl.html

This works by creating
a model for each existing category
(or just a single model
if you do not have any categories to start from)
and cross-validating all the samples
against this model.
Those samples which fail the cross-validation
will be moved to a new category,
which will then be cross-validated again
until we have enough categories
that all samples pass cross-validation
(or no further subdivision is possible
-- a typical outcome is
for a small number of samples to defy
grouping together with other samples,
many times because they are too small
to have enough useful features).

# Running drone

The input should be divided into a directory with
a zero suffix for each existing category.
For example,

    drone spam0/* ham0/*

... to classify the samples in the directories
`spam0` and `ham0` into further subcategories
`spam1`, `spam2`, etc and correspondingly
`ham1`, `ham2` etc for the second directory.

For example, by the end of a run, the samples in
`spam0` will have been redistributed into the directories
`spam0`, `spam1`, `spam2`, etc. based on their similarity,
and the `ham0` samples similarly redistributed
into `ham0`, `ham1`, `ham2`, etc.
The zero category will basically contain the samples which
are close to the original combined model for that category
(so the most "spammy" spam samples in `spam0`
and the most "hammy" ham samples in `ham0`)
and the following numbered categories will contain
increasingly smaller subcategories with some sort of
internal similarity.

If you do not have any classifications to start from,
just create a single directory like `all0`
and put all your samples in there,
and run `drone` on that.
The tool requires there to be a directory,
and its name must end with a number.

The `dbacl` models for these subcategories
are stored away in a separate directory;
by default, they are in `drone_lm/`.
If you wish to change this,
use the `-D` option.

The hash size parameter of `dbacl` has been tweaked to 8
for individual classes and 20 for the superclasses.
This is not a scientific result,
just a quick operational ad-hoc decision.
You may wish to experiment.
Look at the `-h` and `-s` options for `drone`.

`dbacl` inherently supports only a maximum of 64 categories.
In order to work around this limitation,
the classifier will merge categories into "superclasses"
whenever the number of categories is about to exceed this limit.
This allows for up to 64*64 categories = 4,096 categories.

(The `-C` option to `drone` allows you to tweak
the superclass limit down
from the default of 64;
this is mainly useful for testing the superclass behavior
with a smaller number of categories.)
