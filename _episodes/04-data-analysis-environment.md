---
title: "Data analysis environment"
teaching: 10
exercises: 40
questions:
- "But how do I open these data files?"
objectives:
- "See What can be done if the data are not in quite common format?"
- "Learn how containers are used to provide the analysis enviroment for CMS open data"
- "Reflect on need and possibilities to provide an analysis environment compatible with your research data"
keypoints:
- "Complex research data may require a specific environment and a specific set of software tools for analysis and access"
- "Software containers are practical for providing an analysis environment for open data."
---

## Data formats

This section addresses the concept of "interoperability", I in FAIR. In the description of the [FAIR principles](https://www.go-fair.org/fair-principles/), for an inpatient and/or simple-minded reader, this translates to a requirement of data being in some common format that can be opened and analysed with the standard tools in their research domain.

For CMS open data, this is both true and false. The data files are in the ROOT format, [ROOT](https://root.cern/) being a framework for data processing commonly in use in particle physics. However, data are processsed with the CMS specific software (CMSSW) which gives additional structure and interconnections to the data objects. CMSSW is open source, yet it is certainly not a "broadly applicable language for knowledge representation". But this is what we have and this is what we release.

In many other cases, data can be stored and provided in open access in simpler formats, such CSV, JSON or others.

> ## Format of your research data?
>
> - In which format(s) do you store your research data?
> - Are those formats readable with commonly available software tools?
>
{: .challenge}

## Analysis environment

For CMS open data, we need to extend the concept of interoperability to the question "Can external users access and analyse CMS open with the means they have available?".

The answer is "No": a specific environment and a specific set of software tools are needed. So we provide them in the form of software container images.

Why containers and not just software? For complex data such as those from CMS, the analysis environment must be compatible with the software to process the data. Open data releases happen after an embargo time, so the software version (and the required operating system), compatible with the released data, are already "old" at the time of the release. It would not be enough just to provide the software, as the users would not be able to install it anywhere.

> ## Your analysis environment
>
> - Does opening and analysing your research data require some specific software tools?
> - Does it require some specific versions of common software tools?
>
{: .challenge}

## Hands-on

As an exercise, open the CMSSW container that you have downloaded and 

{% include links.md %}

