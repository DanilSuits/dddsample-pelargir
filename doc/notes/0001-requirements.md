# Requirements Gathering

Write only databases aren't very interesting.

So we start from the questions: what do we, as a cargo
business, need to be able to ask the domain model?

To keep the scope simple for the moment, let's narrow
the focus to the case of an individual cargo.

The physical manifestation of cargo looks something like this:

* The customer delivers cargo to us at some origin
* We convey the cargo to some other destination
* The customer claims the cargo at the destination

All of this is sequenced in time; the customer can't
claim the cargo unless we convey it.  We can't convey
it unless it has been delivered to us.  Subsequent stages
happen after, and depend on the outcome of, predecessors.

All of the manipulation of cargo is happening in the
_real world_.  The app's role is to keep track of
the business -- bookkeeping and reporting.
  



