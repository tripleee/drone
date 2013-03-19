drone - Divide and Reclassify on Near Error
===========================================

Depends: dbacl

A simple wrapper for [`dbacl`][1]
to divide a collection of preclassified samples
into subclasses until a perfect classification
can be obtained.

[1]: http://www.lbreyer.com/dbacl.html

The input should be divided into a directory with
a zero suffix for each existing category.
For example,

    drone spam0/* ham0/*

... to classify the samples in the directories
`spam0` and `ham0` into further subcategories
`spam1`, `spam2`, etc and correspondingly
`ham1`, `ham2` etc for the second directory.

The hash size parameter of `dbacl` has been tweaked to 8
for individual classes and 20 for the superclasses.
This is not a scientific result,
just a quick operational ad-hoc decision.
You may wish to experiment.

`dbacl` inherently supports only a maximum of 64 categories.
In order to work around this limitation,
the classifyer will merge categories into "superclasses"
whenever the limit is reached.
This allows for up to 64*64 categories = 4,096 categories.
