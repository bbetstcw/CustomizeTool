<properties
	pageTitle="What is Azure Search | Windows Azure | Hosted cloud search service"
	description="Azure Search is a fully-managed hosted cloud search service. Learn more in this feature overview."
	services="search"
	authors="ashmaka"
	documentationCenter=""/>

<tags
	ms.service="search"
	ms.date="12/18/2015"
	wacn.date=""/>

# What is Azure Search?
Azure Search is a cloud search-as-a-service solution that delegates server and infrastructure management to Microsoft, leaving you with a ready-to-use service that you can populate with your data and then search against. Azure Search allows you to easily add a robust search experience to their applications using a simple REST API or .NET SDK without managing search infrastructure and becoming an expert in search.

## Give your users a powerful search experience

**Powerful queries** using logical operators, comparison expressions, and more can be formulated using [OData syntax](https://msdn.microsoft.com/zh-cn/library/azure/dn798921.aspx) and the [simple query syntax](https://msdn.microsoft.com/zh-cn/library/azure/dn798920.aspx). Additionally, the [Lucene query syntax](https://msdn.microsoft.com/zh-cn/library/azure/mt589323.aspx) (currently in preview) can enable fuzzy search, proximity search, term boosting, and regular expressions. Azure Search also supports custom lexical analyzers to allow your application to handle complex search queries using phonetic matching and regular expressions.

**Language support** is [included for 56 different languages](https://msdn.microsoft.com/zh-cn/library/azure/dn879793.aspx). Using both Lucene analyzers and Microsoft analyzers (refined by years of natural language processing in Office and Bing), Azure Search can analyze text using word-breaking, text normalization, lemmatization, and more. This allows your application's search box to intelligently handle misspellings, verb tenses, irregular plural nouns (e.g. 'mouse' vs. 'mice'), and more.

**Search suggestions** can be enabled for autocompleted search bars and type-ahead queries. [Actual documents in your index are suggested](https://msdn.microsoft.com/zh-cn/library/azure/dn798936.aspx) as users enter partial search input.

**Hit highlighting** [allows](https://msdn.microsoft.com/zh-cn/library/azure/dn798927.aspx) users to see the snippet of text in each result that contains the matches for their query. You can pick and choose which fields return highlighted snippets.

**Faceted navigation** is easily added to your search results page with Azure Search. Using [just a single query parameter](https://msdn.microsoft.com/zh-cn/library/azure/dn798927.aspx), Azure Search will return all the necessary information to construct a faceted search experience in your app's UI to allow your users to drill-down and filter search results  (e.g. filter catalog items by price-range or brand).

**Geo-spatial** [support](https://msdn.microsoft.com/zh-cn/library/azure/dn798921.aspx) allows you to intelligently process, filter, and display geographic locations. Azure Search enables your users to explore data based on the proximity of a search result to a specified location or based on a specific geographic region.

**Filters** can be used to easily incorporate faceted navigation (e.g. filtering by category or price), enhance query formulation, and filter based on user- or developer-specified criteria.

## Empower your developers with an easy-to-use service

**High availability** ensures an extremely reliable search service experience. When scaled properly, [Azure Search offers a 99.9% SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Fully managed** as an end-to-end solution, Azure Search requires absolutely no infrastructure management. Your service can be easily tailored to your needs by scaling in two dimensions to handle more document storage, higher query loads, or both.

**Data integration** using [indexers](https://msdn.microsoft.com/zh-cn/library/azure/dn946891.aspx) allows Azure Search to automatically crawl Azure SQL Database or Azure DocumentDB to sync your search index's content with your primary data store.

**Document cracking** is available [to read and index major file formats](/documentation/articles/search-howto-indexing-azure-blob-storage) including Microsoft Office as well as PDF and HTML documents.

**Search traffic analytics** are [easily collected and analyzed](/documentation/articles/search-traffic-analytics) to unlock insights from what users are typing into the search box.

**Simple scoring** is a key benefit of Azure Search. [Scoring profiles](https://msdn.microsoft.com/zh-cn/library/azure/dn798928.aspx) are used to allow organizations to model relevance as a function of values in the documents themselves. For example, you might want newer products or discounted products to appear higher in the search results. You can also build scoring profiles using tags for personalized scoring based on customer search preferences you've tracked and stored separately.

**Sorting** is offered for multiple fields via the index schema and then toggled at query-time with a single search parameter.

**Paging** and throttling your search results is straightforward with the finely tuned control that Azure Search offers over your search results.  

**Search explorer** allows you to issue queries against all of your indexes right from your account's Azure Management Portal so you can easily test queries and refine scoring profiles.

## How it works

### 1. Provision service
You can spin up an Azure Search service using either the [Azure Management Portal](https://manage.windowsazure.cn/) or the [Azure Resource Management API](https://msdn.microsoft.com/zh-cn/library/azure/dn832684.aspx).

Depending on how you configure the service, you'll use either the Free tier that is shared with other Azure Search subscribers, or the Standard [pricing tier](/home/features/search/#price) that dedicates resources to be used only by your service. When provisioning your service, you also choose the region of the data center that hosts your service.

When using Azure Search in the Standard tier, you can scale your service in two dimensions: 1) Add Replicas to grow your capacity to handle heavy query loads and 2) add Partitions to add storage for more documents. By handling document storage and query throughput separately, you can customize your search service for your specific needs.

### 2. Create index
Before you can upload your content to your Azure Search service, you must first define an Azure Search index. An index is like a database table that holds your data and can accept search queries. You define the index schema to map to the structure of the documents you wish to search, similar to fields in a database.

The schema of these indexes can either be created in the Azure Management Portal, or programmatically [using the .NET SDK](/documentation/articles/search-howto-dotnet-sdk) or [REST API](https://msdn.microsoft.com/zh-cn/library/azure/dn798941.aspx). Once the index is defined, you can then upload your data to the Azure Search service where it is subsequently indexed.

### 3. Index data
Once you have defined the fields and attributes of your index, you're ready to upload your content into the index. You can use either a push or pull model to upload data to the index.

The pull model is provided through indexers that can be configured for on demand or scheduled updates (see [Indexer operations (Azure Search Service REST API)](https://msdn.microsoft.com/zh-cn/library/azure/dn946891.aspx)), allowing you to easily ingest data and data changes from an Azure DocumentDB, Azure SQL Database, Azure Blob Storage, or SQL Server hosted in an Azure VM.

The push model is provided through the SDK or REST APIs used for sending updated documents to an index. You can push data from virtually any dataset using the JSON format. See [Add, update, or delete Documents](https://msdn.microsoft.com/zh-cn/library/azure/dn798930.aspx) or [How to use the .NET SDK)](/documentation/articles/search-howto-dotnet-sdk) for guidance on loading data.

### 4. Search
Once you have populated your Azure Search index, you can now [issue search queries](https://msdn.microsoft.com/zh-cn/library/azure/dn798927.aspx) to your service endpoint using simple HTTP requests with REST API or the .NET SDK.

## Try it now (for free!)
You can try Azure Search today! If you already have an Azure account, you can [provision a service in the Free tier](/documentation/articles/search-create-service-portal).

If you don't have an Azure account you can try a free, 60-minute session with no sign up required. Go to the [Try Azure Websites](http://go.microsoft.com/fwlink/p/?LinkId=618214) and select "web site." Then select the "ASP.NET + Azure Search" template to get started.
[How to use Azure Search in .NET](search-howto-dotnet-sdk.md)
[Get Started with Azure Search .NET](search-get-started-dotnet.md)
[Azure Search: tutorials, video demos, and samples](search-video-demo-tutorial-list.md)
