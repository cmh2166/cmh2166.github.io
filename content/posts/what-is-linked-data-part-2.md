---
title: 'what is linked data part 2'
date: 2015-07-30T18:52:50-07:00
draft: false
---

In an earlier post, I gave a quick and highly subjective introduction to Linked Data. What will follow are a series of posts detailing some use cases and workflows that follow Linked Data principles, since I best learn and understand via example.

The example in this post as well as many of the future posts on Linked Data workflows will use[OpenRefine](http://www.openrefine.org/ "OpenRefine") with the [DERI RDF extension](http://refine.deri.ie/ "RDF Refine"). If you already have OpenRefine installed, you can just download and install the RDF extension at <http://refine.deri.ie/> (the page also has installation instructions). If you do not have OpenRefine installed, instead install [LODRefine](http://code.zemanta.com/sparkica/download.html#lodrefine "LODRefine") (installation instructions included on that site) - this is OpenRefine with some Linked Open Data (including the DERI RDF) extensions included.

The embedded video is a screencast where I go through the following steps on my own computer. I hope it will help with the introduction.

**Note: in the video, an issue comes up with OpenRefine (unfortunately, a common issue when dealing with this sort of work). I left it in because it is a common error and I don't want to hide those from folks new to this work in OpenRefine (thus throwing those folks off when they encounter it). To understand how I got around it, I eventually had to reload my browser page in which OpenRefine was running, then start from the point in the OpenRefine project that worked last before the error.**

### RDF and Linked Open Authority Data

You may have noticed that many library authorities are now available as Linked Open Data, usually available for download as RDF files in a variety of serializations and using different schemas. We are going to experiment with the Linked Open Data authority files available for download from the Library of Congress at <http://id.loc.gov/download/>.

I am going to work with [this very, very small metadata set](https://drive.google.com/file/d/0B74oOQcTdnHjUTIxVU9qR2tyb0E/edit?usp=sharing). You can download this set and use it to create a project in OpenRefine/LODRefine if you would like to follow along. Note that this metadata is a just a subset of a much larger set that has already been cleaned up using the other, data munging functions of OpenRefine. *Metadata will rarely start out this clean, and half the battle of Linked Open Data is the less glamorous, labor-intensive preparation of the data sets.*

To update this metadata, we need to first set up the RDF reconciliation option in OpenRefine/LODRefine. A different RDF reconciliation option needs to be set up for each, different RDF set that you wish to reconcile against. We are going to create a RDF reconciliation option with the RDF file of the Thesaurus of Graphic Material (available for download at <http://id.loc.gov/download/>). Here are the steps:

-   Download from <http://id.loc.gov/download/> the Thesaurus for
    Graphic Materials (TGM) in the N-Triples (nt) serialization.
-   It will be a compressed file - find the downloaded file on your
    computer and expand the archive.
-   Just to see what we are working with, open up the file in your
    preferred text editor (anything other than Microsoft Office - use
    Notepad or TextEdit if nothing else).
-   Observe how MADS is used - triples indicating the preferred label
    use the predicate
    \<<http://www.loc.gov/mads/rdf/v1#authoritativeLabel>\>. Make a note
    of this, as it will be important for our reconciliation set-up.
-   Let us now set up the RDF file reconciliation option in
    OpenRefine/LODRefine:
    -   Click on RDF (a button in the top right corner), then choose
        'Add reconciliation service...' and 'Based on RDF file...'
    -   In the dialog box that appears, give this reconciliation option
        a name (I chose 'LCTGM').
    -   Next, choose to 'Upload file', then find your expanded authority
        file on your computer.
    -   For the file format, choose 'N-Triple'.
    -   Now, for label properties, unclick 'rdfs:label' and click
        'other'. In the dialog box that appears,
        add<http://www.loc.gov/mads/rdf/v1#authoritativeLabel> - the
        predicate we noted when looking at the downloaded RDF file
        earlier.
    -   Click OK.
-   It will take a second for OpenRefine/LODRefine to load the new RDF
    reconciliation option - which is why we choose a small file to
    experiment with - but once it is finished, find the column of data
    values in your project that you would like to reconcile with the
    Thesaurus of Graphic Materials. For my sample metadata set, it is
    the column with the name 'item - MODS - Form/Genre' (the odd naming
    conventions for these columns came from the process of a data dump
    from an Omeka project and the following flattening into a
    spreadsheet form). Next, for that column, we:
    -   Click on the arrow/triangle at the top of that column.
    -   Choose 'Reconcile' \> 'Start reconciling'.
    -   Choose the RDF reconciliation option that we just created.
    -   Choose to reconcile with the type 'MADS Topic'.
    -   Click on 'start reconciling'.
-   Again, it will take a second for OpenRefine/LODRefine to process
    this request depending on the metadata set size (which is why we
    have a micro-sample for this experiment). Once the reconciliation is
    complete, you can click on the reconciled values to see the
    Thesaurus of Graphic Materials term that our metadata was reconciled
    with, or search for other terms if we disagree with the
    reconciliation.
-   Finally, let's pull in the related Thesaurus of Graphic Materials
    URIs for the values we decided to keep in our reconciled metadata.
    To do so:
    -   Click on the arrow/triangle at the top of the column we
        reconciled.
    -   Choose 'Edit column \> Add column based on this column'.
    -   Give the new column a name.
    -   Enter into the Expression text box 'cell.recon.match.id'. Click
        enter.
    -   You now have a new column with the URIs of the authorized
        values.

This is a pretty awesome tool, right? But there are limitations to using the RDF extension in OpenRefine/LODRefine - particularly when dealing with large authority files (like the LCNAF). So we have to look for other options - in particular, SPARQL endpoints, APIs, and scripting - in those cases. I hope to have future blog posts on these workflows in the near future. As ever, if you have corrections, updates, ideas, questions, issues, or other - leave a comment or get in touch using cmharlow(at)gmail(dot)com! Thanks!
