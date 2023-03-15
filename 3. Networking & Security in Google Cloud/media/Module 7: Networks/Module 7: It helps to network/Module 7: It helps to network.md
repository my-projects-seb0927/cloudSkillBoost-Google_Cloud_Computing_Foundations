# Module 7: it helps to network

## 1. Security in the cloud
In Google Cloud, a "network" is an isolated global resource holding network configuration. Instances are deployed in regional subnetworks, however the policy (firewall, routing, and so on), and access through VPN, are configured at the global network level.

### How Google networking works
Here's an example network diagram for an application that bridges an organization's physical data center
![Diagram example](media/1.png)  
In this example, the subnetworks are configured so that the frontend subnet cannot talk directly to the data center or colocation facility.

## 2. Virtual Private Clouds (VPCs)
A Virtual Private Cloud, or VPC, is a secure, individual, private cloud-computing model hosted within a public cloud.

On a VPC, customers can run code, store data, host websites, and do anything else they could do in an ordinary private cloud, but this private cloud is hosted remotely by a public cloud provider. This means that VPCs combine the scalability and convenience of public cloud computing with the data isolation of private cloud computing. 

### Virtual Private Cloud Option
- VPC networks connect Google Cloud resources to each other and to the internet. 
- - Segmenting networks.
- - Using firewall rules to restrict access to instances.
- - Creating static routes to forward traffic to specific destinations.
- Google VPC networks are global and can have subnets in any Google Cloud region worldwide.

### VPC subnets connect resources in different zones
![VPC subnets](media/2.png)  

### Auto and custom networks
![Different type of networks](media/3.png)  
> You can't switch a network from custom mode to auto mode.


## 3. The basics of public and internal IP addresses

### A VPC is made up of subnets
![VPC Structure](media/4.png)  
- A Virtual Private Cloud (VPC) is composed of subnetworks, or subnets, and each subnet must be configured with a private IP CIDR (*Classless Inter-domain Routing*) address. 
- The CIDR range will determine what internal IP addresses will be used by virtual machines in the subnet. 
- **Internal IP addresses are only used for communication within the VPC** and cannot be routed to the internet. 
- Each octet in an IP address is represented by 8 binary bits. So a typical IPV4 address is 32-bits long. The number at the end of the range determines how many bits will be static or frozen. This number determines how many IP addresses are available with a CIDR address.
- The CIDR range determines how many IP addresses are available. A /16 range will provide 65,536 available IP addresses. Every time you add “1” to the last number, the number of available IP addresses is cut in half.

### The basics of public and Internal IP addresses.
#### Public (external) IP addresses
- Can be assigned from pool or reserved.
- Billed when not atttached to a running VM.
- VM doesn't know the external IP; it's mapped to the internal IP.

#### Private (internal) IP addresses
- Alloccated from subnet range to VMs by DHCP.
- DHCP lease is renewed every 24 hours.
- VM name and IP is registered with network-scoped DNS service.


## 4. The Google Cloud network

### Google Cloud's primary networking products
- **Google Cloud VPC:** Comprehensive networking capabilites and infraestructure.
- - You can connect your google cloud resources to a VPC and isolate them.
- **Cloud Load Balancing:** High performance, scalable load balancing
- **Cloud CDN:** Low-latency, low-cost content delivery.
- - *Content Delivery Network*.
- - Storing files close to the user.
- **Cloud Interconnect:** Fast, high availibility interconnect.
- - Connecting your own infrastructure to Google's global cable.
- **Cloud DNS:** Highly available global DNS network.
- - *Domain Name System*.
- - It translates requests for domain names into IP addresses.


## 5. Routes and firewalls rules in the cloud
### Routing table
- VPCs have routing tables.
- - **Routing table:** Data table of router locations and their IP addresses stored in the memory of a router or a network host.
- VPCs routing tables are built-in.
- They are used to forward travvig from one instance to another within the same networs, subnetworks, etcc

### Firewall
- It's provided by Google Cloud.
- It restricts access to instances through both incoming and outgoing traffic.
- Rules can be defined through metadata tags (Ex: Tag on "WEB" tag) on Compute Engine instances.

## 6. Multiple VPC networks

### VPC peering
- It allows projects to communicate (exhange traffic).
- It's decentralized or distributed approach to multi-project networking, because each VPC network can remain under the control of separate administrator groups and mantain its own global firewall and routing tables.

