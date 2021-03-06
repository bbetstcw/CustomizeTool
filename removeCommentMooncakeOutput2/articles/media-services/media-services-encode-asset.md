<properties 
	pageTitle="Overview and Comparison of Azure On Demand Media Encoders" 
	description="This topic gives an overview and gives a comparison of Azire On Demand Media encoders." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako,anilmur" 
	manager="dwrede" 
	editor=""/>

<tags
	ms.service="media-services"
	ms.date="10/15/2015"
	wacn.date=""/>

#Overview and Comparison of Azure On Demand Media Encoders

##Encoding overview

Azure Media Services provides multiple options for the encoding of media in the cloud. 

When starting out with Media Services, it is important to understand the difference between codecs and file formats. 
Codecs are the software that implements the compression/decompression algorithms whereas file formats are containers that hold the compressed video.

Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming, HDS) without you having to re-package into these streaming formats. 

To take advantage of [dynamic packaging](/documentation/articles/media-services-dynamic-packaging-overview), you need to do the following:

- Encode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).
- Get at least one On-Demand streaming unit for the streaming endpoint from which you plan to delivery your content. For more information, see [How to Scale On-Demand Streaming Reserved Units](/documentation/articles/media-services-manage-origins#scale_streaming_endpoints).

Media Services supports the following on demand encoders that are described in this article:

- **Media Encoder Standard**
- **Azure Media Encoder** 

This article gives a brief overview of on demand media encoders and provides links to articles that give more detailed information. The topic also provides comparison of the encoders.

Note that by default each Media Services account can have one active encoding task at a time. You can reserve encoding units that allow you to have multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase. For information, see [Scaling encoding units](/documentation/articles/media-services-portal-encoding-units).

##Media Encoder Standard

###Overview

It is recommended to use the Media Encoder Standard encoder. However, it is currently not exposed via the Azure Management Portal.

When compared to Azure Media Encoder, this encoder supports more input and output formats and codecs. Other benefits include:

- More tolerant to how the input file was created
- Has a better H.264 codec quality than the Azure Media Encoder
- Is built on a newer and more flexible pipeline
- Is more robust/resilient

###How to use

[How to encode with Media Encoder Standard](/documentation/articles/media-services-dotnet-encode-with-media-encoder-standard)

###Formats

[Formats and codecs](/documentation/articles/media-services-media-encoder-standard-formats)

###Presets

Media Encoder Standard is configured using one of the encoder presets described [here](https://msdn.microsoft.com/zh-cn/library/azure/mt269960.aspx).

###Input and output metadata

The encoders input metadata is described [here](http://msdn.microsoft.com/zh-cn/library/azure/dn783120.aspx).

The encoders output metadata is described [here](http://msdn.microsoft.com/zh-cn/library/azure/dn783217.aspx).

###Thumbnail

For information on how to generate thumbnails, see [How to generate thumbnails using Media Encoder Standard](/documentation/articles/media-services-dotnet-generate-thumbnail-with-mes).

###Audio and/or video overlays

Currently, not supported.

###See also

[The Media Services blog](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##Azure Media Encoder

###Overview

Azure Media Encoder is one of the encoders supported by Media Services. Starting with July 2015, it is recommended to use [Media Encoder Standard](/documentation/articles/media-services-encode-asset#media_encoder_standard).

###How to use

[How to encode with Azure Media Encoder](/documentation/articles/media-services-dotnet-encode-asset)

###Formats

[Formats and codecs](/documentation/articles/media-services-azure-media-encoder-formats)

###Presets

Azure Media Encoder is configured using one of the encoder presets described [here](https://msdn.microsoft.com/zh-cn/library/azure/dn619392.aspx). You can also get the actual Azure Media Encoder preset files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/Azure%20Media%20Encoder).

###Input and output metadata

The encoders input metadata is described [here](http://msdn.microsoft.com/zh-cn/library/azure/dn783120.aspx).

The encoders output metadata is described [here](http://msdn.microsoft.com/zh-cn/library/azure/dn783217.aspx).

###Thumbnail

[Creating a thumbnail](https://msdn.microsoft.com/zh-cn/library/hh973624.aspx)

###Audio and/or video overlays

[Creating Overlays](/documentation/articles/media-services-azure-media-customize-ame-presets#creating-overlays).

###Naming convention

[How to modify the output file names](/documentation/articles/media-services-azure-media-customize-ame-presets#controlling-azure-media-encoder-output-file-names)

###See also

[Encoding your media with Dolby Digital Plus](/documentation/articles/media-services-encode-with-dolby-digital-plus)

##Media Encoder Premium Workflow

###Overview

[Introducing Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

###How to use

Media Encoder Premium Workflow is configured using complex workflows. Workflow files could be created and updated using the [Workflow Designer](/documentation/articles/media-services-workflow-designer) tool.

[How to Use Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

##<a id="compare_encoders"></a>Compare Encoders

###<a id="billing"></a>Billing meter used by each encoder

Media Processor Name|Applicable Pricing|Notes
---|---|---
**Media Encoder Standard** |ENCODER|Encoding Tasks will be charged according to the size of the output Asset, in GBytes, at the rate specified [here][1], under the ENCODER column.
**Azure Media Encoder** |ENCODER|Encoding Tasks will be charged according to the size of the output Asset, in GBytes, at the rate specified [here][1], under the ENCODER column.


This section compares the encoding capabilities of **Media Encoder Standard**, **Azure Media Encoder**.


###Input Container/File Formats

Input Container/File Formats|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
Adobe® Flash® F4V			|Yes|No		|Yes
MXF/SMPTE 377M				|Yes|Limited|Yes
GXF							|Yes|No		|Yes
MPEG-2 Transport Streams	|Yes|Yes	|Yes
MPEG-2 Program Streams		|Yes|Yes	|Yes
MPEG-4/MP4					|Yes|Yes	|Yes
Windows Media/ASF			|Yes|Yes	|Yes
AVI (Uncompressed 8bit/10bit)|Yes|Yes	|Yes
3GPP/3GPP2					|Yes|Yes	|No
Smooth Streaming File Format (PIFF 1.3)|Yes|Yes|No
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/zh-cn/library/windows/desktop/dd692984)|Yes|No|No
Matroska/WebM				|Yes|No|No

###Input Video Codecs

Input Video Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AVC 8-bit/10-bit, up to 4:2:2, including AVCIntra	|8 bit 4:2:0 and 4:2:2|Only 8bit 4:2:0|Yes
Avid DNxHD (in MXF)									|Yes|No|Yes
DVCPro/DVCProHD (in MXF)							|Yes|No|Yes
JPEG2000											|Yes|No|Yes
MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)|Up to 422 Profile|Up to 422 Profile|Yes
MPEG-1												|Yes|Yes|Yes
Windows Media Video/VC-1							|Yes|Yes|Yes
Canopus HQ/HQX										|No|Yes|No
MPEG-4 Part 2										|Yes|No|No
[Theora](https://en.wikipedia.org/wiki/Theora)		|Yes|No|No

###Input Audio Codecs

Input Audio Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AES (SMPTE 331M and 302M, AES3-2003)		|No|No|Yes
Dolby® E									|No|No|Yes
Dolby® Digital (AC3)						|No|Yes|Yes
Dolby® Digital Plus (E-AC3)					|No|No|Yes
AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)|Yes|Yes|Yes
MPEG Layer 2|Yes|Yes|Yes
MP3 (MPEG-1 Audio Layer 3)|Yes|Yes|Yes
Windows Media Audio|Yes|Yes|Yes
WAV/PCM|Yes|Yes|Yes
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Yes|No|No
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format) |Yes|No|No
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Yes|No|No


###Output Container/File Formats

Output Container/File Formats|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
Adobe® Flash® F4V|No|No|Yes
MXF (OP1a, XDCAM and AS02)|No|No|Yes
DPP (including AS11)|No|No|Yes
GXF|No|No|Yes
MPEG-4/MP4|Yes|Yes|Yes
MPEG-TS|Yes|No|Yes
Windows Media/ASF|No|Yes|Yes
AVI (Uncompressed 8bit/10bit)|No|No|Yes
Smooth Streaming File Format (PIFF 1.3)|No|Yes|Yes

###Output Video Codecs

Output Video Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AVC (H.264; 8-bit; up to High Profile, Level 5.2; 4K Ultra HD; AVC Intra)|Only 8 bit 4:2:0|Only 8 bit 4:2:0 up to 1080p|Yes
Avid DNxHD (in MXF)|No|No|Yes
DVCPro/DVCProHD (in MXF)|No|No|Yes
MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)|No|No|Yes
MPEG-1|No|No|Yes
Windows Media Video/VC-1|No|Yes|Yes
JPEG thumbnail creation|No|Yes|Yes

###Output Audio Codecs

Output Audio Codecs|Media Encoder Standard|Azure Media Encoder|Media Encoder Premium Workflow
---|---|---|---
AES (SMPTE 331M and 302M, AES3-2003)|No|No|Yes
Dolby® Digital (AC3)|No|Yes|Yes
Dolby® Digital Plus (E-AC3) up to 7.1|No|Up to 5.1|Yes
AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)|Yes|Yes|Yes
MPEG Layer 2|No|No|Yes
MP3 (MPEG-1 Audio Layer 3)|No|No|Yes
Windows Media Audio|No|Yes|Yes


##Media Services learning paths

You can view AMS learning paths here:

- [AMS Live Streaming Workflow](http://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS on Demand Streaming Workflow](http://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##Related articles

- [Quotas and Limitations](/documentation/articles/media-services-quotas-and-limitations)

 
<!--Reference links in article-->
[1]: /home/features/media-services/#price
