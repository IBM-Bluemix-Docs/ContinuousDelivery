---

copyright:
  years: 2018
lastupdated: "2018-4-4"
---
<!-- Copyright info at top of file: REQUIRED
    The copyright info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be surrounded by 3 dashes.
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    Indentation as per the previous template must be preserved.
-->

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# Working with custom docker images

At some point you may find that the pipeline base image is no longer meeting your build's needs and you for example need
fine-grained control over the versions of node, java or other tools used. You can address the problem by having a
first step in your jobs that installs a series of new packages and carefully configures things like PATH to setup your
environment. A much better alternative is to use the pipeline's support for running a "Custom Docker Image" as
your job's basis.

Each job type (Build, Test, Deploy) gives the option to select a "Custom Docker Image" sub-type that allows you to
take control of your job and provide the "Docker image name" to be used and the "script" to run. For example, to run
a Build job using Maven 3.5.3 and IBM Java 8 you would use:

