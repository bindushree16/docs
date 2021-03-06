page_main_title: Shippable installer
main_section: Shippable Server
sub_section: Installation instructions
page_title: Admiral - Installing on a single machine
page_description: How to install Shippable
page_keywords: install, onebox, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

# Onebox pilot installation guide

This document describes the steps to install Shippable Server for a Onebox pilot. This is useful for server installations that are not used very heavily because it reduces the resources required.  For more information about the installer in general and other installation choices, see [admiral](/platform/server/install/).

## Shippable Server architecture

**Shippable Server comprises of the following -**

* **Control Plane** which consists of:
    * Core services - these are the administration, app (UI), login and core backend runtime services of Admiral.
    * State services - these are open source components that store transient and persistent state.
* **Build Plane** consisting of build servers - these run the execution agents that spin up CI and CD jobs.
* **Source Control Management(SCM) Plane** - this is your on-premise or cloud based SCM such as GitHub, GitHub Enterprise, BitBucket, Bitbucket Server, or GitLab., which needs to be able to communicate with the Control Plane.

**To install Shippable Server as a Onebox pilot, you will need to provision -**

* One Control Plane Server
* One Build Plane Server

<img src="/images/platform/tutorial/server/admiral-onebox.jpg" alt="Admiral-github" height=512px>

## Control Plane Server requirements

### OS versions

We support the following Operating systems:

- Ubuntu 16.04 LTS
- Ubuntu 14.04 LTS
- CentOS 7
- RHEL 7

### Machine Requirements

You will need a machine/VM with:

- 4 cores
- 8 GB memory
- 100 GB disk space

