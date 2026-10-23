# Implementing Virtual Networking

Thinking Breakdown:

## VNet and Subnet creation for CoreServicesVnet:
We chose to go with a /16 CIDR for more availablity incase we want to add many more subnets, this allows for more potential growth if the company were to scale further.
The starting address is simply preference but important to note that we don't accidentially have IP overlap.

Starting address is going to be 10.20.0.0/16

We then move to creating the subnets for this, We'll be creating 2 subnets. One Subnet is going to be used for Shared Services and the other for the Database. For this project we are mainly working on Implementing Virtual Networking rather than building out services such as a proper database and other shared services.

For example, some of the things that would be inside of the SharedServices subnet would be active directory or Identity and user management, or even SIEM like Log analytics.


The IP's for the 2 Subnets are as followed:

SharedServiceSubnet: 10.20.10.0/24

DatabaseSubnet: 10.20.20.0/24


Setting the IP's like this not only provides cleaner IP Organization but also ensures that IP's do not over lap. 

For example,

Shared Service Subnet would follow 10.20.10.0 - 10.20.10.255

Database Subnet would follow 10.20.20.0 - 10.20.20.255

## VNet and Subnet Creation for ManufacturingVnet

To make the process of creating the Vnet and subnets for ManufacturingVNet, I decided to download the ARM template and just create the resources based off of CoreServicesVnet but just configure the IP's and Names.

I used VSCode with the downloaded JSON Template to edit the template with the correct configurations I wanted.

I made the mistake of forgetting to download the parameters file and changing the VNet name from CoreServicesVnet to ManufacturingVNet.

I also made a mistake while In vscode and messed up on the IP's but instead I just used "ctrl & f" to find "CoreServicesVnet" and change it to "ManufacturingVnet" and change the ip's as well accordingly.

## Application Security Groups and NSG's

I created an ASG called asg-web to group web server network interfaces. I created a NSG after and applied rules based on the ASG rather than singular ip's which makes it far easier to scale later down the line with this configurations.

After that I associated the NSG with the SharedServicesSubnet in CoreServericesVnet. I created an inbound rule that allows TCP traffic over ports 80 and 443 from the web-server asg and then added an outbound rule to deny any traffic that is going outbound to the internet.
This is like adding a security checkpoint at the entrance and exit of a network segment. My Inbound rule lets traffic from network interfaces in asg-web to destinations that are protected by the NSG on TCP ports 80 and 443 while my outbound traffic rule denies matching traffic from resources that are covered by the NSG when the destination matches azure's Internet service tag. So for example, If it were to send outbound traffic to an internet destination, it would be blocked but if it is sending traffic to another private ip in another vnet, it'll be allowed. This was made to reduce unneed outbound connectivity.

## Public and Private DNS Zones.

I made a public and a private Azure DNS zones in which the public zone shows as the hostname using an A record while the private dns is linked via a network link to the ManufacturingVnet so resources in that Vnet can resolve internal hostnames like the manufacturing sensorvm.

A better way I liked when thinking of this is when a customer looks up my website, it would be a Public dns as its the public internet

But  for private dns it would be a manufacturing VM looks up an internal sensor VM.

I configured public dns for public hostname resolution and a private dns for interal resource discovery. I linked the private dns zone via a network link to the manufacturing vnet so resourcess in the manufacturing vnet can resolve internal hostnames. DNS handles name resolution, while routing and security controls determine whther communication is allowed or not.
