# Common Textbook Experience - Rulesets

Spike CSS as appropriate language for implementing python-based content collation and numbering

Found 3 applicable libraries: `tinycss`, `cssutils`, `cssselect` (yes, 3 esses)

used example less(css) from https://github.com/philschatz/book-collation/blob/master/rulesets/template.less

Note that Phil`s thoughts in that repo include a variant markup language
that is not actually HTML. The custom tag names sound very much like CNXML
to me (PHIL: I did it for brevity :smile:). They do have the advantage of making the rules much clearer: i.e.

```less
Book {
    Chapter {
        Exercise.homework {
        move-to: rs-123;
        }
    }
}
```

Rather than:

```less
div[data-type="book"] div[data-type="chapter"] div[data-type="exercise"].homework {
  move-to: rs-123;
}
```

This may be implementable via 'detached rulsets' in less. Need to investigate.

Another not-quite CSS feature is the ::div syntax. This is a new
pseudo-selector we`re inventing that basically does ::after { content: <div>new
stuff here } so we that the content moved or copied to pending() targets will
be wrapped with a stylable div.

We`re going to have to deal with rules that create not only new div`s but new
pages as well: Table of Figures, Glossary, Selected Exercise Answers etc.
Basically anything that is moved respective to the whole book will need a
virtual-page to land on, and then will show up in the Table of Contents for the
whole book.

Going to need to define them with uuids, and declare that those uuids return
something from archive in book context, but 404 outside that context. Also, no
raw html for them, since they they only exist as cooked html. 

Steps involved:

- convert via `lessc` to css
- some sort of CSS macro expansion (Book -> div[data=type="book"] etc.)

(or vice versa, depends on details of macro exp. and parsing)

- parse via tinycss
- extract selector, parse w/ cssslect, convert to xpath for applicatio to DOM

Two types of rules:
- collation (moving/copying content)
- numbering

Collation must both precede and follow numbering. In some cases, numbering
happens in sequence in the post-collated document (out of order for original
source - i.e.  diff types of exercises all numbered sequentially at chapter
end) In other cases, the content is numbered in the parent`s context, and 
that number "follows" the content to it`s new location - e.g. answers at the
end-of-book are numbered by the exercise numbers that happen at end of chapter.
On further consideration, I think this is a case of the "references" pattern - the
local number is that assigned in a remote location, so we handle it in the
link-reference cleanup pass.

Collation can be done in a single pass, provided that no copy/move can target
an earlier point in the document (no move _up_). Can validate the RuleSet doc
to be sure that every needed target is at least defined (need the doc to be
sure that pending() rule actually matches anything, though)

_Aside_: Perhaps can use move-to: "delete" as way to delete content? (special target)
_Aside2_: Phil suggests "display: none" as the CSS way to do that.
    
Numbering will require two passes, to allow for forward references. Numbering is a sub-case
of link resolution, in general.

- first numbering pass: 
  1. count all the things (create counters for all object types RuleSet says to count)
  1. bake in the numbers (as a property indicating the computed sequential value -- will use CSS
     to style actual representation in final doc ?)
  1. find all reference links (internal)

- second number/link resolution pass:
  1. fill all the references w/ appropriate value (may include baked-in numbering)
  1. _Aside:_ might use second pass to pull content back out of "delete" state, such as figures or
     tables referenced in exercises when generating an exerciser-only or answer-key PDF
