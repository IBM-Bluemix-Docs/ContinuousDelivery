---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-16"

keywords: Code Risk Analyzer, code repositories, DevOps Insights, scan pull requests, Tekton pipelines

subcollection: ContinuousDelivery

---

{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Configuring Code Risk Analyzer
{: #cd-configure-cra-repos}


Code Risk Analyzer takes all of your Git-based code, configurations, and deployment artifacts, builds a dependency graph, and runs a pipeline of regulatory compliance control checks. It is embedded into existing development workflows, such as creating a change request or promoting a code change into the main development branch. It produces a bill of materials that reflects the composition of a deployment. And it identifies specific versions of packages and Linux&reg; operating systems distributions that contain published vulnerabilities.
{: shortdesc}

## Supported content
{: #cra_supported_content}

Code Risk Analyzer supports the Java&trade;, Node.js, and Python languages. The following table lists and describes the content that Code Risk Analyzer supports.

|Content |Description	|
|:----------|:------------------------------|
|Java		|The repo must use Maven. Dependencies are calculated by using the `pom.xml` file.		|
|Node.js		|Dependencies are calculated by using the `package-lock.json` file.		|
|Python		|Dependencies are calculated by using the `requirements.txt` file.		|
|Golang		|Supports `go mod` and `go dep` dependency management. For `go mod`, the `go.sum` file must be in the repo. For `go dep`, the `Gopkg.lock` file must be in the repo.		|
| Dockerfiles		|Files with the `Dockerfile` pattern in the repo are considered. For container images, the Debian, Alpine, and Ubutu Linux distros are supported.  		|
| Kubernetes		|Files that are suffixed with `.yaml` and `.yml` are considered. The `kind` value must be set to `Pod`, `ReplicaSet`, `ReplicationController`, `Deployment`, `Daemonset`, `Statefulset`, `Job`, or `CronJob`.   		|
|Terraform		|The repo must use Terraform v0.13.5 and IBM Cloud as a Terraform provider for compliance checks.		|
{: caption="Table 1. Supported content" caption-side="top"}

Code Risk Analyzer supports only github.com repos, and {{site.data.keyword.gitrepos}} repos that are hosted by {{site.data.keyword.contdelivery_full}}.
{: important}

## Known issues with Code Risk Analyzer
{: #cra_known_issues}

Code Risk Analyzer has the following current known issues:

* Semantic versioning is supported only for package versioning (major.minor.patch). Code Risk Analyzer does not currently support package versions that contain pre-release versions or build metadata.
* If too many vulnerabilities are found, the Pull Request comment might display only a subset of the vulnerabilities.
* Tekton tasks might appear all green even when minor errors exist.

## Enabling Code Risk Analyzer to scan pull requests and merge requests
{: #cra_enable_requests}

You must use a Tekton pipeline to run Code Risk Analyzer tasks on your code repo. When you are running other Continuous Integration or {{site.data.keyword.contdelivery_short}} flows, you can use either Classic pipelines or Tekton pipelines to run Code Risk Analyzer on your code repo.
{: tip}

Complete the following steps to enable Code Risk Analyzer to scan pull requests and merge requests on your code repositories (repos):

1. Use the Build your own toolchain template to create an empty toolchain that you can add the {{site.data.keyword.DRA_short}} tool integration to.
1. Add the {{site.data.keyword.DRA_short}} tool integration to the toolchain.
1. Add the code repo that contains the code you want to scan to the toolchain. For example, add a GitHub or GitLab repo, or the tekton-catalog repo to the toolchain.
1. Create an API key to authenticate with Code Risk Analyzer.
1. Create and configure a Tekton pipeline.

Code Risk Analyzer is currently available for toolchains only in the Dallas, Washington, Sydney, Osaka, and Toronto regions.
{: important}

## Building your own toolchain
{: #cra_build_toolchain}

You can use the Build your own toolchain template as a starting point to [create an empty toolchain](https://cloud.ibm.com/devops/create){:external} that you can add the {{site.data.keyword.DRA_short}} tool integration to. If you already have a toolchain, you can use the existing toolchain instead of creating a new one.

1. Log in to [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){:external}.
1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg), and select **DevOps**.
1. On the **Toolchains** page, click **Create a Toolchain**.
1. On the **Create a Toolchain** page, click the **Build your own toolchain** template.
1. Review the default information for the toolchain settings:

* The toolchain's name identifies it in {{site.data.keyword.cloud_notm}}. If you want to use a different name, change the toolchain's name.
* The region to create the toolchain in. Code Risk Analytics is available only in the Dallas and Washington regions.
* The resource group or organization to create the toolchain in. Click the link to switch between selecting resource groups and orgs. If you want to use a different resource group or org, select it from the list of available resource groups or orgs.

1. In the Tool Integrations section, select the {{site.data.keyword.DRA_short}} tool integration to create and configure it for your toolchain. 
1. Click **Create**. The toolchain is created and the pipelines are created and triggered.

## Adding {{site.data.keyword.DRA_short}} to your toolchain
{: #cra_add_dra}

1. Log in to [{{site.data.keyword.cloud_notm}}](http://cloud.ibm.com){:external}.
1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg), and select **DevOps**.
1. On the Toolchains page, click the toolchain to open its Overview page. Alternatively, on your app's Overview page, on the Continuous delivery card, click **View toolchain**. Then, click **Overview**.
1. Click **Add tool**.
1. In the Tool Integrations section, click **{{site.data.keyword.DRA_short}}**.
1. Click **Create Integration**.

## Adding a code repository (repo) to your toolchain
{: #cra_add_dra}

Add and configure the repo that contains the code that you want Code Risk Analyzer to scan to your toolchain.

### Adding GitHub to your toolchain
{: #cra_add_github}

1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg) and select **DevOps**. On the Toolchains page, click the toolchain to open its Overview page. Alternatively, on your app's Overview page, on the Continuous delivery card, click **View toolchain**. Then, click **Overview**.
1. Click **Add tool**.
1. In the Tool Integrations section, click **GitHub**.
1. Click the GitHub server that you want to use.
1. If you have a GitHub repo and want to use it, for the repository type, click **Existing** and type the URL.
1. If you want to use a new GitHub repo, type a name for the repo, type the URL for the repo that you are cloning or forking, and select the repository type:

 a. To create an empty repo, click **New**.

 b. To create a copy of a GitHub repo, click **Clone**.

 c. To fork a GitHub repo so that you can contribute changes through pull requests, click **Fork**.

1. Click **Create Integration**.

If you don't have admin privileges for the repo that you are linking to, your integration is limited because you can't use a webhook. Webhooks are required to automatically run a pipeline when a commit is pushed to the repo. Without a webhook, you must start your pipelines manually.
{: tip}

### Adding GitLab to your toolchain
{: #cra_add_gitlab}

1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg) and select **DevOps**. On the Toolchains page, click the toolchain to open its Overview page. Alternatively, on your app's Overview page, on the Continuous delivery card, click **View toolchain**. Then, click **Overview**.
1. Click **Add tool**.
1. In the Tool Integrations section, click **GitLab**.
1. Click the GitLab server that you want to use.
1. If you have a GitLab repo and want to use it, for the repository type, click **Existing** and type the URL.
1. If you want to use a new GitLab repo, type a name for the repo, type the URL for the repo that you are cloning or forking, and select the repository type:

 a. To create an empty repo, click **New**.

 b. To create a copy of a GitLab repo, click **Clone**.

 c. To fork a GitLab repo so that you can contribute changes through merge requests, click **Fork**.

1. If you want to create a public repo on the server, clear the **Make this repository private** checkbox.
1. If you want to use GitLab's Issues for issue tracking, select the **Enable GitLab Issues** checkbox.
1. If you want to track the deployment of code changes by creating tags and comments on commits, and labels and comments on issues that are referenced by the commits, select the **Track deployment of code changes** checkbox. For more information, see [Track where your code is deployed with toolchains](https://www.ibm.com/cloud/blog/announcements/track-code-deployed-toolchains/){:external}.
1. Click **Create Integration**.
1. Click the card for the GitLab repo that you want to work with. Depending on the repo that you selected, either the GitLab website or your company's GitLab repo opens, where you can view the contents of the repo.

  You can use the integrated source code management tools in Eclipse Orion {{site.data.keyword.webide}} to edit the GitLab repo and deploy an app from your workspace.
  {: tip}

1. If you enabled GitLab Issues, click **GitLab Issues** to open it. You can use this instance of GitLab Issues for your entire toolchain, even if the toolchain contains multiple GitLab repos.    

If you don't have owner or master privileges for the repo that you are linking to, your integration is limited because you can't use a webhook. Webhooks are required to automatically run a pipeline when a commit is pushed to the repo. Without a webhook, you must start your pipelines manually.
{: tip}

### Adding tekton-catalog to your toolchain
{: #cra_add_tektoncatalog}

1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg) and select **DevOps**. On the Toolchains page, click the toolchain to open its Overview page. Alternatively, on your app's Overview page, on the Continuous delivery card, click **View toolchain**. Then, click **Overview**.
1. Click **Add tool**.
1. In the Tool Integrations section, click **GitHub**.
1. Click the GitHub server that you want to use.
1. For the repository type, click **Existing** and type **https://github.com/open-toolchain/tekton-catalog** to specify the repo where the Tekton tasks are defined.
1. Click **Create Integration**.

If you don't have admin privileges for the repo that you are linking to, your integration is limited because you can't use a webhook. Webhooks are required to automatically run a pipeline when a commit is pushed to the repo. Without a webhook, you must start your pipelines manually.
{: tip}

## Creating an API key to authenticate with Code Risk Analyzer
{: #cra_api_key}

1. From the menu bar, click **Manage** &gt; **Access (IAM)**, and select **{{site.data.keyword.cloud}} API keys**.
1. Click **Create an {{site.data.keyword.cloud_notm}} API key**.
1. Specify a name and description for the API key.
1. Click **Create**.
1. Create a copy of the generated API key for future use.
 
## Creating and configuring a {{site.data.keyword.deliverypipeline}} for Tekton
{: #cra_tekton_pipeline}

When you configure a {{site.data.keyword.deliverypipeline}} tool integration, you can select the type of pipeline that you want to create.

1. From the {{site.data.keyword.cloud_notm}} console, click the menu icon ![hamburger icon](images/icon_hamburger.svg) and select **DevOps**. On the [Toolchains page](https://cloud.ibm.com/devops/toolchains){:external}, click the toolchain to open its Overview page. Alternatively, on your app's Overview page, on the Continuous delivery card, click **View toolchain**. Then, click **Overview**.
1. Add the Delivery Pipeline integration to your toolchain:

 a. Click **Add tool**.

 b. In the Tool Integrations section, click **{{site.data.keyword.deliverypipeline}}**.

1. Specify a name for your new pipeline.
1. Select **Tekton** to create a Tekton {{site.data.keyword.deliverypipeline}}. This type of pipeline provides a dashboard that you can use to view the output of Tekton pipeline runs on a defined Kubernetes cluster, with support for configuring the pipeline definitions repos, the pipeline triggers, where the pipeline runs, and simple secrets.
1. Click **Create Integration** to add the {{site.data.keyword.deliverypipeline}} to your toolchain.

### Configuring a {{site.data.keyword.deliverypipeline}} for Tekton 
{: #configure_tekton_pipeline}

1. Click the **{{site.data.keyword.deliverypipeline}}** card to open the Tekton {{site.data.keyword.deliverypipeline}} dashboard.
1. In the **Definitions** section, complete the following tasks:

 a. Specify the Git repo and URL (https://github.com/open-toolchain/tekton-catalog) that contains the Tekton pipeline definition and related artifacts.

 b. Select the **master** branch.
 
 c. Add the `toolchain` path. This is the path to your pipeline definition within the Git repo. You can reference a specific definition within the same repo. You can also add multiple definition repos, if they are integrated with the toolchain.
 
 d. Click **Validate** and save your changes.
 
 e. Repeat steps a and b to add each of the following paths to the master branch within your Git repo:
 
  * `git`
  * `utils`
  * `cra`
  * `cra/sample`
 
 f. Click **Validate** and save your changes.

 The pipeline definition is updated automatically.
 
1. In the **Worker** section, select the IBM Managed shared worker or the private worker that you want to use to run your Tekton pipeline. For more information about private workers, see [Working with Delivery Pipeline Private Workers](/docs/ContinuousDelivery?topic=ContinuousDelivery-private-workers).

 The private worker must be defined in the same toolchain as your Tekton pipeline.
 {: important}

1. In the **Triggers** section, click **Add trigger** to create a new Git repository trigger to run when a Pull Request is opened or updated for the specified Git repo and branch. You can add a trigger for each repo that you want to run the Code Risk Analyzer for.

 a. Select the repo that you want to run the pipeline on.

 b. Select the target branch for the Pull Request, such as **master**.
 
 c. Specify the events for the pipeline to run on. Currently, you must specify the `When a pull request is opened or updated` option.
 
 d. Associate the trigger with an event listener:
 
 * For GitHub repos, select **github-pr-listener**.
 * For {{site.data.keyword.gitrepos}} repos, select **gitlab-pr-listener**.

1. Optional. If the repo contains Terraform files, in the **Triggers** section, specify the directory within your Git repo that you want to run Terraform scanning on.

 a. In the **Show trigger properties** section, click **Add**.

 b. Select **Text** and then specify the following property keys:
 
 * `tf-dir` to input the directory path for your Terraform files.
 * Optional. `tf-var-file` to input a comma-delimited list of variable definition files. These files are passed to the Terraform command by using the `var-file` option. Specify the variable definition files relative to the directory path in `tf-dir`.
 
1. Save your changes.
1. In the **Environment properties** section, click **Add property**, and then select **Secure**.

* Enter the `apikey` property key to use the API key that you previously specified. You can access the Secure property in your Tekton pipeline resources. For more information about this property, see [Tekton Pipelines environment and resources](/docs/ContinuousDelivery?topic=ContinuousDelivery-tekton_environment).
* Optional. Enter the property key for each environment variable that you want to use for Terraform.

1. Click **Save**.

Use of this service is not guaranteed to find all vulnerabilities in your applications. The application owner is responsible for testing any fixes that are recommended by the service.
{: important}

## Configuring Code Risk Analyzer to build package manifest files
{: #cra_build_package_manifests}

The Code Risk Analyzer Discovery task automatically searches for the package manifest files that are listed in the table within the [Supported content](#cra_supported_content) section. If a package manifest file is not found in the repo, you can configure the Discovery task to automatically build the package manifest file for the supported languages.

The following table lists and describes each of the supported environment properties.

| Environment Property | Description |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| nodejs-create-package-lock | A Boolean (`true` or `false`) value that indicates whether to create the `package-lock.json` file.  This environment property requires the repo to contain a `package.json` file. |
| python-create-requirements-txt | A Boolean (`true` or `false`) value that indicates whether to create  the `requirements.txt` file.  This feature supports both Python 2 and Python 3. |
{: caption="Table 2. Environment properties" caption-side="top"}

## Configuring Code Risk Analyzer to ignore files
{: #cra_ignore_files}

The Code Risk Analyzer Discovery and CIS tasks search for the file path `.cra/.fileignore` in the repo that is cloned and scanned. The `.fileignore` file must follow the rules of [`.gitignore` files](https://git-scm.com/docs/gitignore){: external}. Similar to a `.gitignore` file, the `.fileignore` file can include comments, directories to ignore, files to ignore, and other [patterns](https://git-scm.com/docs/gitignore#_pattern_format){: external}.

The following example `.fileignore` file shows how to exclude bash scripts, node_modules, and the Dockerfile.

```
# Ignore nested functional_tests directory
**/functional_tests

# Ignore bash scripts
**/*.sh

# This should allow this one file
!test/gatling_tests/loginTobx.sh

# Ignore node_modules
node_modules

# Exclude the dockerfile from scanning
Dockerfile
```

*`.craignore` is being deprecated for `.fileignore`*

## Configuring Code Risk Analyzer to omit vulnerabilities
{: #cra_omit_vulnerabilities}

The Code Risk Analyzer Vulnerability task searches for the file path `.cra/.cveignore` in the repo that is cloned and scanned. This file is used to specify the CVEs to either omit indefinitely, until a remediation is available, or until a specified expiration date.

The following example shows a JSON schema for the `.cveignore` file.

```json
[
    {
        "cve": "string",
	"alwaysOmit": "bool",
	"untilRemediationAvailable": "bool",
	"expiration": "string"
    }
]
```

The following properties are accepted for each entry in the `.cveignore` file:

 * **cve** - The vulnerability to omit. The value of this property is either a CVE ID or a Snyk ID.
 * **alwaysOmit** - If this property is set to true, the vulnerability is omitted until it is changed. This property takes precedence over other property values.
 * **untilRemediationAvailable** - If this property is set to true, the vulnerability is omitted  until a remediation path is available. If a remediation becomes available, the vulnerability is not omitted and a message is displayed. This property takes precedence over the expiration property  value.
 * **expiration** - If this property is set to true and the expiration date is not reached, the vulnerability is omitted. If the expiration date is reached, the vulnerability is not omitted and a message is displayed. 
 
The Code Risk Analyzer uses only these defined properties. You can add properties with no effect on functions. If a vulnerability that is defined in the `.cveignore` is not omitted, a log is generated explaining the reason. If a vulnerability that is defined in the `.cveignore` file is omitted, no individual logging is displayed. The number of omissions and a list of the vulnerability IDs, with the package name, that are omitted are logged after a report is completed.

The following code snippet shows a sample `.cveignore` file.

```json
[
    {
        "cve": "CVE-2021-27290",
        "alwaysOmit": true
    },
    {
        "cve": "CVE-2020-8244",
        "untilRemediationAvailable": true,
    },
    {
        "cve": "SNYK-JAVA-ORGAPACHEHTTPCOMPONENTS-31517",
        "expiration": "2022-02-10T22:08:41+04:00",
        "comment": "Additional security measures are in place"
    }
]
```

*`.cracveomit` is being deprecated for `.cveignore`*
