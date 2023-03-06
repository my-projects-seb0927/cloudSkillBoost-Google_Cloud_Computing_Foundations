
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
- 

### 4. Configuring Elastic Apps with Autoscaling

### 5. Exploring PaaS with App Engine

### 6. Lab App Engine: Qwik Start - Python

### 7. Event Driven Programs with Cloud Functions

### 8. Lab Cloud Function: Qwik Start - Command Linje

### 9. Containerizing and Orchestrating Apps with GKE

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