The minimum requirements for a VM on AWS for example is a [C4.xLarge](https://aws.amazon.com/ec2/instance-types/) machine.

### Ports to open

- 22: ssh into the machine
- 5672: amqp
- 50000: Shippable api
- 50001: Shippable post-login ui
- 50003: Shippable installer webapp

## Build Plane Server requirements

### OS versions

We support the following Operating systems:

- Ubuntu 16.04 LTS
- Ubuntu 14.04 LTS
- CentOS 7
- RHEL 7

### Machine Requirements

You will need a machine/VM with:

- 2 cores
- 4 GB memory
- 30 GB disk space

The minimum requirements for a VM on AWS for example is a [T2.medium](https://aws.amazon.com/ec2/instance-types/) machine.

### Ports to open

- 22: ssh into the machine

## Checklist prior to starting installation

* Ensure that you have two machines which meet the minimum hardware and OS requirements.
* Ensure that you have Shippable Server Installer access and secret key. **If you do not have an access key and secret ket, please contact us [via email](mailto:support@shippable.com) or through the [Server contact form](https://www.shippable.com/enterprise.html#shippable-server-contact).**
* SCM OAuth configuration
    * We suggest that OAuth configuration be completed before you start server installation.
    * You can use your own SCM account to complete the OAuth configuration. Please contact your SCM Admin if you do not have permissions to create an OAuth application.
    * If other users use the Shippable pilot, they will see your account name while signing in for the first time during the OAuth process asking for permissions, if you use your SCM account for the above.
    * The steps to configure each supported SCM are documented [here](http://docs.shippable.com/platform/server/auth-source-control/). For BitBucket Server only, please contact your SCM admin to [install add-ons](/platform/server/auth-source-control/#install-add-ons).
* Ensure that you install Shippable server on the the Control plane server on a private IP.
* Ensure that the ports mentioned above are open on the Control plane and Build plane server.
* Communication between the Control plane server and SCM server
The important question to answer here is whether the Control plane server will run in the same subnet as the SCM server.
If the subnet is the same, then traffic between the two servers is already secured by default. For different subnets, it is important to secure the communication between the core services and the SCM.
    * Run the SCM on a secure port
    * Install a publicly trusted certificate on the SCM
    * Verify that the SCM server is accessible from the Control plane server by running curl or telnet commands.
        * GitHub connectivity test: `curl https://api.github.com`
        * GitHub enterprise connectivity test: `curl https://my.github.com/api/v3.`
        * BitBucket Cloud connectivity test: `curl https://api.bitbucket.org`
        * BitBucket Server connectivity test: `curl https://my.bitbucket.com/`
        * GitLab 9.0 and above connectivity test: `curl https://my.gitlab.com/api/v4`
        * GitLab 8.17 or earlier and above connectivity test: `curl https://my.gitlab.com/api/v3`

* The Build plane server sends traffic to ports 50000 and 5672 on the Control plane server. Verify that the Control plane server is accessible from the Build plane server using ping or ssh.


## Install Shippable Server

###1. Install Admiral CLI
SSH into the machine where you are installing Admiral and run the following commands.

```
$ git clone https://github.com/Shippable/admiral.git
$ cd admiral
$ git checkout v6.3.4
```

You will see the following output:

```
$ ubuntu@ip-172-31-29-44:~$ git clone Cloning into 'admiral'...
remote: Counting objects: 15276, done.
remote: Compressing objects: 100% (97/97), done.
remote: Total 15276 (delta 85), reused 105 (delta 58), pack-reused 15121
Receiving objects: 100% (15276/15276), 13.10 MiB | 8.18 MiB/s, done.
Resolving deltas: 100% (9106/9106), done.
Checking connectivity... done.
ubuntu@ip-172-31-29-44:~$ cd admiral/
ubuntu@ip-172-31-4-17:~/admiral$ git checkout v6.3.4
HEAD is now at 9018791... updating version.txt to v6.3.4
```

We have checked out tag v6.3.4, which is the latest tag available as of March 2018. To see the complete list of versions and install a specific version, you can run `git tag` to see all the tags and then checkout the tag you want. Please note that versions more recent that v6.3.4, will be available April 2018 onwards and we recommend installing the latest version (which you can find out by running `git tag`).

###2. Run Admiral CLI

Ensure you have the installer access key, secret and public ip address of the state machine.
Admiral will install the postgres database in this step and download all the Shippable docker images.

Run the following command:

```
sudo ./admiral.sh install --onebox --installer-access-key <access_key> --installer-secret-key <secret_key>
```

Follow the steps below:

```
root@802727a5be4e:/admiral# sudo ./admiral.sh install --onebox --installer-access-key XXXXXXXXXXXXXXXX --installer-secret-key XXXXXXXXXXXXXXXXXXXX
|___ ADMIRAL_ENV does not exist, creating
'/admiral/common/scripts/configs/admiral.env.template' -> '/etc/shippable/admiral.env'
|___ Successfully created admiral env
|___ SSH keys not available, generating
|___ SSH keys successfully generated

# 22:27:15 #######################################
# Shippable Server Software License Agreement
##################################################
|___ BY INSTALLING OR USING THE SOFTWARE, YOU ARE CONFIRMING THAT YOU UNDERSTAND THE SHIPPABLE SERVER SOFTWARE LICENSE AGREEMENT, AND THAT YOU ACCEPT ALL OF ITS TERMS AND CONDITIONS.
|___ This agreement is available for you to read here: /admiral/SHIPPABLE_SERVER_LICENSE_AGREEMENT
|___ Do you wish to continue? (Y to confirm)
```

Choose `Y`. The installation will continue.

```
Y
|___ Thank you for accepting the Shippable Server Software License Agreement. Continuing with installation.

# 22:27:16 #######################################
# Validating runtime
##################################################
|___ Using release: master
|___ Updating RELEASE in ADMIRAL_ENV file
|___ LOGIN_TOKEN not defined, generating
|___ Generating login token
|___ Successfully generated login token
|___ Updating LOGIN_TOKEN in ADMIRAL_ENV file
|___ LOGIN_TOKEN updated in ADMIRAL_ENV file
|___ Services: /admiral/common/scripts/configs/services.json

# 22:27:16 #######################################
# Collecting required information
##################################################
|___ ACCESS_KEY already set, skipping
|___ SECRET_KEY already set, skipping
|___ ONEBOX_MODE already set, skipping
|___ ADMIRAL_IP not set
|___ Setting value of admiral IP address
|___ List of your machine IP addresses including default:
|___ 1 IP addresses available for the host
|___ 1 - (127.0.0.1)
|___ 2 - (172.17.0.2)
|___ Please pick one of the IP addresses from the above list by selecting the index (1, 2, etc) or enter a custom IP
```

Choose of the above IP addresses or another IP.

```
|___ 172.17.0.2 was selected as admiral IP
|___ Admiral IP is set to 172.17.0.2
|___ DB_IP already set, skipping
|___ DB_INSTALLED already set, skipping
|___ DB_PORT already set, skipping
|___ DB_PASSWORD is not set
|___ Setting database password
|___ Please enter the password for your database. This password will also be used for gitlab & rabbitmq, by default. This can be changed from the Admiral UI, if required.
```

Enter a password that is easy to remember. Thereafter, Admiral will continue running for a bit and install a whole bunch of things. You will see a message like this once Admiral installation completes.

```
# 17:16:55 #######################################
# Booting Admiral UI
##################################################
|___ Checking if admiral is running
|___ Generating admiral ENV variables
|___ Generating admiral mounts
4737408e9638340ffe8be3465aaab2772281e6b35f19d9589c404ba883fa7111
|___ Admiral container successfully running
|___ Go to 35.172.229.57:50003 to access the admin panel
|___ Login Token: fcd956d9-e471-4811-91d5-13c663f63b66
|___ Command successfully completed!
```

Please note down the `Login Token` and the IP address/port of the admin panel. You can run `sudo ./admiral.sh info` at any time to retrieve your Login token and Admiral URL.

###3. Initialize Control pane

*  First, you need to connect to the Admiral web app in your browser by entering IP address and port, for example `http://35.172.229.57:50003`. It might be a good idea to bookmark this Admiral URL.

* Enter the admin token.

<img src="/images/platform/tutorial/server/admiral-login.png">

* You will see a page like this:

<img src="/images/platform/tutorial/server/controlpane-1.png">

* Scroll to the bottom of the page and click `Apply`.

<img src="/images/platform/tutorial/server/controlpane-2.png">

* Click `Yes`.

<img src="/images/platform/tutorial/server/controlpane-accept.png">

* Admiral will now start initializing all the components such as Database, Redis etc.

<img src="/images/platform/tutorial/server/controlpane-3.png">

* If you scroll up, you can see the state of all the components in the `Summary` panel.

<img src="/images/platform/tutorial/server/controlpane-4.png">

* Once initialization is complete, you should see `Initialized` status for these sections (after collapsing those section panes).

<img src="/images/platform/tutorial/server/controlpane-5.png">

If anything shows **failed** status, read the [**Troubleshooting**](#troubleshooting) section at the bottom of this page for tips. If you still run into issues, please contact us [over email](mailto:support@shippable.com) or through a [github issue](https://github.com/Shippable/support/issues).

* Expand the **Authorization and Source Control Management (SCM) Providers** section. You will need to configure one
authorization provider.

* For **GitHub**, follow instructions on the [GitHub configuration page](/platform/server/auth-source-control/#github)

* For **GitHub Enterprise**, follow instructions on the [GitHub Enterprise configuration page](/platform/server/auth-source-control/#github-enterprise)

* For **Bitbucket Server**, follow instructions on the [Bitbucket Server configuration page](/platform/server/auth-source-control/#bitbucket-server)

* For **Bitbucket Cloud**, follow instructions on the [Bitbucket Cloud configuration page](/platform/server/auth-source-control/#bitbucket-cloud).

* For **Gitlab (Cloud, CE, EE)**, follow instructions on the [GitLab configuration page](/platform/server/auth-source-control/#gitlab-cloud-ce-ee).

* Click **Apply**.

###3. Initialize Build pane

* Click on `Build plane` in the left navigation bar.

* Expand the `BUILD CONFIGURATION` panel and change any settings you might want to change. The default settings will also work.

<img src="/images/platform/tutorial/server/buildplane-1.png">

* You need nodes to run your jobs. For more on different node types and detailed instructions on choosing and configuring a type, please read our docs on [Choosing and configuring a node type](/platform/server/build-config/#choosing-node-types)

* Choose the appropriate `Default cluster type`, which is the operating system and version of your build nodes. For example, if you want to build your repositories on a Ubuntu 16.04 build machine, you would choose `custom__x86_64_Ubuntu_16.04`. Do not choose any type starting with `dynamic`.

<img src="/images/platform/tutorial/server/buildplane-cluster.png">

* Scroll down and set `default build timeout` in the `Settings` panel.

 <img src="/images/platform/server/shippable-server-timeout.png" alt="Admiral-2-server">

This is the default time(in milliseconds) after which you want your CI and runSh jobs to timeout. This timeout is used when your [CI project](/platform/management/project/settings/), [runSh job](/platform/workflow/job/runsh/), the [Node Pool](/platform/management/subscription/node-pools/) on which the CI or runSh job is running and [Subscription](/platform/management/subscription/settings/) do not have any timeout specified.
Timeout values are given preference in the order `Job(CI or runSH) level timeout > Node Pool level timeout > Subscription level timeout > Default timeout`.

**NOTE:** Please note that the actual default timeout value applied to your jobs is **twice** of the value you specified in the setting above.

* Click on `Save and Restart services` in the left navigation bar.

<img src="/images/platform/tutorial/server/buildplane-2.png">

* Click `Yes`.

<img src="/images/platform/tutorial/server/buildplane-3.png">

###4. Configure add-ons

If you want your users to be able to connect to third party services through [integrations](/platform/integration/overview), you need to configure them in this section.

* Click on **Add-ons**

<img src="/images/platform/tutorial/server/addons-1.png">

* Expand the `CLOUD PROVIDERS`, `NOTIFICATIONS`, `REGISTRIES` or `OTHERS` panels and choose the integrations you want to enable.

<img src="/images/platform/tutorial/server/addons-2.png">
<img src="/images/platform/tutorial/server/addons-3.png">
<img src="/images/platform/tutorial/server/addons-4.png">

* You can turn on caching by following the steps below:
    * Expand the `OTHERS` panel.
    * Toggle `Enable build artifact caching using AWS S3`.
    * Enter your AWS Access and Secret Keys.

<img src="/images/platform/tutorial/server/addons-caching.png">

To learn more about the benefits of caching, go [here](/platform/runtime/caching/#caching).

* Click **Install Add-ons**.

###5. Setup superuser

* You will first need to log in to Shippable Server with your Admin account for the source control provider. To find the Server URL, look in the **Control plane->UI** section of the Admiral UI:

<img src="/images/platform/tutorial/server/shippable-server-url.png" alt="Admiral-2-server">

* From the Shippable Server URL, sign in to Shippable Server with your Admin account for the SCM provider, and authorize the application. You will need to click **Authorize** twice.

* You should see your team/organizations/repositories sync and appear under the **Subscriptions** section of the left navbar.

<img src="/images/platform/tutorial/server/shippable-left-navbar.png" alt="Admiral-2-server">

* Click **Profile** in the left sidebar and note down the `Account Id` at the bottom of the screen.

<img src="/images/platform/admiral/Admiral-accountid.png" alt="Admiral-github">

* Switch back to Admiral UI and click on **System Settings->Manage System SuperUsers (+ icon)** section.

<img src="/images/platform/server/admiral-system-settings.png" alt="admiral-system-settings">

* Paste the Account id and click **Add**.

<img src="/images/platform/tutorial/server/systemsettings-2.png" alt="Admiral-github">

###6. Create a Shared Node pool to run builds

* Switch back to the Shippable Server UI in your browser and click refresh. You will now see an `Admin` dropdown at the bottom of the left navigation bar.

<img src="/images/platform/server/admin-dropdown.png" height="512px" alt="Admiral-github">

* Expand `Admin->Nodes` and click on `Shared Node pools`.

<img src="/images/platform/server/shared-node-pool-option.png" height="512px" alt="Admiral-github">

* Click on the `+` button on the top right corner of the screen.

<img src="/images/platform/server/shared-node-pool.png" alt="Admiral-github">

* Create a Node pool using instructions found [here](/platform/management/subscription/node-pools/#creating-a-node-pool).

* Click on the `Add node` button to add a build node using instructions found [here](http://docs.shippable.com/platform/management/subscription/node-pools/#creating-a-node-pool).

* Now you can [run a sample CI project](/getting-started/ci-sample/), [setup Continuous Delivery](/getting-started/cd-sample/) or [enable your first CI project for your repository](/ci/enable-project/).

## Advanced options

- [Database (Postgres) config](/platform/server/install-db)
- [Secrets (Vault) config](/platform/server/install-vault)
- [Redis config](/platform/server/install-redis)
- [RabbitMQ config](/platform/server/install-rabbitmq)
- [Swarm config](/platform/server/install-swarm-workers)
- [Managing node types](/platform/server/build-config/#choosing-node-types)
