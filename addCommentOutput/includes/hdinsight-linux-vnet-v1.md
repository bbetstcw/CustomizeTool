<!-- not suitable for Mooncake -->


<!-- not suitable for Mooncake -->


> You cannot use a v1 (Classic,) Azure Virtual Network with Linux-based HDInsight. The Virtual Network must be v2 (Azure Resource Manager,) in order for it to be listed as an option during the HDInsight cluster creation process in the Azure Portal Preview, or to be usable when creating a cluster from the Azure CLI or Azure PowerShell.
> 
> If you have resources on a v1 network, and you wish to make HDInsight directly accessible to those resources through a virtual network, see [Connecting classic VNets to new  VNets](/documentation/articles/vpn-gateway-connect-different-deployment-models-portal/)  VNets](/documentation/articles/virtual-networks-arm-asm-s2s/)  for information on how to connect a v2 Virtual Network to a v1 Virtual Network. Once this connection is established, you can create the HDInsight cluster in the v2 Virtual Network.