### Shared VPC
- You are using here IAM for controling who and what in one project can interact with a VPC.
- It allows organizations to connect resources from multiple projects to a common VPC network.
- Resources to communicate with each other securely and efficiently by using internal IPs from that network.
- **Name:** Shared VPC network.

## 7. Lab: Multiple VPC newtorks
Here we create the next infrastructure
![Infrastructure diagram](media/5.png)

### Task 1. Create custom mode VPC networks with firewall rules
#### Create the managementnet network
1. In the Cloud Console, navigate to **Navigation menu (Navigation menu icon) > VPC network > VPC networks.**
2. Click **Create VPC Network**.
3. Set the **Name** to ´managementnet´.
4. For **Subnet creation mode**, click **Custom**.
5. You can set the following values: Name, Region and IPv4 range.
6. Click **Done**.
7. Click **EQUIVALENT COMMAND LINE**.
> These commands illustrate that networks and subnets can be created using the Cloud Shell command line. You will create the privatenet network using these commands with similar parameters.  
8. Click **Close**.
9. Click **Create**.

#### Create the privatenet network
Go to Cloud Shell command line.
1. Run to create the **privatenet** network:
`gcloud compute networks create privatenet --subnet-mode=custom`
2. Run the command to create the **privatesubnet-us** subnet:
`gcloud compute networks subnets create privatesubnet-us --network=privatenet --region=us-east1 --range=172.16.0.0/24`
> We need subnets because, remember, custom mode networks doesn't have subnet automatically created as auto mode networks.
3. Run the command to create the **privatesubnet-eu** subnet:
`gcloud compute networks subnets create privatesubnet-eu --network=privatenet --region=europe-west1 --range=172.20.0.0/20`
4. You can list the available VPC networks:
`gcloud compute networks list`
5. You can list the available VPC subnets (sorted bt VPC network):
`gcloud compute networks subnets list --sort-by=NETWORK`
6. Go to **Navigation menu > VPC network > VPC networks**.
7. You see that the same networks and subnets are listed in the Cloud Console.

#### Create the firewall rules for managementnet
Create firewall rules to allow **SSH**, **ICMP**, and **RDP** ingress traffic to VM instances on the **managementnet** network.
1. Go to **Navigation menu (Navigation menu icon) > VPC network > Firewall**.
2. Click **+ Create Firewall Rule**.
3. You can see the following values: Name, Network, Targets, Source filter, Source IPv4 ranges, Protocols and ports.
4. Click **EQUIVALENT COMMAND LINE**.
> These commands illustrate that firewall rules can also be created using the Cloud Shell command line. You will create the **privatenet**'s firewall rules using these commands with similar parameters.
5. Click **Close**.
6. Click **Create**.

#### Create the firewall rules for privatenet
Open Cloud Shell
1. Run the command to create the **privatenet-allow-icmp-ssh-rdp** firewall rule:
`gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0`
2. Run the command to list all the firewall rules (sorted by VPC network):
`gcloud compute firewall-rules list --sort-by=NETWORK`

### Task 2. Create VM instances
#### Create the managementnet-us-vm instance
1. In the Cloud Console, navigate to **Navigation menu > Compute Engine > VM instances**.
2. Click **Create instance**.
3. You can set the next values: Name, Region, Zone, Series, Machine type, etc.
4. From **Advanced options**, click **Networking, Disks, Security, Management, Sole-tenancy** dropdown.
5. Click **Networking**.
6. For **Network interfaces**, click the dropdown to edit.
7. You can set the following values: Network and Subnetwork.
8. Click **Done**.
9. You can see the code viewing **EQUIVALENT CODE**.
10 Click **Create**.

#### Create the privatenet-us-vm instance
Opne the Cloud Shell
1. Run the command to create the **privatenet-us-vm** instance:
`gcloud compute instances create privatenet-us-vm --zone="" --machine-type=e2-micro --subnet=privatesubnet-us`

### Task 3. Explore the connectivity between VM instances
#### Ping the external IP addresses



### 


### 

## 8. Lab VPC Networks - Controlling Access


## 9. Buidling hybrid clouds


## 10. Load balancing options


## 11. Lab: HTTP Load Balancer with Google Cloud Armor


