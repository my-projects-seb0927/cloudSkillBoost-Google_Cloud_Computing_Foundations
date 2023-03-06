
# Cloud Computing Fundamentals
## Course Introduction
Insert courseObjectives.png here

## Module 1: So, what's the cloud anyway?
### 1.1. Cloud Computing
Cloud computing is a way of using information technology that has five traits:
- Customers get computing resources that are on-demand and self-service.
- Customers get access to those resources over the internet.
- The provider of the resources allocates them to users out of that pool.
- The resources are elastic (They can increase or decrease as needed).
- Customers pay only for what they use, or reserve as they go

### 1.2. Cloud vs. traditional architecture
#### History of Cloud Computing:
- **First wave:** Colocation (Physycal location)
- **Second wave:** Virtualized data center
- **Third wave:** Container-based architecture

### 1.3 IaaS, PaaS & SaaS
- **IaaS:** Infrastructure as a service.
It provides raw compute, storage, and network capabilities, organized virtually into resources that are similar to physical data centers.  (Pay for what they allocate).
- **PaaS:** Platform as a service.
It binds code to libraries that provide access to the infrastructure applications need. (Pay what they use)
- **SaaS:** Software as a service.
SaaS applications are not installed on your local computer; they run in the cloud as a service and are consumed directly over the internet by end users

### 1.4 Google Cloud architecture
(insert picture here)
Google offers: Computing services, Storage services, Big data and ML product line.

## Module 2: Start with a solid platform

### 1. The Cloud Console
I can interact with the Google Cloud console by: Cloud Console, Cloud SKD and Cloud Shell, APIs and Mobile App.

### 2. Understanding projects
The Cloud console is used for accesing and using resources. Resources are organized in projects
(Insert picture)
Projects help to separate entities, hold resources for every Project, have different owners and users and Projects are billed separately.
Projects have:
- **ID** (unique)
- **Name** (Made by the user)
- **Number** (unique)

The Resource Manager manages Projects (Delete, Recover, Update, etc.)
Folders help to group resources.
### 3. Google Cloud billing
- Billing is established at the projetc level.
- Billing account can be linked to zero or more projetcs.
- They are charged automatically.

How to avoid problems?
- Defining budgets.
- Setting alerts.
- Viewing reports.
- Putting Quotas (Prevent over-prices).
  - Rate quota: Resets after specific time
  - Allocation quota: Governs the number of resources in a project.

Do you want to know a budget? Visit: cloud.google.com/products/calculator

### 4. Install and configure the Cloud SDK (Software Development Kit)
**SDK: ** Se of tools to manage resources and apps hosted on Google Cloud: gcloud CLI, gsutil & bq.

For installing: cloud.google.com/sdk

### 5. Cloud shell
Cloud Shell provides you with command-line access to computing resources hosted on Google Cloud. Cloud Shell is a Debian-based virtual machine with a persistent 5-GB home directory, which makes it easy for you to manage your Google Cloud projects and resources. The gcloud command-line tool and other utilities you need are pre-installed in Cloud Shell, which allows you to get up and running quickly.

### 6. Getting Started with Cloud Shell and gcloud
`gcloud` is the command-line tool for Google Cloud. It comes pre-installed on Cloud Shell. Here you can see some examples:

**Listing active accounts:**
`gcloud auth list`
> ``ACTIVE: *
ACCOUNT: student-01-xxxxxxxxxxxx@qwiklabs.net
To set the active account, run:
    $ gcloud config set account `ACCOUNT` ``
    
**Listing the project ID:**
`gcloud config list project`
> `[core]
project = <project_ID>`

#### Task 1: Configuring your environment
##### Understanding regions and zones
How to set a region
``gcloud config set compute/region ``  
To view the project region setting
``gcloud config get-value compute/region``  
How to set a zone
``gcloud config set compute/zone ``  
To view the project zone setting
``gcloud config get-value compute/zone``  

##### Finding project information
Viewing the project id
``gcloud config get-value project``

