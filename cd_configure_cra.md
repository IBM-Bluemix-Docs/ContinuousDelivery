---

copyright:
  years: 2020
lastupdated: "2020-11-27"

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

Code Risk Analyzer supports the Java&trade;, Node.js, and Python languages. The following table lists and describes the content that Code Risk Analyzer supports.

|Content |Description	|
|:----------|:------------------------------|
|Java		|The repo must use Maven. Dependencies are calculated by using the `pom.xml` file.		|
|Node.js		|Dependencies are calculated by using the `package-lock.json` file.		|
|Python		|Dependencies are calculated by using the `requirements.txt` file.		|
| Dockerfiles		|Files with the `Dockerfile` pattern in the repo are considered. For container images, the Debian, Alpine, and Ubutu Linux distros are supported.  		|
| Kubernetes		|Files that are suffixed with `.yaml` and `.yml` are considered. The `kind` value must be set to `Pod`, `ReplicaSet`, `ReplicationController`, `Deployment`, `Daemonset`, `Statefulset`, `Job`, or `CronJob`.   		|
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

Code Risk Analyzer is currently available for toolchains only in the Dallas region.
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
* The region to create the toolchain in. Code Risk Analytics is available only in the Dallas region.
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
1. If you have a GitHub or {{site.data.keyword.ghe_short}} repo and want to use it, for the repository type, click **Existing** and type the URL.
1. If you want to use a new GitHub or {{site.data.keyword.ghe_short}} repo, type a name for the repo, type the URL for the repo that you are cloning or forking, and select the repository type:

 a. To create an empty repo, click **New**.

 b. To create a copy of a GitHub or {{site.data.keyword.ghe_short}} repo, click **Clone**.

 c. To fork a GitHub or {{site.data.keyword.ghe_short}} repo so that you can contribute changes through pull requests, click **Fork**.

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
 * For {{site.data.keyword.ghe_short}} repos, select **github-ent-pr-listener**.

1. Save your changes.
1. In the **Environment properties** section, click **Add property**.
1. Select **Secure** and specify the `apikey` property key to use the API key that you previously specified. You can access the Secure property in your Tekton pipeline resources. For more information about this property, see [Tekton Pipelines environment and resources](/docs/ContinuousDelivery?topic=ContinuousDelivery-tekton_environment).
1. Click **Save**.

Use of this service is not guaranteed to find all vulnerabilities in your applications. The application owner is responsible for testing any fixes that are recommended by the service.
{: important}
