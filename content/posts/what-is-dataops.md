---
title: 'what is dataops'
date: 2017-11-25T18:52:50-07:00
draft: true
---
I started writing this blog over 3 years ago to share some starting thoughts on Linked Data in my daily library metadata work. Then I wanted to talk about how I could see an evolutionary path for library cataloging departments. Then I fell off the planet.

Now, I'm now revisiting this blog to write up some starting thoughts on my new area of work. This area of work has decidedly split away from, but is influenced by (in an entirely how-do-we-weather-this-storm way), traditional library metadata and especially the Linked Data talking in cultural heritage space. I'm in a new-ish work area that I'm trying to gain traction and definition for in part because of the lack of pragmatic understanding or approaches for how many of our library - or cultural heritage - data technology presumptions, decisions, or incomplete-but-believed-ideas. These ideas are for things like RDF modeling in all "library data", making everything entirely open, sharing our described entities, and expanding our digital repository networks. And these, as many people know, are much, much harder propositions than simply getting an entire domain to agree to a standard, or a standards-mapping, or understand better RDF, or agree to uses for things like Solr indexes.

As more and more institutions or community members talk about things like AI or computational access to "library data", or even just mentioning 'APIs' a lot without fulling understanding what that means, I think what I'm proposing as an emerging work area will just gain traction - or need to, to make these new ideas more readily achievable given our resources, infrastructures, and community technologies.

### Blah blah What is "DataOps"

So my new job title - "DataOps Engineer" reflects what I'm trying to sell to libraries and cultural heritage sectors. For me, "DataOps (data operations)" is a few things:

1.  Kinda made up for cultural heritage, but entirely exists and **is an
    actual thing** in enterprise sectors;
2.  Heavily Influenced by DevOps (I once called DataOps at my place of
    the work the little sister to DevOps, though I'm not sure our lead
    DevOps Engineer appreciated that);
3.  Focused at the intersection of designing, developing, and
    maintaining a data architecture (broadly defined here) that can
    leverage, but is not tied into necessarily, distributed processing,
    big data technologies, and data-forward applications.

So a lot of the DataOps idea in enterprise is tied to Big Data and using distributed architecture and processing frameworks (Hadoop, Spark, serverless architecture, streaming data systems, Kafka, etc. etc.). While I think that is something we need to incorporate better in our library work, I try to follow two tenets in making dataOps a thing:

1.  Try to implement any changes to our data architecture based on
    driving requirements (i.e. don't implement Spark for the sake of
    Spark, it won't win me any support from our engineers and sys
    admins);
2.  Not all of dataOps promise (maybe not even a majority of it) is tied
    to big data processing, but instead attempting an infrastructure and
    system architecture approach that recognizes and tries to build
    flexibility around the complexity of the specifications, semantics
    and OSS agreements we (we here being cultural heritage workers)
    attach to our "entities" and "resources" (and "information"). And
    how that ish is not going to implement itself.

My first pass at a Metadata Operations work scope proposal at my last position.
<https://docs.google.com/document/d/1mUnZkznsR3rMxkKTBcCNBMCwPMJRXUq6mmwNeNhb6zM/edit?usp=sharing>

Currently, I find myself juggling a few work areas tied to this that I will discuss in hopes that it helps clarify the vision I have for this position and role.

### Understanding "Dataflows"

In my current position, we have a relatively complicated digital repository system. I don't mean we have a highly-customized, single Hydra instance; I mean, we have two proto-Hydra (its Rails-y, it uses Active Fedora, it feeds into a Fedora 3 instance of sorts) self-deposit interfaces ; an extension to ActiveFedora that loosely serves as a Ruby gem guiding our data models, functions, behaviors, expectations, etc. ; a robots framework that ultimately grabs the processing state of given resource, understands the workflow it is going through, queues up the next workflow, and kicks off a robot process that is a self-contained process of the resource and the step ; extremely tightly coupled connection between a home-grown preservation solution and our digital repository resources' versioning modeling and editing states ; among many other components, applications, systems, NFS mounts, databases, publication mechanisms, etc. Our system was based originally off a guiding architecture that leverages microservices, but it was implemented over time, by many different developers, following different sets of practices, etc. So it became quite the complex bohemeth (relatively speaking).

This repository system is very intimidating to approach as someone new to my department, especially as someone trying to plan new data models, data pipelines, analytics, transform or load operations, etc. There are no clear ways to know what 'resources' or information is in what part of this system; error reporting is sporadic; few people (some of my colleagues would argue none) understand the *full implementation of this system*.

In trying to untangle this simply so I could start my job, or parts of this at least, I attempted to map dataflows through the various components of this system from very first ingest points to final discovery, preservation, or publication. This in itself was difficult given that there are so many places where a resource in this digital repository system could start and end up; but I made some decisions on focal points and gave it a hard scope of dataflows - "flow through various components, APIs, or persistence layers, of the data making up in part, whole, or other, this digital repository resource \[abstraction of metadata and binaries\] our system manages".

This led to many, many complicated diagrams generated by folks coming together for originally 'side channel' meetings where I ask our engineers to walk me through a component, ask a lot of questions, and we all read the code, VMs, logs, etc. to figure out the implementation. You can see below a guiding diagram that is simply a list of the main codebases in our GitHub Organization that I needed to work through for various groupings of components - i.e. that is far from a complete list of all of the codebases involved in our digital repository, and it doesn't even touch on servers, VMs, databases, or other ops-oriented system components.

![SDR current state overview](https://docs.google.com/drawings/d/e/2PACX-1vS0eWlm7_ETunGzpkX0-_2VnZL1N7NpqfmFivPaUbWWfwfi5TXZeKDEMGxIEDT-XCTq9ylutKlb34Ks/pub?w=960&h=720)

In trying to uncover this 'current state' via dataflows through our digital repository,

### ETL Framework(s)

Data functions not application-specific data

### Streaming Data Architecture, Pipelines, APIs

Re-architecting our digital repository architecture

Logs analysis and extension of 'logs'

Object state management and analytics

### Serverless Data-focused Microservices Development

AWS AWS AWS

Not just running our traditional applications on a EC2 instance

Exploration

### Consistent Data Modeling & Specifications

I do not mean metadata models here in the way that I have been attached
to in previous jobs

I'm not here to specify what *it should be*, but instead, to figure out
what we need and how we can make consistent, complex data structures not
context-specific data structures that vary for each possibly application
or even class.

### [](#Going-Forward "Going Forward"){.headerlink}Going Forward {#Going-Forward}

Struggling with gender presumptions in library technology