##### Setting environment variables
Creating an environment variable to store your Project ID
``export PROJECT_ID=$(gcloud config get-value project)``  
Creating an environment variable to store your Zone
``export ZONE=$(gcloud config get-value compute/zone)``  

##### Creating a virtual machine with the gcloud tool
Creating a VM
``gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE``
> ``Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-326fae68bc3d/zones/us-east1-c/instances/gcelab2].
NAME     ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP   STATUS
gcelab2       e2-medium               10.128.0.2   34.67.152.90  RUNNING``

##### Exploring gcloud commands
For getting help
``gcloud -h``
``gcloud --help``
``gcloud help``  
Viewing the list of configurations in my environment
``gcloud config list``  
To see all properties and their settings
``gcloud config list --all``  
List your components:
``gcloud components list``  

#### Task 2: Filtering command-line output
Listing the compute instance available in the project:
``gcloud compute instances list``
> ``NAME: gcelab2
ZONE: 
MACHINE_TYPE: e2-medium
PREEMPTIBLE:
INTERNAL_IP: 10.142.0.2
EXTERNAL_IP: 35.237.43.111
STATUS: RUNNING``

Listing the gcelab2 virtual machine:
``gcloud compute instances list --filter="name=('gcelab2')"``
> ``NAME: gcelab2
ZONE: 
MACHINE_TYPE: e2-medium
PREEMPTIBLE:
INTERNAL_IP: 10.142.0.2
EXTERNAL_IP: 35.237.43.111
STATUS: RUNNING``

P.D: Basically, you can use the `--filter` tag for filtering any data :)

#### Task 3: Connecting to your VM instance
`gcloud compute` makes connecting to your instances easy. The `gcloud compute ssh` command provides a wrapper around SSH, which takes care of authentication and the mapping of instance names to IP addresses.  
Connecting to my VM with SSH:
`gcloud compute ssh gcelab2 --zone $ZONE`
> ``WARNING: The public SSH key file for gcloud does not exist.
WARNING: The private SSH key file for gcloud does not exist.
WARNING: You do not have an SSH key for gcloud.
WARNING: [/usr/bin/ssh-keygen] will be executed to generate a key.
This tool needs to create the directory
[/home/gcpstaging306_student/.ssh] before being able to generate SSH Keys.
Do you want to continue? (Y/n)``
``Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase)``

At this moment you are inside a VM, so you can make anything you'd like to do:  
Installing `nginx` web server on a virtual machine
`sudo apt install -y nginx`

#### Task 4: Updating the firewall
List the firewalls rules for the project:
``gcloud compute firewall-rules list``
Trying to access the `nginx` service running on the gcelab2 virtual machine, by adding a tag to the virtual machine
``gcloud compute instances add-tags gcelab2 --tags http-server,https-server``
Update the firewall rule to allow:
``gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server``
Listing the firewall rules for the project:
``Update the firewall rule to allow:``
Verifying communication is possible for http to the virtual machine:
``Verify communication is possible for http to the virtual machine:``

#### Task 5: Viewing the system logs
Viewing the available logs on the system: 
``gcloud logging logs list``
Viewing the logs that relate to compute resources:
``gcloud logging logs list --filter="compute"``
Reading the logs related to the resource type of `gce_instance`:
``gcloud logging read "resource.type=gce_instance" --limit 5``

### 7. Google Cloud APIs
Application developers structure the software they write in a clean, well-defined interface that abstracts away needless detail, and then they document that interface. Those are APIs (Applicction Programming Interfaces). 

The services that make up Google Cloud offer APIs so that code you write can control them. The Cloud console includes a tool called the Google APIs Explorer that shows what APIs are available, and in what versions.

### 8. The Cloud Console Mobile App
The Cloud console includes a tool called the Google APIs Explorer that shows what APIs are available, and in what versions.

## Module 3: Use Google Cloud to build your apps

### 1. Compute Options in the Cloud
(Insert picture 4 here)

