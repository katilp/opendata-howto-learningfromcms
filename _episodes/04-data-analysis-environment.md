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
{: .discussion}

## Analysis environment

For CMS open data, we need to extend the concept of interoperability to the question "Can external users access and analyse CMS open with the means they have available?".

The answer is "No": a specific environment and a specific set of software tools are needed. So we provide them in the form of software container images.

Why containers and not just software? For complex data such as those from CMS, the analysis environment must be compatible with the software to process the data. Open data releases happen after an embargo time, so the software version (and the required operating system), compatible with the released data, are already "old" at the time of the release. It would not be enough just to provide the software, as the users would not be able to install it anywhere.

## CMS open data analysis environment

> ## Hands-on!
>
> Start the CMSSW container that you have downloaded and follow the ["Getting started" instructions](http://opendata.cern.ch/docs/cms-getting-started-2015#data) to inspect some of the data files.
>
{: .challenge}

## Your analysis environment

> ## What do you need?
>
> - Does opening and analysing your research data require some specific operating system and/or software tools?
> - Does it require some specific versions of common software tools?
>
{: .discussion}

Many different container images are available, such as the `jupyter/datascience-notebook` container image that we used in the an earlier episode to look at the weather data. You can explore more in [Docker Hub](https://hub.docker.com/search?q=), the default repository for docker container images.

For security reasons, it is best to use the official images or images provided by otherwise trusted organizations.

Assume you could use the `jupyter/datascience-notebook` container for your data analysis, but it would require some libraries in addition to those already installed.

### 1. Build a new image

It is easy to build a new container image starting from an existing one. The "recipe" to build a new image is in a file called [Dockerfile](https://docs.docker.com/engine/reference/builder/). To install an additional python library (e.g. [`howdoi`](https://github.com/gleitz/howdoi)), your `Dockerfile` would be:

~~~
FROM jupyter/datascience-notebook
RUN pip install howdoi
~~~
{: .source}

Save that as `Dockerfile` in your working directory and build the new container image with:

~~~
docker build --tag my-new-image .
~~~
{: .language-bash}

Here we use `my-new-image` as the image name, but you can choose another name. Once the build has finished, you would start a container from this new image with

~~~
docker run -it --rm -p 8888:8888  my-new-image
~~~
{: .language-bash}

Open the web interface, and a terminal in it, and test that the new library is available. With this library, you can query free text topics e.g. for using datetime in python:

~~~
howdoi use datetime in python
~~~
{: .language-bash}


> ## Build a new container image
>
> - Build a new container image with a python library of your choice starting from `jupyter/datascience-notebook` image. If applicable, use a library relevant to your field of study.
>
{: .challenge}

### 2. Share your new image

Your new image is now available locally. To make it available to others, you would upload it to an image repository e.g. to [dockerhub](https://hub.docker.com/).

First, create a dockerhub account.

Then, find the image id (the 12-character alphanumeric string) with

~~~
docker image ls | grep my-new-image
~~~
{: .language-bash}

Tag the image with

~~~
docker tag <IMAGE ID>  <DOCKERHUB USER NAME>/my-new-image:v1.0.0
~~~
{: .language-bash}

Login to dockerhub and push the image there

~~~
docker login --username=<DOCKERHUB USER NAME>
docker push <DOCKERHUB USER NAME>/my-new-image:v1.0.0
~~~
{: .language-bash}

{% include links.md %}

