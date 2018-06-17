# UN/LOCODE

There seem to be at least two different abstractions to consider here.

First, there is the question of the UN/LOCODE entry itself: which is to say
the _data structure_.  What data are we going to include in the current
representation, how tight should the validation of the representations
be, and so on.

Second, there is the question of _issues_; which sets of codes are
valid?  Codes aren't fixed in time forever  They have a lifecycle, 
which goes from `request-under-consideration` to `in-use`, to `deleted`, 
to `reserved` to `available` to `nominated`.

Entries are only `in-use` or `deleted` and sometimes `request-under-consideration`; 
which is to say, there's no such thing as an entry not associated with an issue.

_Note: these state names are made up on the spot, they aren't official._

> Entries existing in the UN/LOCODE will be deleted only in the case of 
> duplication of entries, of misspelling or manifest misunderstanding of 
> an entry name for which a correct version already exists elsewhere in 
> the UN/LOCODE, or on notification by an authoritative body that the 
> location is no longer used for goods movements associated with trade.

If a location is no longer used for goods movements, then a cargo
shipping application probably doesn't care much about it when booking
new cargo.  However, for tracking a history of what we have booked
in the past, we would still probably need to old codes, and to avoid
confusing them if they get re-issued.

The codes themselves are versioned.

> [Date][5] displays the last date when the location was updated/entered.

So in a sense, `/un-locode/issues/2017-2/entries/HKHKG` refers to 
`/un-locode/locodes/HKHKG/updates/9601`.

For the purposes of a demonstration piece - maybe we don't need all
of that complexity right now; just fix the allowed codes to a single
issue, and move on.

The codes specified in the legacy implementation are not current.
The code used for _Hong Kong_ in the sample is `CNHKG` but the 
the official UNECE 2017-2 issue uses the code `HKHKG`.  So for
backwards compatibility, we may want to include a legacy sample issue.

## Validation

Something I happened to notice in passing: validating that some
untrusted data is a "real" locode follows the same process that
you do as a person.  You find the current issue, and then you
scan it to see whether the code is present.  In other words, we
can associate the validation capability with specific domain
objects in our model.

This also hints that we might want flexibility in our design
that allows us to us the UNECE web portal as the "repository"
of active shipping locations.



## Links

 * [UNECE Recommendation No. 16][2]
 * [UNECE UN/LOCODE Manual][1]
 * [UN/LOCODE Code List By Country][3]
 * [Contents and Layout of UN/LOCODE: Codes and Abbreviations Used][4]
  
 [1]: http://www.unece.org/fileadmin/DAM/cefact/locode/unlocode_manual.pdf
 [2]: http://www.unece.org/trade/untdid/download/99trd227.pdf
 [3]: https://www.unece.org/cefact/locode/service/location
 [4]: https://service.unece.org/trade/locode/Service/LocodeColumn.htm
 [5]: https://service.unece.org/trade/locode/Service/LocodeColumn.htm#Date
 