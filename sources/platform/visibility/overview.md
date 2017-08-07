page_main_title: Visibility Overview
main_section: Platform
sub_section: Visibility
page_title: Visibility Overview - Shippable DevOps Assembly Lines
page_description: Overview of Visibility capabilities of Shippable DevOps Assembly Lines Platform
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# Visibility Overview
Visualizing your entire DevOps assembly line in an end-to-end view is critical to understand where the inefficiencies and friction exist in your DevOps Assembly Lines. In addtion, you want to know what progress has been made by all the teams involved in building the next great feature of your product. Shippable's platform comes with capability built-in and this makes it easier for you to go faster and faster

In addition, you can drill down into individual problem areas and even debug the error at a console level with Shippable.

## How is it organized?
We fundamentally believe that source control systems is where the right roles and permissions exist, especially if "Everything as Code" is already an accepted philosophy in your organization. As a result, Shippable uses your source control to organize all your DevOps activities too. Shippable UI is organized with the same hierarchy in mind

* **Account**: this is the highest level entity. It represents a persona of a user. for e.g. github user
	* Dashboard: This is the view where you get to see all active CI Projects and Jobs in a single view
	* Integrations: List of all Integrations owned by the Account of the user logged in
	* Profile: Account profile of the user logged in
	* Search: Search for runs of Jobs or CI Projects across all hierarchies
* **Subscription**: this is typically an organization on your source control system. This is the entity that contains repositories
* **Project**: this is a representation of your source code repository. It is also the CI view
* **Jobs/Resources**: this is a representation of a [Job](/platform/workflow/job/overview) or a [Resource](/platform/workflow/resource/overview)





##Viewing job console output
Just like resources, Jobs are also versioned on Shippable. Every run of a job creates a new version of the job, including a unique build object which stores the console output of the Job run.

<img src="../../images/platform/jobs/jobModal.png" alt="Console output and trace, properties, run, and pause buttons for a job" style="vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

You can view console output for a job by clicking on it in the SPOG view. The job console looks like this:

<img src="../../images/platform/jobs/jobModal.png" alt="Console output and trace, properties, run, and pause buttons for a job" style="vertical-align: middle;display: block;margin-left: auto;margin-right: auto;"/>

Please note that in most cases, the logs you are interested in will be under the **Executing Task -> /home/shippable/micro/nod/stepExec/managed/run.sh** section. This section is shown as expanded in the screenshot above.

In addition to viewing logs for the latest run, you can also view logs for historical runs by choosing a past run in the UI.



# Further Reading
* Working with Resources
* Working with Integrations
* Jobs