### 2. Exploring IaaS with Compute Engine
- There are no upfront investments.
- Thousands of virtual CPUs can run on a system that's designed to be fast and offer consistent performance.
- Each VM contains the power and functionality of a full-fledged operating system.
- Run any computing workload such as web-server hosting, application hosting, and/or application backends.
- They can be created via the Google Cloud Console.
- Can run Linux and Windows Server images or any customized versions of these images.
- Can build and run images of other OS and flexibly reconfigure virtual machines.

### 3. Lab: Creating a Virtual Machine
#### Task 1: Create a new instance from the Cloud Console
1. In the Cloud Console, on the Navigation menu (Navigation menu icon), click Compute Engine > VM Instances.
This may take a minute to initialize for the first time.
2. To create a new instance, click **CREATE INSTANCE**.
3. There are many parameters you can configure when creating a **new instance**. Use the following for this lab:

|Field|Value|Additional Information|
|-----|-----|----------------------|
|**Name**|**gcelab**|Name for the VM instance|
|**Region**|`<filled in at lab start>`|For more information about regions, see the Compute Engine guide, Regions and Zones.|
|**Zone**|`<filled in at lab start>`|Note: Remember the zone that you selected: you'll need it later. For more information about zones, see the Compute Engine guide, Regions and Zones.|
|**Series**|**E2**|Name of the series|
|**Machine Type**|**2 vCPU**|This is an (e2-medium), 2-CPU, 4GB RAM instance. Several machine types are available, ranging from micro instance types to 32-core/208GB RAM instance types. For more information, see the Compute Engine guide, About machine families. **Note:** A new project has a default resource quota, which may limit the number of CPU cores. You can request more when you work on projects outside this lab.|
|**Boot Disk**|**New 10 GB balanced persistent disk OS Image: Debian GNU/Linux 11 (bullseye)**|Several images are available, including Debian, Ubuntu, CoreOS, and premium images such as Red Hat Enterprise Linux and Windows Server. For more information, see Operating System documentation.|
|**Firewall**|**Allow HTTP traffic**|Select this option in order to access a web server that you'll install later. **Note:** This will automatically create a firewall rule to allow HTTP traffic on port 80.|

4. Click **Create**
It should take about a minute for the machine to be created. After that, the new virtual machine is listed on the **VM Instances** page.
5. To use **SSH** to connect to the virtual machine, in the row for your machine, click SSH.
This launches an SSH client directly from your browser.

#### Task 2: Install an NGINX web server
1. Update the OS:
`sudo apt-get update`
2. Install NGINX:
`sudo apt-get install -y nginx`
3 Confirm that NGINX is running:
` ps auwx | grep nginx`
4. To see the web page, return to the Cloud Console and click the **External IP** link in the row for your machine, or add the **External IP** value to http://EXTERNAL_IP/ in a new browser window or tab.
The default web page should open

#### Task 3: Create a new instance with gcloud
1. In the Cloud Shell, use `gcloud` to create a new virtual machine instance from the command line:
`gcloud compute instances create gcelab2 --machine-type e2-medium --zone `
2. To see all the defaults, run:
` gcloud compute instances create --help`
3. You can also use SSH to connect to your instance via gcloud. Make sure to add your zone, or omit the --zone flag if you've set the option globally:
`gcloud compute ssh gcelab2 --zone`

### 4. Configuring Elastic Apps with Autoscaling
With Compute Engine you can choose the most appropriate machine properties for your instances, like the number of virtual CPUs and the amount of memory. You can use a set of predefined machine types or create your own custom machine types. 
To do this, Compute Engine has a feature called autoscaling, where VMs can be added to or subtracted from an application based on load metrics. The other part of making that work is balancing the incoming traffic among the VMs. Google’s Virtual Private Cloud (VPC) supports several different kinds of load balancing.

### 5. Exploring PaaS with App Engine
App Engine builds highly scalable applications on a fully managed, serverless platform. it's ideal if time-to-market is valuable and **you need to focus on writing code** without touching a server, cluster or infrastructure.
It offers:
- Interfaces with development tools (Languages, Libraries, Frameworks)
- Full range of built-in services (NoSQL datastores, Memcache, Health checks, etc.)
- Software development kits (SDKs)

