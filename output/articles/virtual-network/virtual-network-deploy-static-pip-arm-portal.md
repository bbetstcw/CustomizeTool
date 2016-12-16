<properties
    pageTitle="Create a VM with a static public IP using the Azure portal | Azure"
    description="Learn how to create a VM with a static public IP address through Azure Resource Manager using the Azure portal."
    services="virtual-network"
    documentationcenter="na"
    author="jimdial"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.assetid="e9546bcc-f300-428f-b94a-056c5bd29035"
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="02/04/2016"
    wacn.date=""
    ms.author="jdial" />

# Create a VM with a static public IP using the Azure portal

> [!div class="op_single_selector"]
- [Azure portal](/documentation/articles/virtual-network-deploy-static-pip-arm-portal/)
- [PowerShell](/documentation/articles/virtual-network-deploy-static-pip-arm-ps/)
- [Azure CLI](/documentation/articles/virtual-network-deploy-static-pip-arm-cli/)
- [Template](/documentation/articles/virtual-network-deploy-static-pip-arm-template/)
- [PowerShell (Classic)](/documentation/articles/virtual-networks-reserved-public-ip/)

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [AZURE.NOTE]
> Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](/documentation/articles/resource-manager-deployment-model/). This article covers using the Resource Manager deployment model, which Azure recommends for most new deployments instead of the classic deployment model.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## Create a VM with a static public IP

To create a VM with a static public IP address in the Azure portal, complete the following steps:

1. From a browser, navigate to the [Azure portal](https://portal.azure.cn) and, if necessary, sign in with your Azure account.
2. On the top left hand corner of the portal, click **New**>>**Compute**>**Windows Server 2012 R2 Datacenter**.
3. In the **Select a deployment model** list, select **Resource Manager** and click **Create**.
4. In the **Basics** blade, enter the VM information as shown below, and then click **OK**.
   
    ![Azure portal - Basics](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. In the **Choose a size** blade, click **A1 Standard** as shown below, and then click **Select**.
   
    ![Azure portal - Choose a size](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. In the **Settings** blade, click **Public IP address**, then in the **Create public IP address** blade, under **Assignment**, click **Static** as shown below. And then click **OK**.
   
    ![Azure portal - Create public IP address](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. In the **Settings** blade, click **OK**.
8. Review the **Summary** blade, as shown below, and then click **OK**.
   
    ![Azure portal - Create public IP address](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Notice the new tile in your dashboard.
   
    ![Azure portal - Create public IP address](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Once the VM is created, the **Settings** blade will be displayed as shown below
    
    ![Azure portal - Create public IP address](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

