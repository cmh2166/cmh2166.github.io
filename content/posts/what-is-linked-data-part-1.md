---
title: "what is linked data part 1"
date: 2014-09-14T18:52:50-07:00
draft: false
---

Last weekend, I got to attend the [NYC Archives Unconference](http://nycarchivesunconference.wordpress.com/) (held by the wonderful archivists at Barnard College). It was a great day, although I admit that I skipped out at points to work on Columbia’s test BIBFRAME Editor (more on that later). In a minute of cheek, I proposed to lead a session on Linked Data - and I hoped it would be the kind of Linked Data session that I’m always looking for, by which I mean one based on real world examples. Since these examples-based and libraries-focused sessions don’t appear often in my world, I thought this might be a nice place to start such a conversation.

For better or worse, the session turned out to be really popular - and so became more of a workshop/lecture than a breakout session. And as ever, it took longer than I hoped to cover the Linked Data theory basics so there wasn’t as much time to work with the examples. But I think it turned out well for some of the audience…?

As a follow-up to that session, I made [my speaker notes](https://docs.google.com/document/d/1MayAPDpxT05EwrxCaat826hk0U7g8GiRHQ6Y42V5SzY/edit?usp=sharing) available, typos, ramblings, and all. Folks should be able to leave comments on the notes, but no one has (so far - I’m kind of amazed that people are actually looking at that document). I am also aiming to blog parts of those notes here, but with expansions and, for the examples, screencasts.

So, here is my highly subjective, mostly correct, library metadata-centric, cheat-sheet introduction to Linked Data. Later this week I will start posting the examples using OpenRefine.

## Part I : Linked Data what?

To start with, what do libraries and archives people think of when they hear ‘Linked Data’? Here are a few ideas that come to my mind:

* The [pretty amazing network visualizations](http://linkedjazz.org/network/) (although the Linked Jazz Project, which I link out to, is _so much more_ than a beautiful viz)
* Maybe it just seems to be a buzzword, as some of my cataloging colleagues tend to think (and to be fair, many people are treating Linked Data like the silver bullet for all library data ills right now)
* Perhaps it is the hot topic this year for many conference workshops (and here are two GREAT upcoming conferences that have some linked data overtures: [LITA 2014 Forum](http://www.ala.org/lita/conferences/forum/2014/preconferences), [Code4Lib 2015](http://code4lib.org/conference/2015))
* Or maybe some folks feel fatigue because, dang it, we _just_ got a handle on RDA (again, the catalogers might be feeling fatigued…)

Well, it may or may not be all of those things and more. But here, by way of another list, is what I understand Linked Data to be:

* Linked Data is NOT a single standard or technology. Rather, Linked Data should be seen as a set of best practices for creating a ‘web of data’.
* Web of data? Don’t we already have a web of data encoded in HTML (made pretty with CSS, made interactive with Javascript/JQuery, etc.)? But Linked Data is not about just making data available, but making structured, interoperable data available on the web, so that both humans AND machines (search engines, robots, etc.) can understand it. This make the web a richer experience and allows the Semantic Web - a web of data with meaning - to exist.

To best understand Linked Data, we need to review the oft-quoted Berners Lee tenets (proposed in 2006):
* Use URIs as names for things
* Use HTTP URIs so folks can look up more information on those named things
* Provide useful information following certain standards (RDF, SPARQL) for folks looking up those URIs
* In that information, include further links to other URIs for added discovery

Uh oh. You can tell this is being discussed by someone who works in libraries because of all the acronyms! Here are some common Linked Data acronyms spelled out:

* URL = a uniform resource locator (you probably already know this one)
* URI = uniform resource identifier. Here are some examples:
  * URI that is a also a URL: [http://id.loc.gov/authorities/subjects/sh85101653](http://id.loc.gov/authorities/subjects/sh85101653)
  * URI that is not a URL: urn:isbn:096139210x
* HTTP = Hypertext Transfer Protocol, or a protocol for sharing hypertext (duh), which is structured text that uses links (or hyperlinks). This may all seem obvious, but HTTP is the basis for our web of data and supporting data linking, integration and interoperability.
* RDF = Resource Description Framework. RDF is but one standard data model for exchanging data via the web. RDF expresses everything in triples : subject predicate object. Here are some examples of RDF triples:
  * [http://id.loc.gov/authorities/subjects/sh85101653](http://id.loc.gov/authorities/subjects/sh85101653) [http://www.w3.org/2004/02/skos/core#prefLabel](http://www.w3.org/2004/02/skos/core#prefLabel) “Physics” .
  * the above triple translated: [subject: concept with specific URI - predicate: has preferred label - object: text string of ‘Physics’]
  * [http://id.loc.gov/authorities/subjects/sh85101653](http://id.loc.gov/authorities/subjects/sh85101653) [http://www.loc.gov/mads/rdf/v1#authoritativeLabel](http://www.loc.gov/mads/rdf/v1#authoritativeLabel) “Physics” .
  * the above triple translated: [subject: concept with specific URI - predicate: has authoritative label - object: text string of ‘Physics’]
  * Note that the above examples use URIs and text strings. Your triples could be all strings - but then you are not linking to other data via URIs. Also note that RDF aims to be schema-neutral, but this is not as easy as one would think. Just in the above two examples, we use predicates from two different schema ([MADS‘](http://www.loc.gov/standards/mads/) authoritative label and [SKOS](http://www.w3.org/2004/02/skos/)‘ preferred label) that _could_ be seen as given the same relationship - the primary label for the concept with URI [http://id.loc.gov/authorities/subjects/sh85101653](http://id.loc.gov/authorities/subjects/sh85101653). While RDF intends to be schema-neutral, you need to know what schema are used in your data to best use, model and query that data.
* SPARQL = SPARQL Protocol and RDF Query Language (yes, it really is a recursive acronym)
  * SPARQL is how you query RDF datastores
  * We will see SPARQL in future posts with examples - where I hope it will make more sense.

We already have XML, why use RDF?

* While it is common to want to compare XML with RDF, we need to be clear: RDF is a model (or framework), while XML is a syntax.
* That means that, in fact, we can have the RDF model expressed in the XML syntax, i.e. [RDF/XML](http://www.w3.org/TR/REC-rdf-syntax/).
* But RDF can also be expressed in the serializations [Turtle](http://www.w3.org/TR/turtle/) or [N-Triples](http://www.w3.org/2001/sw/RDFCore/ntriples/).

Back to XML versus RDF - we can, however, compare the two data requisite data models:

* XML models data as a tree, while RDF models data as a graph. The difference in these two models, in my opinion, is best explained with visuals. Below are some simple illustrations of these different data models, with another popular model thrown in.
  * The tabular data model (aka CSV/spreadsheets): ![](/images/tabular_model.png)
  * The tree model (or the markup languages / XML model): ![](/images/tree_model.png)
  * The graph model (or the RDF model): ![](/images/rdf_model.png)
* The RDF data model allows more flexibility and requires less verbosity than XML. However, XML still has benefits, particularly as certain domains remain schema-loyal (i.e. prefer to use MODS, MADS, DC, VRACore). This links back to the earlier triples examples, where predicates from two different schema were used to express roughly equivalent statements. While RDF is flexible because it should be schema-neutral, choice of schema - even just among different libraries - will still guide many to using XML for schema-specific validation work.

Alright, so this has been a quick and dirty introduction to Linked Data. Here are a few statements on the role of Linked Data in libraries, particular for cataloging and metadata folks. I warn you now, these points have become my soapbox:

* Traditionally, online (or, for that matter, offline) library catalogs are data silos - i.e., searchers have to go to the data source to find resources and records; the library data does not go out to the patron. Linked Data practices can help get our online records, data, resources out ‘in the wider world’ and where our patrons are searching - like in search engines in Google. This is where the example/post on microdata will help [to be posted].
* Furthermore, traditionally in MARC catalogs, we do not link out to various authorities that are now available as Linked Open Data (like VIAF or the Library of Congress Authorities) - and by link out, I mean we are not storing the value URIs from those authorities. Instead, we store these ‘authorized access points’ as a string or piece of text that follow the preferred form of the name of the object as designated by the authority. Once this name is entered into the record, without a URI, generally integrated library systems cannot 1. confirm this access point is indeed that person (what if that person’s name changes? What if someone records a similar name but it wasn’t from the authority?) ; 2. pull in updated forms or alternate forms of the name automatically as the authority source changes ; and 3. pull in related information via the authority sources about that name. Some systems now may be able to do some of this, but most cannot. Instead, we hire authority vendors to come in and update these authorized access points as the authority data sources update, then leave them static in between. No links, no additional information, no alternate titles pulled in - again the patron has to go out to the authority to find that information.
* Can Linked Data help in these and other cases? Will it improve discoverability and, in turn, make our metadata and cataloging work more appreciated? I think so, although we have a lot of work to do to get there.

Finally, I should note that some libraries are already using Linked Data. I enjoyed reviewing what others are up to as reported in the results of a Linked Data in Libraries Survey conducted by OCLC Research [here](http://hangingtogether.org/?p=4137). Some quick facts for this survey:

* There were 122 responses, but 26 of the 122 are not currently implementing linked data projects.
* The other 96 responses reported implementing 172 linked data projects or services, including 25 which consume linked data, 4 which publish linked data, and 47 which both consume and publish linked data.
* In these projects, the most used linked data sources included three that we will see in the examples posts:
  * Worldcat.org
  * id.loc.gov
  * VIAF

Thanks for reading through my post, and I hope it was helpful. I hope to start posting some examples of using Linked Data with OpenRefine specifically this weekend, then perhaps other tools and scripts in the near future. Feel free to contact me with any corrections, questions, or feedback - or leave a comment below!
