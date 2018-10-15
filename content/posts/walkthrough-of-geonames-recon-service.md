---
title: 'walkthrough of geonames recon service'
date: 2015-07-29T18:52:50-07:00
draft: false
---

> This came out of documentation I was writing up for staff here at UTK.
> I apologize if it is too UTK-workflow specific.

I'm working currently on migrating a lot of our non-MARC metadata collections from older platforms using a kind of simple Dublin Core to MODS/XML (version 3.5, we're currently looking at 3.6) that will be ingested into Islandora. That 'kind of simple Dublin Core' should be taken as: there was varying levels of metadata oversight over the years, and folks creating the metadata had different interpretations of the Dublin Core schema - a well-documented and well-known issue/consideration for working with such a general/flexible schema. Yes, there are guidelines from DCMI, but for on-the-ground work, if there is no overarching metadata application profile to guide and nobody with some metadata expertise (or investment) to verify that institution-wide, descriptive (or any type, for that matter) metadata fields are being used consistently, it is no surprise that folks will interpret metadata fields in different ways with an eye to their own collection/context. This issue increases when metadata collections grow over time, occur with little to no documentation, and a lot of the metadata creation is handed off to content specialists, who might then hand it off to their student workers. If you are actually reading my thoughts right now, well thanks, but also you probably know the situation I'm describing well.

Regardless, I'm not here to talk about why I think my job is important, but rather about a very particular but useful procedure and tool that make up my general migration/remediation work, which also happens to be something I'm using and documenting right now for UTK cataloger reskilling purposes. I have been working with some of the traditional MARC catalogers to help with this migration process, and so far the workflow is something like this:

1.  I pull the original DC (or other) data, either from a csv file stored somewhere, or, preferably, from an existing OAI-PMH DC/XML feed for collections in (soon to be legacy) platforms. This data is stored in a GitHub repository \[See note below\] as the original data for both version control and "But we didn't write this" verification purposes.
2.  A cleaned data directory is made in that GitHub repo, where I put a remediation files subdirectory. I will review the original data, see if an existing, documented mapping makes sense (unfortunately, each collection usually requires separate mapping/handling), and pull the project into OpenRefine. In OpenRefine, I'll do a preliminary 'mapping' (rename columns, review the data to verify my mapping as best I can without looking at the digitized objects due to time constraints). At this point, I will also note what work needs to be done in particular for that dataset. I'll export that OpenRefine project and put it into the GitHub repo remediation files subdirectory, and also create or update the existing wiki documentation page for that collection.
3.  At this point, I will hand off the OpenRefine project to one of the catalogers currently working on this metadata migration project. They are learning OpenRefine from scratch but doing a great job of getting the hang of both the tool and the mindset for batch metadata work. I will tell them some of the particular points they need to work on for that dataset, but also they are trained to check that the mapping holds according to the UTK master MODS data dictionary and MAP, as well as that controlled access points have appropriate terms taken from the selected vocabularies/ontologies/etc. that we use. With each collection they complete, I'm able to give them a bit more to handle with the remediation work, which has been great.
4.  Once the catalogers are done with their remediation work/data verification, I'll take that OpenRefine project they worked on, bring it back into OpenRefine on my computer, and run some of the reconciliation services for pulling in URIs/other related information we are currently capturing in our MODS/XML. One of the catalogers is starting to run some of these recon services herself, but it is something I'm handing over slowly because there is a lot of nuance/massaging to some of these services, and the catalogers working on this project only currently do so about 1 day a week (so it takes longer to get a feeling for this).
5.  I review, do some reconciliation stuff, get the complex fields together that need to be for the transform, then export as simple XML, take that simple XML and use my UTK-standard OpenRefine XML to MODS/XML XSLT to generate MODS/XML, then run encoding/well-formed/MODS validation checks on that set of MODS/XML files.
6.  Then comes the re-ingest to Islandora part, but this is already beyond the scope of what I meant this post to be.

