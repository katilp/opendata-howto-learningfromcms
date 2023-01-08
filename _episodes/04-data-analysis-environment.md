---
title: "Data analysis environment"
teaching: 10
exercises: 40
questions:
- "But how do I open these data files?"
objectives:
- "Learn how containers are used to provide the analysis enviroment for CMS open data"
- "Build a new container image from an existing base image"
keypoints:
- "Complex research data may require a specific environment and a specific set of software tools for analysis and access"
- "Software containers are practical for providing and sharing an analysis environment for open data."
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
> Start the [previously built](https://katilp.github.io/opendata-howto-docker-pre-exercise/03-docker-for-cms-opendata/#start-a-cmssw-open-data-container) CMSSW container `my_od` (if that was name you have chosen for it)) with
>
> ~~~
> docker run -i my_od
> ~~~
> {: .language-bash}
>
> and follow the middle part (the section of "What is in the CMS data?") of the ["Getting started" instructions](http://opendata.cern.ch/docs/cms-getting-started-2015#data) to inspect some of the data files. Note that
> 
> - this is the environment used in data analysis within the CMS collaboration
> - it includes all software and helper scripts needed for analysis of CMS data
> - it has a configurable executable called `cmsRun`.
>
{: .challenge}

## Your analysis environment

> ## What do you need?
>
> - Does opening and analysing your research data require some specific operating system and/or software tools?
> - Does your analysis run only on a specific processor architecture (arm vs amd/x86)?
>
{: .discussion}

Many different container images are available, such as `jupyter/datascience-notebook` that we used in the an earlier episode to look at the weather data. A [`mathworks/matlab`](https://hub.docker.com/r/mathworks/matlab) container image exists as well, but to access it requires the licence. You can explore the available images in [Docker Hub](https://hub.docker.com/search?q=), the default repository for docker container images.

For security reasons, it is best to use the official images or images provided by otherwise trusted organizations.

If the image size is an important factor, as it may be today during this tutorial, you may want to choose the smallest available base image and only install the libraries that you need.

Assume you could use a [`python` container](https://hub.docker.com/_/python) for your data analysis, but it would require some libraries in addition to those already installed.

### 1. Build a new image

It is easy to build a new container image starting from an existing one. The "recipe" to build a new image is in a file called [Dockerfile](https://docs.docker.com/engine/reference/builder/). To [install](https://pip.pypa.io/en/stable/cli/pip_install/) additional python libraries (e.g. [`emoji`](https://pypi.org/project/emoji/) although it is highly unlikely that your analysis depends on the capability of printing out emojis...), your `Dockerfile` would be:

~~~
FROM python:slim-bullseye
RUN pip install emoji
~~~
{: .source}

Save that as `Dockerfile` in your working directory and build the new container image with:

~~~
docker build --tag my-new-image .
~~~
{: .language-bash}

Here we use `my-new-image` as the image name, but you can choose another name. Once the build has finished, you would start a container from this new image with

~~~
docker run -it --rm my-new-image
~~~
{: .language-bash}

Test that the new library is available and it works with:

~~~
from emoji import emojize as em
print(em(":snowflake:"))
~~~
{: .language-python}

Exit with `exit()`.

> ## Build a new container image
>
> - Build a new container image with a python library of your choice starting from the `python:slim-bullseye` image (or another small-size image). If applicable, use a library relevant to your field of study.
>
{: .challenge}

### 2. Share your new image

Your new image is now available locally. To make it available to others, you would upload it to an image repository e.g. to [Docker Hub](https://hub.docker.com/).

First, create a Docker Hub account.

Then, tag the image with

~~~
docker tag my-new-image <YOUR DOCKERHUB USER NAME>/my-new-image:v1.0.0
~~~
{: .language-bash}

Login to Docker Hub and push the image there

~~~
docker login --username=<YOUR DOCKERHUB USER NAME>
docker push <YOUR DOCKERHUB USER NAME>/my-new-image:v1.0.0
~~~
{: .language-bash}

The image appears in your area in [Docker Hub](https://hub.docker.com/) and anyone can download it with

~~~
docker pull <YOUR DOCKERHUB USER NAME>/my-new-image:v1.0.0
~~~
{: .language-bash}

> ## Share your new container image
>
> - Upload your new docker image to Docker Hub.
>
{: .challenge}

{% include links.md %}

