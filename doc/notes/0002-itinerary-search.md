A small thing, that may serve as a good starting point, is the implementation
of the itinerary search.

The existing implementation is a bit inside out; the application is pulling data
out of the domain model rather than passing it in.  The existing implementation
is 

```java
return routingService.fetchRoutesForSpecification(cargo.routeSpecification());

```

But it's not clear to me that the RouteSpecification should be exposed here.

```java
return cargo.fetchRoutes(routingService)

```

Might be more reasonable -- it puts the decision of whether or not to use
the service into the hands of the cargo (which might already have candidate
iterary cached, for example).

It's also odd, in so far as we're querying data that lives "somewhere else"
to fetch what we need to render the web page.  There is a very real sense
in which what we are doing is something more like

```http request
GET /routingService?{urlencode(cargo.routeSpecification)}
```

and then taking the representations we get back from the routing service
and decorating them with forms that can be used to deliver a choice of
itinerary back to the booking system

The form, as a widget, is interesting; the HTML is a composition
of a cached copy of the booking, and a cached copy of the intinerary;
represented as a form that aligns with the expectation of the
_end point_ that will be handling the form submission (if it happens).

```html
<input type="hidden" name="legs[2].voyageNumber" value="0200T" />
```
The _name_ of this input control is coupled to the endpoint that receives
the form submission; the _value_ comes from the Routing service (well,
more precisely the value comes from the schedule of carrier movements,
the fact that it appears as the second leg of this particular itinerary
service is the contribution of this routing service -- sheesh, maybe
I need a diagram of the relations....)

Do we have nested widgets here?  Maybe we should make the fact that are
using a composite UI.  What about the location codes; that's not
very friendly - we might prefer to have something more human friendly
which would require yet another source of truth.

Ultimately, the search result is really just a sequence of transforms

HttpRequest -> TrackingId
TrackingId  -> Cargo (read model)
Cargo -> RoutingSpecification
RoutingSpecification -> TravelDates
TravelDates -> CarrierMovements
CarrierMovements -> RoutingGraph
(RoutingSpecification, RoutingGraph) -> ItineraryCandidates
(TrackingId, ItineraryCandidates) -> HtmlForms