*GitHub Note: I can hear someone now: 'Git repositories/GitHub is not made for data storage!' Yes, yes, I know, I know. It's a cheat. But I'm putting these things under version control for my own verification purposes, as well as using GitHub because it has a nice public interface I can point to whenever a question comes up about 'What happened to this datapoint' (and those questions do come up). I don't currently, but I have had really good luck with using the Issues component of GitHub too for guiding/centralizing discussion about a dataset. Using GitHub also has had the unintended but helpful consequence of highlighting to content specialists who are creating the metadata just why we need metadata version control, and why the metadata updates get frozen during the review, enhancement and ingest process (and after that, metadata edits can only happen in the platform). But, yes, GitHub was not made for this, I know. Maybe we need dataHub. Maybe there is something else I *should* be using. Holla if you know what that is.*

Okay, so I'm in step 4 right now, with a dataset that was a particular pain to remediate/migrate because the folks who did the grouping/digitization pulled together a lot of different physical objects into one digital object. This is basically the digital equivalent of 'bound-withs'. However, the cataloger who did some of the remediation did a great job of finding, among other datapoints, the subject\_geographic terms, getting them to subject\_geographic, and normalizing the datapoint to a LCNAF/LCSH heading where possible. I'm about to take this and run my OpenRefine Geonames recon service against it to pull in coordinates for these geographic headings where possible. As folks seem to be interested in that recon service, I'm going to walk through that process here and now with this real life dataset.

## WHERE I STOP BLABBING AND TALK ABOUT GEONAMES RECON SERVICE FINALLY

So here is that ready-for-step-4 dataset in LODRefine (Linked Open Data Refine, or OpenRefine with some Linked Data extensions baked in; I need to write more about that later):

[![Metadata in LODRefine](/images/lodrefine_bass.png)](/images/lodrefine_bass.png)

You can see from that portion a bit of what work is going on here. What I'm going to target in on right now is the subject\_geographic column, which has multiple values per record (records in this instance are made up of a number of rows. This helps centralize the reconciliation work, but will need to be changed to 1 record = 1 row before pulling out for XML transformations). Here is the column, along with a text facet view to see the values we will be reconciling against Geonames:

[![LODRefine Geographic Text Facet](/images/bass_subj_geo_facet.png)](/images/bass_subj_geo_facet.png)

Look at those wonderfully consistent geographic terms, thanks to the cataloger's work! But, some have LoC records and URIs, some don't, some maybe have Geonames records (and so coordinates), some might not... so let's go ahead and reconcile with Geonames first. To use the Geonames service, I already have a copy of the Geonames Recon Service on my computer, and I have updated my local machine's code to have my own private Geonames API name. See more here: <https://github.com/cmh2166/geonames-reconcile>.

I'm then going to a CLI (on my work computer, just plain old Mac Terminal),

[![Mac Terminal & Bash](/images/terminal_bash.png)](/images/terminal_bash.png)

change to the directory where I have my local Geonames recon service code stored,

[![Mac Terminal CD to Geonames directory](/images/terminal_cd_geonames.png)](/images/terminal_cd_geonames.png)

then type in the command 'python reconcile.py --debug'. The Geonames endpoint should fire up on your computer now. You may get some warning notes like I have below, which means I need to do some updating to this recon service or to my computer's dependencies installation (but am going to ignore for time being while recon service still works because, well, time is at a premium).

[![Terminal with Geonames flask app running](/images/terminal_geonames_run_with_warnings.png)](/images/terminal_geonames_run_with_warnings.png)

Note, during all of this, I already have LODRefine running in a separate terminal and the LODRefine GUI in my browser.

[![LODRefine running in terminal](/images/lodrefine_terminal.png)](/images/lodrefine_terminal.png)

