---
title: 'moving beyond authorities accessyyz speaker notes'
date: 2015-09-09T18:52:50-07:00
draft: false
---

Here is a post that details some thoughts and experiences that lead to my short slides and other speaking notes for the Navigating Linked Data panel given at the [Access YYZ conference](http://www.accessconference.ca). This was originally envisioned as a sort of interactive workshop, since I've learned a lot of this by tinkering, so forgive me if it flows a bit awkwardly in place. It was wrapped into a panel to give a better range of projects and approaches, which I'm excited about. All of the following notes were built off of experimentation for particular use cases from a data munger's viewpoint, not a semantic web developer's viewpoint, so any corrections, updates, or additions for future reference or edification are very much welcome.

If you have questions, please let me know - \@cm\_harlow or <cmharlow@gmail.com>.

### Exploding Library Data Authorities: The Theoretical Context

So this post/presentation notes/etc is going to work off the assumption that you know about RDF, that you're down with the idea that library data should move towards a RDF model, and that you understand the benefits of exposing this data on the web.

One area that I'm particularly interested in experimenting with is the concept of Library Authorities in a Linked Open Data world. Not just the concept of Library Authorities, however; but how can we leverage years, nay, decades of structured (to varying quality levels) data that describes concepts we use regularly in library data creation? How do we best do this, without recreating the more complicated parts of Library Authorities in RDF? I would imagine we want to create RDF data and drop many of the structures around Libraries Authorities that make it not sustainable as it exists today. Why then call it authority here? Here is a quote I particularly agree with from Kevin Ford, when he was still working at the Library of Congress, and Ted Fons of OCLC:

> \[Authority the term\] expresses, subtly or not so subtly, the
> opportunities libraries and other cultural organizations have in
> re-asserting credibility on the Web along with providing new means for
> connecting and contextualizing users to content. The word "Authority"
> (along with managing "authoritative" information on People, Places,
> Organizations, etc.) is more valuable and accurate in a larger Web of
> interconnected data. Nevertheless, because a BIBFRAME Authority is not
> conceptually identical to the notion of a traditional library
> authority, the name - Authority - may be confusing and distracting to
> traditional librarians and their developers. -- "On BIBFRAME", section
> 3.6, <http://bibframe.org/documentation/bibframe-authority/>

*Okay, please don't run away now because there is a mention of BIBFRAME. This post is not about BIBFRAME, and I just really like the approach to discussing the term 'Authority' here.*

I continue to use the term Libraries Authority (but always with some sort of air quotes context) though because I'm uncertain what else to call this process I'm about to unravel, to be frank. And because Libraries Authorities carries with it a whole question of infrastructure as well as data creation/use, in my mind. I'd like to see Libraries Authorities in LOD evolve into how we interact with datastores that don't directly describe some sort of binary or physical object. It would explain concepts we want to unite across platforms, systems, projects, and other. It would be curating data not just about physical/digital objects, but concepts, as expressed in the above quote.

In line with RDF best practices for ontology development, LOD Authorities should try to reuse concepts where possible, and where not possible, build relationships between the vocabularies and ontologies that exist to what we're trying to describe locally. I want those relationships to be explicit and documented so we can then use Libraries Authorities data more readily to enhance our records. Though a necessary first step in Libraries Authorities in LOD (or any other, honestly) usage is just getting all our possible controlled access points - WHEREVER THEY MIGHT APPEAR IN LIBRARY DATA - connected to metadata describing concepts, i.e. LOD Authorities, through URI capture of some sort. Hence my really strong interest and a lot of tinkering in the area of library data reconciliation.

I should also mention that a lot of thinking on this was born of the fact that updating many commonly used Library Authorities is difficult if not impossible for many to do. Adding Library of Congress Name Authority Files in particular requires the institution to have the resources to support their employees deal with a huge training bureacracy, whether we are talking about PCC (Program for Cooperative Cataloging) institutional-level membership or NACO funnel cataloging. Training often takes months; then there is a review period that can last years. This has caused Libraries Authorities work, which is very much a public service, to be not really equitable nor sustainable, as many institutions find that navigating that bureacracy is prohibitive.

As I critique the system, this does not mean that I, in any way, devalue the individual effort that goes into Libraries Authority work. Many people donate time and energy to accurately describe concepts that don't relate directly to their work or institution. They realize that good metadata is a public service, and I appreciate that because it is. I'm critiquing here the larger system, not the individuals working therein.

Regardless, back on the agenda, the following describes one way I am attempting to begin expand the concept of Library Authorities for a particular use case at the University of Tennessee Knoxville (UTK) Libraries. I'm hoping that this will then grow into a new way for us to handle Library Authorities, but exploding authorities to become eventually a store of RDF statements for metadata we use to describe concepts, or to negotiate our concepts with external data sources' descriptions of them.

### Exploding Library Data Authorities: The Use Case

At UTK, there is a special unit in the Special Collections department: the Great Smoky Mountains Regional Collection. This deals with representing all kinds of materials and collections that focus on the Great Smoky Mountains in southern Appalachia, southeastern United States. They also try to represent concepts that are important to this region and culture, but weren't represented adequately, or were open to their edits, in the usual Library Authorities and vocabularies. This became Database of the Smokes (DOTS) terms, a very simple list of terms, sort of in taxonomy form, that was primarily used for sorting citations of works and indexing works on the region and culture that goes into [Database of the Smokies](http://dots.lib.utk.edu/).

DOTS is currently just a Drupal plugin working off a database of citations/works with appropriate DOTS index terms applied. Some of these terms were haphazardly applied to non-MARC descriptive metadata records (largely applied when the DOTS project managers were creating the metadata themselves), and these digital objects with DOTS terms assigned do not show up in the DOTS database of works/citations/objects. These terms were not applied to MARC records describing resources that go into the larger Great Smoky Mountains Regional Collection either. The DOTS terms did not have identifiers, nor a hierarchical structure that was machine-parseable. Finally, the DOTS terms often loosely mirrored the preferred access point text string construction to a point, making inconsistent facets for subject terms in the digital collections platform (as LC authorities and DOTS are both used).

What we wanted to do with DOTS was a number of things, including addressing the above:

1.  Get the hierarchy formalized and machine-readable
2.  Assign terms unique identifiers instead of using text-string
    matching as a kind of identifier.
3.  Build out reconciliation services for using the updated DOTS in MARC
    and non-MARC metadata, replete with capturing the valueURI or \$0
    fields as well.
4.  Pull in subject terms used for DOTS resources outside of DOTS (in
    the digital collections platform)
5.  Allow the content specialists to be able to add information about
    these terms - such as local/alternative names, relationships to
    other terms, other such information describing the concept
6.  Allow external datasets, in particular
    [Geonames](http://www.geonames.org/) and [LoC](http://id.loc.gov/),
    to enhance the DOTS terms through explicitly declaring relationships
    between DOTS terms and those datasets.
7.  Look to eventually building this taxonomy to a LOD vocabulary, then
    enhancing the LOD vocabulary into a full-fledged LOD ontology (the
    difference between vocabulary and ontology being that ontology have
    fuller use of formal statements so that accurate reasoning and
    inference based off DOTS can be done; a vocabulary may have some
    formal statements but the reasoning cannot entirely be trusted to
    return accurate results.)
8.  Find ways to then pull the updates to the term description where
    there are relationships to LoC vocabulary terms for seeding updated
    Authority records.
9.  Use this work and experimentation to support further exploding of
    the concept of Library Authorities.

We are focusing on relating DOTS to LoC (primarily LCSH and LCNAF) and Geonames at the moment because they are the vocabularies that have something to offer to DOTS - for the LCSH and LCNAF, they offer a broader context to place the DOTS vocabulary within; for Geonames, it offers hierarchical geographic information and coordinates in consistent encoding and part of the record. Inconsistency in use, coordinates encoding, and where the relationships are declared in the record are the reasons why we did not just rely on the LoC authorities to link out to Geonames, since there has been some matching of the two vocabularies done recently.

### Building a Vocabulary: The Setup

The DOTS taxonomy currently lives as a list of text terms. We first wanted to link those terms to LC Authorities and Geonames. This was done by pulling the terms into LODRefine and using existing reconciliation services for the vocabularies. For the LCNAF terms, a different approach was needed as there is no LODRefine reconciliation service currently - this was recently solved by using [Linked Data Fragments server](http://linkeddatafragments.org/) and [HDT](http://www.rdfhdt.org/) files to create an endpoint through which reconciliation scripts could work. We pulled in the URI and the label from the external vocabularies for the matching term.

We've then taken these terms and the matching URIs and created simple SKOS RDF N-Triples records with basic information included. In short:

1.  DOTS was declared as a skos:ConceptScheme and given some simple SKOS
    properties for name, contact.
2.  all terms were declared as skos:Concepts and skos:inScheme of DOTS.
3.  all terms were given a URI to be made into a URL by the platform
    below.
4.  the external URIs were applied with skos:closeMatch\* then reviewed
    by content specialists for ones that could become skos:exactMatch.
5.  for all labels that end up with an skos:exactMatch to external
    vocabularies, the external vocabularies' labels were brought in as
    skos:altLabel.

A snippet of one example of the SKOS RDF N-triples created:

```
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2000/01/rdf-schema#type> <http://www.w3.org/2004/02/skos/core#Concept> .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#prefLabel> "Tellico"@en .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#inScheme> <http://dots.lib.utk.edu/DOTS> .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#altLabel> "Talequo"@en .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#related> <http://id.loc.gov/authorities/names/no94017139> .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#related> <http://id.loc.gov/authorities/names/n86034608> .
<http://dots.lib.utk.edu/p54274> <http://www.w3.org/2004/02/skos/core#related> <http://sws.geonames.org/4662016/> .
...
```

This SKOS RDF N-triples document was then passed through [Skosify](https://github.com/NatLibFi/Skosify) for improving and 'validating' the SKOS document. Next, it was loaded into a [Jena Fuseki](http://jena.apache.org/documentation/serving_data/) SPARQL server and triple store, for then being access and used in [Skosmos](http://skosmos.org/), "a web-based tool providing services for accessing controlled vocabularies, which are used by indexers describing documents and searchers looking for suitable keywords. Vocabularies are accessed via SPARQL endpoints containing SKOS vocabularies." (<https://github.com/NatLibFi/Skosmos/wiki)>. Skosmos, developed by the National Library of Finland, is open source and built originally for Finto, the Linked Open Data Vocabulary service used by government agencies in Finland. It means to help support interoperability of SKOS vocabularies, as well as allow editing.

We've got a local instance of Skosmos with the basic DOTS SKOS vocabulary in it, used primarily as a proof of concept for the content specialists. Our DOTS Skosmos test instance supports browsing and using the vocabulary, but not editing currently. I'm hoping we can use a simple form to connect to the SPARQL server (as many existing RDF vocabulary and ontology editors are too complicated for this use), but this has been a lower priority than working on a general MODS editor first. There is the ability to visualize relationships in Skosmos that supports the content specialists really understanding how SKOS structure can help better define their work and discovery.

With the SKOS document, both MARC and non-MARC data with subject terms can now be reconciled and the URI captured either though OpenRefine reconciliation services, or some reconciliation with scripts. This has already helped clean up so much metadata related to this collection. We hope to start using the SPARQL endpoint directly for this reconciliation work.

### DOTS SKOS Feedback

This work has inspired the DOTS librarians to want to expand a lot of the kind of 'Library Authority' information captured, and the inclusion of other schema/systems for additional classes and properties to support other types of information. This included everything from hiking trail lengths to the cemetery where a person is buried. In the above N-triples snippet example, a particularly strong use case is put forward: extending the Libraries Authorities record, so to speak, to better cover Cherokee concepts. 'Tellico' was a Cherokee town that was been partially replaced by the current U.S. town of Tellico Plains, as well as the site of many Tellico archaelogical digs. The LCNAF has authority records for the latter two concepts, but not the first - not the Cherokee town. What would happen with automated reconciliation is that Tellico was often linked to/overwritten by Tellico Plains (or other Tellico towns currently existing in the U.S.). We are building out DOTS and, we hope, other negotiation layers for Libraries Authorities being migrated to RDF then extended in a way that will not erase concepts like Tellico that don't exist in the authority file. This is also an important motivation for extending many researcher identity management systems to work between the metadata that wants to link to a particular person and the authority file that may or may not have a record for that person. In moving from, in the case of the LoC vocabularies, relying on unique text-string matching to identifiers, we have moved from a sort of but not entirely (but kind of) closed world assumption to an open world assumption. So identifiers are not just pulled in as preparation for RDF.

Additionally, having this local vocabulary better connected in our local data systems to the external authorities has started many discussions about how we can create a new way of updating or expanding external authorities. UTK is not a PCC institution, but we do have 1 cataloger able to create NACO records for the Tennessee NACO funnel. This work still needs to be reviewed by a number of parties and follow the RDA standard for creating MARC Authorities, and it is limited by the amount of work we need to do beyond NACO work. So there is not much time at the present for this cataloger to spend on updating and/or creating records that relate in some way to DOTS terms. This should not mean that 1. the terms of regional interest to UTK will continue to not be adequately described in Library Authorities, and 2. we continue to keep out the content specialists from updating this metadata (though in a negotiated or moderated way, i.e. some kind of ingest form that can handle the data encoding and formation in the back end).

Another question that this project has brought up is where to keep this RDF metadata that we are using currently to negotiate with and extend external Library Authorities. The idea of keeping as a separate store from say descriptive metadata attached to objects has been mostly accepted as a default, but this doesn't mean that storing concepts say in a Fedora instance as objects without binaries, then use Fedora to build the relationships, is not worth investigating. And, additionally, do we want to pull in the full external datasets as well? It is definitely possible with many LOD Library Authorities available as data dumps. I think at this moment, I would like to see this vocabulary and other vocabularies continue to expand in Fuseki and Skosmos, with an eye to making this work in some ways like [VIVO](http://vivoweb.org/) has done for negotiating multiples datasources in describing researchers.

### DOTS SKOS Next Steps

In going forward, we would like to:

-   Expand the editing capabilities of DOTS SKOS so that the content
    specialists can more readily and directly do this work.
-   Enhance the hierarchical relationships that we can now support with
    SKOS. This will mostly involve a lot of manual review that can be
    done once we've got an actual editor in place.
-   Review beyond SKOS for properties that can support extending the
    descriptions.
-   Discuss pulling in full external datasets for better relationship
    building and querying locally, which is somewhat described above.
-   See if this is really is part of evolution to store Library
    Authorities & further concept descriptions not directly related to a
    physical/digital object in local data ecosystem, and how.

### Resources + References

Here are some links to tools, projects, or resources that we found helpful in working on this project. They are in alphabetical order:

-   [Current Database of the Smokies](http://dots.lib.utk.edu/)
-   [Geonames](http://www.geonames.org/)
-   [HDT](http://www.rdfhdt.org/)
-   [id.loc.gov](http://id.loc.gov/)
-   [Linked Data Fragments](http://linkeddatafragments.org/)
-   [Jena Fuseki](http://jena.apache.org/documentation/serving_data/)
-   [LODRefine](https://github.com/sparkica/LODRefine)
-   [Prefix.cc](http://prefix.cc/)
-   [Semantic Web for the Working
    Ontologist](http://workingontologist.org/)
-   [SKOS Primer](http://www.w3.org/TR/skos-primer/)
-   [Skosify](https://github.com/NatLibFi/Skosify)
-   [Skosmos](http://skosmos.org/)
-   [VIVO](http://vivoweb.org/)
