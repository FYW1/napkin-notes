
## Background

`OX Tutor` creates immutable `Ecosystems` from `OX Cnx` and `OX Exercises` content.
These `Ecosystems` are associated with a `Course`,
and `Courses` maintain knowledge of their current and past `Ecosystems`.
`Ecosystems` are built from various elements
(`Book`, `Chapter`, `Page`, `PageExercise`, etc.).

`Tasks` (`Readings`, `Homeworks`, etc.) in `OX Tutor`
are created from, and associated with,
a specific `Ecosystem`.
As updated `Ecosystems` are added to a `Course`,
existing `Tasks` and their associated `Ecosystems` are unaffected.

The UX for `OX Tutor` desires that
content updates be as transparent as possible
to the end users.
This means that `OX Tutor` will need to
automatically 'merge' information 
associated with elements in a `Course`'s past `Ecosystems`
to elements in its current `Ecosystem`
for display purposes.
Because content updates tend to be incremental and additive,
this process can be largely (perhaps entirely) automated.

It is possible that each `OX Tutor` use case
will need its own custom
`Past Ecosystems-to-Current Ecosystem Map` (`PEs-to-CE Map`),
but it seems likely that at most a handful of maps
could address all foreseeable needs.

Because `Ecosystems` are bounded and immutable,
all `PEs-to-CE Maps` can be computed
(and inspected/verified) in advance, 
before a `Course`'s current `Ecosystem` is actually updated.

In the event that an element from a `Course`'s past `Ecosystem`
cannot be mapped to an element in its current `Ecosystem`,
it will be considered an `orphan`
and placed in a special `orphan bin`.
Exactly how `orphan` are handled depends on the UX
for the affected `OX Tutor` use case.

## `Course` Stats

### Overview

It will be necessary for `OX Tutor` to show
a variety of summaries of assignment exercise statistics
(number assigned, number completed, number correct, etc.)
to both teachers and students.
These statistics will be organized in several ways
(by `LO`, by `Page`, by `Chapter`, etc.).

### Implementation

Internally, `OX Tutor` stores assignments as `Tasks`.
Each `Task` contains one or more `TaskSteps`,
some (or all) of which might be `TaskedExercises`.

Each `TaskedExercise` is associated with
a specific `PageExercise`
inside a specific `Course` `Ecosystem`
(past or current).

#### Per `LO`

To organize stats by `LO`,
a pre-computed `PEs-to-CE Map`
will be used.
This `Map` will take as input a `PageExercise` 
from any of the `Course`'s past or current `Ecosystems`,
and will output an `LO` from the `Course`'s current `Ecosystem`
or indicate that the given `PageExercise` is an `orphan`.

#### Per `Page`

#### Per `Chapter`

## Assumptions

### Schema

## Mapping `LOs` to `LOs`

## Mapping `PageExercises` to `LOs`

## Mapping `PageExercises` to `Pages`

## Mapping `PageExercises` to `Chapters`


