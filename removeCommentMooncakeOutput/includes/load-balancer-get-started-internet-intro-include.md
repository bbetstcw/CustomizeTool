You can use a load balancer to provide high availability for your workloads in Azure. An Azure load balancer is a Layer-4 (TCP, UDP) type load balancer that distributes incoming traffic among healthy service instances in cloud services or virtual machines defined in a load balancer set.
 
You can configure a load balancer to.

- Load balance incoming Internet traffic to virtual machines (VMs). We refer to a load balancer in this scenario as an [Internet facing load balancer](/documentation/articles/load-balancer-internet-overview).
- Load balance traffic between VMs in a virtual network (VNet), VMs in cloud services, or between on-premises computers and VMs in a cross-premises virtual network. We refer to a load balancer in this scenario as an [internal load balancer (ILB)](/documentation/articles/load-balancer-internal-overview).
- 	Forward external traffic to a specific VM instance.
