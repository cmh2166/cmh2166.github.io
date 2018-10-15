---
title: 'openrefine reconciliation workshop c4lmdc'
date: 2015-08-12T18:52:50-07:00
draft: false
---

Here are my slides, handouts, and other information from a OpenRefine Reconciliation Workshop given at the Code4Lib Maryland, DC, and Commonwealth of Virginia 2015 meeting. I want to make them available for others who may be interested in the topic, or those who attended (since this is a lot of information to cover in one workshop). It was built off of experimentation for particular use cases from a data munger's viewpoint, not a developer's viewpoint, so any corrections, updates, or additions are very much welcome.

If you have questions, please let me know - \@cm\_harlow or <cmharlow@gmail.com>.

Folks should feel free to repurpose this work for other such OpenRefine Reconciliation Service workshops/events/whatever.

Links to the slides (available as HTML file or PDF doc), breakout session handouts/guides, and some sample data: [GitHub Repo for Workshop](http://www.github.com/cmh2166/c4lMDCpres)

## Draft of My Workshop Talking

I've included my rough speaker's notes below, following each slide.

### About this workshop

I'm Christina Harlow, \@cm\_harlow on twitter and a data munger in Tennessee.

This workshop came together rather late in the day, so there were no installation requirements or preliminary guidance from you on where the focus should be. As such, I want to get a feeling for what you want to hear. Raise your hand for the following questions:

1.  How many people are here with laptops with OpenRefine running?
2.  How many people are here with laptops on which they want to install OpenRefine?
3.  How many people are here just to learn by watching, discussion?

Different focus:

1.  How many people here want to learn how to use OpenRefine generally?
2.  How many people here want to learn specifically how to *use* reconciliation services?
3.  How many people here want to learn how to *build* reconciliation services?

Depending on this, I may/may not cover more of the general OpenRefine functions or more on how OpenRefine handles reconciliation objects.

Finally, what is presented comes from the perspective of a data munger, namely myself, who was building out OpenRefine as part of a metadata remediation workflow involving non-technical metadata workers. I'm not a developer. I'm not a programmer. I'm guessing at how this works in a lot of places, and why it works that way, based off of breaking it a lot. Basically...

### Left-sharking it

I'm left-sharking it. Please shout out if you understand something that I don't. I should also mention that this work builds off of some really awesome and talented library tech folks, including...

### LibTech Developers to Thank

these 4 people, a few of which are hear today. Thanks to them is first and foremost for experimenting, building, sharing and documenting their work. I built off of what they did. Their Twitter handles are there, so go ahead and tweet at them to tell them how awesome they are.

### Slides, Examples, + Install

Everything from today's presentation - slides, sample data, example workflows - should be in this GitHub repository - <http://github.com/cmh2166/c4lMDCpres>. Go ahead and clone/download it on your computer for following along with some of the examples and using the sample data if needed.

If you don't have OpenRefine running but would like to play with it either while I'm going through examples or during the more interactive sessions at the end, go ahead to this site - <http://openrefine.org/download.html> - and download the installer for your OS. Note: OpenRefine requires certain versions of java packages (detailed on openrefine.org/downloads page). Downloading these specific Java packages if you don't have them or have to upgrade will be what takes the longest. If you download OpenRefine now and run into an install error, check your java version then prepare for the long wait for the update to download. Also interrupt me to ask for help.

Additionally, we will briefly cover the DERI RDF Extension options for reconciliation. If you are interested in working with that, you'll need to go to the DERI RDF Extension site to download it, then put that code into the extensions subdirectory in the OpenRefine files (wherever you are keeping the OpenRefine application files on your computer, go there and find the extensions directory. Depending on version/OS, it may be in a lib subdirectory).

Nota bene: the DERI RDF Extension, and LODRefine (or Linked Open Data Refine), which is a fork of OpenRefine with this and other helpfuls extensions baked in, are very helpful but no longer actively supported. Just be aware of this and that some issues you might run into will not necessarily have anyone there who can answer or update the codebase.

### Agenda

With modifications from what I asked you at the beginning, this is the rough agenda. We will start with a very brief introduction, more an overview, to OpenRefine, then review 3 different OpenRefine reconciliation options, with the bulk of the time focused on working with and building or modifying the Standard Reconciliation Service API. I'm reviewing 3 options because they all have valid use cases, as well as considerations or issues to keep in mind. They also all required different levels of expertise and can help you get a feel for the data sources you're working with.

Then I'm proposing we have breakout sessions, where folks can work through what was shown. If people want to learn more about other OpenRefine functions, we can have a breakout that is just about working with library metadata in OpenRefine, and I'm happy to guide that. In the GitHub repository, I've included files that have some procedures to walk through for both working with reconciliation services as well as a marked up python file explaining how an example Standard Reconciliation Service API was built.

### Quick Intro to OpenRefine

OpenRefine is a power tool for cleaning up data. It has gained a lot of popularity as of late for library data work, as it offers a nice GUI for doing data normalization, enhancement, review and cleanup. It has gone through a number of iterations in the past few years, including management by Google - I mention this because you will still see GoogleRefine instances running and still working, as well as some other Google naming holdovers. Since 2012, it has been an open source and community supported project, with a website at openrefine.org and a pretty active github repository at github.com/OpenRefine/Openrefine. Because it is community-sourced now, the documentation is somewhat haphazard; parts are very good, other parts, lacking or out-of-date. The documentation on the standard reconciliation service API is out-of-date, and although somewhat helpful, read it with a grain of salt.

More about the technology, OpenRefine is built with java and jetty, a server applet that handles HTTP requests between the backend of the program and the javascript/jquery-based UI that runs in a web browser. While it does act like a web service, it is not on the Internet; it runs locally, and the projects and data worked within OpenRefine are stored locally. This structure is how OpenRefine can handle the reconciliation options we'll be discussing below, all reliant on using HTTP requests to either talk to the external data sources or an intermediary API we create.

The GUI in the web browser is predominately javascript and jquery, while the backend (like I mentioned above) is java. There is no database; all the data you are working with is stored in memory. This means that with larger or really complex datasets, depending on your computer, you will run into performance issues. I generally start to see work-stopping performance issues working with \~50,000 or more 'standard complexity' MODS records. Your mileage may vary. There is a way to allocate more memory for the work at startup - check the OpenRefine GitHub wiki docs for doing this in your particular OS.

### OpenRefine Functionality Checklist

So most of the people at the workshop declared that they had worked with OpenRefine before and were primarily interested in learning about building the standard reconciliation service API. As such, I'm going to breeze through this part.

OpenRefine has a lot of utility for data cleanup and munging, and these are the core functionalities you'll hear about and/or use.

For the Import/Export options, OpenRefine out of the box supports working with a number of data formats - CSV, Excel, JSON, XML, RDF/XML, other... for import. For export, there is support for exporting a full OpenRefine project, CSV, or possibly JSON/XML with the templating option (this requires some wrangling and is not always possible for the data you're working with to be faithfully represented upon export to that format). Some extensions, such as the DERI/RDF extension, so allow export to RDF serializations - just RDF/XML and RDF N-triples at the moment - with you setting up a RDF skeleton for how to map the records to nodes.

Yet, keep in mind that all of this requires that you transform the original data to a tabular model for working with it as an OpenRefine project, then eventually getting that tabular model data back to or transformed to the data model you want. This is easy if working with CSV, Excel, or other files already in that tabular format. Working with JSON or XML, however, you'll need to do some data massaging to make OpenRefine work efficiently for you. For heavily-nested JSON or XML, this can be a problem, and OpenRefine may not always be the best option. There are some handy tools, packages, script libraries, and other for moving between data models, and I'm happy to talk about these with you afterwards. For now, the sample datasets in the GitHub repo are already flattened and ready for easy import into OpenRefine.  

Cleans - OpenRefine allows a good UI for cleaning, normalizing and updating - either in batch or manually, though batch is better support - your data.

Facets - these are very helpful for both finding data value outliers, again, normalizing values, as well as just getting a handle of what is the state of your data.

Clusters - when faceting, you can then access a number of grouping algorithms for clustering the values. This can help you normalize data, again, and see what values probably should have the same label/datapoint.

GREL - Google Refine Extension Language. This is a sort of Javascript-y, application-specific language for performing normalization and data munging work in OpenRefine. Some GREL functions can be very powerful for data cleanup, and it is recommended you check out the OpenRefine GitHub repository, which covers GREL very well.

Extensions - these are written by folks who want to add a functionality to OpenRefine, such as with the DERI researchers and the DERI RDF Extension. The OpenRefine documentation has somewhat a list of what is avaiable, though most you will need to do further searching.

If folks want to learn more about just working with library data in OpenRefine, that can be a breakout, or you can ask me directly at some other point.

### OpenRefine Reconciliation

So what do I mean when I say 'reconciliation': I want to take values in my dataset in OpenRefine, compare them with data values in an external dataset, and if they are a match (decided through a number of ways and algorithms, matching functions, etc.), I then can change my data value to be the same as the external data value, or link the two (perhaps by just pulling in a URI from the external dataset), or possibly just pull in extra information about the external data value into my project.

### OpenRefine Reconciliation Options

There are at least 3 ways to perform reconciliation work in OpenRefine. You can...

1.  Add a column by fetching URL... This is an option within the UI of
    an OpenRefine project that generates a HTTP GET request to an
    external data API and then posts the entire data response from that
    external API in a new column in your project. This method does take
    a really long time to execute, and it does require that you parse
    the data response for what you want to pull out in the OpenRefine
    UI. It does have some uses cases, however.
2.  There is the Standard Reconciliation API... which is a RESTful API
    built to negotiate between OpenRefine and external data. While there
    are examples and templates for building these APIs, it does require
    tinkering knowledge of API construction and the related programming
    language you'll be working with. These can be hosted for easier use
    in OpenRefine, or run locally for faster work.
3.  DERI RDF Extension... while this is no longer actively supported, it
    builds off of the Standard Recon Service API to work with RDF
    documents and SPARQL Endpoints in a similar way to the standard
    reconciliation API. For this to work, though, the RDF document you
    are reconciling is held in memory, meaning that the size of the RDF
    data will be limited to what OpenRefine can actually handle.
    Additionally, the SPARQL reconciliation is very much dependent
    currently on the SPARQL server details - for example, the Getty
    SPARQL endpoint is currently not able to work with this extension.

### Add a column by fetching URL...

This option accesses an external data set by building an external data API query for each cell value in a chosen column in your OpenRefine projects. OpenRefine then issues a HTTP Get request to that URL/API Query, and stores the full result in a new column beside the seed column. This mostly works with RESTful APIs, as you cannot change or add to the HTTP request information beyond generating an API query as a URL. It also takes a long time to perform as there is an individual API call made for each cell with a value in the seed column.

To use this method, find the column that contains the data you wish to query the external data API with, go to Top arrow \> Edit column \> Add column by fetching URLs. In the box that appears, you want to enter the appropriate GREL function(s) to create the API query URL with the cell values. Once you've got that constructed, click on 'Add column', and wait as the calls issues/responses stored. This can take a while. Once it is complete, you'll have a new column with the response data, and you can use GREL on that column to parse the results for what you want to find.

This method is useful if:

1.  You have data with very specific links or references to an external
    data API (such as a column of identifiers), and you want to pull in
    additional information using that.
2.  Don't have the time or comfort (yet) for constructing your own
    standard reconciliation API.
3.  Want to get a better feeling for how a data API works with your
    dataset as queries.

To work with this method further in the break-outs, you can follow the
procedures documented in the GitHub repo file 'addcolumnexamples.md' or
you can review the Mountain West Digital Library, a DPLA service hub,
workflows working with this method and using the Geonames API. I'll warn
you about the sample workflows included in the GitHub repo, they are
based off legacy documentation that I wrote up for my last job when
considering OpenRefine for reconciliation work. They are more of a
proof-of-concept and the use cases modeled in there can be better
handled elsewhere now. But they do walk you through this process.

### Standard Recon Service API

The Standard Reconciliation Service API in OpenRefine takes the data you
wish to reconcile from the OpenRefine UI, uses that to query an external
data set (by either connecting to that external dataset's API or
constructing another way to connect in your recon service API), performs
auto-matching, ranking, and other work according to your specifications,
then generates an OpenRefine reconciliation object, with required
metadata, to return to the OpenRefine UI. This reconciliation object is
used in the UI to create reconciliation options for each value, as well
as offer to the OpenRefine user the ability to pull out the
reconciliation IDs, names/labels, or other match factors (matched or
not, ranking values).

Another way to explain it is that the OpenRefine standard reconciliation
API is a HTTP-based RESTful JSON-formatted API that connects the
OpenRefine project to external datasets. It negotiates HTTP POST and GET
requests between those. This API can be constructed in a number of
languages and frameworks, though I primarily see use of python and the
flask 'micro'framework in the examples and templates I work with.

This reconciliation service API work is originally built off of the
Freebase extension in OpenRefine - and this Freebase extension no longer
works. Because of this, however, there is a lot of Freebase-specific
decisions in how we construct this API to work with OpenRefine, which
we'll discuss as we go over the parts.

### Standard Reconciliation Service API Parts

When building an OpenRefine standard reconciliation service API, you'll
need to have a way:

-   a reconciliation service endpoint -- the URL that can handle HTTP
    GET from OpenRefine asking for information or data like the recon
    service information/metadata, can send HTTP GET requests to an
    external data source to get matches, can handle HTTP POST requests
    from that external data source or from OpenRefine with the original
    values to be reconciled, and sending a HTTP POST requests back to
    OpenRefine with the reconciliation objects.
-   standard reconciliation service metadata: this defines how a
    reconciliation service API works in OpenRefine, as well as adding
    additional UI functionalities like preview boxes and further
    searching of an external data source in the OpenRefine UI itself.
-   entity types. This is a holderover from Freebase, which also
    required things like that each reconciliation service only work in a
    chosen namespace (many of the recon service APIs you'll see don't
    have a relevant namespace). You can however use the entity types for
    other purposes, like defining how to use the same reconciliation
    service but choose between external data API search indices, or to
    choose in a reconciliation service what rdf:property to find in the
    DERI RDF extension reconciliation work.
-   query/response handling: this is where you can construct the
    external data query, as well as decide what parts of the external
    data response should become then the id and name for the
    reconciliation object returned to OpenRefine. How this is performed
    is heavily dependent on the language you decide to construct your
    recon service API in, as well as the external data service you're
    querying.
-   Other bells and whistles that'll we will discuss in looking at an
    example.

### Recon Service API Metadata

On the slide is a quote taken from the OpenRefine GitHub wiki
documentation on the standard reconciliation service API. This lets us
know that for each standard recon API we build, we are required to put
in some basic service metadata. When setting a recon service up in the
OpenRefine UI, OpenRefine will immediately send a request to the service
API's endpoint and expect back a JSON object with 'name',
'identifierSpace', and 'schemaSpace'. Again, these are largely based off
of how this worked for Freebase, and often just putting something that
serves your own data usecase is the best option.

There are some other service metadata options that can be used,
including metadata defining a preview window for a reconciliation object
in OpenRefine, as well as building a way to query in OpenRefine that
external data source. These are seen in the following example of a
standard recon service API metadata. Note this example shows all the
possible metadata options; the FAST reconciliation service, which we
will check out in a minute, has just the minimum to get it up and
running.

### API Metadata Example Part 1

In this section you can see the required service metadata fields: the
name, which will appear in the OpenRefine UI, the identifier space, and
the schema space. The view is what builds the URL/URI for looking at the
reconciliation match in the external data source's system. The preview
array defines a pop out box in OpenRefine UI for seeing the
reconciliation match in the external data source's interface.

### API Metadata Example Part 2

In this section (separated just for slide space reasons), you can see
the suggest array, which constructs a search further option within the
OpenRefine UI but querying the external data source. Note that this
search further option does required a type-ahead style search, as well
as that the external data source has a API to query (or that you
construct that in your API).

Finally, the defaultTypes are for the entity types options discussed
above, and will be defined in the recon service API used as an example
below.

### Query JSON Example

When OpenRefine is talking to the standard reconciliation service you
build, here is a simple example of the JSON query object sent to the
recon service API. This just has the query, pulled from the cell value,
a limit for how many matches can be returned (this can be defined
further in the service API), the entity type to query (here modeled to
be the different search indices possible for the external data query),
and type\_strict field that I'm not honestly sure what it does as the
external data API does the matching and ranking for us.

### Reconciled JSON Example

And here is an example of the JSON reconciliation object returned from
the API to OpenRefine post-reconciliation queries. This object has a
result array with the required metadata of id, name, type, and match.
The id and name are parsed from the external data response in our recon
service API, the type is determined according to the original JSON query
object sent to the recon service API by OpenRefine, and the match field
is determined by us in the reconciliation service API (we can decide to
match high-confidence results).

Note that OpenRefine sends multiple queries and can receive multiple
responses to/from the recon service API; I believe this still works in
batches of 10, which is why this method is far more efficient than other
methods of reconciliation.

### Entity Types

The entity types for reconciliation are usually determined by the
external dataset you are working with, as this is a Freebase holdover.
The example we will look like has used the entity type options to define
queries to different indices in the FAST API. You can only select one
entity type per reconciliation work on a column, however, you can rerun
reconciliation work on a column multiple times.

Let's take some time now to run through an example of using a
Reconciliation Service in the OpenRefine interface, to get a feeling for
how this works for the end user.

\[see this workshop's GitHub repo docs for a walk through of using a
Recon Service\]

### Standard Reconciliation Service API Templates

Alright, if just getting a handle on how OpenRefine handles
reconciliation and talking with an external API is enough to take on,
some great developers have helped with the API construction part. There
are some standard OpenRefine reconciliation service API templates you
can use to then plug in information about a particular data API then
test. This lets you get a recon service API up and running with limited
time, OpenRefine, and programming knowledge.

Most of the templates/examples use python with flask, which is what we
will be reviewing. There are examples of python with django, as well as
PHP, if you're interested I can point those out to you.

### Standard Reconciliation Service Examples

All of the following links are given in the GitHub repo's
reconServiceAPI.md file.

-   The FAST Reconciliation Service, using python and flask and not
    hosted (so you will have to run the API application locally before
    querying in OpenRefine). This queries data using the OCLC FAST API,
    with options for querying different indices. It does also use a
    text.py file for normalizing the OpenRefine queries before sending
    them off to the OCLC FAST API; this is not required for building an
    OpenRefine reconciliation service API, but can be very, very helpful
    in boosting matches.
    -   If you look at the GitHub repository for this workshop, you'll
        see 'pythonflaskmarkedupexample.md'. This is the heart of this
        reconciliation service, but with my extended comments explaining
        what is happening. I'll walk through this here and also with
        folks who need help in break outs.
-   LCNAF - VIAF Reconciliation Service - this has both hosted and
    non-hosted options for reconciliation with the LCNAF through the use
    of the OCLC VIAF API (note that means this can only match to a
    subset of the LCNAF, as VIAF just contains personal names). This I
    *think* uses django, but it is definitely constructed in python and
    work exploring.
-   VIAF Reconciliation Service Example - this reconciliation service
    API is currently hosted, and you'd need to do some work to run it
    locally. It is built in PHP, and for the link in the examples file,
    it is for the whole site, a part of which is this hosted
    reconciliation service. Look for the viaf.php file in that example
    link to see how it is built.
-   Linked in the reconServiceAPI.md file are also a basic and an
    extended python/flask template for an OpenRefine standard recon
    service API.

The marked up FAST recon service API example is built off of the basic
python/flask template linked to.

Now to walk through pythonflaskmarkedupexample.md and take questions.

(just see the file for the comments)

### DERI RDF Extension Recon

This again can be used to reconcile against SPARQL Endpoints and RDF
data. While it is no longer actively supported, and it has the issues I
outlined previously, it can still be very helpful. For one example, I've
generated a very simple RDF document for the subset of LC Genre Terms we
use at my job for our form terms. I can then reconcile our form metadata
against that RDF doc and normalize, pull in URIs, and link.

I'm not going to discuss this extensively, but if you want to work on
this in a breakout, I've included a file in the workshop GitHub
repository that walks through using the extension to reconcile against a
RDF document downloaded from the Library of Congress Linked Data
Service. I would recommend that you take a look at the refine.deri.ie
documentation, which is good but brief and not library data-specific.

### Breakouts

So now, you guys can work on whatever was discussed or something
completely different with my help. There is some starter documentation
in the GitHub repo, but a lot of how I've built these recon services and
service workflows in the past has been a lot of trial and error. We'll
pull together towards the end to take questions.

### Links + Contact

Some helpful links for this work, as well as my Twitter handle. Please
hit me up with questions or improvements to this workshop.
