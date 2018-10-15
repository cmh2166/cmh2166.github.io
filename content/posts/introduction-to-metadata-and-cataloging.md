---
title: 'introduction to metadata and cataloging'
date: 2014-11-30T18:52:50-07:00
draft: false
---

The following notes were created for 1 class of Library Science 653, taught by Dr. Starr Hoffman for Pratt Institute School of Library and Information Science. I had a wonderful time visiting with the class and talking through some high-level issues with cataloging and metadata as well as going through a record in detail. The original notes shared with the class are available at this Google Document: <http://bit.ly/lis653>

In this blog post, I have cleaned up those notes a bit as well as created some screencasts reviewing some of what we covered as well as what we did not have time to cover in depth. I hope that they help.

--------------------------------------------------------------

## Metadata Introduction Class

*Original Notes at <http://bit.ly/lis653>*

*Prepared for Starr Hoffman's class*

*Christina Harlow, <cmharlow@gmail.com>, \@cm_harlow*

**Goal:** I hope to guide students through the creation of a MARC record, a MODS record, and a Dublin Core record (in the class' Omeka interface). Time permitting, we will then explore options for transforming records from one schema/format to different schema/formats (crosswalking and XSLT transformations).

**Introductory Screencast (quick review and updates on some of what we discussed in class):**

[lis653](http://vimeo.com/108115303) from [Christina Harlow](http://vimeo.com/cmh2166) on [Vimeo](https://vimeo.com/).

Last week, Dr. Hoffman give you an introduction to metadata. Building upon that, I will just discuss a few key points before we dive into examining a few records and how they were created.

We are focusing today on **descriptive metadata**. This is exactly what it sounds like - data that describes the object in hand (or on your screen). It gives content and context for that object. Descriptive metadata is created or updated using a suite of standards and guidelines:

- **Record Format**: Specific encodings for a set of elements (or a record). Examples include MARC, METS, Dublin Core...
- **Schema**: System of recording and structuring descriptive information for use in a record. A metadata schema creates and defines elements and any rules governing their use. Examples include MODS, Dublin Core...
- **Model**: High-level (or more generalized) approach to object description. Data models define the entities of description and their relationship to one another. Models can be very domain-specific. Examples includes FRBR, RDF...
- **Encoding**: Conversion of metadata into a definable syntax or coded form. Examples include XML (eXtensible Mark-up Language), RDF NTriples...
- **Vocabularies**: Usually domain-specific lists of allowable values for certain elements. Classification schemes are often connected to a chosen vocabulary. Examples include LCSH, Getty AAT, LCNAF, VIAF...
- **Content Standards**: Guidelines on the creation of data for certain elements, often defining what the primary source of a particular element should be. Examples include RDA
- Other...

Metadata is what enables people to find a resource - it is the backbone of the library catalog and discovery interface. One of the main difficulties is both providing metadata guidelines that can be specific to the needs of particular domains or collections, while also making the metadata interoperable in a larger context. I will explain some of my job and work by my colleagues to highlight this point.

- [Columbia's Blacklight Discovery Interface](http://libraries.columbia.edu/ "Columbia Libraries"):
    - Note the 'bento box' model, where each box shows results from a different datastore.
    - This means the search is sent out in such a way as to query each datastore specific to their metadata standards and context.
- [Columbia's Catalog, CLIO](http://clio.columbia.edu/ "CLIO"):
    - This is the traditional, MARC-based library catalog of the Columbia University Libraries.
    - Note the data model(s) - in CLIO, each record is generally for a *manifestation* (a particular publication, for example), not a *work*. This is not always the case, but is the guiding principle most of the time.
- [Columbia's Institutional Repository, Academic Commons](http://academiccommons.columbia.edu/ "Academic Commons"):
    - The items in Academic Commons have MODS metadata for each descriptive record.
    - In the screencast I show you the current ingest form for creating metadata to go into Academic Commons.
- [Columbia's Online Exhibitions:](http://libraries.columbia.edu/find/online-exhibitions.html "Columbia's Online Exhibitions")
    - We often (but not always) use Omeka for our online exhibitions.
    - However, unlike with your class' Omeka instance, our particular Omeka installation has been modified to allow MODS metadata, instead of Dublin Core, which comes out of the box with Omeka.
- Other:
    - E-resources often have metadata provided from vendors like Serials Solutions.
    - Preservation metadata specific to digitization projects can use local or other schema.
    - Other projects use the schema and format that best fits the discovery interface that they prefer, like how our Human Rights Web Archive uses MARC records.

In my experience, a main difference between 'cataloging' and 'metadata' is that cataloging has firmly established guidelines and tools, while metadata (here meaning non-MARC metadata) does not have a confirmed set of tools and workflows. This will be shown in the examples in the screencast.

## MARC (MAchine Readable Cataloging) Bibliographic Records

**MARC Screencast (doesn't recreate classwork, but does show OCLC Connexion Web Interface and method for copy cataloging):**

[lis653\_MARC](http://vimeo.com/108124687) from [Christina Harlow](http://vimeo.com/cmh2166) on [Vimeo](https://vimeo.com/).

#### References:

-   MARC Official Page Bibliographic Fields: <http://www.loc.gov/marc/bibliographic/ecbdhome.html>
-   RDA Toolkit: <http://access.rdatoolkit.org/> (need subscription)
-   Library of Congress Authorities: <http://authorities.loc.gov/>
-   PCC Guidelines: <http://www.loc.gov/aba/pcc/bibco/index.html>
-   Classification Web: <http://classificationweb.net/> (need subscription)

#### Tools

-   OCLC Connexion: <http://connexion.oclc.org> (need subscription)
-   Many Integrated Library Systems (or ILS) have a cataloging module as well

#### Example

We went through creating a sample record for this book in class. I recreate this record in the screencast but using the OCLC Connexion online interface. I include a copy of this record below.

**Even though we accessed the item as an e-book, we cataloged it instead as if a physical book. To create a record for the electronic version would have require a bit more complicated MARC record.**

Book used in class: [Queen of Soweto](https://ia801405.us.archive.org/7/items/queen-of-soweto/queen-of-soweto.pdf "Queen of Soweto (PDF)"), found through the wonderful e-books platform [Unglue.it](https://unglue.it/ "Unglue.it").

#### Take-aways

-   Depending on your system, a lot of the MARC record can be generated (and validated) for you by the software.
-   Also, depending on where you work, a lot of the time you will be modifying existing records (i.e. **copy cataloging**), not creating the records from scratch (i.e. **original cataloging**).
-   Note that traditional cataloging still very much follows a manifestation-focused record model. In other words, these records describe a particular manifestation of a work. There are people who have 'FRBR-ized' their catalogs, however, where each record focuses on a work with manifestations hang off of it. Check out this catalog for one such example: <http://catalog.perseus.org/>

## MODS Bibliographic Record

**MODS Screencast: Shows creation of MODS record in Hypatia (a metadata ingest tool at Columbia), then the MODS XML record in Oxygen XML Editor. Briefly discussion XML schema namespaces and transformations.**

[lis653\_MODS](http://vimeo.com/108124458) from [Christina Harlow](http://vimeo.com/cmh2166) on [Vimeo](https://vimeo.com/).

#### References 1

-   Library of Congress Official MODS Documentation: <http://www.loc.gov/standards/mods/>
-   Library of Congress Linked Data Authorities: <http://id.loc.gov/>
-   MODS schema: <http://www.loc.gov/standards/mods/v3/mods-3-5.xsd>

#### Tools 1

The tools used can really vary from project to project and institution to institution, depending on the involvement of developers and the needs of the metadata team. I show in the screencast a few ways we can create or modify MODS records at Columbia, then an example of a MODS/XML record.

-   Example of Hypatia (Hydra-head ingest tool): <http://hypatia.cul.columbia.edu> (requires login)
-   Oxygen XML Editor (there are many XML editors, but this one is well known and works well for our purposes).
-   OpenRefine (<http://www.openrefine.org/>) is used a lot to clean up data in a flat format (i.e. spreadsheet), then apply a transformation to make it into MODS

#### Example 1

MODS can be used to describe a whole work (like the book above) or a part of a work (like just one page from a book). Note that, while technically MARC records can do the same, they generally do not. MODS records, however, are often used in both situations (and others).

In the screencast I show how to create a MODS record for an article. Then I take the MODS/XML of an article already in Academic Commons and put it in Oxygen to show how the MODS validation works. Then I show how XSLT can be used to transform a collection of records. The MODS/XML record we created/used (<http://academiccommons.columbia.edu/catalog/ac%3A173687>) appears below:

```xml
<mods xmlns="http://www.loc.gov/mods/v3" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.loc.gov/mods/v3 http://www.loc.gov/standards/mods/v3/mods-3-4.xsd">  
 <titleInfo>  
   <title>Punk as a Zine Library</title>  
 </titleInfo>  
 <name type="personal" ID="jf2156">  
    <namePart type="family">Freedman</namePart>  
    <namePart type="given">Jenna</namePart>  
    <role>  
       <roleTerm type="text">author</roleTerm>  
    </role>  
    <affiliation>Barnard College. Barnard Library and Academic Information Services</affiliation>  
 </name>  
    <name type="corporate">  
    <namePart>Barnard College. Barnard Library and Academic Information Services</namePart>  
    <role>  
       <roleTerm type="text">originator</roleTerm>  
    </role>  
 </name>  
 <extension xmlns:rioxxterms="http://docs.rioxx.net/schema/v1.0/rioxxterms/" xmlns="http://www.loc.gov/mods/v3" xsi:schemaLocation="http://docs.rioxx.net/schema/v1.0/rioxxterms/ http://docs.rioxx.net/schema/v1.0/rioxxterms.xsd"></extension>  
 <typeOfResource>text</typeOfResource>  
 <genre>Articles</genre>  
 <originInfo>  
    <dateIssued encoding="w3cdtf" keyDate="yes">2014</dateIssued>  
 </originInfo>  
 <language>  
    <languageTerm type="text">English</languageTerm>  
 </language>  
 <abstract> The author shares zine libraries' missions, policies, procedures and practices to a punk audience.</abstract>  
 <subject>  
    <topic>Library science</topic>  
 </subject>  
 <subject>  
    <topic>Literature</topic>  
 </subject>  
 <relatedItem type="host">  
    <titleInfo>  
       <title>Maximum Rocknroll</title>  
    </titleInfo>  
    <part>  
       <detail type="issue">  
          <number>371</number>  
       </detail>  
       <extent unit="page"></extent>  
       <date>April 2014</date>  
    </part>  
 </relatedItem>  
 <identifier type="CDRS doi">http://dx.doi.org/10.7916/D82Z13M0</identifier>  
 <location>  
    <physicalLocation authority="marcorg">NNC</physicalLocation>  
 </location>  
 <recordInfo>  
    <recordContentSource authority="marcorg">NNC</recordContentSource>  
    <recordCreationDate encoding="w3cdtf">2014-05-08 15:25:48 -0400</recordCreationDate>  
    <recordChangeDate encoding="w3cdtf">2014-05-08 15:35:54 -0400</recordChangeDate>  
    <recordIdentifier>14407</recordIdentifier>  
    <languageOfCataloging>  
       <languageTerm authority="iso639-2b">eng</languageTerm>  
    </languageOfCataloging>  
 </recordInfo>  
</mods>
```

## Dublin Core - Omeka Descriptive Record

**Dublin Core Screencast: This shows the Omeka Dublin Core interface that we saw in class, with a bit more instruction on each of the elements.**

[lis653\_DC](http://vimeo.com/108125406) from [Christina Harlow](http://vimeo.com/cmh2166) on [Vimeo](https://vimeo.com/).

#### References 2

-   Dublin Core Metadata Initiative - Core Elements Set: <http://dublincore.org/documents/dces/>
-   Dublin Core Metadata - Terms (includes core elements): <http://dublincore.org/documents/dcmi-terms/>

#### Tools 2

-   Your class' Omeka site: <https://starr.omeka.net/admin/>

#### Example 2

In class, I showed you how to log into your class' Omeka site and where to create a Dublin Core record. In the screencast, I walk through this process again, creating a Dublin Core metadata record for the item[available at this link](https://exhibitions.cul.columbia.edu/exhibits/show/ephemera/item/7289 "Digital Asset in Columbia Exhibition"). Yes, this item already has some metadata attached, but we are not entirely cheating by using that for the example since the metadata attached is a Dublin Core - MODS mix (as we have modified our Columbia Omeka installation to include MODS metadata, being a 'MODS-shop').

### Omeka Item Creation Instructions

Finally, I'm leaving these instructions written up for use by your class as you start creating items and metadata records for your projects. Perhaps they will be of help.

1.  Log in at <https://starr.omeka.net/admin/>
2.  Once, there go ahead to 'add an item to your archive'.
3.  **Dublin Core section:** This is where the descriptive metadata goes. At least adding a Title, Subject (if applicable), Creator, Source, Publisher and Date is recommended, depending on the resource. Note that all of these fields are repeatable (and, in theory, optional).
4.  As a handy note, since these two are often confused or misused: **Source** (the physical item that the digitized image came from) and **Relation** (the 'work' that the item is a part of or otherwise related to) are used for linking between records and resources.
5.  **Item Type section:** Select the type of item you're uploading. You can add item-specific and file-specific information here as well. This should complement, not exactly duplicate, your Dublin Core data. This section is optional but usually helpful for matching up Omeka descriptive records with digitized files and directories.
6.  **Collection section:** Here you will choose the collection that matches your group.
7.  **Files section:** Here you upload the actual file(s). Be wary of issues of copyright with this.
8.  **Tags section:** Here you can enter tags. This area is useful if a particular curator, archivist or librarian would like to use a local or department-specific vocabulary for items, but you don't want this local vocabulary as part of the Dublin Core metadata.
9.  After you have entered all of the metadata for the Item, go back to the top of the Add Item page.

### Questions, Corrections, & Follow-up discussion

I'm always available via email (cmharlow(at)gmail(dot)edu) or on Twitter(\@cm_harlow). Thank you for letting me invade your class for one night.
