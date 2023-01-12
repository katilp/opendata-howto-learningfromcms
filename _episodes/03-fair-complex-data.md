---
title: "FAIR principles for complex research data"
teaching: 10
exercises: 30
questions:
- "What does FAIR mean for complex research data"
objectives:
- "Observe how the challenges for complex research data are addressed in CMS open data records"
- "Reflect on reusability of your own research data"
keypoints:
- "Making data open does not make them simple"
- "Describing complex research data so that they are reusable can get involved"
- "Open data users are expected to invest time in learning"
---

In the following, we take the example of CMS open data. Note that we have the luxury of having the [CERN open data portal](http://opendata.cern.ch/) through which all CMS open data can be searched and distributed.

> ## Note on repositories
>
> - Do you have such repositories at your disposal?
> - Would they make your open data FINDABLE and ACCESSIBLE?
>
{: .discussion}

Arranging a repository from which open data can be downloaded should not be a responsibility of a single researcher but of an institution.

Long-term preservation and availability should be considered - and it should be beyond the lifetime of the experiment or the collaboration.

## CMS open data records

We'll go through a quick exercise.

> ## Metadata
>
> - Discuss in groups what you think is necessary to describe a dataset in an open data repository (5 mins). List the items. These would the metadata fields.
>
{: .challenge}

Now, let us have a look at some CMS open data records: e.g. [collison dataset from 2015](http://opendata.cern.ch/record/24120) or a [simulated dataset for the same period](http://opendata.cern.ch/record/16452).

> ## Your observations of metadata in CMS open data records?
>
> - Without background in particle physics, how much do you understand of them?
> - Is there something obvious missing?
> - What's your impression: are these metadata enough to make the data usable?
{: .challenge}

For CMS open data, we have found necessary to provide:

- content metadata (what, how much etc)
- provenance metadata (how were they collected and reprocessed)
- context metadata (how to use them and connect them to other data products).

Data being usable does not necessarily mean that they are easy to use. The CMS collaboration has published more than 1000 [physics analysis papers](https://cms-results-search.web.cern.ch/). It takes several months or more than a year (and most often a Ph.D) for each of them. The open data users need to invest a similar amount of time, it will not be quick and easy to get a new result out of them. Making data open does not make them simple.

## Accessing data

CMS has released more than 3 PB of open data. All that amount is not needed for research-level work on these data, but certainly several TB's or data need to be accessed.

CMS open data is most commonly accessed "streaming" during the analysis job runtime, but the data files can be downloaded with a https download if desired. 

## Your data

> ## Your open data?
>
> - What kind of open data would you provide?
> - Are they yours or from a research collaboration?
> - Would these data be usable in another study than yours?
> - Would these data be usable in another context than yours?
>
{: .discussion}

{% include links.md %}

