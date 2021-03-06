# Azure Identity

Managing identity is just as important in the public cloud is it is on premises. To help with this, Azure supports several different cloud identity technologies. They include these:

- You can run Windows Server Active Directory (commonly called just AD) in the cloud using virtual machines created with Azure Virtual Machines. This approach makes sense when you're using Azure to extend your on-premises datacenter into the cloud.

- You can use Azure Active Directory to give your users single sign-on to Software as a Service (SaaS) applications. Microsoft's Office 365 uses this technology, for example, and applications running on Azure or other cloud platforms can also use it.

- Applications running in the cloud or on-premises can use Azure Active Directory Access Control to let users log in using identities from Facebook, Google, Microsoft, and other identity providers.


This article describes all three of these options.

## Table of Contents

- [Running Windows Server Active Directory in VMs](#adinvm)

- [Using Azure Active Directory](#ad)

- [Using Azure Active Directory Access Control](#ac)


## <a name="adinvm"></a>Running Windows Server Active Directory in VMs

Running Windows Server AD in Azure VMs is much like running it on premises. [Figure 1](#fig1) shows a typical example of how this looks.

![Azure Active Directory in Virtual Machine](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Figure 1: Windows Server Active Directory can run in Azure VMs connected to an organization's on-premises datacenter using Azure Virtual Network.

In the example shown here, Windows Server AD is running in VMs created using Azure Virtual Machines, the platform's IaaS technology. These VMs and a few others are grouped into a virtual network (VNET) connected to an on-premises datacenter using Azure Virtual Network. The VNET carves out a group of cloud VMs that interact with the on-premises network via a virtual private network (VPN) connection. Doing this lets these Azure VMs look like just another subnet to the on-premises datacenter. As the figure shows, two of those VMs are running Windows Server AD domain controllers. The other VMs in the VNET might be running applications, such as SharePoint, or being used in some other way, such as for development and testing. The on-premises datacenter is also running two Windows Server AD domain controllers.

There are several options for connecting the domain controllers in the cloud with those running on premises. They include these:

- Make all of them part of a single Active Directory domain.

- Create separate AD domains on-premises and in the cloud that are part of the same forest.

- Create separate AD forests in the cloud and on-premises, then connect the forests using cross-forest trusts or Windows Server Active Directory Federation Services (AD FS), which can also run in VMs on Azure.

Whatever choice is made, an administrator should make sure that authentication requests from on-premises users go to cloud domain controllers only when necessary, since the link to the cloud is likely to be slower than on-premises networks. Another factor to consider in connecting cloud and on-premises domain controllers is the traffic generated by replication. Domain controllers in the cloud are typically in their own AD site, which allows an administrator to schedule how often replication is done. Azure charges for traffic sent out of an Azure datacenter, although not for bytes sent in, which might affect the administrator's replication choices. It's also worth pointing out that while Azure does provide its own Domain Name System (DNS) support, this service is missing features required by Active Directory (such as support for Dynamic DNS and SRV records). Because of this, running Windows Server AD on Azure requires setting up your own DNS servers in the cloud.

Running Windows Server AD in Azure VMs can make sense in several different situations. Here are some examples:

- If you're using Azure Virtual Machines as an extension of your own datacenter, you might run applications in the cloud that need local domain controllers to handle things such as Windows Integrated Authentication requests or LDAP queries. SharePoint, for example, interacts frequently with Active Directory, and so while it's possible to run a SharePoint farm on Azure using an on-premises directory, setting up domain controllers in the cloud will significantly improve performance. (It's important to realize that this isn't necessarily required, however; plenty of applications can run successfully in the cloud using only on-premises domain controllers.)

- Suppose a faraway branch office lacks the resources to run its own domain controllers. Today, its users must authenticate to domain controllers on the other side of the world - logins are slow. Running Active Directory on Azure in a closer Microsoft datacenter can speed this up without requiring more servers in the branch office.

- An organization that uses Azure for disaster recovery might maintain a small set of active VMs in the cloud, including a domain controller. It can then be prepared to expand this site as needed to take over for failures elsewhere.

There are also other possibilities. For example, you're not required to connect Windows Server AD in the cloud to an on-premises datacenter. If you wanted to run a SharePoint farm that served a particular set of users, for instance, all of whom would log in solely with cloud-based identities, you might create a standalone forest on Azure. How you use this technology depends on what your goals are. (For more detailed guidance on using Windows Server AD with Azure, [see here](http://msdn.microsoft.com/zh-cn/library/azure/jj156090.aspx).)

## <a name="ad"></a>Using Azure Active Directory

As SaaS applications become more and more common, they raise an obvious question: What kind of directory service should these cloud-based applications use? Microsoft's answer to that question is Azure Active Directory.

There are two main options for using this directory service in the cloud:

- Individuals and organizations that use only SaaS applications can rely on Azure Active Directory as their sole directory service.

- Organizations that run Windows Server Active Directory can connect their on-premises directory to Azure Active Directory, then use it to give their users single sign-on to SaaS applications.


[Figure 2](#fig2) illustrates the first of these two options, where Azure Active Directory is all that's required.

![Azure Active Directory in Virtual Machine](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Figure 2: Azure Active Directory gives an organization's users single sign-on to SaaS applications, including Office 365.

As the figure shows, Azure AD is a multi-tenant service. This means that it can simultaneously support many different organizations, storing directory information about users at each of them. In this example, a user at organization A is trying to access a SaaS application. This application might be part of Office 365, such as SharePoint Online, or it might be something else - non-Microsoft applications can also use this technology. Because Azure AD supports the SAML 2.0 protocol, all that's required from an application is the ability to interact using this industry standard. (In fact, applications that use Azure AD can run in any datacenter, not just an Azure datacenter.)

The process begins when the user accesses a SaaS application (step 1). To use this application, the user must present a token issued by Azure AD.

This token contains information that identifies the user, and it's digitally signed by Azure AD. To get the token, the user authenticates himself to Azure AD by providing a username and password (step 2). Azure AD then returns the token he needs (step 3).

This token is then sent to the SaaS application (step 4), which validates the token's signature and uses its contents (step 5). Typically, the application will use the identity information the token contains to decide what information the user is allowed to access and perhaps in other ways.

If the application needs more information about the user than what's contained in the token, it can request this directly from Azure AD using the Azure AD Graph API (step 6). In the initial version of Azure AD, the directory schema is quite simple: It contains just users and groups and relationships among them. Applications can use this information to learn about connections between users. For example, suppose an application needs to know who this user's manager is to decide whether he's allowed access to some chunk of data. It can learn this by querying Azure AD through the Graph API.

The Graph API uses an ordinary RESTful protocol, which makes it straightforward to use from most clients, including mobile devices. The API also supports the extensions defined by OData, adding things such as a query language to let clients access data in more useful ways. (For more on OData, see [Introducing OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Because the Graph API can be used to learn about relationships between users, it lets applications understand the social graph that's embedded in the Azure AD schema for a particular organization (which is why it's called the Graph API). And to authenticate itself to Azure AD for Graph API requests, an application uses OAuth 2.0.

If an organization doesn't use Windows Server Active Directory - it has no on-premises servers or domains - and relies solely on cloud applications that use Azure AD, using just this cloud directory would give the firm's users single sign-on to all of them. Yet while this scenario gets more common every day, most organizations still use on-premises domains created with Windows Server Active Directory. Azure AD has a useful role to play here as well, as [Figure 3](#fig3) shows.

![Azure Active Directory in Virtual Machine](./media/identity/identity_03_AD.png)
<a id="fig3"></a>Figure 3: An organization can federate Windows Server Active Directory with Azure Active Directory to give its users single sign-on to SaaS applications.

In this scenario, a user at organization B wishes to access a SaaS application. Before she does this, the organization's directory administrators must establish a federation relationship with Azure AD using AD FS, as the figure shows. Those admins must also configure data synchronization between the organization's on-premises Windows Server AD and Azure AD. This automatically copies user and group information from the on-premises directory to Azure AD. Notice what this allows: In effect, the organization is extending its on-premises directory into the cloud. Combining Windows Server AD and Azure AD in this way gives the organization a directory service that can be managed as a single entity, while still having a footprint both on-premises and in the cloud.

To use Azure AD, the user first logs in to her on-premises Active Directory domain as usual (step 1). When she tries to access the SaaS application (step 2), the federation process results in Azure AD issuing her a token for this application (step 3). (For more on how federation works, see [Claims-Based Identity for Windows: Technologies and Scenarios](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) As before, this token contains information that identifies the user, and it's digitally signed by Azure AD. This token is then sent to the SaaS application (step 4), which validates the token's signature and uses its contents (step 5). And is in the previous scenario, the SaaS application can use the Graph API to learn more about this user if necessary (step 6).

Today, Azure AD isn't a complete replacement for on-premises Windows Server AD. As already mentioned, the cloud directory has a much simpler schema, and it's also missing things such as group policy, the ability to store information about machines, and support for LDAP. (In fact, a Windows machine can't be configured to let users log in to it using nothing but Azure AD - this isn't a supported scenario.) Instead, the initial goals of Azure AD include letting enterprise users access applications in the cloud without maintaining a separate login and freeing on-premises directory administrators from manually synchronizing their on-premises directory with every SaaS application their organization uses. Over time, however, expect this cloud directory service to address a wider range of scenarios.

## <a name="ac"></a>Using Azure Active Directory Access Control

Cloud-based identity technologies can be used to solve a variety of problems. Azure Active Directory can give an organization's users single sign-on to multiple SaaS applications, for example. But identity technologies in the cloud can also be used in other ways.

Suppose, for instance, that an application wishes to let its users log in using tokens issued by multiple *identity providers (IdPs)*. Lots of different identity providers exist today, including Facebook, Google, Microsoft, and others, and applications frequently let users sign in using one of these identities. Why should an application bother to maintain its own list of users and passwords when it can instead rely on identities that already exist? Accepting existing identities makes life simpler both for users, who have one less username and password to remember, and for the people who create the application, who no longer need to maintain their own lists of usernames and passwords.

But while every identity provider issues some kind of token, those tokens aren't standard - each IdP has its own format. Furthermore, the information in those tokens also isn't standard. An application that wishes to accept tokens issued by, say, Facebook, Google, and Microsoft is faced with the challenge of writing unique code to handle each of these different formats.

But why do this? Why not instead create an intermediary that can generate a single token format with a common representation of identity information? This approach would make life simpler for the developers who create applications, since they now need to handle only one kind of token. Azure Active Directory Access Control does exactly this, providing an intermediary in the cloud for working with diverse tokens. [Figure 4](#fig4) shows how it works

![Azure Active Directory in Virtual Machine](./media/identity/identity_04_IdentityProviders.png) 
<a id="fig4"></a>Figure 4: Azure Active Directory Access Control makes it easier for applications to accept identity tokens issued by different identity providers.

The process begins when a user attempts to access the application from a browser. The application redirects her to an IdP of her choice (and that the application also trusts). She authenticates herself to this IdP, such as by entering a username and password (step 1), and the IdP returns a token containing information about her (step 2).

As the figure shows, Access Control supports a range of different cloud-based IdPs, including accounts created by Google, Yahoo, Facebook, Microsoft (formerly known as Windows Live ID), and any OpenID provider. It also supports identifies created using Azure Active Directory and, through federation with AD FS, Windows Server Active Directory. The goal is to cover the most commonly used identities today, whether they're issued by IdPs in the cloud or on-premises.

Once the user's browser has an IdP token from her chosen IdP, it sends this token to Access Control (step 3). Access Control validates the token, making sure that it really was issued by this IdP, then creates a new token according to the rules that have been defined for this application. Like Azure Active Directory, Access Control is a multi-tenant service, but the tenants are applications rather than customer organizations. Each application can get its own namespace, as the figure shows, and can define various rules about authorization and more.

These rules let each application's administrator define how tokens from various IdPs should be transformed into an Access Control token. For example, if different IdPs use different types for representing usernames, Access Control rules can transform all of these into a common username type. Access Control then sends this new token back to the browser (step 4), which submits it to the application (step 5). Once it has the Access Control token, the application verifies that this token really was issued by Access Control, then uses the information it contains (step 6).

While this process might seem a little complicated, it actually makes life significantly simpler for the creator of the application. Rather than handle diverse tokens containing different information, the application can accept identities issued by multiple identity providers while still receiving only a single token with familiar information. Also, rather than require each application to be configured to trust various IdPs, these trust relationships are instead maintained by Access Control - an application need only trust it.

It's worth pointing out that nothing about Access Control is tied to Windows - it could just as well be used by a Linux application that accepted only Google and Facebook identities. And even though Access Control is part of the Azure Active Directory family, you can think of it as an entirely distinct service from what was described in the previous section. While both technologies work with identity, they address quite different problems (although Microsoft has said that it expects to integrate the two at some point).

Working with identity is important in nearly every application. The goal of Access Control is to make it easier for developers to create applications that accept identities from diverse identity providers. By putting this service in the cloud, Microsoft has made it available to any application running on any platform.

##About the Author

David Chappell is Principal of Chappell & Associates [www.davidchappell.com](http://www.davidchappell.com) in San Francisco, California. Through his speaking, writing, and consulting,
