<properties 
	pageTitle="DocumentDB Python SDK | Windows Azure" 
	description="Learn all about the Python SDK including release dates, retirement dates, and changes made between each version of the DocumentDB Python SDK." 
	services="documentdb" 
	documentationCenter="python" 
	authors="ryancrawcour" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags
	ms.service="documentdb"
	ms.date="11/16/2015"
	wacn.date=""/>

# DocumentDB SDK

> [AZURE.SELECTOR]
- [.NET SDK](/documentation/articles/documentdb-sdk-dotnet)
- [Node.js SDK](/documentation/articles/documentdb-sdk-node)
- [Java SDK](/documentation/articles/documentdb-sdk-java)
- [Python SDK](/documentation/articles/documentdb-sdk-python)

##DocumentDB Python SDK

<table>
<tr><td>**Download**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Contribute**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Documentation**</td><td>[Python SDK Reference Documentation](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Get Started**</td><td>[Get started with the Python SDK](/documentation/articles/documentdb-python-application)</td></tr>
<tr><td>**Current Supported Platform**</td><td>[Python 2.7](https://www.python.org/download/releases/2.7/)</td></tr>
</table></br>

## Release Notes

### <a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implement Upsert. New UpsertXXX methods added to support Upsert feature.
- Implement ID Based Routing. No public API changes, all changes internal.

### <a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Supports GeoSpatial index.
- Validates id property for all resources. Ids for resources cannot contain ?, /, #, \, characters or end with a space.
- Adds new header "index transformation progress" to ResourceResponse.

### <a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Implements V2 indexing policy

### <a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Supports proxy connection

### <a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK

## Release & Retirement Dates
Microsoft will provide notification at least **12 months** in advance of retiring an SDK in order to smooth the transition to a newer/supported version.

New features and functionality and optimizations are only added to the current SDK, as such it is  recommend that you always upgrade to the latest SDK version as early as possible. 

Any request to DocumentDB using a retired SDK will be rejected by the service.

> [AZURE.WARNING]
All versions of the Azure DocumentDB SDK for Python prior to version **1.0.0** will be retired on **February 29, 2016**. 

<br/>

| Version | Release Date | Retirement Date 
| ---	  | ---	         | ---
| [1.4.2](#1.4.2) | October 06, 2015 |---
| [1.4.1](#1.4.1) | October 06, 2015 |---
| [1.2.0](#1.2.0) | August 06, 2015 |---
| [1.1.0](#1.1.0) | July 09, 2015 |---
| [1.0.1](#1.0.1) | May 25, 2015 |---
| [1.0.0](#1.0.0) | April 07, 2015 |---
| 0.9.4-prelease | January 14, 2015 | February 29, 2016
| 0.9.3-prelease | December 09, 2014 | February 29, 2016
| 0.9.2-prelease | November 25, 2014 | February 29, 2016
| 0.9.1-prelease | September 23, 2014 | February 29, 2016
| 0.9.0-prelease | August 21, 2014 | February 29, 2016

## FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../includes/documentdb-sdk-faq.md)]

## See Also

To learn more about DocumentDB, see [Windows Azure DocumentDB](/home/features/documentdb/) service page. 
