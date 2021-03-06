<properties
	pageTitle=" Get started with delivering content on demand using the Azure Management Portal | Windows Azure"
	description="This tutorial walks you through the steps of implementing a Video-on-Demand (VoD) content delivery application with Azure Media Services using the Azure Management Portal."
	services="media-services"
	documentationCenter=""
	authors="Juliako"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="media-services"
	ms.date="10/05/2015"
	wacn.date=""/>


# Get started with delivering content on demand using the Azure Management Portal


[AZURE.INCLUDE [media-services-selector-get-started](../includes/media-services-selector-get-started.md)]


This tutorial walks you through the steps of implementing a basic Video-on-Demand (VoD) content delivery application using the Azure Management Portal.

> [AZURE.NOTE] To complete this tutorial, you need an Azure account. If you don't have an account, you can create a trial account in just a couple of minutes. For details, see <a href="/pricing/1rmb-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure Trial</a>.

This tutorial includes the following tasks:

1.  Create an Azure Media Services account.
2.  Configure streaming endpoint.
1.  Upload a video file.
1.  Encode the source file into a set of adaptive bitrate MP4 files.
1.  Publish the asset and get streaming and progressive download URLs.  
1.  Play your content.


## Create an Azure Media Services account

1. In the [Azure Management Portal](https://manage.windowsazure.cn/), click **New**, click **Media Service**, and then click **Quick Create**.

	![Media Services Quick Create](./media/media-services-portal-get-started/wams-QuickCreate.png)

2. In **NAME**, enter the name of the new account. A Media Services account name is all lowercase numbers or letters with no spaces, and is 3 to 24 characters in length.

3. In **REGION**, select the geographic region that will be used to store the metadata records for your Media Services account. Only the available Media Services regions appear in the drop-down list box.

4. In **STORAGE ACCOUNT**, select a storage account to provide blob storage of the media content from your Media Services account. You can select an existing storage account in the same geographic region as your Media Services account, or you can create a new storage account. A new storage account is created in the same region.

5. If you created a new storage account, in **NEW STORAGE ACCOUNT NAME**, enter a name for the storage account. The rules for storage account names are the same as for Media Services accounts.

6. Click **Quick Create** at the bottom of the form.

	You can monitor the status of the process in the message area at the bottom of the window.

	Once the account is successfully created, the status changes to Active.

	At the bottom of the page, the **MANAGE KEYS** button appears. When you click this button, a dialog box with the Media Services account name and the primary and secondary keys is displayed. You will need the account name and the primary key information to programmatically access the Media Services account.

	![Media Services Page](./media/media-services-portal-get-started/wams-mediaservices-page.png)

	When you double-click the account name, the Quick Start page is displayed by default. This page enables you to do some management tasks that are also available on other pages of the portal. For example, you can upload a video file from this page, or do it from the CONTENT page.


## Configure streaming endpoint using the portal

When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients. With adaptive bitrate streaming, the client can switch to a higher or lower bitrate stream as the video is displayed based on the current network bandwidth, CPU utilization, and other factors. Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH, and HDS (for Adobe PrimeTime/Access licensees only).

Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming, HDS) without you having to re-package into these streaming formats.

To take advantage of dynamic packaging, you need to do the following:

- Encode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).  
- Get at least one streaming unit for the *streaming endpoint* from which you plan to delivery your content.

With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.

To change the number of streaming reserved units, do the following:

