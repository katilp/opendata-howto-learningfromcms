---
title: "Analysis example workflows"
teaching: 10
exercises: 40
questions:
- "But how do I analyse these data?"
objectives:
- "See what kind of examples are provided to make CMS open data reusable"
- "Exercise running an analysis workflow in a software container"
keypoints:
- "Complex research data may require extensive processing with a specific set of software tools in a specific environment."
- "It is easiest to get started with a new analysis if a human and machine-readable analysis example workflow is available."
---

This section addresses the concept of “reusability”, R in FAIR. In the description of the [FAIR principles](https://www.go-fair.org/fair-principles/), again for an inpatient and/or simple-minded reader, this translates to a requirement of data being described accurately.

This is not necessarily enough to make complex research data reusable. For example, for CMS open data, a good understanding is required of how different particles are identified and selected for a specific analysis and of the uncertainties involved. The knowledge about these procedures must be passed to open data users, but there is no single recipe that applies for all cases.

## CMS data analysis example workflows

Analysis of the CMS data is most commonly done in two steps: first, selecting events of
interest and writing them to a new, smaller format, and second, analysing the selected events.
Due to the experiment-specific data format, the first step will almost inevitably be done using
the CMS software CMSSW in a computing environment compatible with the open data, that is, the CMSSW container that you have downloaded.

> ## Hands-on!
>
> Start the CMSSW container `my_od` (if that was name you have chosen for it) with
>
> ~~~
> docker run -i my_od
> ~~~
> {: .language-bash}
>
> and run the example code called ["Physics Object Extractor Tool"](http://opendata.cern.ch/record/12502) or POET for short as instructed in the last part of the ["Getting started" instructions](http://opendata.cern.ch/docs/cms-getting-started-2015#nice). Note that
> 
> - the example configuration file indicates how additional data products are combined with the actual data
> - the example code documents how different selections and correction should be applied to the data
> - open data users can use this code as a base for their own event selection step.
>
{: .challenge}

In this exercise, the commands to download the code and run it were typed by hand in the command prompt. They are also provided in a short bash script [commands.sh](https://github.com/cms-opendata-analyses/PhysObjectExtractorTool/blob/2015MiniAOD/commands.sh) which could be passed to the container when using automated workflows.

You could download and run it in the container prompt with

~~~
curl -O https://raw.githubusercontent.com/cms-opendata-analyses/PhysObjectExtractorTool/2015MiniAOD/commands.sh
source commands.sh
~~~
{: .language-bash}

> ## Note!
>
> Making public the exact code that has been used for a physics analysis paper is not part of the CMS open data policy. The analysis examples provided with open data have been simplified and adapted from existing studies. The input data for these examples are the output data of POET tool (see above) or another similar selection step.
>
{: .callout}

## Your analysis workflows



{% include links.md %}