Alright, with all that running, lets hop back to our web browser window where LODRefine GUI is running with my dataset up. I've already added this Geonames as a reconciliation service, but in case you haven't, you would still go first to the dropdown arrow for any column (I'm using the column I want to reconcile here, subject\_geographic), then to Reconcile \> Start Reconciling.

[![LODRefine drop-down menu to reconcile](/images/lodrefine_dropdown_reconcile.png)](/images/lodrefine_dropdown_reconcile.png)

A dialog box like this should pop up:

[![LODRefine reconcile dialog box](/images/lodrefine_recon_dialog_box.png)](/images/lodrefine_recon_dialog_box.png)

I've already got GeoNames Reconciliation Service added, but if you don't, click on 'Add Standard Service' (in the bottom left corner), then add the localhost URL that the Geonames python flask app you started up in the Terminal before is running on (for me and most standard set ups, this will be <http://0.0.0.0:5000/reconcile)>:

[![LODRefine add recon service dialog box with URL input](/images/lodrefine_add_standard_recon_service.png)](/images/lodrefine_add_standard_recon_service.png)

I will cancel out of that because I already have it running, and then click on the existing GeoNames Reconciliation Service in the 'Reconcile column' dialog box. If you just added the service, you should have the same thing as me showing now upon adding the service:

[![LODRefine Geonames reconciliation options](/images/lodrefine_geonames_recon_option.png)](/images/lodrefine_geonames_recon_option.png)

There are a few type options to choose from:

-   `geonames/name` = search for the cells' text just in the names field in a Geonames record
-   `geonames/name_startWith` = search for Geonames records where the label starts with the cells' text
-   `geonames/name_equals` = search for an exact match between the Geonames records and the cells' text
-   `geonames/all` = just do keyword search of Geonames records with our cells' text.

Depending on the original data you are working with, the middle two options can return much more accurate results for your reconciliation work. However, because these are LoC-styled headings (with the mismatching of headings style with Geonames I've [described recently](http://christinaharlow.com/thoughts-on-geospatial-metadata/) in other posts as well as in the [README.md for this Geonames Recon code](https://github.com/cmh2166/geonames-reconcile/blob/master/README.md)), I'm going to go with geonames/all. If you haven't read those other thoughts, basically, the Geonames name for Richmond, Virginia is just 'Richmond', with Virginia, United States, etc. noted instead in the hierarchy portion of the record. This makes sense but makes for bad matching with LoC-styled headings. Additionally, the fact that a lot of these geographic headings refer to archaeological dig sites and not cities/towns/other geopolitical entities also means a keyword search will return better results (in that it will return results at all).

*Sidenote: See that 'Also use relevant details from other columns' part? This is something I'd love to use for future enhancements to this recon service (maybe refer to hierarchical elements there?) as well as part of a better names (either LCNAF or VIAF) recon service I'm wanting to work more on. Names, in particular, personal names and reconciliation is a real nightmare right now.*

Alright, so I select 'geonames/all' then I click on the 'Start reconciling' button in the bottom right corner. Up should pop a yellow notice that reconciliation is happening. Unfortunately, you can't do more LODRefine work while that is occuring, and depending on your dataset size, it might take a while. However, one of the benefits of using this reconciliation service (versus a few others ways that exist for reconciliation against an API in LODRefine) is speed.

[![LODRefine note that reconciliation service is running](/images/lodrefine_geonames_running.png)](/images/lodrefine_geonames_running.png)

Once the reconciliation work is done, up should pop a few more facet boxes in LODRefine - the judgement and the best candidate's score boxes, as well as the matches found as hyperlinked options below each cell in the column. Any cell with a value considered a high score match to a Geonames value will be associated with and hyperlinked to that Geonames value automatically.

[![Geonames reconciliation done in LODRefine](/images/lodrefine_geonames_done.png)](/images/lodrefine_geonames_done.png)

Before going through matches and choosing the correct ones where needed, I recommend you change the LODRefine view from rows to records - as long as the column you are reconciling in *is not the first column*. Changing from records to rows then editing the first column means that, once you go back to records view, the records groupings may have changed and no longer be what you intent. But for any other column, the groupings remain intact.

[![LODRefine records to rows buttons](/images/lodrefine_records_to_rows.png)](/images/lodrefine_records_to_rows.png)

Also, take a second to look at the Terminal again where Geonames is running. You should see a bunch of lines showing the API query URLs used for each call, as well as a 200 response when a match is found (I'm not going to show you this on my computer as each API call has my personal Geonames API name/key in it). Just cool to see this, I think.

Back to the work in LODRefine, I'm going to first select to facet the results with judgement:none and then unselect 'error' in the best candidate's score facet box.

[![LODRefine geonames reconciliation facet boxes](/images/lodrefine_geonames_facets_review.png)](/images/lodrefine_geonames_facets_review.png)

If you're looking at this and thinking '1 match? that is not really good', well...

1.  yes, there are definite further improvements needed to have Geonames and LoC-styled headings work better together, but...
2.  library data has a much, much higher bar for this sort of batch work and the resultant quality/accuracy expected, so 1 auto-determined match in a set of geographic names focused on perhaps not well known archaeological sites is okay with me.\ Plus, the Geonames recon service is not done helping us yet.

Now you should have a list of cells with linked options below each value:

[![LODRefine geonames reconciliation choices within cells](/images/lodrefine_geonames_recon_choices.png)](/images/lodrefine_geonames_recon_choices.png)

What I do now is review the options, and choose the double check box for what is the correct Geonames record to reconcile against. The double check box means that what I choose for this cell value will also be applied to all other cells in LODRefine that have that same value.

[![](/images/lodrefine_geonames_choose_option.png)](/images/lodrefine_geonames_choose_option.png)

If I'm uncertain, I can also click on any of the options, and the Geonames record for that option will show up for my review. Also, for each option you select as the correct one for that cell, those relevant cells should then disappear from the visible set due to our facet choices.

Using these functionalities, I can go through the possible matches fairly quickly, and much more quickly all other work included than doing this matching entirely manually. Due to the constraints of library data's expected quality, this sort of semi-automated, enhanced-manual reconciliation is really where a lot of this work will occur for many (but not all) institutions.

If in reviewing the matches, if there is no good match presented, you can choose 'create new topic' to pass through the heading as found, unreconciled with Geonames.

Now I'm done my review (which took about 5 minutes for this set), I can see that I have moved from 1 matched heading to 106 matched headings (I deselected the 'None' facet in the judgment box and closed the 'best match facet' box).

[![LODRefine Geonames reconciliation facet boxes after review](/images/lodrefine_geonames_post_review.png)](/images/lodrefine_geonames_post_review.png)

However, there are still 134 headings that were matched to nothing in Geonames. Click on that 'none' facet in the judgment box, and leaving the geographic\_subject column text facet box up, I can do a quick perusal of what didn't find a match, as well as check on headings that seem like should have had a match in Geonames. However, for this dataset, I see a lot of these are archaeological dig sites, which probably aren't in Geonames, so the service worked fairly well so far. This is also how you'll find some typos or other errors as well, and any historical changes in names that may have occured. For the facet values that I do find in Geonames, I click to edit the facet value and go ahead and add the coordinates, which is what I pull from Geonames currently (we opt to choose the LoC URI for these headings at present, but this is under debate).

*Note: datasets with more standard geographic names (cities, states, etc) will have much better results doing the above described work. However, I want to show here a real life example of something I want to pull in coordinates from Geonames for, like archaeological or historical sites.*

[![LODRefine post-Geonames Reconciliation Text Facet for unmatched values](/images/lodrefine_geonames_updated_unmatched.png)](/images/lodrefine_geonames_updated_unmatched.png)

I end up adding Coordinates for 5 values which weren't matched to Geonames either because of typos or because the site is on the border of 2 states (a situation LoC and Geonames handle differently). I fixed 7 typos as well in this review.

Now I'm done reconciling, I want to capture the Geonames Coordinates in my final value. First I close all the open facet boxes in LODRefine. Now on that subject\_geographic column, I am going to click on the column header triangle/arrow and choose Edit Cells \> Transform.

[![LODRefine Column Text Transform Selection](/images/lodrefine_editcells_transform.png)](/images/lodrefine_editcells_transform.png)

In the Custom text transform on column subject\_geographic box that appears, in the Expression text area, I will put in the following:


```
if(isNonBlank(cell.recon.match.name), value + substring(cell.recon.match.name, cell.recon.match.name.indexOf(" | ")), value)
```

[![](/images/lodrefine_text_transform_geonames.png)](/images/lodrefine_text_transform_geonames.png)

Lets break this out a bit:

-   `value` = the cell's original value that was then matched against Geonames.
-   `cell.recon.match.name` = the name (and coordinates because we're using the Geonames recon service I cobbled together) of the value we choose as a match in the reconciliation process.
-   `cell.recon.match.id` = the URI for that matched value from the reconciliation process.
-   *Why isn't there cell.recon.match.coords? Yes, I tried that, but it involves hacking the core OpenRefine recon service backend more than I'm willing to do right now*
-   `if(test, do this, otherwise do that)` = not all of the cells had a match in Geonames, so I don't want to change those unmatched cells. The if statements then says "if there is a reconciliation match, then pull in that custom bit, otherwise leave the cell value as is."
-   `substring(cell.recon.match.name, cell.recon.match.name.indexOf(" \| "))` = means I just want to pull everything in that cell.recon.match.name value after the pipe - namely, the coordinates. I am leaving the name values as is because they are currently matched against LoC for our metadata.

Why do we need to run this transform? Because although we have done reconciliation work in LODRefine, if I was to pull this data out now (say export as CSV), the reconciliation data would not come with it. LODRefine is still storing the original cell values in the cells, with the reconciliation data laid over top of it. This transform will change the underlying cell values to the reconciled values I want, where applicable.

After running the transform, you can remove the reconciliation data to see exactly what the underlying values now look like. And remember there is always the Undo tab in LODRefine if you need to go back.

[![LODRefine clear reconciliation data menu option](/images/lodrefine_clear_recon_data.png)](/images/lodrefine_clear_recon_data.png)

What our cell values look like now:

[![LODRefine data post-reconciliation](/images/lodrefine_subj_geo_reconciled.png)](/images/lodrefine_subj_geo_reconciled.png)

And, ta-da! Hooray! At this point, I can shut down the Geonames reconciliation python flask app running in that terminal by going to the Terminal window it is running in and typing in cntl + C. Back in LODRefine, remember to change back from rows view to records view (links to do this are in the top left corner).

## Thoughts on this process

Some may think this process seems a bit extreme for pulling in just coordinates. However...

1.  Remember that this seems extreme for the first few times or when you are writing up documentation explaining it (especially if you are as verbose as I am). In practice, this takes me maybe at most 20 minutes for a dataset of this size (73 complex MODS records with 98 unique subject\_geographic headings). It gets faster and easier, and is definitely more efficient still, than completely manually updates, and remains far more accurate than completely automated reconciliation options.
2.  If so moved, I could pull in Geonames URIs as part of this work, which would be even better. However, because of how we handle our MODS at present, we don't. But the retrieval of URIs and other identifiers for such datapoints is a key benefit.
3.  For datasets larger than 100 quasi-complex records, this is really the only way to go **at present** and considering our workflows. I don't want to give these datasets to the catalogers and ask them to add coordinates, URIs, or other reconciled information because they need to focus on the batch work on this process - checking the mappings, getting values in appropriate formats or encodings - and not manually searching each controlled access point in a record or row then copy and pasting that information from some authority source. But this is all a balancing act.
4.  This process also has the added benefit of making very apparent typos and other such mistakes. Unfortunately, I'm not as aware in my quick blog ramblings.

Hope this is helpful for others.
