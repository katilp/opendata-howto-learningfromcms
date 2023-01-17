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
- "For complex research data to be reusable, ``FAIRness´´ of data is not enough: the example workflows to work on these data need to be FAIR as well."
---

This section addresses the concept of “reusability”, R in FAIR. In the description of the [FAIR principles](https://www.go-fair.org/fair-principles/), again for an inpatient and/or simple-minded reader, this translates to a requirement of data being described accurately.

This is necessarily not enough to make complex research data reusable. For example, for CMS open data, a good understanding is required of how different particles are identified and selected for a specific analysis and of the uncertainties involved. The knowledge about these procedures must be passed to open data users, but there is no single recipe that applies to all cases.

There are also additional data products that are needed in different steps of the analysis, for example for data selection,
as correction factors to be applied to physics objects, or for evaluating the final measurable
results in terms of cross sections. Analysis example code accompanied with some specific guide pages turns out to be the most efficient method to explain and instruct the use of this associated information.

## CMS data analysis example workflows

Analysis of the CMS data is most commonly done in two steps: first, selecting events of
interest and writing them to a new, smaller format, and second, analysing the selected events.
Due to the experiment-specific data format, the first step will almost inevitably be done using
the CMS software CMSSW in a computing environment compatible with the open data, that is, the CMSSW container that you have downloaded.

> ## Hands-on!
>
> Start the [previously built](https://katilp.github.io/opendata-howto-docker-pre-exercise/03-docker-for-cms-opendata/#start-a-cmssw-open-data-container) CMSSW container `my_od` (if that was the name you have chosen for it) with
>
> ~~~
> docker start -i my_od
> ~~~
> {: .language-bash}
>
> and follow the instructions in the last part of the ["Getting started" instructions](http://opendata.cern.ch/docs/cms-getting-started-2015#nice) run the example code called "Physics Object Extractor Tool" or POET for short. Note that, in the container, you are already in the working directory and the command to get there, if needed, would be `cd /code/CMSSW_7_6_7/src`.
>
> Note that
> 
> - the example configuration file indicates how additional data products are combined with the actual data
> - the example code documents how different selections and corrections should be applied to the data
> - open data users can use this code as a base for their own event selection step.
>
{: .challenge}

In this exercise, the commands to download the code and run it were typed by hand in the command prompt. They are also provided in a short bash script [commands.sh](https://github.com/cms-opendata-analyses/PhysObjectExtractorTool/blob/2015MiniAOD/commands.sh) which could be passed to the container when using automated workflows.

Instead of typing the commands, you could have downloaded and run it in the container prompt with

~~~
curl -O https://raw.githubusercontent.com/cms-opendata-analyses/PhysObjectExtractorTool/2015MiniAOD/commands.sh
source commands.sh
~~~
{: .language-bash}

> ## Note!
>
> Making public the exact code that has been used for a physics analysis paper is not part of the CMS open data policy. The analysis examples provided with open data have been simplified and adapted from existing studies. The input data for these examples are the output data of the POET tool (see above) or another similar selection step.
>
{: .callout}

[Examples](http://opendata.cern.ch/search?page=1&size=20&q=&experiment=CMS&subtype=Analysis&type=Software) of different types of analyses are made available on the CERN open data portal or worked through in [tutorials](https://cms-opendata-workshop.github.io/workshop2022-lesson-ttbarljetsanalysis/) during the CMS open data workshops.

Most of them still rely on users typing the commands by hand, but an effort is ongoing to include command scripts and properly scalable multi-step workflows. For testing purposes, they can be implemented as [GitHub action workflows](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions) or [GitLab pipelines](https://docs.gitlab.com/ee/ci/pipelines/). Some are also implemented as examples on the [REANA](https://reanahub.io/) platform at CERN.

The next challenge for CMS is to further investigate different computational workflow engines (e.g. argo, airflow, luigi...) and provide analysis workflow implementations so that they can scale easily from initial testing on small amounts of data to full-scale processing on computing clusters or public cloud resources. 

## Your analysis workflows

In the following, for the sake of simplicity, we set aside the data access and concentrate on the analysis workflow part. As an exercise, use the container image that you built in the previous section to run a simple script. That script will fake an example workflow on your open data and it can be a simple placeholder doing something completely different. 

To run a script in a container, you have two options:

- use the image as it is and pass the analysis script to it (or download it there)
- build a new image with the script included in the image.

The choice depends on how the users would use the analysis script: would they modify it extensively or would they just pass arguments to it?

### Pass the analysis script

Depending on the base image that you've chosen, the container image may or may not include some basic tools. For example, while the standard `python` image has several options to read in files (e.g. `curl`, `wget`, `git`), the slimmer `python:slim-bullseye` has none of them, and reading files into the container will not be possible from the container shell.

In all cases, you can pass the files through a [bind mount](https://docs.docker.com/storage/bind-mounts/), where part of the local filesystem is shared with the container. You can do as instructed under "Run a single Python script" in the [python docker image documentation](https://hub.docker.com/_/python), in the directory where you have your script (here `myscript.py`).

Create new directory and save these lines in a file `myscript.py`

~~~
from emoji import emojize as em
print(em(":snowflake:"))
~~~
{: .language-python}

Run that script in your new container with

~~~
docker run -it --rm -v "$PWD":/usr/src/myapp -w /usr/src/myapp my-new-image python myscript.py
~~~
{: .language-bash}

In this case, users of your container and analysis script would need to download them both. But it will be easy to modify the script just by editing it locally. If you have data files in the shared directory, they will be available in the container as well.

### Include the script in the image

If your script is fully configurable with input parameters, you could build a new image with the script included. To follow up from the example of the previous section, modify your `Dockerfile` to be:

~~~
FROM python:slim-bullseye
WORKDIR /usr/src/app
RUN pip install emoji
COPY . .
~~~
{: .source}

Your script could be:

~~~
from emoji import emojize as em
import sys
mystring = "I want to print "+sys.argv[1]
print(em(mystring))
~~~
{: .language-python}

Build the container image in the directory of your `Dockerfile` and the script:

~~~
docker build --tag my-new-image:v1.1.0 .
~~~
{: .language-bash}

If you have data files in the local directory, they will be copied to the container as well.

You can run the script in the image with

~~~
docker run -it --rm my-new-image:v1.1.0 python myscript.py :yellow_heart::blue_heart:"
~~~
{: .language-bash}

> ## Hands-on!
>
> Updated your image to include a script and share it:
> - add a script to the image you built in the previous section
> - upload the new version of it to Docker Hub
> - add instructions on how to run the script in the Readme of the image repository.
>
{: .challenge}

> ## Test!
>
> Exchange images with someone and test if you get each other's analysis scripts running!
>
{: .challenge}

{% include links.md %}