---
title: Readable facet labels in Summon
published: true
layout: post
permalink: "summon-facet-labels"
---


Customizing Summon: Readable facet labels
----

At the UA Libraries, we use Summon as our discovery tool. As many libraries noticed, however, the facets got less readable in a recent update to the tool. Long labels became truncated and and large portions of each term became ellipses. One of the labels reads *history & ar...*

![]({{ site.url }}/downloads/summon-discipline-facet.png)

Solutions proposed on the Summon listserv included increasing the `width` of the facet field to a wider number of pixels and changing the `text-overflow` property. These solutions could help to save some horizontal real estate and to display more of the label, but there wasn't a guarantee that the entire facet label would display.

We decided to take up more vertical real estate in order to show the entire facet label no matter what, and to adjust the vertical spacing between the labels and also the result counts so they stayed next to each other.

The implementation has two parts:

1. Our custom javascript file that we added in Summon's configuration
2. A CSS file (the javascript writes a link to it)

We had added a custom javascript file and a custom CSS file when we first launched Summon to smooth out the ezproxy situation between the discovery interface and our website and to create our own feedback link (when we first launched, the feedback link provided in the Summon admin UI wasn't working for us.) We were able to use this structure to add modifications for the facet label problem.

### ualibraries-custom.js

	/**
	 * Write a link to our custom CSS file
	 */	
	$('<link rel="stylesheet" href="//www.library.arizona.edu/vendor-support/summon/ualibraries-custom.css?ver=062414-1">').appendTo( $('head') );

We (usually) manually update the `?ver=062414` part so the browser cache gets a fresh copy of the CSS each time we release a new set of changes.

This part of our CSS file relates to the facets

### ualibraries-custom.css

	.Filter .count {
	  padding: 4px 0;
	  display: table-cell;
	  vertical-align: middle;
	 }
	 
	.facetFields .Filter a,
	.moreFacetsPane .FilterGroup .Filter a {
	  white-space: normal;
	  width: 155px;
	  padding-top: 4px;
	  padding-bottom: 4px;
	  padding-right: 0;
	}

This should handle the facet labels that display initially when you load search results, the full list of facets that display under the "more" toggle, and it'll add vertical space to the result counts to make sure they stay visually lined up with the labels. The longest label in our installation (*pharmacy, therapeutics, & pharmacology*) takes up three lines with this solution, but it does make the discipline more clear than its previous display, which was *pharmacy, ...*.

![]({{ site.url }}/downloads/summon-longest-label.png)