1. In the [Azure Management Portal](https://manage.windowsazure.cn/), click **Media Services**. Then, click the name of the media service.

2. Select the STREAMING ENDPOINTS page. Then, click the streaming endpoint that you want to modify.

3. To specify the number of streaming units, select the **SCALE** tab and move the **reserved capacity** slider.

	![Scale page](./media/media-services-portal-get-started/media-services-origin-scale.png)

4. Click the **SAVE** button to save your changes.

	The allocation of any new units takes around 20 minutes to complete.

	>[AZURE.NOTE] Currently, going from any positive value of streaming units back to none, can disable streaming for up to an hour.
	>
	> The highest number of units specified for the 24-hour period is used in calculating the cost. For information about pricing details, see [Media Services Pricing Details](/home/features/media-services/#price).

## Upload content


1. In the [Azure Management Portal](http://manage.windowsazure.cn), click **Media Services** and then click the Media Services account name.
2. Select the CONTENT page.
3. Click the **Upload** button on the page or at the bottom of the portal.
4. In the **Upload content** dialog box, browse to the desired asset file. Click the file and then click **Open** or press Enter.

	![UploadContentDialog][uploadcontent]

5. In the **Upload content** dialog box, click the check button to accept the **File** and **Content Name**.
6. The upload will start and you can track progress from the bottom of the portal.  

	![JobStatus][status]

Once the upload has completed, you will see the new asset listed in the Content list. By convention the name will have "**-Source**" appended at the end to help track new content as source content for encoding tasks.

![ContentPage][contentpage]

If the file size value does not get updated after the uploading process stops, select the **Sync Metadata** button. This synchronizes the asset file size with the actual file size in storage and refreshes the value on the Content page.


## Encode content

### Overview
In order to deliver digital video over the Internet you must compress the media. Media Services provides a media encoder that allows you to specify how you want your content to be encoded (for example, the codecs to use, file format, resolution, and bitrate.)

When working with Azure Media Services one of the most common scenarios is delivering adaptive bitrate streaming to your clients. With adaptive bitrate streaming, the client can switch to a higher or lower bitrate stream as the video is displayed based on the current network bandwidth, CPU utilization, and other factors. Media Services supports the following adaptive bitrate streaming technologies: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH, and HDS (for Adobe PrimeTime/Access licensees only).

Media Services provides dynamic packaging which allows you to deliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming, or HDS) without you having to re-package into these streaming formats.

To take advantage of dynamic packaging, you need to do the following:

- Encode your mezzanine (source) file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (the encoding steps are demonstrated later in this tutorial).
- Get at least one On-Demand streaming unit for the streaming endpoint from which you plan to delivery your content. For more information, see [How to scale On-Demand Streaming reserved units](/documentation/articles/media-services-manage-origins#scale_streaming_endpoints).

With dynamic packaging you only need to store and pay for the files in single storage format and Media Services will build and serve the appropriate response based on requests from a client.

Note that in addition to being able to use the dynamic packaging capabilities, On-Demand Streaming reserved units provide you with dedicated egress capacity that can be purchased in increments of 200 Mbps. By default, on-demand streaming is configured in a shared-instance model for which server resources (for example, compute, or egress capacity) are shared with all other users. To improve an on-demand streaming throughput, it is recommended that you purchase On-Demand Streaming reserved units.

### Encode

This section describes the steps you can take to encode your content with Azure Media Encoder using the Azure Management Portal.

1.  Select the file that you would like to encode.
	If encoding is supported for this file type, the **PROCESS** button will be enabled on the bottom of the CONTENT page.
4. In the **Process** dialog box, select the** Azure Media Encoder **processor.
5. Choose one of the **encoding configurations**.

	![Process2][process2]

	The [Task Preset Strings for Azure Media Encoder](https://msdn.microsoft.com/zh-cn/library/azure/dn619392.aspx) topic explains what each preset in **Presets for Adaptive Streaming (dynamic packaging)**, **Presets for Progressive Download**, **Legacy Prsests for Adaptive Streaming**  categories means.  

	The **Other** configurations are described next.

	+ **Encode with PlayReady content protection**. This preset produces an asset encoded with PlayReady content protection.  

		By default the Media Services PlayReady license service is used. To specify some other service from which clients can obtain a license to play the PlayReady encrypted content, use REST or Media Services .NET SDK APIs. For more information, see [Using Static Encryption to Protect your Content]() and set the **licenseAcquisitionUrl** property in the Media Encryptor preset. Alternatively, you can use dynamic encryption and set the **PlayReadyLicenseAcquisitionUrl** property as described in [Using PlayReady Dynamic Encryption and License Delivery Service](https://msdn.microsoft.com/zh-cn/library/azure/dn783467.aspx ).
	+ **Playback on PC/Mac (via Flash/Silverlight)**. This preset produces a Smooth Streaming asset with the following characteristics: 44.1 kHz 16 bits/sample stereo audio CBR encoded at 96 kbps using AAC, and 720p video CBR encoded at 6 bitrates ranging from 3400 kbps to 400 kbps using H.264 Main Profile, and two second GOPs.
	+ **Playback via HTML5 (IE/Chrome/Safari)**. This preset produces a single MP4 file with the following characteristics: 44.1 kHz 16 bits/sample stereo audio CBR encoded at 128 kbps using AAC, and 720p video CBR encoded at 4500 kbps using H.264 Main Profile.
	+ **Playback on iOS devices and PC/Mac**. This preset produces an asset with the same characteristics as the Smooth Streaming asset (described earlier), but in a format that can be used to deliver Apple HLS streams to iOS devices.

5. Then, enter the desired friendly output content name or accept the default. Then click the check button to start the encoding operation and you can track progress from the bottom of the portal.
6. Select **OK**.

	After the encoding is done, the CONTENT page will contain the encoded file.

	To view the progress of the encoding job, switch to the **JOBS** page.  

	If the file size value does not get updated after the encoding is done, select the **Sync Metadata** button. This synchronizes the output asset file size with the actual file size in storage and refreshes the value on the Content page.


## Publish content

### Overview

To provide your user with a  URL that can be used to stream or download your content, you first need to "publish" your asset by creating a locator. Locators provide access to files contained in the asset. Media Services supports two types of locators: OnDemandOrigin locators, used to stream media (for example, MPEG DASH, HLS, or Smooth Streaming) and Access Signature (SAS) locators, used to download media files.

When you use the Azure Management Portal to publish your assets, the locators are created for you and you are provided with an OnDemand-based URL (if your asset contains an .ism file) or a SAS URL.

A SAS URL has the following format.

	{blob container name}/{asset name}/{file name}/{SAS signature}

A streaming URL has the following format and you can use it to play Smooth Streaming assets.

	{streaming endpoint name-media services account name}.streaming.mediaservices.chinacloudapi.cn/{locator ID}/{filename}.ism/Manifest

To build an HLS streaming URL, append (format=m3u8-aapl) to the URL.

	{streaming endpoint name-media services account name}.streaming.mediaservices.chinacloudapi.cn/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

To build an  MPEG DASH streaming URL, append (format=mpd-time-csf) to the URL.

	{streaming endpoint name-media services account name}.streaming.mediaservices.chinacloudapi.cn/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Locators have an expiration date. When using the portal to publish your assets, locators with a 100 year expiration date are created.

>[AZURE.NOTE] If you used the portal to create locators before March 2015, locators with a two year expiration date were created.  

To update an expiration date on a locator, use [REST](http://msdn.microsoft.com/zh-cn/library/azure/hh974308.aspx#update_a_locator ) or [.NET](https://msdn.microsoft.com/zh-cn/library/azure/microsoft.windowsazure.mediaservices.client.ilocator.update(v=azure.10).aspx) APIs. Note that when you update the expiration date of a SAS locator, the URL changes.

### Publish

To use the portal to publish an asset, do the following:

1. Select the asset.
2. Then, click the publish button.

 ![PublishedContent][publishedcontent]


## Play content from the portal

The Azure Management Portal provides a content player that you can use to test your video.

Click the desired video and then click the **Play** button at the bottom of the portal.

Some considerations apply:

- Make sure the video has been published.
- The **MEDIA SERVICES CONTENT PLAYER** plays from the default streaming endpoint. If you want to play from a non-default streaming endpoint, use another player. For example, [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]




<!-- Anchors. -->


<!-- URLs. -->
[Azure Management Portal]: http://manage.windowsazure.cn/


<!-- Images -->
[portaloverview]: ./media/media-services-portal-get-started/media-services-content-page.png
[publishedcontent]: ./media/media-services-portal-get-started/media-services-upload-content-published.png
[uploadcontent]: ./media/media-services-portal-get-started/UploadContent.png
[status]: ./media/media-services-portal-get-started/Status.png
[encoder]: ./media/media-services-manage-content/EncoderDialog2.png
[branding]: ./media/branding-reporting.png
[contentpage]: ./media/media-services-portal-get-started/media-services-content-page.png
[process]: ./media/media-services-manage-content/media-services-process-video.png
[process2]: ./media/media-services-portal-get-started/media-services-process-video2.png
[encrypt]: ./media/media-services-manage-content/media-services-encrypt-content.png
[AMSPlayer]: ./media/media-services-portal-get-started/media-services-portal-player.png
