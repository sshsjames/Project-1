# Interview Questions
The following questions were regarding the outcome of the project and were to be a simulated interview
situation.  

Domain Network Security: 
Suppose you have a firewall that's supposed to block SSH connections, but instead lets them through. 
How would you debug it?:

It is important for organizations to have established network security protocols in place.  These protocols
should include the first line of defense in monitoring incoming and outgoing network traffic such as a firewall.  
Within Project 1 at the University of Denver Cybersecurity Bootcamp, I built an Azure virtual network 
with three web servers controlled by a Jump Box Provisioner.  All commands given were over a secured 
SSH connection on port 22.  If you tried to connect to a VM within this network that did not accept SSH 
connections; you would be denied.  A network Security Group Rule was created to ensure that only connections
within the virtual network, and through a specific internal IP (Jump Box Server) could be established.  
This creates a network of secured tunnels for information to pass through, limiting outside attacks.

If I was told that one of the Web servers within the network I created did accept an SSH connection, 
I would ensure that the Network Security Group rules I created had the correct source and destination 
IP addresses, as well as the correct port protocols in place.  
To ensure that the problem was fixed, I would again attempt to remotely control the server through an SSH
connection with the incorrect IP address that was brought to my attention.  This would require looking at
the Network Security Groups that I created.  As you can see in the image included below, the rule labeled 
SSH_From_Jump_Box would be a great place to start. 

![alt text](https://github.com/sshsjames/Project-1/blob/main/Images/NSG_Example.png) 

This is where either the edits would need to be made or would lead me towards creating an additional
rule with a higher priority (the lower number assigned) to ensure it was in place and acted on 
before moving to other network connections.   

Additionally, I would also test this with a larger range to ensure that all connections outside the scope
of the internal rule would not manifest.   
A solution such as this does not entirely guarantee that this network is immune to all unauthorized access.
The difficulty of the limited controls means it necessary to continually monitor the servers.  
Additionally, any expansion or additional servers, load balancers, or even networks added may require changes.  
With the currently limited control of the server, the network is entirely dependant on one connection from 
outside the network.  This could be a challenge when it comes to events such as power outages to employee movement.  
A system like this would need additional controls within a live environment.  In order to monitor 
potentially suspicious authentication attempts, I would also add steps to monitor events such as data packet
capture analysis that could be installed on a Linux machine.  I would also protect the virtual networks by
adding an Azure Firewall and enabling a vulnerability assessment solution that is provided through Azure
to ensure thought has been given to the configuration beyond just my personal recommendations.  

As technology grows, so do the paths to implementation. It is best to keep an umbrella policy in place to ensure
nothing new or just released is not a consideration!


# The Cloud

Domain Cloud Security
How would you control access to a cloud network?:

It is important to control access to the cloud network.  Without controls, it is as if you are giving
away the keys to the castle that is your enterprise.

Within the Ansible Project completed through the University of Denver, we built a cloud network that
included deployments of Ansible and containers within different zones across the continental US.  

A network that is completely virtual with no server rooms or physical machines to touch requires local
firewalls and Network Security Groups (NSGs).  WIthin this project,  a jump box provisioner server was
used as a way to configure each of the container deployments within one centralized access point.  
There are significant advantages to deployments such as this.  
Here are just a few:
Minimal Investment in Equipment - With the servers, switches, and firewalls being virtual, there are 
no expensive server rooms to manage on site.  No busy cables or cooling systems to keep it all functioning.
Not to mention the on-premise security that would need to be provided.  
Overhead Costs - There are of course costs to virtual deployments.  The advantage for businesses that
are growing is the costing is scalable, with upgrades performed in an instant, without the worry of
outdated equipment being part of the costing or depreciation considerations.  The size and value of
equipment available virtually is endless.
Less is More - Virtual Machines (VMs) and individual processing power applications can get expensive.  
When using a container deployment, the amount of storage and processing speed is significantly less
over the span of the larger operation.  That means containers do not require multiple technicians to
configure and update each workspace.  Instead, updates, protocols, storage and even cleanups can be 
completed from centralized locations and pushed out to each container within a fraction of the time it
takes on a cluster of VMs.  

Within the Ansible Deployment, I had to configure access controls that protect the servers and limits
who has access to the working environment.  An NSG was created to control who would be able to access
the jump box server.  

Since the jump box controls all of the commerce and MX servers, this required the most stringent of 
controls to include the following inbound rules:
Allow Personal to SSH - Since I would be managing the network remotely, I needed to ensure that I would be
able to remote in from my home computer through a secured connection.  The control was set to allow only
TCP traffic from my home IP address to the local IP address of the jump box on port 22 (SSH connections).  
Allow Jump Box to Web SSH - The next protocol to configure was to allow the jump box to use an SSH connection
on any port to communicate with servers within the virtual network using port 22.  
Allow HTTP from the Internet - In order to receive the necessary data over the internet, a rule was set up
to allow HTTP traffic from the internal virtual network back to my personal or home IP address.

Load Balancer Communication - A default rule was already in place to allow the load balance to communicate 
over any port, with any protocol, within the virtual network.  

Thank you Azure for making simple defaults!

Deny All Inbound - Finally, after all these rules were met, a final rule of denying all inbound traffic that
is otherwise not specifically mentioned above.  

Once again, this was a default that Azure recognizes and puts in place as a last catch safety net!

For the ELK or Elastic Stack, a separate security group was created to control access to the ELK Container.
Since this was built within Azure, the above defaults were also already included.  Pretty sweet!   

The inbound rules that needed to be added were the following:
Allow SSH All TCP - traffic arriving on port 22 would be allowed
Personal to ELK - A rule put in place that restricts access to the ELK secure network by only allowing
a connection through my local or home IP as configured.

With those inbound rules in place, another consideration was the outbound rules for the security group.
For this deployment, the default rules in place were all that was necessary for both security groups.  
Those included allowing outbound communication over the internal network and the greater internet.  

Within project 1, I had to consider the access controls around the Virtual Network and the Virtual Machines.    
Traffic was limited to the web servers as being ultimately denied both inbound and outbound traffic as a 
full-stop solution.  

Azure has the setup and build out phase like this to ensure all applications and rules are in place before
opening the metaphorical floodgates if you will.  
It’s kind of like setting up the storefront on black friday, only to save the unlocking of the front doors
until the very last moment, when everything else is in place.  

A solution like this is scalable.  In fact, all of the Ansible environment is quickly scalable
for expansions, as well as unlikely contractions as needed.  When it comes to using a jump box, there are better
(or more secure) solutions.  
A jump box is a way to remotely control environments through an SSH connection.  This allows one person, 
at a central location to control the push and pull actions throughout the enterprise.  

There are however, disadvantages of a VPN.  One of the most common complaints is the possibility of slowing
your internet connection speed.  Due to the nature of exchanging data over a VPN, things can get slow and
laggy.  Another posibility is that as the network grows, it could be difficult to configure out to scale.
This can be avoided with the right team dedicated to the expansion of the network, and a solid start 
that is designed to build at scale.  



