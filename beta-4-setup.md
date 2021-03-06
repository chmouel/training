<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [OpenShift Beta 4](#openshift-beta-4)
  - [Architecture and Requirements](#architecture-and-requirements)
    - [Architecture](#architecture)
    - [Requirements](#requirements)
  - [Setting Up the Environment](#setting-up-the-environment)
    - [Use a Terminal Window Manager](#use-a-terminal-window-manager)
    - [DNS](#dns)
    - [Assumptions](#assumptions)
    - [Git](#git)
    - [Preparing Each VM](#preparing-each-vm)
    - [Docker Storage Setup (optional, recommended)](#docker-storage-setup-optional-recommended)
    - [Grab Docker Images (optional, recommended)](#grab-docker-images-optional-recommended)
    - [Clone the Training Repository](#clone-the-training-repository)
    - [Add Development Users](#add-development-users)
  - [Ansible-based Installer](#ansible-based-installer)
    - [Install Ansible](#install-ansible)
    - [Generate SSH Keys](#generate-ssh-keys)
    - [Distribute SSH Keys](#distribute-ssh-keys)
    - [Clone the Ansible Repository](#clone-the-ansible-repository)
    - [Configure Ansible](#configure-ansible)
    - [Modify Hosts](#modify-hosts)
    - [Run the Ansible Installer](#run-the-ansible-installer)
    - [Add Cloud Domain](#add-cloud-domain)
  - [Regions and Zones](#regions-and-zones)
    - [Scheduler and Defaults](#scheduler-and-defaults)
    - [The NodeSelector](#the-nodeselector)
    - [Customizing the Scheduler Configuration](#customizing-the-scheduler-configuration)
    - [Node Labels](#node-labels)
  - [Useful OpenShift Logs](#useful-openshift-logs)
  - [Auth, Projects, and the Web Console](#auth-projects-and-the-web-console)
    - [Configuring htpasswd Authentication](#configuring-htpasswd-authentication)
    - [A Project for Everything](#a-project-for-everything)
    - [Web Console](#web-console)
  - [Your First Application](#your-first-application)
    - [Resources](#resources)
    - [Applying Quota to Projects](#applying-quota-to-projects)
    - [Applying Limit Ranges to Projects](#applying-limit-ranges-to-projects)
    - [Login](#login)
    - [Grab the Training Repo Again](#grab-the-training-repo-again)
    - [The Hello World Definition JSON](#the-hello-world-definition-json)
    - [Run the Pod](#run-the-pod)
    - [Looking at the Pod in the Web Console](#looking-at-the-pod-in-the-web-console)
    - [Quota Usage](#quota-usage)
    - [Extra Credit](#extra-credit)
    - [Delete the Pod](#delete-the-pod)
    - [Quota Enforcement](#quota-enforcement)
  - [Services](#services)
  - [Routing](#routing)
    - [Creating a Wildcard Certificate](#creating-a-wildcard-certificate)
    - [Creating the Router](#creating-the-router)
    - [Router Placement By Region](#router-placement-by-region)
    - [Viewing Router Stats](#viewing-router-stats)
  - [The Complete Pod-Service-Route](#the-complete-pod-service-route)
    - [Creating the Definition](#creating-the-definition)
    - [Project Status](#project-status)
    - [Verifying the Service](#verifying-the-service)
    - [Verifying the Routing](#verifying-the-routing)
    - [The Web Console](#the-web-console)
  - [Project Administration](#project-administration)
    - [Deleting a Project](#deleting-a-project)
  - [Preparing for S2I: the Registry](#preparing-for-s2i-the-registry)
    - [Storage for the registry](#storage-for-the-registry)
    - [Creating the registry](#creating-the-registry)
  - [S2I - What Is It?](#s2i---what-is-it)
    - [Create a New Project](#create-a-new-project)
    - [Switch Projects](#switch-projects)
    - [A Simple Code Example](#a-simple-code-example)
    - [CLI versus Console](#cli-versus-console)
    - [Adding the Builder ImageStreams](#adding-the-builder-imagestreams)
    - [Wait, What's an ImageStream?](#wait-whats-an-imagestream)
    - [Adding Code Via the Web Console](#adding-code-via-the-web-console)
    - [The Web Console Revisited](#the-web-console-revisited)
    - [Examining the Build](#examining-the-build)
    - [Testing the Application](#testing-the-application)
    - [Adding a Route to Our Application](#adding-a-route-to-our-application)
    - [Implications of Quota Enforcement on Scaling](#implications-of-quota-enforcement-on-scaling)
  - [Templates, Instant Apps, and "Quickstarts"](#templates-instant-apps-and-quickstarts)
    - [A Project for the Quickstart](#a-project-for-the-quickstart)
    - [A Quick Aside on Templates](#a-quick-aside-on-templates)
    - [Adding the Template](#adding-the-template)
    - [Create an Instance of the Template](#create-an-instance-of-the-template)
    - [Using Your App](#using-your-app)
  - [Creating and Wiring Disparate Components](#creating-and-wiring-disparate-components)
    - [Create a New Project](#create-a-new-project-1)
    - [Stand Up the Frontend](#stand-up-the-frontend)
    - [Expose the Service](#expose-the-service)
    - [Add the Database Template](#add-the-database-template)
    - [Create the Database From the Web Console](#create-the-database-from-the-web-console)
    - [Visit Your Application Again](#visit-your-application-again)
    - [Replication Controllers](#replication-controllers)
    - [Revisit the Webpage](#revisit-the-webpage)
  - [Rollback/Activate and Code Lifecycle](#rollbackactivate-and-code-lifecycle)
    - [Fork the Repository](#fork-the-repository)
    - [Update the BuildConfig](#update-the-buildconfig)
    - [Change the Code](#change-the-code)
- [ Welcome to an OpenShift v3 Demo App! ](#welcome-to-an-openshift-v3-demo-app)
- [ This is my crustom demo! ](#this-is-my-crustom-demo)
    - [Start a Build with a Webhook](#start-a-build-with-a-webhook)
    - [Rollback](#rollback)
    - [Activate](#activate)
  - [A Simple PHP Example](#a-simple-php-example)
    - [Create a PHP Project](#create-a-php-project)
    - [Build the App](#build-the-app)
    - [Create a Route](#create-a-route)
    - [Test Your App](#test-your-app)
    - [Kill Your Pod](#kill-your-pod)
  - [Using Persistent Storage (Optional)](#using-persistent-storage-optional)
    - [Export an NFS Volume](#export-an-nfs-volume)
    - [NFS Firewall](#nfs-firewall)
    - [Allow NFS Access in SELinux Policy](#allow-nfs-access-in-selinux-policy)
    - [Create a PersistentVolume](#create-a-persistentvolume)
    - [Claim the PersistentVolume](#claim-the-persistentvolume)
    - [Use the Claimed Volume](#use-the-claimed-volume)
    - [Revisit and Reupload](#revisit-and-reupload)
    - [Kill the Pod](#kill-the-pod)
  - [More Exec Examples](#more-exec-examples)
    - [Introduction to exec](#introduction-to-exec)
  - [Customized Build and Run Processes](#customized-build-and-run-processes)
    - [Add a Script](#add-a-script)
    - [Kick Off a Build](#kick-off-a-build)
    - [Watch the Build Logs](#watch-the-build-logs)
  - [Lifecycle Pre and Post Deployment Hooks](#lifecycle-pre-and-post-deployment-hooks)
    - [Examining Deployment Hooks](#examining-deployment-hooks)
    - [Modifying the Hooks](#modifying-the-hooks)
    - [Quickly Clean Up](#quickly-clean-up)
    - [Build Again](#build-again)
    - [Verify the Migration](#verify-the-migration)
  - [Arbitrary Docker Image (Builder)](#arbitrary-docker-image-builder)
    - [Create a Project](#create-a-project)
    - [Build Wordpress](#build-wordpress)
    - [Test Your Application](#test-your-application)
    - [Application Resource Labels](#application-resource-labels)
  - [EAP Example](#eap-example)
    - [Create a Project](#create-a-project-1)
    - [Instantiate the Template](#instantiate-the-template)
    - [Update the BuildConfig](#update-the-buildconfig-1)
    - [Watch the Build](#watch-the-build)
    - [Visit Your Application](#visit-your-application)
  - [Conclusion](#conclusion)
- [APPENDIX - DNSMasq setup](#appendix---dnsmasq-setup)
    - [Verifying DNSMasq](#verifying-dnsmasq)
- [APPENDIX - LDAP Authentication](#appendix---ldap-authentication)
    - [Prerequirements:](#prerequirements)
    - [Setting up an example LDAP server:](#setting-up-an-example-ldap-server)
    - [Creating the Basic Auth service](#creating-the-basic-auth-service)
    - [Using an LDAP server external to OpenShift](#using-an-ldap-server-external-to-openshift)
    - [Upcoming changes](#upcoming-changes)
- [APPENDIX - Import/Export of Docker Images (Disconnected Use)](#appendix---importexport-of-docker-images-disconnected-use)
- [APPENDIX - Cleaning Up](#appendix---cleaning-up)
- [APPENDIX - Pretty Output](#appendix---pretty-output)
- [APPENDIX - Troubleshooting](#appendix---troubleshooting)
- [APPENDIX - Infrastructure Log Aggregation](#appendix---infrastructure-log-aggregation)
  - [Enable Remote Logging on Master](#enable-remote-logging-on-master)
  - [Enable logging to /var/log/openshift](#enable-logging-to-varlogopenshift)
  - [Configure nodes to send openshift logs to your master](#configure-nodes-to-send-openshift-logs-to-your-master)
  - [Optionally Log Each Node to a unique directory](#optionally-log-each-node-to-a-unique-directory)
- [APPENDIX - JBoss Tools for Eclipse](#appendix---jboss-tools-for-eclipse)
  - [Installation](#installation)
  - [Connecting to the Server](#connecting-to-the-server)
- [APPENDIX - Working with HTTP Proxies](#appendix---working-with-http-proxies)
  - [Importing ImageStreams](#importing-imagestreams)
  - [S2I Builds](#s2i-builds)
  - [Setting Environment Variables in Pods](#setting-environment-variables-in-pods)
  - [Git Repository Access](#git-repository-access)
  - [Proxying Docker Pull](#proxying-docker-pull)
  - [Future Considerations](#future-considerations)
- [APPENDIX - Installing in IaaS Clouds](#appendix---installing-in-iaas-clouds)
  - [Generic Cloud Install](#generic-cloud-install)
  - [Automated AWS Install With Ansible](#automated-aws-install-with-ansible)
- [APPENDIX - Linux, Mac, and Windows clients](#appendix---linux-mac-and-windows-clients)
  - [Downloading The Clients](#downloading-the-clients)
  - [Log In To Your OpenShift Environment](#log-in-to-your-openshift-environment)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# OpenShift Beta 4
## Architecture and Requirements
### Architecture
The documented architecture for the beta testing is pretty simple. There are
three systems:

* Master + Node
* Node
* Node

The master is the scheduler/orchestrator and the API endpoint for all commands.
This is similar to V2's "broker". We are also running the node software on the
master.

The "node" is just like in OpenShift 2 -- it hosts user applications. The main
difference is that "gears" have been replaced with Docker container instances.
You will learn much more about the inner workings of OpenShift throughout the
rest of the document.

### Requirements
Each of the virtual machines should have 4+ GB of memory, 20+ GB of disk space,
and the following configuration:

* RHEL >=7.1 (Note: 7.1 kernel is required for openvswitch)
* "Minimal" installation option

The majority of storage requirements are related to Docker and etcd (the data
store). Both of their contents live in /var, so it is recommended that the
majority of the storage be allocated to /var.

As part of signing up for the beta program, you should have received an
evaluation subscription. This subscription gave you access to the beta software.
You will need to use subscription manager to both register your VMs, and attach
them to the *OpenShift Enterprise High Touch Beta* subscription.

All of your VMs should be on the same logical network and be able to access one
another.

In almost all cases, when referencing VMs you must use hostnames and the
hostnames that you use must match the output of `hostname -f` on each of your
nodes. Forward DNS resolution of hostnames is an **absolute requirement**. This
training document assumes the following configuration:

* ose3-master.example.com (master+node)
* ose3-node1.example.com
* ose3-node2.example.com

We do our best to point out where you will need to change things if your
hostnames do not match.

If you cannot create real forward resolving DNS entries in your DNS system, you
will need to set up your own DNS server in the beta testing environment.
Documentation is provided on DNSMasq in an appendix, [APPENDIX - DNSMasq
setup](#appendix---dnsmasq-setup)

Remember that NetworkManager may make changes to your DNS
configuration/resolver/etc. You will need to properly configure your interfaces'
DNS settings and/or configure NetworkManager appropriately.

More information on NetworkManager can be found in this comment:

    https://github.com/openshift/training/issues/193#issuecomment-105693742

## Setting Up the Environment
### Use a Terminal Window Manager
We **strongly** recommend that you use some kind of terminal window manager
(Screen, Tmux).

### DNS
You will need to have a wildcard for a DNS zone resolve, ultimately, to the IP
address of the OpenShift router. For this training, we will ensure that the
router will end up on the OpenShift server that is running the master. Go
ahead and create a wildcard DNS entry for "cloudapps" (or something similar),
with a low TTL, that points to the public IP address of your master.

For example:

    *.cloudapps.example.com. 300 IN  A 192.168.133.2

It is possible to use dnsmasq inside of your beta environment to handle these
duties. See the [appendix on dnsmasq](#appendix---dnsmasq-setup) if you can't
easily manipulate your existing DNS environment.

### Assumptions
In most cases you will see references to "example.com" and other FQDNs related
to it. If you choose not to use "example.com" in your configuration, that is
fine, but remember that you will have to adjust files and actions accordingly.

### Git
You will either need internet access or read and write access to an internal
http-based git server where you will duplicate the public code repositories used
in the labs.

### Preparing Each VM
Once your VMs are built and you have verified DNS and network connectivity you
can:

* Configure yum / subscription manager as follows:

        subscription-manager repos --disable="*"
        subscription-manager repos \
        --enable="rhel-7-server-rpms" \
        --enable="rhel-7-server-extras-rpms" \
        --enable="rhel-7-server-optional-rpms" \
        --enable="rhel-server-7-ose-beta-rpms"

    **Note:** You will have had to register/attach your system first.  Also,
    *rhel-server-7-ose-beta-rpms* is not a typo.  The name will change at GA to be
    consistent with the RHEL channel names.

* Import the GPG key for beta:

        rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-beta

Onn **each** VM:

1. Install deltarpm to make package updates a little faster:

        yum -y install deltarpm

1. Install missing packages:

        yum -y install wget vim-enhanced net-tools bind-utils tmux git

1. Update:

        yum -y update

### Docker Storage Setup (optional, recommended)
**IMPORTANT:** The default docker storage configuration uses loopback devices
and is not appropriate for production. Red Hat considers the dm.thinpooldev
storage option to be the only appropriate configuration for production use.

If you want to configure the storage for Docker, you'll need to first install
Docker, as the installer currently does not auto-configure this storage setup
for you.

    yum -y install docker

Make sure that you are running at least `docker-1.6.2-6.el7.x86_64`.

In order to use dm.thinpooldev you must have an LVM thinpool available, the
`docker-storage-setup` package will assist you in configuring LVM. However you
must provision your host to fit one of these three scenarios :

*  Root filesystem on LVM with free space remaining on the volume group. Run
`docker-storage-setup` with no additional configuration, it will allocate the
remaining space for the thinpool.

*  A dedicated LVM volume group where you'd like to reate your thinpool

        echo <<EOF > /etc/sysconfig/docker-storage-setup
        VG=docker-vg
        SETUP_LVM_THIN_POOL=yes
        EOF
        docker-storage-setup

*  A dedicated block device, which will be used to create a volume group and thinpool

        cat <<EOF > /etc/sysconfig/docker-storage-setup
        DEVS=/dev/vdc
        VG=docker-vg
        SETUP_LVM_THIN_POOL=yes
        EOF
        docker-storage-setup

Once complete you should have a thinpool named `docker-pool` and docker should
be configured to use it in `/etc/sysconfig/docker-storage`.

    # lvs
    LV                  VG        Attr       LSize  Pool Origin Data%  Meta% Move Log Cpy%Sync Convert
    docker-pool         docker-vg twi-a-tz-- 48.95g             0.00   0.44

    # cat /etc/sysconfig/docker-storage
    DOCKER_STORAGE_OPTIONS=--storage-opt dm.fs=xfs --storage-opt dm.thinpooldev=/dev/mapper/openshift--vg-docker--pool

**Note:** If you had previously used docker with loopback storage you should
clean out `/var/lib/docker` This is a destructive operation and will delete all
images and containers on the host.

    systemctl stop docker
    rm -rf /var/lib/docker/*
    systemctl start docker

### Grab Docker Images (optional, recommended)
**If you want** to pre-fetch Docker images to make the first few things in your
environment happen **faster**, you'll need to first install Docker if you didn't
install it when (optionally) configuring the Docker storage previously.

    yum -y install docker

Make sure that you are running at least `docker-1.6.2-6.el7.x86_64`.

You'll need to add `--insecure-registry 0.0.0.0/0` to your
`/etc/sysconfig/docker` `OPTIONS`. Then:

    systemctl start docker

On all of your systems, grab the following docker images:

    docker pull registry.access.redhat.com/openshift3_beta/ose-haproxy-router:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-deployer:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-sti-builder:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-sti-image-builder:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-docker-builder:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-pod:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/ose-docker-registry:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/sti-basicauthurl:latest
    docker pull registry.access.redhat.com/openshift3_beta/ose-keepalived-ipfailover:v0.5.2.2

It may be advisable to pull the following Docker images as well, since they are
used during the various labs:

    docker pull registry.access.redhat.com/openshift3_beta/ruby-20-rhel7:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/mysql-55-rhel7:v0.5.2.2
    docker pull registry.access.redhat.com/openshift3_beta/php-55-rhel7:v0.5.2.2
    docker pull registry.access.redhat.com/jboss-eap-6/eap-openshift:v0.5.2.2
    docker pull openshift/hello-openshift:v0.4.3

**Note:** If you built your VM for a previous beta version and at some point
used an older version of Docker, you need to *reinstall* or *remove+install*
Docker after removing `/etc/sysconfig/docker`. The options in the config file
changed and RPM will not overwrite your existing file if you just do a "yum
update".

    yum -y remove docker
    rm /etc/sysconfig/docker*
    yum -y install docker

### Clone the Training Repository
On your master, it makes sense to clone the training git repository:

    cd
    git clone https://github.com/openshift/training.git

**REMINDER**
Almost all of the files for this training are in the training folder you just
cloned.

### Add Development Users
In the "real world" your developers would likely be using the OpenShift tools on
their own machines (`osc` and the web console). For the Beta training, we
will create user accounts for two non-privileged users of OpenShift, *joe* and
*alice*, on the master. This is done for convenience and because we'll be using
`htpasswd` for authentication.

    useradd joe
    useradd alice

We will come back to these users later. Remember to do this on the `master`
system, and not the nodes.

## Ansible-based Installer
The installer uses Ansible. Eventually there will be an interactive text-based
CLI installer that leverages Ansible under the covers. For now, we have to
invoke Ansible manually.

### Install Ansible
Ansible currently comes from the EPEL repository.

Install EPEL:

    yum -y install \
    http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm

Disable EPEL so that it is not accidentally used later:

    sed -i -e "s/^enabled=1/enabled=0/" /etc/yum.repos.d/epel.repo

Install the packages for Ansible:

    yum -y --enablerepo=epel install ansible

### Generate SSH Keys
Because of the way Ansible works, SSH key distribution is required. First,
generate an SSH key on your master, where we will run Ansible:

    ssh-keygen

Do *not* use a password.

### Distribute SSH Keys
An easy way to distribute your SSH keys is by using a `bash` loop:

    for host in ose3-master.example.com ose3-node1.example.com \
    ose3-node2.example.com; do ssh-copy-id -i ~/.ssh/id_rsa.pub \
    $host; done

Remember, if your FQDNs are different, you would have to modify the loop
accordingly.

### Clone the Ansible Repository
The configuration files for the Ansible installer are currently available on
Github. Clone the repository:

    cd
    git clone https://github.com/detiber/openshift-ansible.git -b v3-beta4
    cd ~/openshift-ansible

### Configure Ansible
Copy the staged Ansible configuration files to `/etc/ansible`:

    /bin/cp -r ~/training/beta4/ansible/* /etc/ansible/

### Modify Hosts
If you are not using the "example.com" domain and the training example
hostnames, modify `/etc/ansible/hosts` accordingly. 

Also, if you are using multiple NICs and will be trying to direct various
traffic to different places, you will need to take a look at [Generic Cloud
Install](#generic-cloud-install) to learn more about the syntax of Ansible's
`hosts` file.

### Run the Ansible Installer
Now we can simply run the Ansible installer:

    ansible-playbook ~/openshift-ansible/playbooks/byo/config.yml

If you looked at the Ansible hosts file, note that our master
(ose3-master.example.com) was present in both the `master` and the `node`
section.

Effectively, Ansible is going to install and configure node software on all the
nodes and master software just on `ose3-master.example.com` .

### Add Cloud Domain
If you want default routes (we'll talk about these later) to automatically get
the right domain (the one you configured earlier with your wildcard DNS), then
you should edit `/etc/sysconfig/openshift-master` and add the following:

    OPENSHIFT_ROUTE_SUBDOMAIN=cloudapps.example.com

Or modify it appropriately for your domain.

There was also some information about "regions" and "zones" in the hosts file.
Let's talk about those concepts now.

## Regions and Zones
If you think you're about to learn how to configure regions and zones in
OpenShift 3, you're only partially correct.

In OpenShift 2, we introduced the specific concepts of "regions" and "zones" to
enable organizations to provide some topologies for application resiliency. Apps
would be spread throughout the zones in a region and, depending on the way you
configured OpenShift, you could make different regions accessible to users.

The reason that you're only "partially" correct in your assumption is that, for
OpenShift 3, Kubernetes doesn't actually care about your topology. In other
words, OpenShift is "topology agnostic". In fact, OpenShift 3 provides advanced
controls for implementing whatever topologies you can dream up, leveraging
filtering and affinity rules to ensure that parts of applications (pods) are
either grouped together or spread apart.

For the purposes of a simple example, we'll be sticking with the "regions" and
"zones" theme. But, as you go through these examples, think about what other
complex topologies you could implement. Perhaps "secure" and "insecure" hosts,
or other topologies.

First, we need to talk about the "scheduler" and its default configuration.

### Scheduler and Defaults
The "scheduler" is essentially the OpenShift master. Any time a pod needs to be
created (instantiated) somewhere, the master needs to figure out where to do
this. This is called "scheduling". The default configuration for the scheduler
looks like the following JSON (although this is embedded in the OpenShift code
and you won't find this in a file):

    {
      "predicates" : [
        {"name" : "PodFitsResources"},
        {"name" : "MatchNodeSelector"},
        {"name" : "HostName"},
        {"name" : "PodFitsPorts"},
        {"name" : "NoDiskConflict"}
      ],"priorities" : [
        {"name" : "LeastRequestedPriority", "weight" : 1},
        {"name" : "ServiceSpreadingPriority", "weight" : 1}
      ]
    }

When the scheduler tries to make a decision about pod placement, first it goes
through "predicates", which essentially filter out the possible nodes we can
choose. Note that, depending on your predicate configuration, you might end up
with no possible nodes to choose. This is totally OK (although generally not
desired).

These default options are documented in the link above, but the quick overview
is:

* Place pod on a node that has enough resources for it (duh)
* Place pod on a node that doesn't have a port conflict (duh)
* Place pod on a node that doesn't have a storage conflict (duh)

And some more obscure ones:

* Place pod on a node whose `NodeSelector` matches
* Place pod on a node whose hostname matches the `Host` attribute value

The next thing is, of the available nodes after the filters are applied, how do
we select the "best" one. This is where "priorities" come in. Long story short,
the various priority functions each get a score, multiplied by the weight, and
the node with the highest score is selected to host the pod.

Again, the defaults are:

* Choose the node that is "least requested" (the least busy)
* Spread services around - minimize the number of pods in the same service on
    the same node

And, for an extremely detailed explanation about what these various
configuration flags are doing, check out:

    http://docs.openshift.org/latest/admin_guide/scheduler.html

In a small environment, these defaults are pretty sane. Let's look at one of the
important predicates (filters) before we move on to "regions" and "zones".

### The NodeSelector
`NodeSelector` is a part of the Pod data model. And, if we think back to our pod
definition, there was a "label", which is just a key:value pair. In the case of
a `NodeSelector`, our labels (key:value pairs) are used to help us try to find
nodes that match, assuming that:

* The scheduler is configured to MatchNodeSelector
* The end user creating the pod knows which labels are out there

But this use case is also pretty simplistic. It doesn't really allow for a
topology, and there's not a lot of logic behind it. Also, if I specify a
NodeSelector label when using MatchNodeSelector and there are no matching nodes,
my workload will never get scheduled. Bummer.

How can we make this more intelligent? We'll finally use "regions" and "zones".

### Customizing the Scheduler Configuration
The Ansible installer is configured to understand "regions" and "zones" as a
matter of convenience. However, for the master (scheduler) to actually do
something with them requires changing from the default configuration Take a look
at `/etc/openshift/master/master-config.yaml` and find the line with `schedulerConfigFile`.

You should see:

    schedulerConfigFile: "/etc/openshift/master/scheduler.json"

Then, take a look at `/etc/openshift/master/scheduler.json`. It will have the
following content:

    {
      "predicates" : [
        {"name" : "PodFitsResources"},
        {"name" : "PodFitsPorts"},
        {"name" : "NoDiskConflict"},
        {"name" : "Region", "argument" : {"serviceAffinity" : { "labels" : ["region"]}}}
      ],"priorities" : [
        {"name" : "LeastRequestedPriority", "weight" : 1},
        {"name" : "ServiceSpreadingPriority", "weight" : 1},
        {"name" : "Zone", "weight" : 2, "argument" : {"serviceAntiAffinity" : { "label" : "zone" }}}
      ]
    }

To quickly review the above (this explanation sort of assumes that you read the
scheduler documentation, but it's not critically important):

* Filter out nodes that don't fit the resources, don't have the ports, or have
    disk conflicts
* If the pod specifies a label with the key "region", filter nodes by the value.

So, if we have the following nodes and the following labels:

* Node 1 -- "region":"infra"
* Node 2 -- "region":"primary"
* Node 3 -- "region":"primary"

If we try to schedule a pod that has a `NodeSelector` of "region":"primary",
then only Node 1 and Node 2 would be considered.

OK, that takes care of the "region" part. What about the "zone" part?

Our priorities tell us to:

* Score the least-busy node higher
* Score any nodes who don't already have a pod in this service higher
* Score any nodes whose zone label's value **does not** match higher

Why do we score a zone that **doesn't** match higher? Note that the definition
for the Zone priority is a `serviceAntiAffinity` -- anti affinity. In this case,
our anti affinity rule helps to ensure that we try to get nodes from *different*
zones to take our pod.

If we consider that our "primary" region might be a certain datacenter, and that
each "zone" in that datacenter might be on its own power system with its own
dedicated networking, this would ensure that, within the datacenter, pods of an
application would be spread across power/network segments.

The documentation link has some more complicated examples. The topoligical
possibilities are endless!

### Node Labels
The assignments of "regions" and "zones" at the node-level are handled by labels
on the nodes. You can look at how the labels were implemented by doing:

    osc get nodes

    NAME                      LABELS                                                                     STATUS
    ose3-master.example.com   kubernetes.io/hostname=ose3-master.example.com,region=infra,zone=default   Ready
    ose3-node1.example.com    kubernetes.io/hostname=ose3-node1.example.com,region=primary,zone=east     Ready
    ose3-node2.example.com    kubernetes.io/hostname=ose3-node2.example.com,region=primary,zone=west     Ready

At this point we have a running OpenShift environment across three hosts, with
one master and three nodes, divided up into two regions -- "*infra*structure"
and "primary".

From here we will start to deploy "applications" and other resources into
OpenShift.

## Useful OpenShift Logs
RHEL 7 uses `systemd` and `journal`. As such, looking at logs is not a matter of
`/var/log/messages` any longer. You will need to use `journalctl`.

Since we are running all of the components in higher loglevels, it is suggested
that you use your terminal emulator to set up windows for each process. If you
are familiar with the Ruby Gem, `tmuxinator`, there is a config file in the
training repository. Otherwise, you should run each of the following in its own
window:

    journalctl -f -u openshift-master
    journalctl -f -u openshift-node

**Note:** You will want to do this on the other nodes, but you won't need the
"-master" service. You may also wish to watch the Docker logs, too.

**Note:** There is an appendix on configuring [Log
Aggregation](#appendix---infrastructure-log-aggregation)

## Auth, Projects, and the Web Console
### Configuring htpasswd Authentication
OpenShift v3 supports a number of mechanisms for authentication. The simplest
use case for our testing purposes is `htpasswd`-based authentication.

To start, we will need the `htpasswd` binary, which is made available by
installing:

    yum -y install httpd-tools

From there, we can create a password for our users, Joe and Alice:

    touch /etc/openshift/openshift-passwd
    htpasswd -b /etc/openshift/openshift-passwd joe redhat
    htpasswd -b /etc/openshift/openshift-passwd alice redhat

Remember, you created these users previously.

The OpenShift configuration is kept in a YAML file which currently lives at
`/etc/openshift/master/master-config.yaml`. Ansible was configured to edit
the `oauthConfig`'s `identityProviders` stanza so that it looks like the following:

    identityProviders:
    - challenge: true
      login: true
      name: htpasswd_auth
      provider:
        apiVersion: v1
        file: /etc/openshift/openshift-passwd
        kind: HTPasswdPasswordIdentityProvider

More information on these configuration settings (and other identity providers) can be found here:

    http://docs.openshift.org/latest/admin_guide/configuring_authentication.html#HTPasswdPasswordIdentityProvider

### A Project for Everything
V3 has a concept of "projects" to contain a number of different resources:
services and their pods, builds and so on. They are somewhat similar to
"namespaces" in OpenShift v2. We'll explore what this means in more details
throughout the rest of the labs. Let's create a project for our first
application.

We also need to understand a little bit about users and administration. The
default configuration for CLI operations currently is to be the `master-admin`
user, which is allowed to create projects. We can use the "admin"
OpenShift command to create a project, and assign an administrative user to it:

    osadm new-project demo --display-name="OpenShift 3 Demo" \
    --description="This is the first demo project with OpenShift v3" \
    --admin=joe

This command creates a project:
* with the id `demo`
* with a display name
* with a description
* with an administrative user `joe` who can login with the password defined by
    htpasswd

Future use of command line statements will have to reference this project in
order for things to land in the right place.

Now that you have a project created, it's time to look at the web console, which
has been completely redesigned for V3.

### Web Console
Open your browser and visit the following URL:

    https://fqdn.of.master:8443

Be aware that it may take up to 90 seconds for the web console to be available
any time you restart the master.

On your first visit your browser will need to accept the self-signed SSL
certificate. You will then be asked for a username and a password. Remembering
that we created a user previously, `joe`, go ahead and enter that and use
the password (`redhat`) you set earlier.

Once you are in, click the *OpenShift 3 Demo* project. There really isn't
anything of interest at the moment, because we haven't put anything into our
project.

## Your First Application
At this point you essentially have a sufficiently-functional V3 OpenShift
environment. It is now time to create the classic "Hello World" application
using some sample code.  But, first, some housekeeping.

Also, don't forget, the materials for these labs are in your `~/training/beta4`
folder.

### Resources
There are a number of different resource types in OpenShift 3, and, essentially,
going through the motions of creating/destroying apps, scaling, building and
etc. all ends up manipulating OpenShift and Kubernetes resources under the
covers. Resources can have quotas enforced against them, so let's take a moment
to look at some example JSON for project resource quota might look like:

    {
      "apiVersion": "v1beta3",
      "kind": "ResourceQuota",
      "metadata": {
        "name": "test-quota"
      },
      "spec": {
        "hard": {
          "memory": "512Mi",
          "cpu": "200m",
          "pods": "3",
          "services": "3",
          "replicationcontrollers": "3",
          "resourcequotas": "1"
        }
      }
    }

The above quota (simply called *test-quota*) defines limits for several
resources. In other words, within a project, users cannot "do stuff" that will
cause these resource limits to be exceeded. Since quota is enforced at the
project level, it is up to the users to allocate resources (more specifically,
memory and CPU) to their pods/containers. OpenShift will soon provide sensible
defaults.

* Memory

    The memory figure is in bytes, but various other suffixes are supported (eg:
    Mi (mebibytes), Gi (gibibytes), etc.

* CPU

    CPU is a little tricky to understand. The unit of measure is actually a
    "Kubernetes Compute Unit" (KCU, or "kookoo"). The KCU is a "normalized" unit
    that should be roughly equivalent to a single hyperthreaded CPU core.
    Fractional assignment is allowed. For fractional assignment, the
    **m**illicore may be used (eg: 200m = 0.2 KCU)

More details on CPU will come in later betas and documentation.

We will get into a description of what pods, services and replication
controllers are over the next few labs. Lastly, we can ignore "resourcequotas",
as it is a bit of a trick so that Kubernetes doesn't accidentally try to apply
two quotas to the same namespace.

### Applying Quota to Projects
At this point we have created our "demo" project, so let's apply the quota above
to it. Still in a `root` terminal in the `training/beta4` folder:

    osc create -f quota.json --namespace=demo

If you want to see that it was created:

    osc get -n demo quota
    NAME
    test-quota

And if you want to verify limits or examine usage:

    osc describe quota test-quota -n demo
    Name:                   test-quota
    Resource                Used    Hard
    --------                ----    ----
    cpu                     0m      200m
    memory                  0       512Mi
    pods                    0       3
    replicationcontrollers  0       3
    resourcequotas          1       1
    services                0       3

If you go back into the web console and click into the "OpenShift 3 Demo"
project, and click on the *Settings* tab, you'll see that the quota information
is displayed.

**Note:** Once creating the quota, it can take a few moments for it to be fully
processed. If you get blank output from the `get` or `describe` commands, wait a
few moments and try again.

### Applying Limit Ranges to Projects
In order for quotas to be effective you need to also create Limit Ranges
which set the maximum, minimum, and default allocations of memory and cpu at
both a pod and container level. Without default values for containers projects
with quotas will fail because the deployer and other infrastructure pods are
unbounded and therefore forbidden.

As `root` in the `training/beta4` folder:

    osc create -f limits.json --namespace=demo

Review your limit ranges

    osc describe limitranges limits -n demo
    Name:           limits
    Type            Resource        Min     Max     Default
    ----            --------        ---     ---     ---
    Pod             memory          5Mi     750Mi   -
    Pod             cpu             10m     500m    -
    Container       cpu             10m     500m    100m
    Container       memory          5Mi     750Mi   100Mi

### Login
Since we have taken the time to create the *joe* user as well as a project for
him, we can log into a terminal as *joe* and then set up the command line
tooling.

Open a terminal as `joe`:

    # su - joe

Then, execute:

    osc login -u joe \
    --certificate-authority=/etc/openshift/master/ca.crt \
    --server=https://ose3-master.example.com:8443

OpenShift, by default, is using a self-signed SSL certificate, so we must point
our tool at the CA file.

The `login` process created a file called named `~/.config/openshift/config`
folder. Take a look at it, and you'll see something like the following:

    apiVersion: v1
    clusters:
    - cluster:
        certificate-authority: ../../../../etc/openshift/master/ca.crt
        server: https://ose3-master.example.com:8443
      name: ose3-master-example-com:8443
    contexts:
    - context:
        cluster: ose3-master-example-com:8443
        namespace: demo
        user: joe/ose3-master-example-com:8443
      name: demo/ose3-master-example-com:8443/joe
    current-context: demo/ose3-master-example-com:8443/joe
    kind: Config
    preferences: {}
    users:
    - name: joe/ose3-master-example-com:8443
      user:
        token: _ebJfOdcHy8TW4XIDxJjOQEC_yJp08zW0xPI-JWWU3c

This configuration file has an authorization token, some information about where
our server lives, our project, etc.

**Note:** See the [troubleshooting guide](#appendix---troubleshooting) for
details on how to fetch a new token once this one expires.  The installer sets
the default token lifetime to 4 hours.

### Grab the Training Repo Again
Since Joe and Alice can't access the training folder in root's home directory,
go ahead and grab it inside Joe's home folder:

    cd
    git clone https://github.com/openshift/training.git
    cd ~/training/beta4

### The Hello World Definition JSON
In the beta4 training folder, you can see the contents of our pod definition by
using `cat`:

    cat hello-pod.json
    {
      "kind": "Pod",
      "apiVersion": "v1beta3",
      "metadata": {
        "name": "hello-openshift",
        "creationTimestamp": null,
        "labels": {
          "name": "hello-openshift"
        }
      },
      "spec": {
        "containers": [
          {
            "name": "hello-openshift",
            "image": "openshift/hello-openshift:v0.4.3",
            "ports": [
              {
                "hostPort": 36061,
                "containerPort": 8080,
                "protocol": "TCP"
              }
            ],
            "resources": {
              "limits": {
                "cpu": "10m",
                "memory": "16Mi"
              }
            },
            "terminationMessagePath": "/dev/termination-log",
            "imagePullPolicy": "IfNotPresent",
            "capabilities": {},
            "securityContext": {
              "capabilities": {},
              "privileged": false
            },
            "nodeSelector": {
              "region": "primary"
            }
          }
        ],
        "restartPolicy": "Always",
        "dnsPolicy": "ClusterFirst",
        "serviceAccount": ""
      },
      "status": {}
    }

In the simplest sense, a *pod* is an application or an instance of something. If
you are familiar with OpenShift V2 terminology, it is similar to a *gear*.
Reality is more complex, and we will learn more about the terms as we explore
OpenShift further.

### Run the Pod
As `joe`, to create the pod from our JSON file, execute the following:

    osc create -f hello-pod.json

Remember, we've "logged in" to OpenShift and our project, so this will create
the pod inside of it. The command should display the ID of the pod:

    pods/hello-openshift

Issue a `get pods` to see the details of how it was defined:

    osc get pods
    POD               IP         CONTAINER(S)      IMAGE(S)                           HOST                                   LABELS                 STATUS    CREATED      MESSAGE
    hello-openshift   10.1.1.2                                                        ose3-node1.example.com/192.168.133.3   name=hello-openshift   Running   16 seconds   
                                 hello-openshift   openshift/hello-openshift:v0.4.3                                                                 Running   2 seconds   

The output of this command shows all of the Docker containers in a pod, which
explains some of the spacing.

On the node where the pod is running (`HOST`), look at the list of Docker
containers with `docker ps` (in a `root` terminal) to see the bound ports.  We
should see an `openshift3_beta/ose-pod` container bound to 36061 on the host and
bound to 8080 on the container, along with several other `ose-pod` containers.

    CONTAINER ID        IMAGE                              COMMAND              CREATED             STATUS              PORTS                    NAMES
    ded86f750698        openshift/hello-openshift:v0.4.3   "/hello-openshift"   7 minutes ago       Up 7 minutes                                 k8s_hello-openshift.b69b23ff_hello-openshift_demo_522adf06-0f83-11e5-982b-525400a4dc47_f491f4be
    405d63115a60        openshift3_beta/ose-pod:v0.5.2.2   "/pod"               7 minutes ago       Up 7 minutes        0.0.0.0:6061->8080/tcp   k8s_POD.ad86e772_hello-openshift_demo_522adf06-0f83-11e5-982b-525400a4dc47_6cc974dc

The `openshift3_beta/ose-pod` container exists because of the way network
namespacing works in Kubernetes. For the sake of simplicity, think of the
container as nothing more than a way for the host OS to get an interface created
for the corresponding pod to be able to receive traffic. Deeper understanding of
networking in OpenShift is outside the scope of this material.

To verify that the app is working, you can issue a curl to the app's port *on
the node where the pod is running*

    [root@ose3-node1 ~]# curl localhost:36061
    Hello OpenShift!

Hooray!

### Looking at the Pod in the Web Console
Go to the web console and go to the *Overview* tab for the *OpenShift 3 Demo*
project. You'll see some interesting things:

* You'll see the pod is running (eventually)
* You'll see the SDN IP address that the pod is associated with (10....)
* You'll see the internal port that the pod's container's "application"/process
    is using
* You'll see the host port that the pod is bound to
* You'll see that there's no service yet - we'll get to services soon.

### Quota Usage
If you click on the *Settings* tab, you'll see our pod usage has increased to 1.

You can also use `osc` to determine the current quota usage of your project. As
`joe`:

    osc describe quota test-quota -n demo

### Extra Credit
If you try to curl the pod IP and port, you get "connection refused". See if you
can figure out why.

### Delete the Pod
As `joe`, go ahead and delete this pod so that you don't get confused in later examples:

    osc delete pod hello-openshift

Take a moment to think about what this pod exercise really did -- it referenced
an arbitrary Docker image, made sure to fetch it (if it wasn't present), and
then ran it. This could have just as easily been an application from an ISV
available in a registry or something already written and built in-house.

This is really powerful. We will explore using "arbitrary" docker images later.

### Quota Enforcement
Since we know we can run a pod directly, we'll go through a simple quota
enforcement exercise. The `hello-quota` JSON will attempt to create four
instances of the "hello-openshift" pod. It will fail when it tries to create the
fourth, because the quota on this project limits us to three total pods.

As `joe`, go ahead and use `osc create` and you will see the following:

    osc create -f hello-quota.json 
    pods/hello-openshift-1
    pods/hello-openshift-2
    pods/hello-openshift-3
    Error from server: Pod "hello-openshift-4" is forbidden: Limited to 3 pods

Let's delete these pods quickly. As `joe` again:

    osc delete pod --all

**Note:** You can delete most resources using "--all" but there is *no sanity
check*. Be careful.

## Services
From the [Kubernetes
documentation](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/services.md):

    A Kubernetes service is an abstraction which defines a logical set of pods and a
    policy by which to access them - sometimes called a micro-service. The goal of
    services is to provide a bridge for non-Kubernetes-native applications to access
    backends without the need to write code that is specific to Kubernetes. A
    service offers clients an IP and port pair which, when accessed, redirects to
    the appropriate backends. The set of pods targetted is determined by a label
    selector.

If you think back to the simple pod we created earlier, there was a "label":

      "labels": {
        "name": "hello-openshift"
      },

Now, let's look at a *service* definition:

    {
      "kind": "Service",
      "apiVersion": "v1beta3",
      "metadata": {
        "name": "hello-service"
      },
      "spec": {
        "selector": {
          "name":"hello-openshift"
        },
        "ports": [
          {
            "protocol": "TCP",
            "port": 80,
            "targetPort": 9376
          }
        ]
      }
    }

The *service* has a `selector` element. In this case, it is a key:value pair of
`name:hello-openshift`. If you looked at the output of `osc get pods` on your
master, you saw that the `hello-openshift` pod has a label:

    name=hello-openshift

The definition of the *service* tells Kubernetes that any pods with the label
"name=hello-openshift" are associated, and should have traffic distributed
amongst them. In other words, the service itself is the "connection to the
network", so to speak, or the input point to reach all of the pods. Generally
speaking, pod containers should not bind directly to ports on the host. We'll
see more about this later.

But, to really be useful, we want to make our application accessible via a FQDN,
and that is where the routing tier comes in.

## Routing
The OpenShift routing tier is how FQDN-destined traffic enters the OpenShift
environment so that it can ultimately reach pods. In a simplification of the
process, the `openshift3_beta/ose-haproxy-router` container we will create below
is a pre-configured instance of HAProxy as well as some of the OpenShift
framework. The OpenShift instance running in this container watches for route
resources on the OpenShift master.

Here is an example route resource JSON definition:

    {
      "kind": "Route",
      "apiVersion": "v1beta3",
      "metadata": {
        "name": "hello-openshift-route"
      },
      "spec": {
        "host": "hello-openshift.cloudapps.example.com",
        "to": {
          "name": "hello-openshift-service"
        },
        "tls": {
          "termination": "edge"
        }
      }
    }

When the `osc` command is used to create this route, a new instance of a route
*resource* is created inside OpenShift's data store. This route resource is
affiliated with a service.

The HAProxy/Router is watching for changes in route resources. When a new route
is detected, an HAProxy pool is created. When a change in a route is detected,
the pool is updated.

This HAProxy pool ultimately contains all pods that are in a service. Which
service? The service that corresponds to the `serviceName` directive that you
see above.

You'll notice that the definition above specifies TLS edge termination. This
means that the router should provide this route via HTTPS. Because we provided
no certificate info, the router will provide the default SSL certificate when
the user connects. Because this is edge termination, user connections to the
router will be SSL encrypted but the connection between the router and the pods
is unencrypted.

It is possible to utilize various TLS termination mechanisms, and more details
is provided in the router documentation:

    http://docs.openshift.org/latest/architecture/core_objects/routing.html#securing-routes

We'll see this edge termination in action shortly.

### Creating a Wildcard Certificate
In order to serve a valid certificate for secure access to applications in our
cloud domain, we will need to create a key and wildcard certificate that the
router will use by default for any routes that do not specify a key/cert of their
own. OpenShift supplies a command for creating a key/cert signed by the OpenShift
CA which we will use.

On the master, as `root`:

    CA=/etc/openshift/master
    osadm create-server-cert --signer-cert=$CA/ca.crt \
          --signer-key=$CA/ca.key --signer-serial=$CA/ca.serial.txt \
          --hostnames='*.cloudapps.example.com' \
          --cert=cloudapps.crt --key=cloudapps.key

Now we need to combine `cloudapps.crt` and `cloudapps.key` with the CA into
a single PEM format file that the router needs in the next step.

    cat cloudapps.crt cloudapps.key $CA/ca.crt > cloudapps.router.pem

Make sure you remember where you put this PEM file.

### Creating the Router
The router is the ingress point for all traffic destined for OpenShift
v3 services. It currently supports only HTTP(S) traffic (and "any"
TLS-enabled traffic via SNI). While it is called a "router", it is essentially a
proxy.

The `openshift3_beta/ose-haproxy-router` container listens on the host network
interface, unlike most containers that listen only on private IPs. The router
proxies external requests for route names to the IPs of actual pods identified
by the service associated with the route.

OpenShift's admin command set enables you to deploy router pods automatically.
Let's try to create one:

    osadm router
    error: router could not be created; you must specify a .kubeconfig file path containing credentials for connecting the router to the master with --credentials

Just about every form of communication with OpenShift components is secured by
SSL and uses various certificates and authentication methods. Even though we set
up our `.kubeconfig` for the root user, `osadm router` is asking us what
credentials the *router* should use to communicate. We also need to specify the
router image, since the tooling defaults to upstream/origin:

    osadm router --dry-run \
    --credentials=/etc/openshift/master/openshift-router.kubeconfig

Adding that would be enough to allow the command to proceed, but if we want
this router to work for our environment, we also need to specify the beta
router image (the tooling defaults to upstream/origin otherwise) and we need
to supply the wildcard cert/key that we created for the cloud domain.

    osadm router --default-cert=cloudapps.router.pem \
    --credentials=/etc/openshift/master/openshift-router.kubeconfig \
    --selector='region=infra' \
    --images='registry.access.redhat.com/openshift3_beta/ose-${component}:${version}'

If this works, you'll see some output:

    services/router
    deploymentConfigs/router

**Note:** You will have to reference the absolute path of the PEM file if you
did not run this command in the folder where you created it.

Let's check the pods:

    osc get pods 

In the output, you should see the router pod status change to "running" after a
few moments (it may take up to a few minutes):

    POD              IP         CONTAINER(S)   IMAGE(S)                                                                 HOST                                    LABELS                                                      STATUS    CREATED      MESSAGE
    router-1-cutck   10.1.0.4                                                                                           ose3-master.example.com/192.168.133.2   deployment=router-1,deploymentconfig=router,router=router   Running   18 minutes   
                                router         registry.access.redhat.com/openshift3_beta/ose-haproxy-router:v0.5.2.2                                                                                                       Running   18 minutes

Note: This output is huge, wide, and ugly. We're working on making it nicer. You
can chime in here:

    https://github.com/GoogleCloudPlatform/kubernetes/issues/7843

In the above router creation command (`osadm router...`) we also specified
`--selector`. This flag causes a `nodeSelector` to be placed on all of the pods
created. If you think back to our "regions" and "zones" conversation, the
OpenShift environment is currently configured with an *infra*structure region
called "infra". This `--selector` argument asks OpenShift:

*Please place all of these router pods in the infra region*.

### Router Placement By Region
In the very beginning of the documentation, we indicated that a wildcard DNS
entry is required and should point at the master. When the router receives a
request for an FQDN that it knows about, it will proxy the request to a pod for
a service. But, for that FQDN request to actually reach the router, the FQDN has
to resolve to whatever the host is where the router is running. Remember, the
router is bound to ports 80 and 443 on the *host* interface. Since our wildcard
DNS entry points to the public IP address of the master, the `--selector` flag
used above ensures that the router is placed on our master as it's the only node
with the label `region=infra`.

For a true HA implementation, one would want multiple "infra" nodes and
multiple, clustered router instances. We will describe this later.

### Viewing Router Stats
Haproxy provides a stats page that's visible on port 1936 of your router host.
Currently the stats page is password protected with a static password, this
password will be generated using a template parameter in the future, for now the
password is `cEVu2hUb` and the username is `admin`.

To make this acessible publicly, you will need to open this port on your master:

    iptables -I OS_FIREWALL_ALLOW -p tcp -m tcp --dport 1936 -j ACCEPT

You will also want to add this rule to `/etc/sysconfig/iptables` as well to keep it
across reboots. However, don't restart the iptables service, as this would destroy
docker networking. Use the `iptables` command to change rules on a live system.

Feel free to not open this port if you don't want to make this accessible, or if
you only want it accessible via port fowarding, etc.

**Note**: Unlike OpenShift v2 this router is not specific to a given project, as
such it's really intended to be viewed by cluster admins rather than project
admins.

Ensure that port 1936 is accessible and visit:

    http://admin:cEVu2hUb@ose3-master.example.com:1936 

to view your router stats.

## The Complete Pod-Service-Route
With a router now available, let's take a look at an entire
Pod-Service-Route definition template and put all the pieces together.

Don't forget -- the materials are in `~/training/beta4`.

### Creating the Definition
The following is a complete definition for a pod with a corresponding service
and a corresponding route. It also includes a deployment configuration.

    {
      "kind": "Config",
      "apiVersion": "v1beta3",
      "metadata": {
        "name": "hello-service-complete-example"
      },
      "items": [
        {
          "kind": "Service",
          "apiVersion": "v1beta3",
          "metadata": {
            "name": "hello-openshift-service"
          },
          "spec": {
            "selector": {
              "name": "hello-openshift"
            },
            "ports": [
              {
                "protocol": "TCP",
                "port": 27017,
                "targetPort": 8080
              }
            ]
          }
        },
        {
          "kind": "Route",
          "apiVersion": "v1beta3",
          "metadata": {
            "name": "hello-openshift-route"
          },
          "spec": {
            "host": "hello-openshift.cloudapps.example.com",
            "to": {
              "name": "hello-openshift-service"
            },
            "tls": {
              "termination": "edge"
            }
          }
        },
        {
          "kind": "DeploymentConfig",
          "apiVersion": "v1beta3",
          "metadata": {
            "name": "hello-openshift"
          },
          "spec": {
            "strategy": {
              "type": "Recreate",
              "resources": {}
            },
            "replicas": 1,
            "selector": {
              "name": "hello-openshift"
            },
            "template": {
              "metadata": {
                "creationTimestamp": null,
                "labels": {
                  "name": "hello-openshift"
                }
              },
              "spec": {
                "containers": [
                  {
                    "name": "hello-openshift",
                    "image": "openshift/hello-openshift:v0.4.3",
                    "ports": [
                      {
                        "name": "hello-openshift-tcp-8080",
                        "containerPort": 8080,
                        "protocol": "TCP"
                      }
                    ],
                    "resources": {},
                    "terminationMessagePath": "/dev/termination-log",
                    "imagePullPolicy": "PullIfNotPresent",
                    "capabilities": {},
                    "securityContext": {
                      "capabilities": {},
                      "privileged": false
                    },
                    "livenessProbe": {
                      "tcpSocket": {
                        "port": 8080
                      },
                      "timeoutSeconds": 1,
                      "initialDelaySeconds": 10
                    }
                  }
                ],
                "restartPolicy": "Always",
                "dnsPolicy": "ClusterFirst",
                "serviceAccount": "",
                "nodeSelector": {
                  "region": "primary"
                }
              }
            }
          },
          "status": {
            "latestVersion": 1
          }
        }
      ]
    }

In the JSON above:

* There is a pod whose containers have the label `name=hello-openshift-label` and the nodeSelector `region=primary`
* There is a service:
  * with the id `hello-openshift-service`
  * with the selector `name=hello-openshift`
* There is a route:
  * with the FQDN `hello-openshift.cloudapps.example.com`
  * with the `spec` `to` `name=hello-openshift-service`

If we work from the route down to the pod:

* The route for `hello-openshift.cloudapps.example.com` has an HAProxy pool
* The pool is for any pods in the service whose ID is `hello-openshift-service`,
    via the `serviceName` directive of the route.
* The service `hello-openshift-service` includes every pod who has a label
    `name=hello-openshift-label`
* There is a single pod with a single container that has the label
    `name=hello-openshift-label`

If you are not using the `example.com` domain you will need to edit the route
portion of `test-complete.json` to match your DNS environment.

**Logged in as `joe`,** go ahead and use `osc` to create everything:

    osc create -f test-complete.json

You should see something like the following:

    services/hello-openshift-service
    routes/hello-openshift-route
    pods/hello-openshift

You can verify this with other `osc` commands:

    osc get pods

    osc get services

    osc get routes

**Note:** May need to force resize:

    https://github.com/openshift/origin/issues/2939

### Project Status
OpenShift provides a handy tool, `osc status`, to give you a summary of
common resources existing in the current project:

    osc status
    In project OpenShift 3 Demo (demo)
    
    service hello-openshift-service (172.30.197.132:27017 -> 8080)
      hello-openshift deploys docker.io/openshift/hello-openshift:v0.4.3
        #1 deployed 3 minutes ago - 1 pod

    To see more information about a Service or DeploymentConfig, use 'osc describe service <name>' or 'osc describe dc <name>'.
    You can use 'osc get all' to see lists of each of the types described above.

`osc status` does not yet show bare pods or routes. The output will be
more interesting when we get to builds and deployments.

### Verifying the Service
Services are not externally accessible without a route being defined, because
they always listen on "local" IP addresses (eg: 172.x.x.x). However, if you have
access to the OpenShift environment, you can still test a service.

    osc get services
    NAME                      LABELS    SELECTOR                     IP              PORT(S)
    hello-openshift-service   <none>    name=hello-openshift-label   172.30.17.229   27017/TCP

We can see that the service has been defined based on the JSON we used earlier.
If the output of `osc get pods` shows that our pod is running, we can try to
access the service:

    curl `osc get services | grep hello-openshift | awk '{print $4":"$5}' | sed -e 's/\/.*//'`
    Hello OpenShift!

This is a good sign! It means that, if the router is working, we should be able
to access the service via the route.

### Verifying the Routing
Verifying the routing is a little complicated, but not terribly so. Since we
specified that the router should land in the "infra" region, we know that its
Docker container is on the master. Log in there as `root`.

We can use `osc exec` to get a bash interactive shell inside the running
router container. The following command will do that for us:

    osc exec -it -p $(osc get pods | grep router | awk '{print $1}' | head -n 1) /bin/bash

You are now in a bash session *inside* the container running the router.

Since we are using HAProxy as the router, we can cat the `routes.json` file:

    cat /var/lib/containers/router/routes.json

If you see some content that looks like:

    "demo/hello-openshift-service": {
      "Name": "demo/hello-openshift-service",
      "EndpointTable": {
        "10.1.0.9:8080": {
          "ID": "10.1.0.9:8080",
          "IP": "10.1.0.9",
          "Port": "8080"
        }
      },
      "ServiceAliasConfigs": {
        "demo-hello-openshift-route": {
          "Host": "hello-openshift.cloudapps.example.com",
          "Path": "",
          "TLSTermination": "edge",
          "Certificates": {
            "hello-openshift.cloudapps.example.com": {
              "ID": "demo-hello-openshift-route",
              "Contents": "",
              "PrivateKey": ""
            }
          },
          "Status": "saved"
        }
      }
    }

You know that "it" worked -- the router watcher detected the creation of the
route in OpenShift and added the corresponding configuration to HAProxy.

Go ahead and `exit` from the container.

    [root@router-1-2yefi /]# exit
    exit

You can reach the route securely and check that it is using the right certificate:

    curl --cacert /etc/openshift/master/ca.crt \
             https://hello-openshift.cloudapps.example.com
    Hello OpenShift!

And:

    openssl s_client -connect hello.cloudapps.example.com:443 \
                       -CAfile /etc/openshift/master/ca.crt
    CONNECTED(00000003)
    depth=1 CN = openshift-signer@1430768237
    verify return:1
    depth=0 CN = *.cloudapps.example.com
    verify return:1
    [...]

Since we used OpenShift's CA to create the wildcard SSL certificate, and since
that CA is not "installed" in our system, we need to point our tools at that CA
certificate in order to validate the SSL certificate presented to us by the
router. With a CA or all certificates signed by a trusted authority, it would
not be necessary to specify the CA everywhere.

### The Web Console
Take a moment to look in the web console to see if you can find everything that
was just created.

## Project Administration
When we created the `demo` project, `joe` was made a project administrator. As
an example of an administrative function, if `joe` now wants to let `alice` look
at his project, with his project administrator rights he can add her using the
`osadm policy` command:

    [joe]$ osadm policy add-role-to-user view alice

**Note:** `osadm` will act, by default, on whatever project the user has
selected. If you recall earlier, when we logged in as `joe` we ended up in the
`demo` project. We'll see how to switch projects later.

Open a new terminal window as the `alice` user:

    su - alice

and login to OpenShift:

    osc login -u alice \
    --certificate-authority=/etc/openshift/master/ca.crt \
    --server=https://ose3-master.example.com:8443

You'll interact with the tool as follows:

    Authentication required for https://ose3-master.example.com:8443 (openshift)
    Password:  <redhat>
    Login successful.

    Using project "demo"

`alice` has no projects of her own yet (she is not an administrator on
anything), so she is automatically configured to look at the `demo` project
since she has access to it. She has "view" access, so `osc status` and `osc get
pods` and so forth should show her the same thing as `joe`:

    [alice]$ osc get pods
    POD               IP         CONTAINER(S)      IMAGE(S)                           HOST                                   LABELS                 STATUS    CREATED      MESSAGE
    hello-openshift   10.1.1.2                                                        ose3-node1.example.com/192.168.133.3   name=hello-openshift   Running   14 minutes   
                                 hello-openshift   openshift/hello-openshift:v0.4.3                                                                 Running   14 minutes   

However, she cannot make changes:

    [alice]$ osc delete pod hello-openshift
    Error from server: User "alice" cannot delete pods in project "demo"

Also login as `alice` in the web console and confirm that she can view
the `demo` project.

`joe` could also give `alice` the role of `edit`, which gives her access
to do nearly anything in the project except adjust access.

    [joe]$ osadm policy add-role-to-user edit alice

Now she can delete that pod if she wants, but she can not add access for
another user or upgrade her own access. To allow that, `joe` could give
`alice` the role of `admin`, which gives her the same access as himself.

    [joe]$ osadm policy add-role-to-user admin alice

There is no "owner" of a project, and projects can certainly be created
without any administrator. `alice` or `joe` can remove the `admin`
role (or all roles) from each other or themselves at any time without
affecting the existing project.

    [joe]$ osadm policy remove-user joe

Check `osadm policy help` for a list of available commands to modify
project permissions. OpenShift RBAC is extremely flexible. The roles
mentioned here are simply defaults - they can be adjusted (per-project
and per-resource if needed), more can be added, groups can be given
access, etc. Check the documentation for more details:

* http://docs.openshift.org/latest/dev_guide/authorization.html
* https://github.com/openshift/origin/blob/master/docs/proposals/policy.md

Of course, there be dragons. The basic roles should suffice for most uses.

**Note:** There is a bug that actually prevents the remove-user from removing
the user:

https://github.com/openshift/origin/issues/2785

It appears to be fixed but may not have made beta4.

### Deleting a Project
Since we are done with this "demo" project, and since the `alice` user is a
project administrator, let's go ahead and delete the project. This should also
end up deleting all the pods, and other resources, too.

As the `alice` user:

    osc delete project demo

If you quickly go to the web console and return to the top page, you'll see a
warning icon that will pop-up a hover tip saying the project is marked for
deletion.

If you switch to the `root` user and issue `osc get project` you will see that
the demo project's status is "Terminating". If you do an `osc get pod -n demo`
you may see the pods, still. It takes about 60 seconds for the project deletion
cleanup routine to finish.

Once the project disappears from `osc get project`, doing `osc get pod -n demo`
should return no results.

## Preparing for S2I: the Registry
One of the really interesting things about OpenShift v3 is that it will build
Docker images from your source code, deploy them, and manage their lifecycle.
OpenShift 3 will provide a Docker registry that administrators may run inside
the OpenShift environment that will manage images "locally". Let's take a moment
to set that up.

### Storage for the registry
The registry stores docker images and metadata. If you simply deploy a pod
with the registry, it will use an ephemeral volume that is destroyed once the
pod exits. Any images anyone has built or pushed into the registry would
disappear. That would be bad.

What we will do for this demo is use a directory on the master host for
persistent storage. In production, this directory could be backed by an NFS
mount supplied from the HA storage solution of your choice. That NFS mount
could then be shared between multiple hosts for multiple replicas of the
registry to make the registry HA.

For now we will just show how to specify the directory and leave the NFS
configuration as an exercise. On the master, as `root`, create the storage
directory with:

    mkdir -p /mnt/registry

### Creating the registry

`osadm` again comes to our rescue with a handy installer for the
registry. As the `root` user, run the following:

    osadm registry --create \
    --credentials=/etc/openshift/master/openshift-registry.kubeconfig \
    --images='registry.access.redhat.com/openshift3_beta/ose-${component}:${version}' \
    --selector="region=infra" --mount-host=/mnt/registry

You'll get output like:

    services/docker-registry
    deploymentConfigs/docker-registry

You can use `osc get pods`, `osc get services`, and `osc get deploymentconfig`
to see what happened. This would also be a good time to try out `osc status`
as root:

    osc status
    In project default

    service docker-registry (172.30.17.196:5000 -> 5000)
      docker-registry deploys registry.access.redhat.com/openshift3_beta/ose-docker-registry
        #1 deployed about a minute ago

    service kubernetes (172.30.17.2:443 -> 443)

    service kubernetes-ro (172.30.17.1:80 -> 80)

    service router (172.30.17.129:80 -> 80)
      router deploys registry.access.redhat.com/openshift3_beta/ose-haproxy-router
        #1 deployed 7 minutes ago

The project we have been working in when using the `root` user is called
"default". This is a special project that always exists (you can delete it, but
OpenShift will re-create it) and that the administrative user uses by default.
One interesting features of `osc status` is that it lists recent deployments.
When we created the router and registry, each created one deployment. We will
talk more about deployments when we get into builds.

Anyway, you will ultimately have a Docker registry that is being hosted by OpenShift
and that is running on the master (because we specified "region=infra" as the
registry's node selector).

To quickly test your Docker registry, you can do the following:

    curl -v `osc get services | grep registry | awk '{print $4":"$5}/v2/' | sed 's,/[^/]\+$,/v2/,'`

And you should see [a 200
response](https://docs.docker.com/registry/spec/api/#api-version-check) and a
mostly empty body. Your IP addresses will almost certainly be different.

    * About to connect() to 172.30.53.223 port 5000 (#0)
    *   Trying 172.30.53.223...
    * Connected to 172.30.53.223 (172.30.53.223) port 5000 (#0)
    > GET /v2/ HTTP/1.1
    > User-Agent: curl/7.29.0
    > Host: 172.30.53.223:5000
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < Content-Length: 2
    < Content-Type: application/json; charset=utf-8
    < Docker-Distribution-Api-Version: registry/2.0
    < Date: Thu, 11 Jun 2015 13:07:11 GMT
    <
    * Connection #0 to host 172.30.53.223 left intact
    {}

If you get "connection reset by peer" you may have to wait a few more moments
after the pod is running for the service proxy to update the endpoints necessary
to fulfill your request. You can check if your service has finished updating its
endpoints with:

    osc describe service docker-registry

And you will eventually see something like:

    Name:                   docker-registry
    Labels:                 docker-registry=default
    Selector:               docker-registry=default
    Type:                   ClusterIP
    IP:                     172.30.239.41
    Port:                   <unnamed>       5000/TCP
    Endpoints:              <unnamed>       10.1.0.4:5000
    Session Affinity:       None
    No events.

Once there is an endpoint listed, the curl should work and the registry is available.

Highly available, actually. You should be able to delete the registry pod at any
point in this training and have it return shortly after with all data intact.

## S2I - What Is It?
S2I stands for *source-to-image* and is the process where OpenShift will take
your application source code and build a Docker image for it. In the real world,
you would need to have a code repository (where OpenShift can introspect an
appropriate Docker image to build and use to support the code) or a code
repository + a Dockerfile (so that OpenShift can pull or build the Docker image
for you).

### Create a New Project
By default, users are allowed to create their own projects. Let's try this now.
As the `joe` user, we will create a new project to put our first S2I example
into:

    osc new-project sinatra --display-name="Sinatra Example" \
    --description="This is your first build on OpenShift 3" 

Logged in as `joe` in the web console, if you click the OpenShift image you
should be returned to the project overview page where you will see the new
project show up. Go ahead and click the *Sinatra* project - you'll see why soon.

### Switch Projects
As the `joe` user, let's switch to the `sinatra` project:

    osc project sinatra

You should see:

    Now using project "sinatra" on server "https://ose3-master.example.com:8443".

### A Simple Code Example
We'll be using a pre-build/configured code repository. This repository is an
extremely simple "Hello World" type application that looks very much like our
previous example, except that it uses a Ruby/Sinatra application instead of a Go
application.

For this example, we will be using the following application's source code:

    https://github.com/openshift/simple-openshift-sinatra-sti

Let's see some JSON:

    osc new-app -o json https://github.com/openshift/simple-openshift-sinatra-sti.git

Take a look at the JSON that was generated. You will see some familiar items at
this point, and some new ones, like `BuildConfig`, `ImageStream` and others.

Essentially, the S2I process is as follows:

1. OpenShift sets up various components such that it can build source code into
a Docker image.

1. OpenShift will then (on command) build the Docker image with the source code.

1. OpenShift will then deploy the built Docker image as a Pod with an associated
Service.

### CLI versus Console
There are currently two ways to get from source code to components on OpenShift.
The CLI has a tool (`new-app`) that can take a source code repository as an
input and will make its best guesses to configure OpenShift to do what we need
to build and run the code. You looked at that already.

You can also just run `osc new-app --help` to see other things that `new-app`
can help you achieve.

The web console also lets you point directly at a source code repository, but
requires a little bit more input from a user to get things running. Let's go
through an example of pointing to code via the web console. Later examples will
use the CLI tools.

### Adding the Builder ImageStreams
While `new-app` has some built-in logic to help automatically determine the
correct builder ImageStream, the web console currently does not have that
capability. The user will have to first target the code repository, and then
select the appropriate builder image.

Perform the following command as `root` in the `beta4`folder in order to add all
of the builder images:

    osc create -f image-streams-rhel7.json \
    -f image-streams-jboss-rhel7.json -n openshift

You will see the following:

    imageStreams/ruby
    imageStreams/nodejs
    imageStreams/perl
    imageStreams/php
    imageStreams/python
    imageStreams/mysql
    imageStreams/postgresql
    imageStreams/mongodb
    imageStreams/jboss-webserver3-tomcat7-openshift
    imageStreams/jboss-webserver3-tomcat8-openshift
    imageStreams/jboss-eap6-openshift
    imageStreams/jboss-amq-62
    imageStreams/jboss-mysql-55
    imageStreams/jboss-postgresql-92
    imageStreams/jboss-mongodb-24

What is the `openshift` project where we added these builders? This is a
special project that can contain various elements that should be available to
all users of the OpenShift environment.

### Wait, What's an ImageStream?
If you think about one of the important things that OpenShift needs to do, it's
to be able to deploy newer versions of user applications into Docker containers
quickly. But an "application" is really two pieces -- the starting image (the
S2I builder) and the application code. While it's "obvious" that we need to
update the deployed Docker containers when application code changes, it may not
have been so obvious that we also need to update the deployed container if the
**builder** image changes.

For example, what if a security vulnerability in the Ruby runtime is discovered?
It would be nice if we could automatically know this and take action. If you dig
around in the JSON output above from `new-app` you will see some reference to
"triggers". This is where `ImageStream`s come together.

The `ImageStream` resource is, somewhat unsurprisingly, a definition for a
stream of Docker images that might need to be paid attention to. By defining an
`ImageStream` on "ruby-20-rhel7", for example, and then building an application
against it, we have the ability with OpenShift to "know" when that `ImageStream`
changes and take action based on that change. In our example from the previous
paragraph, if the "ruby-20-rhel7" image changed in the Docker repository defined
by the `ImageStream`, we might automatically trigger a new build of our
application code.

An organization will likely choose several supported builders and databases from
Red Hat, but may also create their own builders, DBs, and other images. This
system provides a great deal of flexibility.

Feel free to look around `image-streams.json` for more details.  As you can see,
we have provided definitions for EAP and Tomcat builders as well as other DBs
and etc. Please feel free to experiment with these - we will attempt to provide
sample apps as time progresses.

When finished, let's go move over to the web console to create our
"application".

### Adding Code Via the Web Console
If you go to the web console and then select the "Sinatra Example" project,
you'll see a "Create +" button in the upper right hand corner. Click that
button, and you will see two options. The second option is to create an
application from a template. We will explore that later.

The first option you see is a text area where you can type a URL for source
code. We are going to use the Git repository for the Sinatra application
referenced earlier. Enter this repo in the box:

    https://github.com/openshift/simple-openshift-sinatra-sti

When you hit "Next" you will then be asked which builder image you want to use.
This application uses the Ruby language, so make sure to click
`ruby:latest`. You'll see a pop-up with some more details asking for
confirmation. Click "Select image..."

The next screen you see lets you begin to customize the information a little
bit. The only default setting we have to change is the name, because it is too
long. Enter something sensible like "*ruby-example*", then scroll to the bottom
and click "Create".

At this point, OpenShift has created several things for you. Use the "Browse"
tab to poke around and find them. You can also use `osc status` as the `joe`
user, too.

If you run (as `joe`):

    osc get pods

You will see that there are currently no pods. That is because we have not
actually gone through a build yet. While OpenShift has the capability of
automatically triggering builds based on source control pushes (eg: Git(hub)
webhooks, etc), we will have to trigger our build manually in this example.

By the way, most of these things can (SHOULD!) also be verified in the web
console. If anything, it looks prettier!

To start our build, as `joe`, execute the following:

    osc start-build ruby-example

You'll see some output to indicate the build:

    ruby-example-1

We can check on the status of a build (it will switch to "Running" in a few
moments):

    osc get builds
    NAME             TYPE      STATUS     POD
    ruby-example-1   Source    Running   ruby-example-1

The web console would've updated the *Overview* tab for the *Sinatra* project to
say:

    A build of ruby-example is running. A new deployment will be
    created automatically once the build completes.

Let's go ahead and start "tailing" the build log (substitute the proper UUID for
your environment):

    osc build-logs ruby-example-1

**Note: If the build isn't "Running" yet, or the sti-build container hasn't been
deployed yet, build-logs will give you an error. Just wait a few moments and
retry it.**

### The Web Console Revisited
If you peeked at the web console while the build was running, you probably
noticed a lot of new information in the web console - the build status, the
deployment status, new pods, and more.

If you didn't, go to the web console now. The overview page should show that the
application is running and show the information about the service at the top:

    SERVICE: RUBY-EXAMPLE routing traffic on 172.30.17.20 port 8080 - 8080 (tcp)

### Examining the Build
If you go back to your console session where you examined the `build-logs`,
you'll see a number of things happened.

What were they?

### Testing the Application
Using the information you found in the web console, try to see if your service
is working (as the `joe` user):

    curl `osc get service | grep example | awk '{print $4":"$5}' | sed -e 's/\/.*//'`
    Hello, Sinatra!

So, from a simple code repository with a few lines of Ruby, we have successfully
built a Docker image and OpenShift has deployed it for us.

The last step will be to add a route to make it publicly accessible. You might
have noticed that adding the application code via the web console resulted in a
route being created. Currently that route doesn't have a corresponding DNS
entry, so it is unusable. The default domain is also not currently configurable,
so it's not very useful at the moment.

### Adding a Route to Our Application
Remember that routes are associated with services, so, determine the id of your
services from the service output you looked at above.

**Hint:** You will need to use `osc get services` to find it.

**Hint:** Do this as `joe`.

**Hint:** It is `ruby-example`.

When you are done, create your route:

    osc create -f sinatra-route.json

Check to make sure it was created:

    osc get route
    NAME                 HOST/PORT                                   PATH      SERVICE        LABELS
    ruby-example         ruby-example.sinatra.router.default.local             ruby-example   generatedby=OpenShiftWebConsole,name=ruby-example
    ruby-example-route   hello-sinatra.cloudapps.example.com                   ruby-example

And now, you should be able to verify everything is working right:

    curl http://hello-sinatra.cloudapps.example.com
    Hello, Sinatra!

If you want to be fancy, try it in your browser!

You'll note above that there is a route involving "router.default.local". If you
remember, when creating the application from the web console, there was a
section for "route". In the future the router will provide more configuration
options for default domains and etc. Currently, the "default" domain for
applications is "router.default.local", which is most likely unusable in your
environment.

**Note:** HTTPS will *not* work for this route, because we have not specified
any TLS termination.

### Implications of Quota Enforcement on Scaling
**THIS SECTION IS BROKEN**

There is currently a bug with quota enforcement. Do **NOT** apply the quota to
this project. Skip ahead to the scaling part.

    https://github.com/openshift/origin/issues/2821

** SKIP THIS**

`*
Quotas have implications one may not immediately realize. As `root` assign a
quota to the `sinatra` project.

    osc create -f quota.json -n sinatra

There is currently no default "size" for applications that are created with the
web console. This means that, whether you think it's a good idea or not, the
application is actually unbounded -- it can consume as much of a node's
resources as it wants.

Before we can try to scale our application, we'll need to update the deployment
to put a memory and CPU limit on the pods. Go ahead and edit the
`deploymentConfig`, as `joe`:

    osc edit dc/ruby-example-1 -o json

You'll need to find "spec", "containers" and then the "resources" block in
there. It's after a bunch of `env`ironment variables. Update that "resources"
block to look like this:

        "resources": {
          "limits": {
            "cpu": "10m",
            "memory": "16Mi"
          }
        },

`*

As `joe` scale your application up to three instances using the `osc resize`
command:

    osc resize --replicas=3 rc/ruby-example-1

Wait a few seconds and you should see your application scaled up to 3 pods.

    osc get pods | grep -v "example"
    POD                    IP          CONTAINER(S) ... STATUS  CREATED
    ruby-example-3-6n19x   10.1.0.27   ruby-example ... Running 2 minutes
    ruby-example-3-pfga3   10.1.0.26   ruby-example ... Running 18 minutes
    ruby-example-3-tzt0z   10.1.0.28   ruby-example ... Running About a minute

You will also notice that these pods were distributed across our two nodes
"east" and "west". You can also see this in the web console. Cool!

**SKIP THIS**

*`
Now start another build, wait a moment or two for your build to start.

    osc start-build ruby-example

    osc get builds
    NAME             TYPE      STATUS     POD
    ruby-example-1   Source    Complete   ruby-example-1
    ruby-example-2   Source    New        ruby-example-2

The build never starts, what happened? The quota limits the number of pods in
this project to three and this includes ephemeral pods like S2I builders.
Resize your application to just one replica and your new build will
automatically start after a minute or two.

**Note:** Once the build is complete a new replication controller is
created and the old one is no longer used.
`*

## Templates, Instant Apps, and "Quickstarts"
The next example will involve a build of another application, but also a service
that has two pods -- a "front-end" web tier and a "back-end" database tier. This
application also makes use of auto-generated parameters and other neat features
of OpenShift. One thing of note is that this project already has the
wiring/plumbing between the front- and back-end components pre-defined as part
of its JSON and embedded in the source code. Adding resources "after the fact"
will come in a later lab.

This example is effectively a "quickstart" -- a pre-defined application that
comes in a template that you can just fire up and start using or hacking on.

### A Project for the Quickstart
As `joe`, create a new project:

    osc new-project quickstart --display-name="Quickstart" \
    --description='A demonstration of a "quickstart/template"'

This also changes you to use that project:

    Now using project "quickstart" on server "https://ose3-master.example.com:8443".

### A Quick Aside on Templates
From the [OpenShift
documentation](http://docs.openshift.org/latest/dev_guide/templates.html):

    A template describes a set of resources intended to be used together that
    can be customized and processed to produce a configuration. Each template
    can define a list of parameters that can be modified for consumption by
    containers.

As we mentioned previously, this template has some auto-generated parameters.
For example, take a look at the following JSON:

    "parameters": [
      {
        "name": "ADMIN_USERNAME",
        "description": "administrator username",
        "generate": "expression",
        "from": "admin[A-Z0-9]{3}"
      },

This portion of the template's JSON tells OpenShift to generate an expression
using a regex-like string that will be presented as ADMIN_USERNAME.

### Adding the Template
Go ahead and do the following as `root` in the `~/training/beta4` folder:

    osc create -f integrated-template.json -n openshift

What did you just do? The `integrated-template.json` file defined a template. By
"creating" it, you have added it to the `openshift` project.

### Create an Instance of the Template
In the web console, logged in as `joe`, find the "Quickstart" project, and
then hit the "Create +" button. We've seen this page before, but now it contains
something new -- an "instant app(lication)". An instant app is a "special" kind
of template (relaly, it just has the "instant-app" tag). The idea behind an
"instant app" is that, when creating an instance of the template, you will have
a fully functional application. in this example, our "instant" app is just a
simple key-value storage and retrieval webpage.

Click "quickstart-keyvalue-application", and you'll see a modal pop-up that
provides more information about the template.

Click "Select template..."

The next page that you will see is the template "configuration" page. This is
where you can specify certain options for how the application components will be
insantiated.

* It will show you what Docker images are used
* It will let you add label:value pairs that can be used for other things
* It will let you set specific values for any parameters, if you so choose

Leave all of the defaults and simply click "Create".

Once you hit the "Create" button, the services and pods and
replicationcontrollers etc. will be instantiated

The cool thing about the template is that it has a built-in route. The not so
cool thing is that route is not configurable at the moment. But, it's there!

If you click "Browse" and then "Services" you will see that there is a route for
the *frontend* service:

    `integrated.cloudapps.example.com`

The build was started for us immediately after creating an instance of the
template, so you can wait for it to finish. Feel free to check the build logs.

Once the build is complete, you can go on to the next step.

### Using Your App
Once the app is built, you should be able to visit the routed URL and
actually use the application!

    http://integrated.cloudapps.example.com

**Note: HTTPS will *not* work for this example because the form submission was
written with HTTP links. Be sure to use HTTP. **

## Creating and Wiring Disparate Components
Quickstarts are great, but sometimes a developer wants to build up the various
components manually. Let's take our quickstart example and treat it like two
separate "applications" that we want to wire together.

### Create a New Project
Open a terminal as `alice`:

    # su - alice

Then, create a project for this example:

    osc new-project wiring --display-name="Exploring Parameters" \
    --description='An exploration of wiring using parameters'

Log into the web console as `alice`. Can you see `joe`'s projects and content?

Before continuing, `alice` will also need the training repository:

    cd
    git clone https://github.com/openshift/training.git
    cd ~/training/beta4

### Stand Up the Frontend
The first step will be to stand up the frontend of our application. For
argument's sake, this could have just as easily been brand new vanilla code.
However, to make things faster, we'll start with an application that already is
looking for a DB, but won't fail spectacularly if one isn't found.

The frontend application comes from the following code repository:

    https://github.com/openshift/ruby-hello-world

We want to use the beta branch. We can use the `new-app` command to help get
this started for us. As `alice` go ahead and do the following:

    osc new-app -i openshift/ruby https://github.com/openshift/ruby-hello-world#beta4

You should see something like the following:

    imageStreams/ruby-hello-world
    buildConfigs/ruby-hello-world
    deploymentConfigs/ruby-hello-world
    services/ruby-hello-world
    A build was created - you can run `osc start-build ruby-hello-world` to
    start it.
    Service "ruby-hello-world" created at 172.30.39.167 with port mappings 8080.

The syntax of the command tells us:

* I want to create a new application
* using the ruby builder image in the openshift namespace
* based off of the code in a git repository
* and using the "beta4" branch

**Note:** The beta4 state of the `new-app` subcommand doesn't support the
`--selector` argument, so your frontend pods will potentially land in the
"infrastructure" region. You can edit the DeploymentConfig to specify a
nodeSelector (look at the sample JSON earlier in the training material) but it's
not important. All the other JSON examples included nodeSelectors so you didn't
need to worry about it previously.

**Note:** There is a bug in the `new-app` subcommand:

    https://bugzilla.redhat.com/show_bug.cgi?id=1232003

You will need to manually update the BuildConfig to add the `openshift`
namespace:

    osc edit bc/ruby-hello-world

Make your `strategy` section look like the following:

      strategy:
        sourceStrategy:
          from:
            kind: ImageStreamTag
            name: ruby:latest
            namespace: openshift

Save and exit the editor.

Since we know that we want to talk to a database eventually, let's take a moment
to add the environment variables for it. Conveniently, there is an `env`
subcommand to `osc`. As `alice`, we can use it like so:

    osc env dc/ruby-hello-world MYSQL_USER=root MYSQL_PASSWORD=redhat MYSQL_DATABASE=mydb

If you want to double-check, you can verify using the following:

    osc env dc/ruby-hello-world --list
    # deploymentconfigs ruby-hello-world, container ruby-hello-world
    MYSQL_USER=root
    MYSQL_PASSWORD=redhat
    MYSQL_DATABASE=mydb

Go ahead and kick off your build from the web console. Take a look at your
deployment there, too. You should see the environment variables as well.

### Expose the Service
The `osc` command has a nifty subcommand called `expose` that will take a
service and automatically create a route for us. It will do this in the defined
cloud domain and in the current project as an additional "namespace" of sorts.
For example, the steps above resulted in a service called "ruby-hello-world". We
can use `expose` against it:

    osc expose service ruby-hello-world

After a few moments:

    osc get route
    NAME               HOST/PORT                                       PATH      SERVICE            LABELS
    ruby-hello-world   ruby-hello-world.wiring.cloudapps.example.com             ruby-hello-world 

Take a look at that hostname. It is

* the service name
* the namespace name
* the route domain

all concatenated together. In the future the `expose` command will allow a
hostname to be specified directly.

Now you should be able to access your application with your browser! Go ahead
and do that now. You'll notice that the frontend is happy to run without a
database, but is not all that exciting. We'll fix that in a moment.

### Add the Database Template
Earlier we added a template to the `openshift` namespace to make it available
for all users. Now we'll demonstrate adding a template to our own project. In
the `beta4` folder there is a `mysql-template.json` file. As `alice`, go ahead
and add it to your project:

    osc create -f mysql-template.json

You'll see:

    templates/mysql-ephemeral

### Create the Database From the Web Console
Go to the web console and make sure you are logged in as `joe` and using the
`wiring` project. You should see your front-end already there. Click the
"Create..." button and then the "Browse all templates..." button. You should see
the `mysql-ephemeral` template. Click it and then click "Select template".

You will need to edit the parameters of this template, because the defaults will
not work for us. Change the `DATABASE_SERVICE_NAME` to be "database", because
that is what service the frontend expects to connect to. Make sure that the
MySQL user, password and database match whatever values you specified in the
previous labs.

Click the "Create" button when you are ready.

It may take a little while for the MySQL container to download (if you didn't
pre-fetch it). It's a good idea to verify that the database is running before
continuing.  If you don't happen to have a MySQL client installed you can still
verify MySQL is running with curl:

    curl `osc get services | grep database | awk '{print $4}'`:3306

MySQL doesn't speak HTTP so you will see garbled output like this (however,
you'll know your database is running!):

    5.6.2K\l-7mA<��F/T:emsy'TR~mysql_native_password!��#08S01Got packets out of order

### Visit Your Application Again
Visit your application again with your web browser. Why does it still say that
there is no database?

When the frontend was first built and created, there was no service called
"database", so the environment variable `DATABASE_SERVICE_HOST` did not get
populated with any values. Our database does exist now, and there is a service
for it, but OpenShift could not "inject" those values into the frontend
container.

### Replication Controllers
The easiest way to get this going? Just nuke the existing pod. There is a
replication controller running for both the frontend and backend:

    osc get replicationcontroller

The replication controller is configured to ensure that we always have the
desired number of replicas (instances) running. We can look at how many that
should be:

    osc describe rc ruby-hello-world-1

So, if we kill the pod, the RC will detect that, and fire it back up. When it
gets fired up this time, it will then have the `DATABASE_SERVICE_HOST` value,
which means it will be able to connect to the DB, which means that we should no
longer see the database error!

As `alice`, go ahead and find your frontend pod, and then kill it:

    osc delete pod `osc get pod | grep -e "hello-world-[0-9]" | grep -v build | awk '{print $1}'`

You'll see something like:

    pods/ruby-hello-world-1-wcxiw

That was the generated name of the pod when the replication controller stood it
up the first time.

After a few moments, we can look at the list of pods again:

    osc get pod | grep world

And we should see a different name for the pod this time:

    ruby-hello-world-1-4ikbl

This shows that, underneath the covers, the RC restarted our pod. Since it was
restarted, it should have a value for the `DATABASE_SERVICE_HOST` environment
variable. Go to the node where the pod is running, and find the Docker container
id as `root`:

    docker inspect `docker ps | grep hello-world | grep run | awk \
    '{print $1}'` | grep DATABASE

The output will look something like:

    "MYSQL_DATABASE=mydb",
    "DATABASE_SERVICE_PORT_MYSQL=3306",
    "DATABASE_SERVICE_PORT=3306",
    "DATABASE_PORT=tcp://172.30.249.174:3306",
    "DATABASE_PORT_3306_TCP=tcp://172.30.249.174:3306",
    "DATABASE_PORT_3306_TCP_PROTO=tcp",
    "DATABASE_SERVICE_HOST=172.30.249.174",
    "DATABASE_PORT_3306_TCP_PORT=3306",
    "DATABASE_PORT_3306_TCP_ADDR=172.30.249.174",

### Revisit the Webpage
Go ahead and revisit `http://ruby-hello-world.wiring.cloudapps.example.com` (or your appropriate
FQDN) in your browser, and you should see that the application is now fully
functional!

## Rollback/Activate and Code Lifecycle
Not every coder is perfect, and sometimes you want to rollback to a previous
incarnation of your application. Sometimes you then want to go forward to a
newer version, too.

The next few labs require that you have a Github account. We will take Alice's
"wiring" application and modify its front-end and then rebuild. We'll roll-back
to the original version, and then go forward to our re-built version.

### Fork the Repository
Our wiring example's frontend service uses the following Github repository:

    https://github.com/openshift/ruby-hello-world

Go ahead and fork this into your own account by clicking the *Fork* Button at
the upper right.

### Update the BuildConfig
Remember that a `BuildConfig`(uration) tells OpenShift how to do a build.
Still as the `alice` user, take a look at the current `BuildConfig` for our
frontend:

    osc get buildconfig ruby-example -o yaml
    apiVersion: v1beta1
    kind: BuildConfig
    metadata:
      creationTimestamp: 2015-03-10T15:40:26-04:00
      labels:
        template: application-template-stibuild
      name: ruby-example
      namespace: wiring
      resourceVersion: "831"
      selfLink: /osapi/v1beta1/buildConfigs/ruby-example?namespace=wiring
      uid: 4cff2e5e-c75d-11e4-806e-525400b33d1d
    parameters:
      output:
        to:
          kind: ImageStream
          name: origin-ruby-sample
      source:
        git:
          uri: git://github.com/openshift/ruby-hello-world.git
          ref: beta4
        type: Git
      strategy:
        stiStrategy:
          builderImage: openshift/ruby-20-rhel7
          image: openshift/ruby-20-rhel7
        type: STI
    triggers:
    - github:
        secret: secret101
      type: github
    - generic:
        secret: secret101
      type: generic
    - imageChange:
        from:
          name: ruby-20-rhel7
        image: openshift/ruby-20-rhel7
        imageRepositoryRef:
          name: ruby-20-rhel7
        tag: latest
      type: imageChange

As you can see, the current configuration points at the
`openshift/ruby-hello-world` repository. Since you've forked this repo, let's go
ahead and re-point our configuration. Our friend `osc edit` comes to the rescue
again:

    osc edit bc ruby-example

Change the "uri" reference to match the name of your Github
repository. Assuming your github user is `alice`, you would point it
to `git://github.com/alice/ruby-hello-world.git`. Save and exit
the editor.

If you again run `osc get buildconfig ruby-example -o yaml` you should see
that the `uri` has been updated.

### Change the Code
Github's web interface will let you make edits to files. Go to your forked
repository (eg: https://github.com/alice/ruby-hello-world), select the `beta3`
branch, and find the file `main.erb` in the `views` folder.

Change the following HTML:

    <div class="page-header" align=center>
      <h1> Welcome to an OpenShift v3 Demo App! </h1>
    </div>

To read (with the typo):

    <div class="page-header" align=center>
      <h1> This is my crustom demo! </h1>
    </div>

You can edit code on Github by clicking the pencil icon which is next to the
"History" button. Provide some nifty commit message like "Personalizing the
application."

If you know how to use Git/Github, you can just do this "normally".

### Start a Build with a Webhook
Webhooks are a way to integrate external systems into your OpenShift
environment so that they can fire off OpenShift builds. Generally
speaking, one would make code changes, update the code repository, and
then some process would hit OpenShift's webhook URL in order to start
a build with the new code.

Your GitHub account has the capability to configure a webhook to request
whenever a commit is pushed to a specific branch; however, it would only
be able to make a request against your OpenShift master if that master
is exposed on the Internet, so you will probably need to simulate the
request manually for now.

To find the webhook URL, you can visit the web console, click into the
project, click on *Browse* and then on *Builds*. You'll see two webhook
URLs. Copy the *Generic* one. It should look like:

    https://ose3-master.example.com:8443/osapi/v1beta3/namespaces/wiring/buildconfigs/ruby-example/webhooks/secret101/generic

**Note**: As of the cut of beta 4, the generic webhook URL was incorrect in the
webUI. Note the correct syntax above. This is fixed already, but did not make it
in:

    https://github.com/openshift/origin/issues/2981

If you look at the `frontend-config.json` file that you created earlier,
you'll notice the same "secret101" entries in triggers. These are
basically passwords so that just anyone on the web can't trigger the
build with knowledge of the name only. You could of course have adjusted
the passwords or had the template generate randomized ones.

This time, in order to run a build for the frontend, we'll use `curl` to hit our
webhook URL.

First, look at the list of builds:

    osc get build

You should see that the first build had completed. Then, `curl`:

    curl -i -H "Accept: application/json" \
    -H "X-HTTP-Method-Override: PUT" -X POST -k \
    https://ose3-master.example.com:8443/osapi/v1beta3/namespaces/wiring/buildconfigs/ruby-example/webhooks/secret101/generic

And now `get build` again:

    osc get build
    NAME                  TYPE      STATUS     POD
    ruby-example-1   Source    Complete   ruby-example-1
    ruby-example-2   Source    Pending    ruby-example-2

You can see that this could have been part of some CI/CD workflow that
automatically called our webhook once the code was tested.

You can also check the web interface (logged in as `alice`) and see
that the build is running. Once it is complete, point your web browser
at the application:

    http://ruby-hello-world.wiring.cloudapps.example.com/

You should see your big fat typo.

**Note: Remember that it can take a minute for your service endpoint to get
updated. You might get a `503` error if you try to access the application before
this happens.**

Since we failed to properly test our application, and our ugly typo has made it
into production, a nastygram from corporate marketing has told us that we need
to revert to the previous version, ASAP.

If you log into the web console as `alice` and find the `Deployments` section of
the `Browse` menu, you'll see that there are two deployments of our frontend: 1
and 2.

You can also see this information from the cli by doing:

    osc get replicationcontroller

The semantics of this are that a `DeploymentConfig` ensures a
`ReplicationController` is created to manage the deployment of the built `Image`
from the `ImageStream`.

Simple, right?

### Rollback
You can rollback a deployment using the CLI. Let's go and checkout what a rollback to
`frontend-1` would look like:

    osc rollback frontend-1 --dry-run

Since it looks OK, let's go ahead and do it:

    osc rollback frontend-1

If you look at the `Browse` tab of your project, you'll see that in the `Pods`
section there is a `frontend-3...` pod now. After a few moments, revisit the
application in your web browser, and you should see the old "Welcome..." text.

### Activate
Corporate marketing called again. They think the typo makes us look hip and
cool. Let's now roll forward (activate) the typo-enabled application:

    osc rollback frontend-2

## A Simple PHP Example
Let's take some time to build a simple PHP example. Using PHP will make it easy
for us to demonstrate persistent storage.

### Create a PHP Project
As `alice`, create a project called `php-upload`.

### Build the App
The source code for the application is here:

    https://github.com/rjleaf/openshift-php-upload-demo

Using the skills you've learned so far, get this code running in your new
project. Remember that you'll have to select the PHP builder and that you will
need to remember to start your build.

### Create a Route
Using the skills you've learned so far, use the `expose` subcommand to create a
route for your app.

### Test Your App
Once the app is built and the route is created, you should be able to access
your application. If your route was `upload.cloudapps.example.com` and you went
to it, you'll notice that you just get the Apache "Welcome" page. That's because
our app doesn't have an `index.html`. You'll need to go to:

    upload.cloudapps.example.com/form.html

You'll see that this is a simple file uploader app. Files go into a folder
`/uploaded` and that folder's index can be viewed.

Go ahead and upload an image file, and then see if you can view it.

### Kill Your Pod
Use the `osc delete` command to kill the deployed instance of your application.
Then try to view the file you just uploaded. Notice a problem? We had no
persistent storage attached to the application. When the pod was killed, the
temporary changes to the local filesystem were lost.

Fortunately, OpenShift 3 provides a mechanism to attach persistent (external)
storage to applications. This mechanism could apply to anything, not just simple
uploaded files. Think databases and other services.

## Using Persistent Storage (Optional)
Pods are ephemeral, and so is their storage by default. For shared or persistent
storage, we need a way to specify that pods should use external volumes.

We can do this a number of ways. [Kubernetes provides methods for directly
specifying the mounting of several different volume
types.](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/volumes.md)
This is perfect if you want to use known external resources. But that's
not very PaaS-y. If I'm using a PaaS, I might really just rather request a
chunk of storage and not need a side channel to provision that. OpenShift 3
provides a mechanism for doing just this.

### Export an NFS Volume
For the purposes of this training, we will just demonstrate the master
exporting an NFS volume for use as storage by the database. **You would
almost certainly not want to do this in production.** If you happen
to have another host with an NFS export handy, feel free to substitute
that instead of setting the following up on the master.

Ensure that nfs-utils is installed (**on all systems**):

        yum install nfs-utils

Then, as `root` on the master:

1. Create the directory we will export:

        mkdir -p /var/export/vol1
        chown nfsnobody:nfsnobody /var/export/vol1
        chmod 700 /var/export/vol1

1. Edit `/etc/exports` and add the following line:

        /var/export/vol1 *(rw,sync,all_squash)

1. Enable and start NFS services:

        systemctl enable rpcbind nfs-server
        systemctl start rpcbind nfs-server nfs-lock 
        systemctl start nfs-idmap

Note that the volume is owned by `nfsnobody` and access by all remote users
is "squashed" to be access by this user. This essentially disables user
permissions for clients mounting the volume. While another configuration
might be preferable, one problem you may run into is that the container
cannot modify the permissions of the actual volume directory when mounted.
In the case of MySQL below, MySQL would like to have the volume belong to
the `mysql` user, and assumes that it is, which causes problems later.
Arguably, the container should operate differently. In the long run, we
probably need to come up with best practices for use of NFS from containers.

### NFS Firewall
We will need to open ports on the firewall on the master to enable the nodes to
communicate with us over NFS. First, let's add rules for NFS to the running
state of the firewall on the master as `root`:

    iptables -I OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 111 -j ACCEPT
    iptables -I OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 2049 -j ACCEPT
    iptables -I OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 20048 -j ACCEPT
    iptables -I OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 50825 -j ACCEPT
    iptables -I OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 53248 -j ACCEPT

Next, let's add the rules to `/etc/sysconfig/iptables`. Put them at the top of
the `OS_FIREWALL_ALLOW` set:

    -A OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 53248 -j ACCEPT
    -A OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 50825 -j ACCEPT
    -A OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 20048 -j ACCEPT
    -A OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 2049 -j ACCEPT
    -A OS_FIREWALL_ALLOW -p tcp -m state --state NEW -m tcp --dport 111 -j ACCEPT

Now, we have to edit NFS' configuration to use these ports. First, let's edit
`/etc/sysconfig/nfs`. Change the RPC option to the following:

    RPCMOUNTDOPTS="-p 20048"

Change the STATD option to the following:

    STATDARG="-p 50825"

Then, edit `/etc/sysctl.conf`:

    fs.nfs.nlm_tcpport=53248
    fs.nfs.nlm_udpport=53248

Then, persist the `sysctl` changes:

    sysctl -p

Lastly, restart NFS:

    systemctl restart nfs

### Allow NFS Access in SELinux Policy
By default policy, containers are not allowed to write to NFS mounted
directories.  We want to do just that with our database, so enable that on
all nodes where the pod could land (i.e. all of them) with:

    setsebool -P virt_use_nfs=true

Once the ansible-based installer does this automatically, we can remove this
section from the document.

### Create a PersistentVolume
It is the PaaS administrator's responsibility to define the storage that is
available to users. Storage is represented by a PersistentVolume that
encapsulates the details of a particular volume which can be backed by any
of the [volume types available via
Kubernetes](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/volumes.md).
In this case it will be our NFS volume.

Currently PersistentVolume objects must be created "by hand". Modify the
`beta4/persistent-volume.json` file as needed if you are using a different
NFS mount:

    {
      "apiVersion": "v1",
      "kind": "PersistentVolume",
      "metadata": {
        "name": "phpvolume"
      },
      "spec": {
        "capacity": {
            "storage": "5Gi"
            },
        "accessModes": [ "ReadWriteMany" ],
        "nfs": {
            "path": "/var/export/vol1",
            "server": "ose3-master.example.com"
        }
      }
    }

Create this object as the `root` (administrative) user:

    # osc create -f persistent-volume.json
    persistentvolumes/phpvolume

This defines a volume for OpenShift projects to use in deployments. The
storage should correspond to how much is actually available (make each
volume a separate filesystem if you want to enforce this limit). Take a
look at it now:

    # osc describe persistentvolumes/phpvolume
    Name:   phpvolume
    Labels: <none>
    Status: Available
    Claim:

### Claim the PersistentVolume
Now that the administrator has provided a PersistentVolume, any project can
make a claim on that storage. We do this by creating a PersistentVolumeClaim
that specifies what kind and how much storage is desired:

    {
      "apiVersion": "v1",
      "kind": "PersistentVolumeClaim",
      "metadata": {
        "name": "phpclaim"
      },
      "spec": {
        "accessModes": [ "ReadWriteMany" ],
        "resources": {
          "requests": {
            "storage": "5Gi"
          }
        }
      }
    }

We can have `alice` do this in the project you created:

    $ osc create -f persistent-volume-claim.json
    persistentVolumeClaim/claim1

This claim will be bound to a suitable PersistentVolume (one that is big
enough and allows the requested accessModes). The user does not have any
real visibility into PersistentVolumes, including whether the backing
storage is NFS or something else; they simply know when their claim has
been filled ("bound" to a PersistentVolume).

    $ osc get pvc
    NAME      LABELS    STATUS    VOLUME
    phpclaim  map[]     Bound     phpvolume

If as `root` we now go back and look at our PV, we will also see that it has
been claimed:

    # osc describe pv/phpvolume
    Name:   phpvolume
    Labels: <none>
    Status: Bound
    Claim:  upload/phpclaim

The PersistentVolume is now claimed and can't be claimed by any other project.

Although this flow assumes the administrator pre-creates volumes in
anticipation of their use later, it would be possible to create an external
process that watches the API for a PersistentVolumeClaim to be created,
dynamically provisions a corresponding volume, and creates the API object
to fulfill the claim.

### Use the Claimed Volume
Finally, we need to modify the `DeploymentConfig` to specify that this volume
should be mounted where the database will use it. As `alice`:

    $ osc edit dc/upload

**Note:** This assumes you named your app `upload` when you created it. You may
need to figure out what the name of your `DeploymentConfig` is.

The part we will need to edit is the pod template. We will need to add two
parts: 

* a definition of the volume
* where to mount it inside the container

First, directly under the `template` `spec:` line, add this YAML (indented from the `spec:` line):

          volumes:
          - name: php-upload-volume
            persistentVolumeClaim:
              claimName: phpclaim

Then to have the container mount this, add this YAML after the
`terminationMessagePath:` line:

            volumeMounts:
            - mountPath: /opt/openshift/src/uploaded
              name: php-upload-volume

Remember that YAML is sensitive to indentation. The final template should
look like this:

    template:
      metadata:
        creationTimestamp: null
        labels:
          deploymentconfig: database
      spec:
        volumes:
        - name: php-upload-volume
          persistentVolumeClaim:
            claimName: phpclaim
        containers:
        - capabilities: {}
    [...]
          terminationMessagePath: /dev/termination-log
          volumeMounts:
          - mountPath: /opt/openshift/src/uploaded
            name: php-upload-volume
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        serviceAccount: ""

Save and exit. This change to configuration will trigger a new deployment
of the database, and this time, it will be using the NFS volume we exported
from master.

### Revisit and Reupload
Go back to your application and upload a file. Did it work?

On your master as `root`, check out the contents of `/var/export/vol1`. Do you
see your file?

### Kill the Pod
Go ahead and kill the pod again, and wait for it to come back up. Now try to
view your file again. Did it work? Great!

Further information on use of PersistentVolumes is available in the
[OpenShift Origin documentation](http://docs.openshift.org/latest/dev_guide/volumes.html).
This is a very new feature, so it is very manual for now, but look for more tooling
taking advantage of PersistentVolumes to be created in the future.

## More Exec Examples
The whole time that we have been working with OpenShift 3 so far we have not
been dealing with SSH and our containers. In fact, the builder images provided
by Red Hat do not have an SSH daemon installed and port 22 is never exposed by
any services.

That does not mean, though, that you cannot access the inside of your
application containers. There is an `exec` subcommand that lets you execute
arbitrary commands inside of your containers. We've previously used `osc exec`
to get a session inside our router, but that was as the cluster administrator.
Regular users can `exec`, too!

### Introduction to exec
Still in your PHP project, take a look at the help for `exec`:

    osc exec -h
    Execute a command in a container.
    
    Usage: 
      osc exec -p POD -c CONTAINER -- COMMAND [args...] [options]
    
    Examples:
      // Get output from running 'date' in ruby-container from pod 123456-7890
      $ osc exec -p 123456-7890 -c ruby-container date
    
      // Switch to raw terminal mode, sends stdin to 'bash' in ruby-container from pod 123456-780 and sends stdout/stderr from 'bash' back to the client
      $ osc exec -p 123456-7890 -c ruby-container -i -t -- bash -il
    
    Options:
      -c, --container='': Container name
      -p, --pod='': Pod name
      -i, --stdin=false: Pass stdin to the container
      -t, --tty=false: Stdin is a TTY
    
    Use "osc --help" for a list of all commands available in osc.
    Use "osc options" for a list of global command-line options (applies to all commands).

To start, let's try to look at the filesystem of our PHP application. Find the
ID if your pod and then try something like the following:

    osc exec -p upload-2-8no16 -- ls -l /opt/openshift/src
    total 8
    -rw-r--r--. 1 default default 365 Jun 15 09:18 form.html
    -rw-r--r--. 1 default default 700 Jun 15 09:18 upload.php
    drwx------. 2   65534   65534  33 Jun 15 09:55 uploaded

We can create a file and put some content in it, too:

    osc exec -p upload-2-8no16 -- /bin/bash -c 'echo "foo" > /opt/openshift/src/bar'

You should be able to visit your PHP application and see the content of the file
"bar".

Lastly, you can execute an interactive bash session inside the container, too.

    osc exec -it -p upload-2-8no16 -- /bin/bash -il

## Customized Build and Run Processes
OpenShift v3 supports customization of both the build and run processes.
Generally speaking, this involves modifying the various S2I scripts from the
builder image. When OpenShift builds your code, it checks to see if any of the
scripts in the `.sti/bin` folder of your repository override/supercede the
builder image's scripts. If so, it will execute the repository script instead.

More information on the scripts, their execution during the process, and
customization can be found here:

    http://docs.openshift.org/latest/creating_images/sti.html#sti-scripts

### Add a Script
You will find a script called `custom-assemble.sh` in the `beta3` folder. Go to
your Github repository for your application from the previous lab, find the
`beta3` branch, and find the `.sti/bin` folder.

* Click the "+" button at the top (to the right of `bin` in the
    breadcrumbs).
* Name your file `assemble`.
* Paste the contents of `custom-assemble.sh` into the text area.
* Provide a nifty commit message.
* Click the "commit" button.

**Note:** If you know how to Git(hub), you can do this via your shell.

Once the file is added, we can now do another build. The "custom" assemble
script will log some extra data.

### Kick Off a Build
Our old friend `curl` is back:

    curl -i -H "Accept: application/json" \
    -H "X-HTTP-Method-Override: PUT" -X POST -k \
    https://ose3-master.example.com:8443/osapi/v1beta3/namespaces/wiring/buildconfigs/ruby-example/webhooks/secret101/generic

### Watch the Build Logs
Using the skills you have learned, watch the build logs for this build. If you
miss them, remember that you can find the Docker container that ran the build
and look at its Docker logs.

Did You See It?

    2015-03-11T14:57:00.022957957Z I0311 10:57:00.022913       1 sti.go:357]
    ---> CUSTOM S2I ASSEMBLE COMPLETE

But where's the output from the custom `run` script? The `assemble` script is
run inside of your builder pod. That's what you see by using `build-logs` - the
output of the assemble script. The
`run` script actually is what is executed to "start" your application's pod. In
other words, the `run` script is what starts the Ruby process for an image that
was built based on the `ruby-20-rhel7` S2I builder. 

To look inside the builder pod, as `alice`:

    osc logs `osc get pod | grep -e "[0-9]-build" | tail -1 | awk {'print $1'}` | grep CUSTOM

You should see something similar to:

    2015-04-27T22:23:24.110630393Z ---> CUSTOM S2I ASSEMBLE COMPLETE

## Lifecycle Pre and Post Deployment Hooks
Like in OpenShift 2, we have the capability of "hooks" - performing actions both
before and after the **deployment**. In other words, once an S2I build is
complete, the resulting Docker image is pushed into the registry. Once the push
is complete, OpenShift detects an `ImageChange` and, if so configured, triggers
a **deployment**.

The *pre*-deployment hook is executed just *before* the new image is deployed.

The *post*-deployment hook is executed just *after* the new image is deployed.

How is this accomplished? OpenShift will actually spin-up an *extra* instance of
your built image, execute your hook script(s), and then shut the instance down.
Neat, huh?

Since we already have our `wiring` app pointing at our forked code repository,
let's go ahead and add a database migration file. In the `beta4` folder you will
find a file called `1_sample_table.rb`. Add this file to the `db/migrate` folder
of the `ruby-hello-world` repository that you forked. If you don't add this file
to the right folder, the rest of the steps will fail.

### Examining Deployment Hooks
Take a look at the following JSON:

    "strategy": {
        "type": "Recreate",
        "resource": {},
        "recreateParams": {
            "pre": {
                "failurePolicy": "Abort",
                "execNewPod": {
                    "command": [
                        "/bin/true"
                    ],
                    "env": [
                        {
                            "name": "CUSTOM_VAR1",
                            "value": "custom_value1"
                        }
                    ],
                    "containerName": "ruby-helloworld"
                }
            },
            "post": {
                "failurePolicy": "Ignore",
                "execNewPod": {
                    "command": [
                        "/bin/false"
                    ],
                    "env": [
                        {
                            "name": "CUSTOM_VAR2",
                            "value": "custom_value2"
                        }
                    ],
                    "containerName": "ruby-helloworld"
                }
            }
        }
    },

You can see that both a *pre* and *post* deployment hook are defined. They don't
actually do anything useful. But they are good examples.

The pre-deployment hook executes "/bin/true" whose exit code is always 0 --
success. If for some reason this failed (non-zero exit), our policy would be to
`Abort` -- consider the entire deployment a failure and stop.

The post-deployment hook executes "/bin/false" whose exit code is always 1 --
failure. The policy is to `Ignore`, or do nothing. For non-essential tasks that
might rely on an external service, this might be a good policy.

More information on these strategies, the various policies, and other
information can be found in the documentation:

    http://docs.openshift.org/latest/dev_guide/deployments.html

### Modifying the Hooks
Since we are talking about **deployments**, let's look at our
`DeploymentConfig`s. As the `alice` user in the `wiring` project:

    osc get dc

You should see something like:

    NAME       TRIGGERS       LATEST VERSION
    database   ConfigChange   1
    frontend   ImageChange    7

Since we are trying to associate a Rails database migration hook with our
application, we are ultimately talking about a deployment of the frontend. If
you edit the frontend's `DeploymentConfig` as `alice`:

    osc edit dc frontend -ojson

Yes, the default for `osc edit` is to use YAML. For this exercise, JSON will be
easier as it is indentation-insensitive. Find the section that looks like the
following before continuing:

    "spec": {
        "strategy": {
            "type": "Recreate",
            "resources": {}
        },

A Rails migration is commonly performed when we have added/modified the database
as part of our code change. In the case of a pre- or post-deployment hook, it
would make sense to:

* Attempt to migrate the database
* Abort the new deployment if the migration fails

Otherwise we could end up with our new code deployed but our database schema
would not match. This could be a *Real Bad Thing (TM)*.

In the case of the `ruby-20` builder image, we are actually using RHEL7 and the
Red Hat Software Collections (SCL) to get our Ruby 2.0 support. So, the command
we want to run looks like:

    /usr/bin/scl enable ruby200 ror40 'cd /opt/openshift/src ; bundle exec rake db:migrate'

This command:

* executes inside an SCL "shell"
* enables the Ruby 2.0.0 and Ruby On Rails 4.0 environments
* changes to the `/opt/openshift/src` directory (where our applications' code is
    located)
* executes `bundle exec rake db:migrate`

If you're not familiar with Ruby, Rails, or Bundler, that's OK. Just trust us.
Would we lie to you?

The `command` directive inside the hook's definition tells us which command to
actually execute. It is required that this is an array of individual strings.
Represented in JSON, our desired command above represented as a string array
looks like:

    "command": [
        "/usr/bin/scl",
        "enable",
        "ruby200",
        "ror40",
        "cd /opt/openshift/src ; bundle exec rake db:migrate"
    ]

This is great, but actually manipulating the database requires that we talk
**to** the database. Talking to the database requires a user and a password.
Smartly, our hook pods inherit the same environment variables as the main
deployed pods, so we'll have access to the same datbase information.

Looking at the original hook example in the previous section, and our command
reference above, in the end, you will have something that looks like:

    "strategy": {
        "type": "Recreate",
        "resources": {},
        "recreateParams": {
            "pre": {
                "failurePolicy": "Abort",
                "execNewPod": {
                    "command": [
                        "/usr/bin/scl",
                        "enable",
                        "ruby200",
                        "ror40",
                        "cd /opt/openshift/src ; bundle exec rake db:migrate"
                    ],
                    "containerName": "ruby-helloworld"
                }
            },
        }
    },

Remember, indentation isn't critical in JSON, but closing brackets and braces
are. When you are done editing the deployment config, save and quit your editor.

### Quickly Clean Up
When we did our previous builds and rollbacks and etc, we ended up with a lot of
stale pods that are not running (`Succeeded`). Currently we do not auto-delete
these pods because we have no log store -- once they are deleted, you can't view
their logs any longer.

For now, we can clean up by doing the following as `alice`:

    osc get pod |\
    grep -E "[0-9]-build" |\
    awk {'print $1'} |\
    xargs -r osc delete pod

This will get rid of all of our old build and lifecycle pods. The lifecycle pods
are the pre- and post-deployment hook pods, and the sti-build pods are the pods
in which our previous builds occurred.

### Build Again
Now that we have modified the deployment configuration and cleaned up a bit, we
need to trigger another deployment. While killing the frontend pod would trigger
another deployment, our current Docker image doesn't have the database migration
file in it. Nothing really useful would happen.

In order to get the database migration file into the Docker image, we actually
need to do another build. Remember, the S2I process starts with the builder
image, fetches the source code, executes the (customized) assemble script, and
then pushes the resulting Docker image into the registry. **Then** the
deployment happens.

As `alice`:

    osc start-build ruby-example

Or go into the web console and click the "Start Build" button in the Builds
area.

### Verify the Migration
About a minute after the build completes, you should see something like the following output
of `osc get pod` as `alice`:

    POD                                IP          CONTAINER(S)               IMAGE(S)                                                                                                                HOST                                    LABELS                                                                                                                  STATUS       CREATED         MESSAGE
    database-2-rj72q                   10.1.0.15                                                                                                                                                      ose3-master.example.com/192.168.133.2   deployment=database-2,deploymentconfig=database,name=database                                                           Running      About an hour   
                                                   ruby-helloworld-database   registry.access.redhat.com/openshift3_beta/mysql-55-rhel7                                                                                                                                                                                                                               Running      About an hour   
    deployment-frontend-7-hook-4i8ch                                                                                                                                                                  ose3-node1.example.com/192.168.133.3    <none>                                                                                                                  Succeeded    41 seconds      
                                                   lifecycle                  172.30.118.110:5000/wiring/origin-ruby-sample@sha256:2984cfcae1dd42c257bd2f79284293df8992726ae24b43470e6ffd08affc3dfd                                                                                                                                                                   Terminated   36 seconds      exit code 0
    frontend-7-nnnxz                   10.1.1.24                                                                                                                                                      ose3-node1.example.com/192.168.133.3    deployment=frontend-7,deploymentconfig=frontend,name=frontend                                                           Running      29 seconds      
                                                   ruby-helloworld            172.30.118.110:5000/wiring/origin-ruby-sample@sha256:2984cfcae1dd42c257bd2f79284293df8992726ae24b43470e6ffd08affc3dfd                                                                                                                                                                   Running      26 seconds      
    ruby-example-7-build                                                                                                                                                                         ose3-master.example.com/192.168.133.2   build=ruby-example-7,buildconfig=ruby-example,name=ruby-example,template=application-template-stibuild   Succeeded    2 minutes       
                                                   sti-build                  openshift3_beta/ose-sti-builder:v0.5.2.2                                                                                                                                                                                                                                                Terminated   2 minutes       exit code 0

Yes, it's ugly, thanks for reminding us.

You'll see that there is a single `hook`/`lifecycle` pod -- this corresponds
with the pod that ran our pre-deployment hook.

Inspect this pod's logs:

    osc logs deployment-frontend-7-hook-4i8ch

The output should show something like:

    == 1 SampleTable: migrating ===================================================
    -- create_table(:sample_table)
       -> 0.1075s
    == 1 SampleTable: migrated (0.1078s) ==========================================

If you have no output, you may have forgotten to actually put the migration file
in your repo. Without that file, the migration does nothing, which produces no
output.

For giggles, you can even talk directly to the database on its service IP/port
using the `mysql` client and the environment variables (you would need the
`mysql` package installed on your master, for example).

As `alice`, find your database:

    [alice@ose3-master beta4]$ osc get service
    NAME       LABELS    SELECTOR        IP(S)            PORT(S)
    database   <none>    name=database   172.30.108.133   5434/TCP
    frontend   <none>    name=frontend   172.30.229.16    5432/TCP

Then, somewhere inside your OpenShift environment, use the `mysql` client to
connect to this service and dump the table that we created:

    mysql -u userJKL \
      -p 5678efgh \
      -h 172.30.108.133 \
      -P 5434 \
      -e 'show tables; describe sample_table;' \
      root
    +-------------------+
    | Tables_in_root    |
    +-------------------+
    | sample_table      |
    | key_pairs         |
    | schema_migrations |
    +-------------------+
    +-------+--------------+------+-----+---------+----------------+
    | Field | Type         | Null | Key | Default | Extra          |
    +-------+--------------+------+-----+---------+----------------+
    | id    | int(11)      | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(255) | NO   |     | NULL    |                |
    +-------+--------------+------+-----+---------+----------------+

## Arbitrary Docker Image (Builder)
One of the first things we did with OpenShift was launch an "arbitrary" Docker
image from the Docker Hub. However, we can also build Docker images from Docker
files, too. While this is a "build" process, it's not a "source-to-image"
process -- we're not working with only a source code repo.

As an example, the CentOS community maintains a Wordpress all-in-one Docker
image:

    https://github.com/CentOS/CentOS-Dockerfiles/tree/master/wordpress/centos7

We've taken the content of this subfolder and placed it in the GitHub
`openshift/centos7-wordpress` repository. Let's run `osc new-app` and see what
happens:

    osc new-app https://github.com/openshift/centos7-wordpress.git -o yaml

This all looks good for now.

### Create a Project
As `alice`, go ahead and create a new project:

    osc new-project wordpress --display-name="Wordpress" \
    --description='Building an arbitrary Wordpress Docker image'

### Build Wordpress
Let's choose the Wordpress example:

    osc new-app -l name=wordpress https://github.com/openshift/centos7-wordpress.git

    imageStreams/centos
    imageStreams/centos7-wordpress
    buildConfigs/centos7-wordpress
    deploymentConfigs/centos7-wordpress
    services/centos7-wordpress
    A build was created - you can run `osc start-build centos7-wordpress` to start it.
    Service "centos7-wordpress" created at 172.30.135.252 with port mappings 22.

Then, start the build:

    osc start-build centos7-wordpress

**Note: This can take a *really* long time to build.**

You will need a route for this application, as `curl` won't do a whole lot for
us here. Additionally, `osc new-app` currently has a bug in the way services are
detected, so we'll have a service for SSH (thus port 22 above) but not one for
httpd. So we'll add on a service and route for web access.

    osc create -f wordpress-addition.json

### Test Your Application
You should be able to visit:

    http://wordpress.cloudapps.example.com

Check it out!

Remember - not only did we use an arbitrary Docker image, we actually built the
Docker image using OpenShift. Technically there was no "code repository". So, if
you allow it, developers can actually simply build Docker containers as their
"apps" and run them directly on OpenShift.

### Application Resource Labels

You may have wondered about the `-l name=wordpress` in the invocation above. This
applies a label to all of the resources created by `osc new-app` so that they can
be easily distinguished from any other resources in a project. For example, we
can easily delete only the things with this label:

    osc delete all -l name=wordpress

    buildConfigs/centos7-wordpress
    builds/centos7-wordpress-1
    imageStreams/centos
    imageStreams/centos7-wordpress
    deploymentConfigs/centos7-wordpress
    replicationcontrollers/centos7-wordpress-1
    services/centos7-wordpress

Notice that the things we created from wordpress-addition.json didn't
have this label, so they didn't get deleted:

    osc get services

    NAME                      LABELS    SELECTOR                             IP             PORT(S)
    wordpress-httpd-service   <none>    deploymentconfig=centos7-wordpress   172.30.17.83   80/TCP

    osc get route

    NAME              HOST/PORT                         PATH      SERVICE                   LABELS
    wordpress-route   wordpress.cloudapps.example.com             wordpress-httpd-service

Labels will be useful for many things, including identification in the web console.

## EAP Example
This example requires internet access because the Maven configuration uses
public repositories.

If you have a Java application whose Maven configuration uses local
repositories, or has no Maven requirements, you could probably substitute that
code repository for the one below.

### Create a Project
Using the skills you have learned earlier in the training, create a new project
for the EAP example. Choose a user as the administrator, and make sure to use
that user in the subsequent commands as necessary.

### Instantiate the Template
When we imported the imagestreams into the `openshift` namespace earlier, we
also brought in JBoss EAP and Tomcat S2I builder images.

Take a look at the `eap6-basic-sti.json` in the `beta4` folder.  You'll see that
there are a number of bash-style variables (`${SOMETHING}`) in use in this
template. This template is already configured to use the EAP builder image, so
we can use the web console to simply isntantiate it in the desired way.

We want to:

* set the application name to *helloworld*
* set the application hostname to *helloworld.cloudapps.example.com*
* set the Git URI to
    *https://github.com/jboss-developer/jboss-eap-quickstarts/*
* set the Git ref to *6.4.x*
* set the Git context dir to *helloworld*
* set Github and Generic trigger secrets to *secret*

Ok, we're ready:

1. Add the `eap6-basic-sti.json` template to your project using the commandline:

        osc create -f eap6-basic-sti.json

1. Create the secret for the EAP template:

        osc create -f eap-app-secret.json

1. Go into the web console.

1. Find the project you created and click on it.

1. Click the "Create..." button.

1. Click the "Browse all templates..." button.

1. Click the "eap6-basic-sti" example.

1. Click "Select template".

Now that you are on the overview page, you'll have to click "Edit Paremeters"
and fill in the values with the things we wanted above. Hit "Create" when you
are done.

In the UI you will see a bunch of things get created -- several services, some
routes, and etc.

### Update the BuildConfig
The template assumes that the imageStream exists in our current project, but
that is not the case. The EAP imageStream exists in the `openshift` namespace.
So we need to edit the resulting `buildConfig` and specify that.

    osc edit bc helloworld

You will need to edit the `strategy` section to look like the following:

    strategy:
      sourceStrategy:
        from:
          kind: ImageStreamTag
          name: jboss-eap6-openshift:6.4
          namespace: openshift

**REMEMBER** indentation is *important* in YAML.

### Watch the Build
In a few moments a build will start. You can watch the build if you choose, or
just look at the web console and wait for it to finish. If you do watch the
build, you might notice some Maven errors.  These are non-critical and will not
affect the success or failure of the build.

### Visit Your Application
We specified a route via defining the application hostname, so you should be able to
visit your app at:

    http://helloworld.cloudapps.example.com/jboss-helloworld

The reason that it is "/jboss-helloworld" and not just "/" is because the
helloworld application does not use a "ROOT.war". If you don't understand this,
it's because Java is confusing.

## Conclusion
This concludes the Beta 4 training. Look for more example applications to come!

# APPENDIX - DNSMasq setup
In this training repository is a sample
[dnsmasq.conf](./beta4/dnsmasq.conf) file and a sample [hosts](./beta4/hosts)
file. If you do not have the ability to manipulate DNS in your
environment, or just want a quick and dirty way to set up DNS, you can
install dnsmasq on one of your nodes. Do **not** install DNSMasq on
your master. OpenShift now has an internal DNS service provided by
Go's "SkyDNS" that is used for internal service communication.

    yum -y install dnsmasq

Replace `/etc/dnsmasq.conf` with the one from this repository, and replace
`/etc/hosts` with the `hosts` file from this repository.

Copy your current `/etc/resolv.conf` to a new file such as
`/etc/resolv.conf.upstream`.  Ensure you *only* have an upstream
resolver there (eg: Google DNS @ `8.8.8.8`), not the address of your
dnsmasq server.

Enable and start the dnsmasq service:

    systemctl enable dnsmasq; systemctl start dnsmasq

You will need to ensure the following, or fix the following:

* Your IP addresses match the entries in `/etc/hosts`
* Your hostnames for your machines match the entries in `/etc/hosts`
* Your `cloudapps` domain points to the correct node ip in `dnsmasq.conf`
* Each of your systems has the same `/etc/hosts` file
* Your master and nodes `/etc/resolv.conf` points to the IP address of the node
  running DNSMasq as the first nameserver
* Your dnsmasq instance uses the `resolv-file` option to point to `/etc/resolv.conf.upstream` only.
* That you also open port 53 (TCP and UDP) to allow DNS queries to hit the node

Following this setup for dnsmasq will ensure that your wildcard domain works,
that your hosts in the `example.com` domain resolve, that any other DNS requests
resolve via your configured local/remote nameservers, and that DNS resolution
works inside of all of your containers. Don't forget to start and enable the
`dnsmasq` service.

### Verifying DNSMasq

You can query the local DNS on the master using `dig` (provided by the
`bind-utils` package) to make sure it returns the correct records:

    dig ose3-master.example.com

    ...
    ;; ANSWER SECTION:
    ose3-master.example.com. 0  IN  A 192.168.133.2
    ...

The returned IP should be the public interface's IP on the master. Repeat for
your nodes. To verify the wildcard entry, simply dig an arbitrary domain in the
wildcard space:

    dig foo.cloudapps.example.com

    ...
    ;; ANSWER SECTION:
    foo.cloudapps.example.com 0 IN A 192.168.133.2
    ...

# APPENDIX - LDAP Authentication
OpenShift currently supports several authentication methods for obtaining API
tokens.  While OpenID or one of the supported Oauth providers are preferred,
support for services such as LDAP is possible today using either the [Basic Auth
Remote](http://docs.openshift.org/latest/admin_guide/configuring_authentication.html#BasicAuthPasswordIdentityProvider)
identity provider or the [Request
Header](http://docs.openshift.org/latest/admin_guide/configuring_authentication.html#RequestHeaderIdentityProvider)
Identity provider.  This example while demonstrate the ease of running a
`BasicAuthPasswordIdentityProvider` on OpenShift.

For full documentation on the other authentication options please refer to the
[Official
Documentation](http://docs.openshift.org/latest/admin_guide/configuring_authentication.html)

### Prerequirements:

* A working Router with a wildcard DNS entry pointed to it
* A working Registry

### Setting up an example LDAP server:

For purposes of this training it is possible to use a preexisting LDAP server
or the example ldap server that comes preconfigured with the users referenced
in this document.  The decision does not need to be made up front.  It is
possible to change the ldap server that is used at any time.

For convenience the example LDAP server can be deployed on OpenShift as
follows:

    osc create -f openldap-example.json

That will create a pod from an OpenLDAP image hosted externally on the Docker
Hub.  You can find the source for it [here](beta4/images/openldap-example/).

To test the example LDAP service you can run the following:

    yum -y install openldap-clients
    ldapsearch -D 'cn=Manager,dc=example,dc=com' -b "dc=example,dc=com" \
               -s sub "(objectclass=*)" -w redhat \
               -h `osc get services | grep openldap-example-service | awk '{print $4}'`

You should see ldif output that shows the example.com users.

### Creating the Basic Auth service

While the example OpenLDAP service is itself mostly a toy, the Basic Auth
service created below can easily be made highly available using OpenShift
features.  It's a normal web service that happens to speak the [API required by
the
master](http://docs.openshift.org/latest/admin_guide/configuring_authentication.html#BasicAuthPasswordIdentityProvider)
and talk to an LDAP server.  Since it's stateless simply increasing the
replicas in the replication controller is all that is needed to make the
application highly available.

To make this as easy as possible for the beta training a helper script has been
provided to create a Route, Service, Build Config and Deployment Config.  The
Basic Auth service will be configured to use TLS all the way to the pod by
means of the [Router's SNI
capabilities](http://docs.openshift.org/latest/architecture/core_objects/routing.html#passthrough-termination).
Since TLS is used this helper script will also generated the required
certificates using OpenShift default CA.

    ./basicauthurl.sh -h

No arguments are required but the help output will show you the defaults:

    --route    basicauthurl.example.com
    --git-repo git://github.com/brenton/basicauthurl-example.git

Once you run the helper script it will output the configuration changes
required for `/etc/openshift/master/master-config.yaml` as well as create
`basicauthurl.json`.  You can now feed that to `osc`:

    osc create -f basicauthurl.json

At this point everything is in place to start the build which will trigger the
deployment.

    osc start-build basicauthurl-build

When the build finished you can run the following command to test that the
Service is responding correctly:

    curl -v -u joe:redhat --cacert /etc/openshift/master/ca.crt \
        --resolve basicauthurl.example.com:443:`osc get services | grep basicauthurl | awk '{print $4}'` \
        https://basicauthurl.example.com/validate

In that case in order for SNI to work correctly we had to trick curl with the `--resolve` flag.  If wildcard DNS is set up in your environment to point to the router then the following should test the service end to end:

    curl -u joe:redhat --cacert /etc/openshift/master/ca.crt \
        https://basicauthurl.example.com/validate

If you've made the required changes to `/etc/openshift/master/master-config.yaml` and
restarted `openshift-master` then you should now be able to log it with the
example users `joe` and `alice` with the password `redhat`.

### Using an LDAP server external to OpenShift

For more advanced usage it's best to refer to the
[README](https://github.com/openshift/sti-basicauthurl) for now.  All
mod_authnz_ldap directives are available.

### Upcoming changes

We've recently worked with Kubernetes upstream to add API support for Secrets.
Before GA the need for S2I builds in this authentication approach may go away.
What this would mean is that admins would run a script to import an Apache
configuration in to a Secret and the Pod could use this on start up.  In this
case the Build Config would go away and only a Deployment Config would be
needed.

# APPENDIX - Import/Export of Docker Images (Disconnected Use)
Docker supports import/save of Images via tarball. These instructions are
general and may not be 100% accurate for the current release. You can do
something like the following on your connected machine:

    docker pull registry.access.redhat.com/openshift3_beta/ose-haproxy-router
    docker pull registry.access.redhat.com/openshift3_beta/ose-deployer
    docker pull registry.access.redhat.com/openshift3_beta/ose-sti-builder
    docker pull registry.access.redhat.com/openshift3_beta/ose-docker-builder
    docker pull registry.access.redhat.com/openshift3_beta/ose-pod
    docker pull registry.access.redhat.com/openshift3_beta/ose-docker-registry
    docker pull openshift/ruby-20-centos7
    docker pull openshift/mysql-55-centos7
    docker pull openshift/hello-openshift
    docker pull centos:centos7

This will fetch all of the images. You can then save them to a tarball:

    docker save -o beta1-images.tar \
    registry.access.redhat.com/openshift3_beta/ose-haproxy-router \
    registry.access.redhat.com/openshift3_beta/ose-deployer \
    registry.access.redhat.com/openshift3_beta/ose-sti-builder \
    registry.access.redhat.com/openshift3_beta/ose-docker-builder \
    registry.access.redhat.com/openshift3_beta/ose-pod \
    registry.access.redhat.com/openshift3_beta/ose-docker-registry \
    openshift/ruby-20-centos7 \
    openshift/mysql-55-centos7 \
    openshift/hello-openshift \
    centos:centos7

**Note: On an SSD-equipped system this took ~2 min and uses 1.8GB of disk
space**

Sneakernet that tarball to your disconnected machines, and then simply load the
tarball:

    docker load -i beta1-images.tar

**Note: On an SSD-equipped system this took ~4 min**

# APPENDIX - Cleaning Up
Figuring out everything that you have deployed is a little bit of a bear right
now. The following command will show you just about everything you might need to
delete. Be sure to change your context across all the namespaces and the
master-admin to find everything:

    for resource in build buildconfig images imagestream deploymentconfig \
    route replicationcontroller service pod; do echo -e "Resource: $resource"; \
    osc get $resource; echo -e "\n\n"; done

Deleting a project with `osc delete project` should delete all of its resources,
but you may need help finding things in the default project (where
infrastructure items are). Deleting the default project is not recommended.

# APPENDIX - Pretty Output
If the output of `osc get pods` is a little too busy, you can use the following
to limit some of what it returns:

    osc get pods | awk '{print $1"\t"$3"\t"$5"\t"$7"\n"}' | column -t

# APPENDIX - Troubleshooting

(STUB)

An experimental diagnostics command is in progress for OpenShift v3.
Once merged it should be available as `openshift ex diagnostics`. There may
be out-of-band updated versions of diagnostics under
[Luke Meyer's release page](https://github.com/sosiouxme/origin/releases).
Running this may save you some time by pointing you in the right direction
for common issues. This is very much still under development however.

**Common problems**

* All of a sudden authentication seems broken for non-admin users.  Whenever I run osc commands I see output such as:

        F0310 14:59:59.219087   30319 get.go:164] request
        [&{Method:GET URL:https://ose3-master.example.com:8443/api/v1beta1/pods?namespace=demo
        Proto:HTTP/1.1 ProtoMajor:1 ProtoMinor:1 Header:map[] Body:<nil> ContentLength:0 TransferEncoding:[]
        Close:false Host:ose3-master.example.com:8443 Form:map[] PostForm:map[]
        MultipartForm:<nil> Trailer:map[] RemoteAddr: RequestURI: TLS:<nil>}]
        failed (401) 401 Unauthorized: Unauthorized

    In most cases if admin (certificate) auth is still working this means the token is invalid.  Soon there will be more polish in the osc tooling to handle this edge case automatically but for now the simplist thing to do is to recreate the .kubeconfig.

        # The login command creates a .kubeconfig file in the CWD.
        # But we need it to exist in ~/.kube
        cd ~/.kube

        # If a stale token exists it will prevent the beta4 login command from working
        rm .kubeconfig

        osc login \
        --certificate-authority=/etc/openshift/master/ca.crt \
        --cluster=master --server=https://ose3-master.example.com:8443 \
        --namespace=[INSERT NAMESPACE HERE]

* When using an "osc" command like "osc get pods" I see a "certificate signed by
    unknown authority error":

        F0212 16:15:52.195372   13995 create.go:79] Post
        https://ose3-master.example.net:8443/api/v1beta1/pods?namespace=default:
        x509: certificate signed by unknown authority

    Check the value of $KUBECONFIG:

        echo $kubeconfig

    If you don't see anything, you may have changed your `.bash_profile` but
    have not yet sourced it. Make sure that you followed the step of adding
    `$KUBECONFIG`'s export to your `.bash_profile` and then source it:

        source ~/.bash_profile

* When issuing a `curl` to my service, I see `curl: (56) Recv failure:
    Connection reset by peer`

    It can take as long as 90 seconds for the service URL to start working.
    There is some internal house cleaning that occurs inside Kubernetes
    regarding the endpoint maps.

    If you look at the log for the node, you might see some messages about
    looking at endpoint maps and not finding an endpoint for the service.

    To find out if the endpoints have been updated you can run:

    `osc describe service $name_of_service` and check the value of `Endpoints:`

# APPENDIX - Infrastructure Log Aggregation
Given the distributed nature of OpenShift you may find it beneficial to
aggregate logs from your OpenShift infastructure services. By default, openshift
services log to the systemd journal and rsyslog persists those log messages to
`/var/log/messages`. We''ll reconfigure rsyslog to write these entries to
`/var/log/openshift` and configure the master host to accept log data from the
other hosts.

## Enable Remote Logging on Master
Uncomment the following lines in your master's `/etc/rsyslog.conf` to enable
remote logging services.

    $ModLoad imtcp
    $InputTCPServerRun 514

Restart rsyslog

    systemctl restart rsyslog



## Enable logging to /var/log/openshift
On your master update the filters in `/etc/rsyslog.conf` to divert openshift logs to `/var/log/openshift`

    # Log openshift processes to /var/log/openshift
    :programname, contains, "openshift"                     /var/log/openshift

    # Log anything (except mail) of level info or higher.
    # Don't log private authentication messages!
    # Don't log openshift processes to /var/log/messages either
    :programname, contains, "openshift" ~
    *.info;mail.none;authpriv.none;cron.none                /var/log/messages

Restart rsyslog

    systemctl restart rsyslog

## Configure nodes to send openshift logs to your master
On your other hosts send openshift logs to your master by adding this line to
`/etc/rsyslog.conf`

    :programname, contains, "openshift" @@ose3-master.example.com

Restart rsyslog

    systemctl restart rsyslog

Now all your openshift related logs will end up in `/var/log/openshift` on your
master.

## Optionally Log Each Node to a unique directory
You can also configure rsyslog to store logs in a different location
based on the source host. On your master, add these lines immediately prior to
`$InputTCPServerRun 514`

    $template TmplMsg, "/var/log/remote/%HOSTNAME%/%PROGRAMNAME:::secpath-replace%.log"
    $RuleSet remote1
    authpriv.*   ?TmplAuth
    *.info;mail.none;authpriv.none;cron.none   ?TmplMsg
    $RuleSet RSYSLOG_DefaultRuleset   #End the rule set by switching back to the default rule set
    $InputTCPServerBindRuleset remote1  #Define a new input and bind it to the "remote1" rule set

Restart rsyslog

    systemctl restart rsyslog


Now logs from remote hosts will go to `/var/log/remote/%HOSTNAME%/%PROGRAMNAME%.log`

See these documentation sources for additional rsyslog configuration information

    https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/s1-basic_configuration_of_rsyslog.html
    http://www.rsyslog.com/doc/v7-stable/configuration/filters.html

# APPENDIX - JBoss Tools for Eclipse
Support for OpenShift development using Eclipse is provided through the JBoss Tools plugin.  The plugin is available
from the Jboss Tools nightly build of the Eclipse Mars.

Development is ongoing but current features include:

- Connecting to an OpenShift server using Oauth
    - Connections to multiple servers using multiple user names
- OpenShift Explorer
    - Browsing user projects
    - Browsing project resources
- Display of resource properties

## Installation
1. Install the Mars release of Eclipse from the [Eclipse Download site](http://www.eclipse.org/downloads/)
1. Add the update site
  1. Click from the toolbar 'Help > Install New Sofware'
  1. Click the 'Add' button and a dialog appears
  1. Enter a value for the name
  1. Enter 'http://download.jboss.org/jbosstools/updates/nightly/mars/' for the location.  **Note:** Alternative updates are available from
     the [JBoss Tools Downloads](http://tools.jboss.org/downloads/jbosstools/mars/index.html).  The various releases and code
     freeze dates are listed on the [JBoss JIRA site](https://issues.jboss.org/browse/JBIDE/?selectedTab=com.atlassian.jira.jira-projects-plugin:versions-panel)
  1. Click 'OK' to add the update site
1. Type 'OpenShift' in the text input box to filter the choices
1. Check 'JBoss OpenShift v3 Tools' and click 'Next'
1. Click 'Next' again, accept the license agreement, and click 'Finish'

After installation, open the OpenShift explorer view by clicking from the toolbar 'Window > Show View > Other' and typing 'OpenShift'
in the dialog box that appears.

## Connecting to the Server
1. Click 'New Connection Wizard' and a dialog appears
1. Select a v3 connection type
1. Uncheck default server
1. Enter the URL to the OpenShift server instance
1. Enter the username and password for the connection

A successful connection will allow you to expand the OpenShift explorer tree and browse the projects associated with the account
and the resources associated with each project.

# APPENDIX - Working with HTTP Proxies

In many production environments direct access to the web is not allowed.  In
these situations there is typically an HTTP(S) proxy available.  Configuring
OpenShift builds and deployments to use these proxies is as simple as setting
standard environment variables.  The trick is knowing where to place them.

## Importing ImageStreams

Since the importer is on the Master we need to make the configuration change
there.  The easiest way to do that is to add environment variables `NO_PROXY`,
`HTTP_PROXY`, and `HTTPS_PROXY` to `/etc/sysconfig/openshift-master` then restart
your master.

~~~
HTTP_PROXY=http://USERNAME:PASSWORD@10.0.1.1:8080/
HTTPS_PROXY=https://USERNAME:PASSWORD@10.0.0.1:8080/
NO_PROXY=master.example.com
~~~

It's important that the Master doesn't use the proxy to access itself so make
sure it's listed in the `NO_PROXY` value.

Now restart the Service:
~~~
systemctl restart openshift-master
~~~

If you had previously imported ImageStreams without the proxy configuration to can re-run the process as follows:

~~~
osc delete imagestreams -n openshift --all
osc create -f image-streams.json -n openshift
~~~

## S2I Builds

Let's take the sinatra example.  That build uses fetches gems from
rubygems.org.  The first thing we'll want to do is fork that codebase and
create a file called `.sti/environment`.  The contents of the file are simple
shell variables.  Most libraries will look for `NO_PROXY`, `HTTP_PROXY`, and
`HTTPS_PROXY` variables and react accordingly.

    NO_PROXY=mycompany.com
    HTTP_PROXY=http://USER:PASSWORD@IPADDR:PORT
    HTTPS_PROXY=https://USER:PASSWORD@IPADDR:PORT

## Setting Environment Variables in Pods

It's not only at build time that proxies are required.  Many applications will
need them too.  In previous examples we used environment variables in
`DeploymentConfig`s to pass in database connection information.  The same can
be done for configuring a `Pod`'s proxy at runtime:

    {
      "apiVersion": "v1beta1",
      "kind": "DeploymentConfig",
      "metadata": {
        "name": "frontend"
      },
      "template": {
        "controllerTemplate": {
          "podTemplate": {
            "desiredState": {
              "manifest": {
                "containers": [
                  {
                    "env": [
                      {
                        "name": "HTTP_PROXY",
                        "value": "http://USER:PASSWORD@IPADDR:PORT"
                      },
    ...

## Git Repository Access

In most of the beta examples code has been hosted on GitHub.  This is strictly
for convenience and in the near future documentation will be published to show
how best to integrate with GitLab as well as corporate git servers.  For now if
you wish to use GitHub behind a proxy you can set an environment variable on
the `stiStrategy`:

    {
      "stiStrategy": {
        ...
        "env": [
          {
            "Name": "HTTP_PROXY",
            "Value": "http://USER:PASSWORD@IPADDR:PORT"
          }
        ]
      }
    }

It's worth noting that if the variable is set on the `stiStrategy` it's not
necessary to use the `.sti/environment` file.

## Proxying Docker Pull

This is yet another case where it may be necessary to tunnel traffic through a
proxy.  In this case you can edit `/etc/sysconfig/docker` and add the variables
in shell format:

    NO_PROXY=mycompany.com
    HTTP_PROXY=http://USER:PASSWORD@IPADDR:PORT
    HTTPS_PROXY=https://USER:PASSWORD@IPADDR:PORT


## Future Considerations

We're working to have a single place that administrators can set proxies for
all network traffic.

# APPENDIX - Installing in IaaS Clouds
This appendix contains two "versions" of installation instructions. One is for
"generic" clouds, where the installer does not provision any resources on the
actual cloud (eg: it does not stand up VMs or configure security groups).
Another is specifically for AWS, which can take your API credentials and
configure the entire AWS environment, too.

## Generic Cloud Install

**An Example Hosts File (/etc/ansible/hosts):**

    [OSEv3:children]
    masters
    nodes

    [OSEv3:vars]
    deployment_type=enterprise

    # The default user for the image used
    ansible_ssh_user=ec2-user

    # host group for masters
    # The entries should be either the publicly accessible dns name for the host
    # or the publicly accessible IP address of the host.
    [masters]
    ec2-52-6-179-239.compute-1.amazonaws.com

    # host group for nodes
    [nodes]
    ec2-52-6-179-239.compute-1.amazonaws.com openshift_node_labels="{'region': 'infra', 'zone': 'default'}" #The master
    ec2-52-4-251-128.compute-1.amazonaws.com openshift_node_labels="{'region': 'primary', 'zone': 'default'}"
    ... <additional node hosts go here> ...

**Testing the Auto-detected Values:**
Run the openshift_facts playbook:

    cd ~/openshift-ansible
    ansible-playbook playbooks/byo/openshift_facts.yml

The output will be similar to:

    ok: [10.3.9.45] => {
        "result": {
            "ansible_facts": {
                "openshift": {
                    "common": {
                        "hostname": "ip-172-31-8-89.ec2.internal",
                        "ip": "172.31.8.89",
                        "public_hostname": "ec2-52-6-179-239.compute-1.amazonaws.com",
                        "public_ip": "52.6.179.239",
                        "use_openshift_sdn": true
                    },
                    "provider": {
                      ... <snip> ...
                    }
                }
            },
            "changed": false,
            "invocation": {
                "module_args": "",
                "module_name": "openshift_facts"
            }
        }
    }
    ...

Next, we'll need to override the detected defaults if they are not what we expect them to be
- hostname
  * Should resolve to the internal ip from the instances themselves.
  * openshift_hostname will override.
* ip
  * Should be the internal ip of the instance.
  * openshift_ip will override.
* public hostname
  * Should resolve to the external ip from hosts outside of the cloud
  * provider openshift_public_hostname will override.
* public_ip
  * Should be the externally accessible ip associated with the instance
  * openshift_public_ip will override

To override the the defaults, you can set the variables in your inventory. For example, if using AWS and managing dns externally, you can override the host public hostname as follows:

    [masters]
    ec2-52-6-179-239.compute-1.amazonaws.com openshift_node_labels="{'region': 'infra', 'zone': 'default'}" openshift_public_hostname=ose3-master.public.example.com

**Running ansible:**

    ansible ~/openshift-ansible/playbooks/byo/config.yml

## Automated AWS Install With Ansible

**Requirements:**
- ansible-1.8.x
- python-boto

**Assumptions Made:**
- The user's ec2 credentials have the following permissions:
  - Create instances
  - Create EBS volumes
  - Create and modify security groups
    - The following security groups will be created:
      - openshift-v3-training-master
      - openshift-v3-training-node
  - Create and update route53 record sets
- The ec2 region selected is using ec2 classic or has a default vpc and subnets configured.
  - When using a vpc, the default subnets are expected to be configured for auto-assigning a public ip as well.
- If providing a different ami id using the EC2_AMI_ID, it is a cloud-init enabled RHEL-7 image.

**Setup (Modifying the Values Appropriately):**

    export AWS_ACCESS_KEY_ID=MY_ACCESS_KEY
    export AWS_SECRET_ACCESS_KEY=MY_SECRET_ACCESS_KEY
    export EC2_REGION=us-east-1
    export EC2_AMI_ID=ami-12663b7a
    export EC2_KEYPAIR=MY_KEYPAIR_NAME
    export RHN_USERNAME=MY_RHN_USERNAME
    export RHN_PASSWORD=MY_RHN_PASSWORD
    export ROUTE_53_WILDCARD_ZONE=cloudapps.example.com
    export ROUTE_53_HOST_ZONE=example.com

**Clone the openshift-ansible repo and configure helpful symlinks:**
    ansible-playbook clone_and_setup_repo.yml

**Configuring the Hosts:**

    ansible-playbook -i inventory/aws/hosts openshift_setup.yml

**Accessing the Hosts:**
Each host will be created with an 'openshift' user that has passwordless sudo configured.

# APPENDIX - Linux, Mac, and Windows clients

The OpenShift client `osc` is available for Linux, Mac OSX, and Windows. You
can use these clients to perform all tasks in this documentation that make use
of the `osc` command.

## Downloading The Clients

Visit [Download Red Hat OpenShift Enterprise Beta](https://access.redhat.com/downloads/content/289/ver=/rhel---7/0.5.2.2/x86_64/product-downloads)
to download the Beta4 clients. You will need to sign into Customer Portal using
an account that includes the OpenShift Enterprise High Touch Beta entitlements.

## Log In To Your OpenShift Environment

You will need to log into your environment using `osc login` as you have
elsewhere. If you have access to the CA certificate you can pass it to osc with
the --certificate-authority flag or otherwise import the CA into your host's
certificate authority. If you do not import or specify the CA you will be
prompted to accept an untrusted certificate which is not recommended.

The CA is created on your master in `/etc/openshift/master/ca.crt`

    C:\Users\test\Downloads> osc --certificate-authority="ca.crt"
    OpenShift server [[https://localhost:8443]]: https://ose3-master.example.com:8443
    Authentication required for https://ose3-master.example.com:8443 (openshift)
    Username: joe
    Password:
    Login successful.

    Using project "sinatra"

On Mac OSX and Linux you will need to make the file executable

    chmod +x osc

In the future users will be able to download clients directly from the OpenShift
console rather than needing to visit Customer Portal.
