---
title: 'thoughts on geospatial metadata'
date: 2015-07-20T18:52:50-07:00
draft: false
---

> I recently decided I need to spend some time after each work day
> writing up my thoughts on work stuff. If nothing else, these can serve
> as an outlet and time for improved pondering. So please forgive the
> winding, rambling nature these posts will take. From these, however, I
> hope to find little nuggets of ideas that I can actually take and make
> into decent presentations, workshops, articles, whatever. As ever, I
> welcome your feedback and questions on these thoughts.

Recently, I made an [OpenRefine Reconciliation Service for
Geonames](https://github.com/cmh2166/geonames-reconcile), and in
particular, for reconciling a metadataset using Library of Congress
Authority terms (think LCSH and LCNAF accessible via id.loc.gov) and
wanting to pull back Geonames identifiers (URIs, rather, as they are in
that vocabulary) and coordinates following ISO 6709 standard.

A number of interesting (perhaps only to me) points came up while
working on this project and using this reconciliation service (which I
do quite often, as we are migrating legacy DC/XML from a variety of
platforms to MODS/XML, and part of this involves batch data
enhancements). The questions and points below are what in particular I
hope to address in this post:

-   Can we standardize, for lack of a better work, the process by which
    someone creates an OpenRefine reconciliation service based off of a
    REST API on top of any vocabulary? Also, API keys are the devil.
-   More specific to geographic terms/metadata, why do I feel the need
    to use Geonames? Why not just use LC Authorities, considering
    they've 'pulled in' Geonames information, matching URIs, in batch?
-   Do we really want to store coordinates and a label AND a URI (and
    whatever else) for a geographic term within a descriptive metadata
    record element? Does it even matter what we want when we have to
    consider what we need and what our systems can currently handle?
-   As a follow-up, where the heck would we even put all those
    datapoints within anything other than MODS? What are some of the RDF
    metadata models doing, and how can folks still working with XML (or
    even MARC) prepare for conversion to RDF? Some ideas on the best
    practices I'm seeing put about, as well as a few proposals for our
    own work.

And other various points that come up while I'm writing this.

------------------------------------------------------------------------

## Lets All Make OpenRefine Reconciliation Services!

Professionally, I'm in some weird space between cataloging, general data
stuff, and systems. So don't take my word on anything, as usually I'm
just translating what already exists in one subdomain to a different
subdomain (despite the fact that library domains just assume they can
already talk to each other, often).

I start with this though to say I'm not a developer of any sort, yet I
was able to pull together the Geonames OpenRefine Reconciliation Service
via trial and error, knowledge of how Geonames REST API works (in
particular, how queries are structured and data returned), and also by
building off all the great community-sourced work that exists. In
particular, [Ted Lawless](http://twitter.com/lawlesst) wrote a great
[FAST OpenRefine Reconciliation
Service](https://github.com/lawlesst/fast-reconcile) that I used to
create something for Geonames. There are some OpenRefine Reconciliation
Service templates for others to build off of - in particular, [a very
simple one in python](https://github.com/mikejs/reconcile-demo), [some
other examples written in
php](https://github.com/rdmpage/phyloinformatics/tree/master/services) -
and an [OpenRefine Reconciliation Service API wiki
document](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API)
that you should take with a grain of salt, as it needs seriously
revisions, updates, and expansions (which, er, maybe I should help
with). This is just scratching the surface of OpenRefine reconciliation
examples, templates, and documentation.

However, once you get into building or modifying an existing
reconciliation service (recon service from this point on for the sake of
my typing hands), you might run into some of the same roadblocks and
questions I did. For example, with the Geonames recon service, I wanted
in particular to return coordinates for a matched name. However, I did
not want to do this via matching with a name, pulling the entire record
for that name serialized however (json, xml, doesn't matter) into a new
column, then parsing that records column to find the particular
datapoints I wanted to add for each row. This method of 'reconciliation'
in OpenRefine - seen when someone adds a new column in OpenRefine by
retrieving URLs generated from the originating column values - takes far
longer than using a recon service, is not scalable unless your
metadatasets are amazingly consistent, and offers more chances for
errors as you have to parse the records in batch for each datapoint
therein you want to pull out (otherwise, you're spending so much time on
each value, you might as well have faceted the column then searched
manually in your authority of choice for the datapoints you're hoping to
retrieve). Yet, the recon service templates and the OpenRefine Recon
Service metadata (explained somewhat in that above wiki page) did not
offer me a place to store and return the coordinates within the recon
service metadata (without a hack I didn't want to make).

As I'm writing this, I realize that a post detailing all the ways one
can use OpenRefine to do 'reconciliation' work would be helpful, so we
know we are comparing apples to apples when discussing. For example,
another way that reconciliation can happen in OpenRefine - using the now
unsupported but still viable and immensely useful [DERI RDF
Extension](http://refine.deri.ie/) - is yet another approach that has
its merits, but could possibly muddle someone's understanding of what
I'm discussing here: the Reconcilation Service API script/app, in my
case built in python and working with a REST API.

For what it's worth, I'd really like to have an OpenRefine in LODLAM
call on the different reconciliation services, examples, and how to
build them. If you're interested in this, let me know. I'm happy to talk
part of this about my own experiences, but I'd like to have at least 1
other person talk.

Regardless, back to building the Geonames recon service, I could get a
basic recon service running by plugging in the Geonames API information
in place of the FAST API information in Lawless' FAST Recon service
code, with minor modifications for changes in how Geonames returns data,
and the inclusion of an API key. The requirement of an API key made this
work that much harder, because it means folks need to go in an add their
own (or use the sample one provided and hit the daily API calls limit
rather quickly) in the core flask app code. I'm sure there are ways to
have the user submit their own code via the CLI before firing up the
service, or in other ways, but I kept it as simple as possible since
this annoyance wasn't my main concern.

My main concern with this API was getting good matches with metadata
using terms taken from the Library of Congress Authorities, in
particular LCSH and LCNAF, and returning top matches along with
coordinates (and the term and term URI from Geonames, luckily built into
the recon service metadata by default). The matching for terms use the
fuzzy-wuzzy library, normally seen in most python Openrefine recon apps
regardless. The coordinates for a match were simply appended to the
matched term with a vertical bar, something easy to split values off of
in OpenRefine (or to remove via the substring function if you happen to
not want the coordinates).

But the first tests of this service described above returned really poor
results (less than 10 direct or above 90% matches for \~100 record test
metadatasets), considering the test metadatasets were already
reconciled, meaning the subject\_geographic terms I was reconciling were
consistent and LCNAF or LCSH (as applicable) form. This is when I took a
few and searched in Geonames manually. I invite you to try this
yourself: search Knoxville (Tenn.) in <http://www.geonames.org>. You get
no matches to records from the Geonames database, and instead (as is the
Geonames default), have results from Wikipedia instead. This is because
Geonames doesn't like that abbreviation - and my sample metadatasets,
all taken from actually metadatasets here at work - are all United
States-centric, particularly as regards subject\_geographic terms.
Search <http://www.geonames.org> now for Knoxville (Tennessee), or
Knoxville Tennessee, or Tennessee Knoxville - the first result will be
exactly what you're searching.

What to do, at least in the context of OpenRefine recon services? Well,
write a simple python script that replaces those LC abbreviations for
states with the full name of the state, then searches Geonames for
matches. See that simple, embarassingly simple solution here:
<http://github.com/cmh2166/geonames-reconcile/blob/master/lc_parse.py>.
Yep, it's very basic, but all of a sudden, the reconciliation service
was returning much, much better results (for my tests, around 80% direct
matches). I invite others to try using this recon service and return
your results, as well as other odd Library of Congress to Geonames
matching roadblocks for more international geographic metadata sets.

There are other things I wish to improve in the Geonames recon service -
some recon services offer the ability, if the top three results returned
from reconciliation are not what you wanted at all, to then search the
vocabulary further without leaving OpenRefine. I played around a bit
with adding this, but had little luck getting it to work. I also want to
see if I can expand the OpenRefine recon service metadata to avoid the
silly Coordinates hack. I'd love to show folks how to host this
somewhere on the web so you do not need to run the Geonames recon
service via the simple python server before being able to use it in
OpenRefine - however, the API key requirement nips this in the bud.

More to the point though, I want to figure out how better to improve
Geonames matching for other, 'standard' library authority sources. It
seems to me like something is fundamentally off with library data work
with the authority services are, from an external data reconciliation
viewpoint, so siloed. Not at all what we want if going towards a library
data on the web, RDF-modeled, world. It seems. to me.

## Geonames versus Library of Congress Authorities

So this brings me to two questions, both of which I got from various
people hearing my talk about this work: why not just reconcile with the
Library of Congress Authorities (which have been matched with Geonames
terms via some batch enhancements recently and should have coordinates
information now, as it is a requirement for geographic names authoirties
in RDA)? And, alternatively, why not just match with Geonames and use
their URI, leaving out LCSH for subject\_geographic or other geographic
metadata (and using it instead for personal/corporate names that aren't
geographic entities, or topical terms, etc.)?

I think this shows better than anything I could say a fundamental divide
in how different parts of library tech world see "Authority" data work.

Here is why I decided to not use the Library of Congress Authorities
entirely for geographic reconciliation:

1.  The reconciliation with Geonames within LCNAF/LCSH is present, but
    is also a second level of work that undermines my wanting to make a
    helpful, fast, error-averse Openrefine recon service. This is not to
    say that I don't think linked authorities data shouldn't have these
    cross-file links; of course they should, be also read my bit below
    on descriptive versus authority record contents.
2.  The hierarchies in LCNAF/LCSH are...lacking. I'd like to know that,
    for example, Richmond (Va.) is in Virginia (yes, I know it says Va.
    in that original heading, but where is the link to the id.loc.gov
    record to Virginia? It's not there), which is in United States, etc.
    etc. Geonames has this information captured.
3.  When there are coordinates, even if matched with Geonames, it is
    often stored in a mads:citation-note, without machine-readable data
    on how the coordinates are encoded. I know I want to pull ISO 6709,
    but not have to check manually the coordinates for each record to
    get the information from the right statement and check the encoding.

*Note:* I'd really love to pull the Library of Congress Name Authority
File linked open dataset from id.loc.gov and test what my limited
experience has led me to believe on LCNAF lacking consistent Geonames
matching, coordinates, and hierarchies - particular for international
geographic names, as my own work leads me often to work just with
geographic names from the United States.

*Note:* Because I don't think the Library of Congress Authorities are
the best currently for geographic metadata DOES NOT MEAN I do not use
them all the time, appreciate the work it took to build them, or think
they should be depreciated or disappear. What I'd like to see is more
open ways for the Library of Congress Authorities to share data and data
modeling improvements with 1. the library tech community already working
on this stuff 2. other, non-traditional 'authorities' like Geonames that
have a lot to offer. Some batch reconciliation work pulling in limited
parts of existing, non-traditional 'authorities' without a mind to how
we can pull that data out in matchine-actionable reconciliation
processes hasn't really helped boost their implementation in the new
world of library data work.

Yet, **I am really, really appreciative of all the work the Library of
Congress folks do, I wish they were so understaffed, and hell, I'd give
my left arm to work there except I'm not smart enough.**

Alright, moving on...why not just use the Geonames URIs and labels
alone, if I feel this way about the Library of Congress Authorities and
geographic terms? The simple reasons is: Facets. Most subject terms are
being reconciled, if they weren't already created using, the LCNAF and
LCSH vocabularies. LCSH and LCNAF makes perfect sense as the remaining
top choice for topical subjects and names (although there are other
players in the non-traditional names authorities field, which I'll
discuss in some other post maybe). As our digital platform discovery
interface, as well as our primary library catalog/all data sources
discovery interface, are not built currently to facet geographic
subjects separate from topical subjects (separate from names as
subjects, etc. etc.). So for the sake of good sorting, intelligible
grouping, etc., LC Authorities for the terms/labels remain the de facto.

Additionally, I'm not sold that the Library of Congress won't catch on
to the need to open up more to cross-referencing and using
non-traditional data sources in more granular and machine-actionable
ways. They seem to be at work on it, so I'd prefer to keep their URIs or
labels there so reconciliation can happen with their authorities. One
must mention too that Geonames does not store references to the same
concepts but in the Library of Congress Authorities, so keeping just the
Geonames term and URI would make later reconciliation with the Library
of Congress Authorities a pain (not to mention, search 'Knoxville
Tennessee', the preference for Geonames queries, in id.loc.gov, and see
all the results you get that aren't 'Knoxville (Tenn.)'. ARgh.)

What to do, what to do... well, build a Geonames recon service that
takes Library of Congress Authorities headings and returns Geonames
additional information, for now.

## Descriptive Metadata & Authority 'Metadata'

Let me start this section by saying that Authorities are within the
realm of 'descriptive metadata', sure. However, when we say 'descriptive
metadata', we normally think of what is known in Cataloging parlance
(for better or for worse) as bibliographic metadata. Item-specific
metadata. This digital resource, or physical resources, present in the
catalog/digital collection, that we are describing so you can discovery,
identify, access (okay okay access metadata isn't descriptive metadata).

What about authority data? We see a lot of authority files/vocabularies
are becoming available as Linked Open Data, but how do we see these
interacting with our descriptive metadata beyond generation of
'authorized' access point URIs and perhaps some reconciliation,
inference, tricks of the discovery interface via the linking and
modeling? The Linked Open Data world is quickly blurring the demarcation
between authority and non-authority, in my opinion - and I find this
really exciting.

So, returning to geospatial metadata, it is not my preference to store
coordinates, label, URI(s) - maybe even multiple URIs if I really want
to make sure I capture both Geonames and LCNAF - in the descriptive
record access point. That's to say, I'm not terribly excited that, in
MODS/XML, this is how I handle the geospatial metadata involved
presently:

```xml
<mods:mods>
  <mods:subject>
    <mods:geographic authority="naf" valueURI="http://id.loc.gov/authorities/names/n79109786">Knoxville (Tenn.)</mods:geographic>
    <mods:cartographics>
      <mods:coordinates>35.96064, -83.92074</mods:coordinates>
    </mods:cartographics>
  </mods:subject>
</mods:mods>
```

Sure, that works, but it can be hell to parse for advanced uses in the
discovery layer, as well as to reconcile/create in the descriptive
metadata records in batch, and to update as information changes. Also,
where is the Geonames authority/uri? Can, and more importantly, should
we repeat the authority and valueURI attributes? Break the MODS
validation and apply perhaps an authority attribute to the coordinates
element, stating from where we retrieved that data? Where is the
attribute on either cartographics or coordinates stating what standard
the coordinates are following for the machine parsing this to know?

Also, more fundamentally, how much of this should be statements in an
Authority record? Wouldn't you rather have (particularly if you're soon
going to MODS/RDF or perhaps another model in RDF that is actually
working at present) something that just gives the locally-preferred
label and a valueURI to 1 authority source that can then link to other
authority sources in a reliable and efficient manner? Perhaps link to
the URI for the Geonames record, then use the Library of Congress
Authorities Geonames batch matching to pull the appropriate, same as
Library of Congress Authority record that way.

So this is something I've been thinking about and working on a lot
lately: creating an intermediate, local datastore that handles authority
data negotiation. Instead of waiting for LC Authorities to add missing
terms to their database (like Cherokee town names, or Southern
Appalachia cross-references for certain regional plants, or whatever),
or parseable coordinates from Geonames, or for Geonames to add LC
Authorities preferred terms or URIs, or whatever other authority you'd
like to work with but has XYZ issues, have a local datastore working
based off an ontology that is built to interact with the chosen
authorities you want to expand upon, links to their records, but puts in
your local authority information too. It is a bit of a pipe dream at the
moment, but I've had some small luck building such a thing using
Skosmos, LCNAF, LCSH, Geonames, and Great Smoky Mountain Regional
Project vocabularies. We'll see if this goes anywhere.

Basically, returning to the point of this post, I want the authority
data to store information related to the access point mentioned in the
descriptive record, not the descriptive record storing all the
information. There are data consistency issues as mentioned, as well as
the need then for discovery interfaces being built for ever more complex
data models (speaking of XML).

However, for the time being, the systems I work with are not great at
this Authority reconciliation, so I put it as consistently as I can all
in that MODS element(s).

I should note, as a final note I think for this post, that I do not add
these URIs or other identifiers as 'preparation for RDF'. Sure, it'll
help, but I'm adding these URIs and identifiers because text matching
has many flaws, especially when it comes to names.

## Things to follow-up on

1.  Getting an OpenRefine Recon Service call together
2.  Discussing some of the geographic data models out there, as well as
    what a person working with something other than MODS can do with
    geographic or other complex authorized accent point data
3.  A million other things under the sun.
