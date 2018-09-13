---

copyright:
  years: 2016, 2018
lastupdated: "2018-8-2"
---
<!-- Copyright info at top of file: REQUIRED
    The copyright info is YAML content that must occur at the top of the MD file, before attributes are listed.
    It must be surrounded by 3 dashes.
    The value "years" can contain just one year or a two years separated by a comma. (years: 2014, 2016)
    Indentation as per the previous template must be preserved.
-->

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Environment properties and resources
{: #deliverypipeline_environment}

You can use environment properties and preinstalled resources to interact with {{site.data.keyword.contdelivery_full}}'s pipeline capability. For example, you might incorporate them into a job script or test command.
{:shortdesc}

You can add your own environment properties to a stage from its **ENVIRONMENT PROPERTIES** tab. Environment properties are available to every job in a stage.

You can add four types of properties from the Environment Properties tab:
* **Text**: A property key with a single-line value.
* **Text Area**: A property key with a multi-line value.
* **Secure**: A property key with a single-line value. The value is displayed as asterisks.
* **Properties**: A file in the project's repository. This file can contain multiple properties. Each property must be on its own line. To separate key-value pairs, use the equals sign (=). Enclose all string values in quotes. For example, MY_STRING="SOME STRING VALUE".

You can examine the environment properties for a pipeline job by running the `env` command in the job's script.
{:tip}

The following properties and resources are available by default in pipeline environments.

<!--##Contents
* [Environment properties](#env)
    * [General purpose properties](#gen)
    * [Runtime and tool properties](#runtime)
    * [Deployment properties](#deployment)
* [Pre-installed resources](#resources)
    * [Runtimes and tools](#tools)
    * [Node modules](#node)-->

## Environment properties
{: #deliverypipeline_envprop}

### General purpose properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ARCHIVE_DIR | The directory to archive or to download archives into. |
| BUILD_ID | The unique ID for the current job execution.  |
| BUILD_DISPLAY_NAME | The BUILD_ID value, prefixed with "#". |
| BUILD_NUMBER | The incremental stage ID that is shown in the pipeline UI.  |
| GIT_BRANCH | The Git branch that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_COMMIT | The Git commit that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| GIT_PREVIOUS_COMMIT | The Git commit value of the job's last successful run. This property is only available in jobs that use a Git repository as input. |
| GIT_URL | The Git repository URL that the job uses as input. This property is only available in jobs that use a Git repository as input. |
| IDS_JOB_ID | The unique ID of the job's configuration. |
| IDS_JOB_NAME | The name of the job's configuration. |
| IDS_OUTPUT_PROPS | Comma-separated names of your stage environment properties. |
| IDS_PROJECT_NAME | The name of the project, for example, <code>Owner - Project Name</code>. |
| IDS_STAGE_NAME | The name of the current stage. |
| IDS_URL | The URL for the current pipeline. |
| IDS_VERSION | The number for the build that is being deployed or the SCM identifier. This property is available only in deploy jobs.
| JOB_NAME | The unique job ID in the context of the current pipeline. |
| PIPELINE_KUBERNETES_CLUSTER_NAME | The name of the Kubernetes cluster that is selected in the current job. |
| PIPELINE_STAGE_INPUT_JOB_ID | The ID of the job that is input for the current stage. |
| PIPELINE_STAGE_INPUT_REV | The revision of the input for the current stage. |
| PIPELINE_INITIAL_STAGE_EXECUTION_ID | The unique ID for the run of the pipeline. |
| PIPELINE_BLUEMIX_API_KEY | The API key provided for the continous delivery pipeline|
| PIPELINE_TRIGGERING_USER | The current user for the pipeline job|
| TASK_ID | The unique ID of the job's current run. |
| TMPDIR | A directory location where temporary files are stored. |
| WORKSPACE | The path for the current working directory. |

### Runtime and tool properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| ANT_HOME | The path to Apache Ant 1.10.5. |
| ANT_JAVA8_HOME | The path to a 1.10+ version of Apache Ant that requires Java 8. |
| GRADLE_HOME | The path to Gradle 4.10. |
| JAVA_HOME | The path to IBM&reg; Java&trade; 8. |
| JAVA7_HOME | The path to IBM Java 7. |
| JAVA8_HOME | The path to IBM Java 8. |
| MAVEN_HOME | The path to Apache Maven 3.5.4. |
| NODE_HOME | The path to Node.js 8.11.3. |

If you want to find out what the default versions of the main tools are, you can run the following command inside your script:
`$HOME/default_versions.sh`
{: tip}

### Deployment properties

| Environment property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| CF_APP | For deployments, the name of the app to deploy. This property is required for deployment and can be specified in the script itself, the deploy job configuration interface, or the project's `manifest.yml` file. |
| CF_ORG | For deployments, the name of the organization (org) to deploy to. |
| CF_ORGANIZATION_ID | For deployments, the ID of the org to deploy to. |
| CF_SPACE | For deployments, the name of the space to deploy to. |
| CF_SPACE_ID | For deployments, the ID of the space to deploy to.  |
| CF_TARGET_URL | For deployments, the URL of {{site.data.keyword.Bluemix_short}} or Cloud Foundry. |
| IDS_VERSION | For deployments, the version of the app that is being deployed or the source identifier. |

## Preinstalled resources
{: #deliverypipeline_resources}

Several runtimes, tools, and Node modules are preinstalled in every pipeline.

### Runtimes and tools

All links are in the home directory.
{: tip}

| Resource | Link name | Path |
|----------|-----------|-----------|
|Apache Ant 1.10.5|ant |/opt/IBM/ant.java8 |
|Cloud Foundry CLI 6.29.0 |cf | /opt/IBM/cf |
|Gradle 1.12|gradle |/opt/IBM/gradle |
|Gradle 2.9 |gradle2 |/opt/IBM/gradle2 |
|Gradle 4.10 |gradle4 |/opt/IBM/gradle4 |
|IBM Java (default)|java |/opt/IBM/java8 |
|IBM Java 7 x86_64-71 |java7 |/opt/IBM/java7 |
|IBM Java 8.0.5.21|java8 |/opt/IBM/java8 |
|Apache Maven 3.2.1 |maven |/opt/IBM/maven |
|Apache Maven 3.5.4 |maven3 |/opt/IBM/maven3 |
|IBM Node 8.11.3|node|/opt/IBM/node-v8.11.3|
|IBM Rational Team Concert&trade; SCM Tools |RTC-SCM-Tools |/opt/IBM/RTC-SCM-Tools |

Different versions of Node.js have been preinstalled in the pipeline. The command `nvm` is used to choose which version should be used. The preinstalled versions list is v0.10.40, v0.10.48, v0.12.7, v0.12.17, v4.2.2, v4.4.5, v4.6.0, v6.2.2, v6.7.0, v8.11.3, v9.11.2, v10.8.0.
The default version of Node.js is v8.11.3.
For example, to use node 6.7.0, enter this command `nvm use v6.7.0`.
If you want to use a version of Node.js that is not listed above, please use the `nvm install` command.
For example, if you want to use v8.0.0, enter this command: `nvm install 8.0.0`. This will install version 8.0.0 and set it as the default version.
If your script used to install a specific version of npm using a command like `npm install -g npm@3.7.1`, it might no longer work if the version you are targetting is lower than the default version in the pipeline. The version of npm that comes with Node.js 8.11.3 is 5.6.0. In order to fix your script, you can either remove that command completely or run the command `nvm use v6.7.0` which uses npm version 3.10.3.

### Node modules

The following Node modules are globally installed in the pipeline environment:

* grunt@1.0.3
* grunt-cli@1.3.0
* grunt-contrib-concat@1.0.1
* grunt-contrib-jshint@1.1.0
* grunt-contrib-nodeunit@2.0.0
* grunt-contrib-qunit@3.0.1
* grunt-contrib-uglify@3.4.0
* grunt-contrib-watch@1.1.0
* karma@3.0.0
* karma-cli@1.0.1
* karma-jasmine@1.1.2
* karma-phantomjs-launcher@1.0.4
* phantomjs-prebuilt@2.1.7