There are two types of App Engine environments: Standard and Flexible.
#### Standard environment
- Persistent storage with queries, sorting and transactions.
- Automatic scaling and load balancing.
- Scheduled tasks for triggering events at specified times or regular intervals.
- Asynchronous task queues for performing work outside the scope of a request.

You only have two requirements: Use a specified version and the app must conform to sandbox constraints that are dependent on runtime.

#### Flexible environment
- Instances are health-checked.
- Critical, backward-compatible updates are automatically applied to the underlying OS.
- VM instances are automatically located by geographical region according to the settings in the project.
- VM instances are restarted on a weekly basis.

Supports many features like: Logging, Traffic splittin, Versioning, Memcache, etc.

### 6. Lab App Engine: Qwik Start - Python
#### Task 1. Enable Google App Engine Admin API
The App Engine Admin API enables developers to provision and manage their App Engine Applications.
1. In the left Navigation menu, click APIs & Services > Library.
2- Type "App Engine Admin API" in the search box.
3- Click the App Engine Admin API card.
4. Click Enable.

#### Task 2. Download the Hello World app
This is an example of how to deploy an app to Google cloud:
1. Enter the following commant to copy the Hello World sampe app repository to your Google Cloud instance:
``git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git``
2. Go to the directory:
`cd python-docs-samples/appengine/standard_python3/hello_world`

#### Task 3. Test the application
Test the application using the Google Cloud development server (dev_appserver.py), which is included with the preinstalled App Engine SDK.
1. From within your helloworld directory where the app's [app.yaml](https://cloud.google.com/appengine/docs/standard/python/config/appref?hl=es-419) configuration file is located, start the Google Cloud development server with the following command:
`dev_appserver.py app.yaml`
2. View the results by clicking the Web preview (web preview icon) > Preview on port 8080. You are going to see a text saying "Hello World!"

#### Task 4. Make a change
You can leave the development server running while you develop your application. The development server watches for changes in your source files and reloads them if necessary.

#### Task 5. Deploy your app
1. To deploy your app to App Engine, run the following command from within the root directory of your application where the app.yaml file is located:
`gcloud app deploy`

#### Task 6. View your application
- Enter the next command for watching the link app:
`gcloud app browse`


### 7. Event Driven Programs with Cloud Functions
Integrated cloud functions handle applications events. Ex: When a picture is uploaded, it needs to convert format, convert thumbnail size, store new files. It allows your code to respond to events:
- Lightweight, event-bases, asynchronous compute solution.
- Allows to create small, single-purpose functions that respond to cloud events without managing a server or runtime environment
- Billed to the nearest 100 ms, and only while code is running.

### 8. Lab Cloud Function: Qwik Start - Command Line

### 9. Containerizing and Orchestrating Apps with GKE (Google Kubernets Engine)
#### Containers group your code and its dependencies
- An invisible box arount your code and its dependencies.
- Has limited access to its own partition of the file system and hardware.
- Only require a few system calls.
- Only needs an OS kernel that supports containers.
- It scales like PaaS, but gives flexibility as IaaS

You can scale by duplicating single containers, or scale application with multiple containers.

#### Kubernetes
It's a container orchestration tool to simplify the management of containerized environments. Can be installed on a group of servers or run as a hosted service in Google Cloud. It helps to:
- Install the system on local servers in the cloud.
- Manage container networking and data storage.
- Deploy rollouts and rollbacks.
- Minotar and manage container and host health.

> A VM imitates a computer. A container imitates a OS
> "GKE is a powerful cluster manager and orchestration system for running Docker containers in Google"

By the way. Kubernetes is Open Source, the service given from Googke is GKE (Google Kubernetes Engine).



### 10. Lab: Kubernetes Engine: Qwik Start

### 11. Managed serverless computing with Cloud Run




````
````
````
````
````

````
****
````
****
````
****
````
#### 
#### 
#### 
#### 

###
###
###
###

``
> `` ``
