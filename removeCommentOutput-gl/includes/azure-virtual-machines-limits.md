Resource|Default Limit|Maximum Limit
---|---|---
[Virtual machines](/documentation/articles/virtual-machines-linux-about/) per cloud service<sup>1</sup>|50|50
Input endpoints per cloud service<sup>2</sup>|150|150

<sup>1</sup>Virtual machines created in Service Management (instead of Resource Manager) are automatically stored in a cloud service. You can add more virtual machines to that cloud service for load balancing and availability. See  [How to Connect Virtual Machines with a Virtual Network or Cloud Service](/documentation/articles/virtual-machines-linux-classic-connect-vms/).

<sup>2</sup>Input endpoints allow communications to a virtual machine from outside the virtual machine's cloud service. Virtual machines in the same cloud service or virtual network can automatically communicate with each other. See [How to Set Up Endpoints to a Virtual Machine](/documentation/articles/virtual-machines-windows-classic-setup-endpoints/). 
