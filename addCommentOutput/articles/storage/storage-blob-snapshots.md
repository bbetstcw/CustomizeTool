<properties
	pageTitle="Create a snapshot of a blob | Windows Azure"
	description="A guide for creating snapshots of Azure storage blobs"
	services="storage"
	documentationCenter=""
	authors="tamram"
	manager="adinah"
	editor=""/>

<tags
	ms.service="storage"
	ms.date="09/01/2015"
	wacn.date=""/>

# Create a blob snapshot

## Overview

A snapshot is a read-only version of a blob that's taken at a point in time. Snapshots are useful for backing up blobs. <!-- deleted by customization After --><!-- keep by customization: begin --> Once <!-- keep by customization: end --> a snapshot has been created, you can read, copy, or delete it, but you cannot modify it.

A snapshot of a blob has the same name as the base blob from which the snapshot is taken, with a **DateTime** value appended to indicate the time at which the snapshot was taken. For example, if a page blob URI is `http://storagesample.core.blob.chinacloudapi.cn/mydrives/myvhd`, the snapshot URI will be similar to <!-- deleted by customization `http://storagesample.core.blob.chinacloudapi.cn/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z` --><!-- keep by customization: begin --> `http://storagesample.blob.core.chinacloudapi.cn/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z` <!-- keep by customization: end -->. All of a blob's snapshots share its URI and are distinguished only by the appended **DateTime** value.

A blob <!-- deleted by customization can --><!-- keep by customization: begin --> may <!-- keep by customization: end --> have any number of snapshots. Snapshots persist until they are explicitly deleted. Note that a snapshot cannot outlive its source blob. You can enumerate the snapshots associated with your blob to track your current snapshots.

When you create a snapshot of a blob, the blob's system properties are copied to the snapshot with the same values.

Any leases associated with the base blob do not affect the snapshot. You cannot acquire a lease on a snapshot.

<!-- deleted by customization
## Copy snapshots
-->
<!-- keep by customization: begin -->
## Copying snapshots 
<!-- keep by customization: end -->

Copy operations involving blobs and snapshots follow these rules:

- You can copy a snapshot over its base blob. By promoting a snapshot to the position of the base blob, you can restore an earlier version of a blob. The snapshot remains, but the base blob is overwritten with a writable copy of the snapshot.

- You can copy a snapshot to a destination blob with a different name. The resulting destination blob is a writable blob and not a snapshot.

- When a source blob is copied, any snapshots of the source blob are not copied to the destination. When a destination blob is overwritten with a copy, any snapshots associated with the destination blob at the time it was overwritten remain intact under its name.

- When you create a snapshot of a block blob, the blob's committed block list is also copied to the snapshot. Any uncommitted blocks are not copied.

<!-- deleted by customization
## Specify an access condition

You can specify an access condition so that the snapshot is created only if a condition is met. To specify an access condition, use the **AccessCondition** property. If the specified condition is not met, the snapshot is not created, and the Blob service returns status code HTTPStatusCode.PreconditionFailed.

## Delete snapshots
-->
<!-- keep by customization: begin -->
## Specifying an access condition 

You can specify an access condition so that the snapshot is created only if a condition is met. To specify an access condition, use the AccessCondition property. If the specified condition is not met, the snapshot is not created, and the Blob service returns status code HTTPStatusCode.PreconditionFailed.

## Deleting snapshots 
<!-- keep by customization: end -->

A blob that has snapshots cannot be deleted unless the snapshots are also deleted. You can delete a snapshot individually, or tell the storage service to delete all snapshots when deleting the source blob. If you attempt to delete a blob that still has snapshots, you'll get an error.

## Snapshots with <!-- deleted by customization Azure --> Premium Storage
Using snapshots with Premium Storage follow these rules:

- The number of snapshots per page blob in a <!-- deleted by customization premium storage --><!-- keep by customization: begin --> Premium Storage <!-- keep by customization: end --> account is limited to 100. If that limit is exceeded, the Snapshot Blob operation returns error code 409 (**SnapshotCountExceeded**).

- A snapshot of a page blob in a <!-- deleted by customization premium storage --><!-- keep by customization: begin --> Premium Storage <!-- keep by customization: end --> account <!-- deleted by customization can --><!-- keep by customization: begin --> may <!-- keep by customization: end --> be taken once every ten minutes. If that rate is exceeded, the Snapshot Blob operation returns error code 409 (**SnaphotOperationRateExceeded**).

- Reading a snapshot of a page blob in a <!-- deleted by customization premium storage --><!-- keep by customization: begin --> Premium Storage <!-- keep by customization: end --> account via Get Blob is not supported. Calling Get Blob on a snapshot in a <!-- deleted by customization premium storage --><!-- keep by customization: begin --> Premium Storage <!-- keep by customization: end --> account returns error code 400 (**InvalidOperation**). However, calling Get Blob Properties and Get Blob Metadata against a snapshot is supported.

- To read a snapshot, you can use the Copy Blob operation to copy a snapshot to another page blob in the account. The destination blob for the copy operation must not have any existing snapshots. If the destination blob does have snapshots, then <!-- deleted by customization the --> Copy Blob <!-- deleted by customization operation --> returns error code 409 (**SnapshotsPresent**).

## <!-- deleted by customization Return --><!-- keep by customization: begin --> Returning <!-- keep by customization: end --> the absolute URI to a snapshot

This C# code example creates a new snapshot and writes out the absolute URI for the primary location.

    //Create the blob service client object.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

<!-- deleted by customization
## Understand how snapshots accrue charges

Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges to your account. When designing your application, it is important to be aware how these charges might accrue so that you can minimize unnecessary costs.
-->
<!-- keep by customization: begin -->
## Understanding how snapshots accrue charges

Creating a snapshot, which is a read-only copy of a blob, can result in additional data storage charges to your account. When designing your application, it is important to be aware how these charges may accrue so that you can minimize unnecessary costs.
<!-- keep by customization: end -->

### Important billing considerations

The following list includes key points to consider when creating a snapshot.

- Charges are incurred for unique blocks or pages, whether they are in the blob or in the snapshot. Your account does not incur additional charges for snapshots associated with a blob until you update the blob on which they are based. <!-- deleted by customization After --><!-- keep by customization: begin --> Once <!-- keep by customization: end --> you update the base blob, it diverges from its snapshots, and you will be charged for the unique blocks or pages in each blob or snapshot.

- When you replace a block within a block blob, that block is subsequently charged as a unique block. This is true even if the block has the same block ID and the same data as it has in the snapshot. <!-- deleted by customization After --><!-- keep by customization: begin --> Once <!-- keep by customization: end --> the block is committed again, it diverges from its counterpart in any snapshot, and you will be charged for its data. The same holds true for a page in a page blob that's updated with identical data.

- Replacing a block blob by calling the <!-- deleted by customization **UploadFile** --><!-- keep by customization: begin --> UploadFile <!-- keep by customization: end -->, <!-- deleted by customization **UploadText** --><!-- keep by customization: begin --> UploadText <!-- keep by customization: end -->, <!-- deleted by customization **UploadStream** --><!-- keep by customization: begin --> UploadStream <!-- keep by customization: end -->, or <!-- deleted by customization **UploadByteArray** --><!-- keep by customization: begin --> UploadByteArray <!-- keep by customization: end --> method replaces all of the blocks in that blob. If you have a snapshot associated with that blob, all of the blocks in the base blob and snapshot <!-- keep by customization: begin --> will <!-- keep by customization: end --> now diverge <!-- deleted by customization, --> and you will be charged for all of the blocks in both blobs. This is true even if the data in the base blob and the snapshot remain identical.

- The Azure Blob service does not have a means to determine whether two blocks contain identical data. Each block that is uploaded and committed is treated as unique, even if it has the same data and the same block ID. Because charges accrue for unique blocks, it is important to consider that updating a blob that has a snapshot <!-- deleted by customization results --><!-- keep by customization: begin --> will result <!-- keep by customization: end --> in additional unique blocks and additional charges.

> [AZURE.NOTE] Best practices dictate that you manage snapshots carefully to avoid extra charges. <!-- deleted by customization We recommend --><!-- keep by customization: begin --> It's recommended <!-- keep by customization: end --> that you manage snapshots in the following manner:

> - Delete and re-create snapshots associated with a blob whenever you update the blob, even if you are updating with identical data, unless your application design requires that you maintain snapshots. By deleting and re-creating the blob's snapshots, you can ensure that the blob and snapshots do not diverge.

<!-- deleted by customization
> - If you are maintaining snapshots for a blob, avoid calling **UploadFile**, **UploadText**, **UploadStream**, or **UploadByteArray** to update the blob, as those methods replace all of the blocks in the blob. Instead, update the fewest possible number of blocks by using the **PutBlock** and **PutBlockList** methods.
-->
<!-- keep by customization: begin -->
> - If you are maintaining snapshots for a blob, avoid calling UploadFile, UploadText, UploadStream, or UploadByteArray to update the blob, as those methods replace all of the blocks in the blob. Instead, update the fewest possible number of blocks by using the PutBlock and PutBlockList methods.
<!-- keep by customization: end -->


### Snapshot billing scenarios


The following scenarios demonstrate how charges accrue for a block blob and its snapshots.

In <!-- deleted by customization scenario --><!-- keep by customization: begin --> Scenario <!-- keep by customization: end --> 1, the base blob has not been updated <!-- deleted by customization after --><!-- keep by customization: begin --> since <!-- keep by customization: end --> the snapshot was taken, so charges are incurred only for unique blocks 1, 2, and 3.

<!-- deleted by customization
![Azure Storage resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In scenario 2, the base blob has been updated, but the snapshot has not. Block 3 was updated, and even though it contains the same data and the same ID, it is not the same as block 3 in the snapshot. As a result, the account is charged for four blocks.

![Azure Storage resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In scenario 3, the base blob has been updated, but the snapshot has not. Block 3 was replaced with block 4 in the base blob, but the snapshot still reflects block 3. As a result, the account is charged for four blocks.

![Azure Storage resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

In scenario 4, the base blob has been completely updated and contains none of its original blocks. As a result, the account is charged for all eight unique blocks. This scenario can occur if you are using an update method such as **UploadFile**, **UploadText**, **UploadFromStream**, or **UploadByteArray**, because these methods replace all of the contents of a blob.

![Azure Storage resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

-->
<!-- keep by customization: begin -->
![Azure Storage Resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In Scenario 2, the base blob has been updated, but the snapshot has not. Block 3 was updated, and even though it contains the same data and the same ID, it is not the same as block 3 in the snapshot. As a result, the account is charged for four blocks.

![Azure Storage Resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In Scenario 3, the base blob has been updated, but the snapshot has not. Block 3 was replaced with block 4 in the base blob, but the snapshot still reflects block 3. As a result, the account is charged for four blocks.
 
![Azure Storage Resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

In Scenario 4, the base blob has been completely updated and contains none of its original blocks. As a result, the account is charged for all eight unique blocks. This scenario can occur if you are using an update method such as UploadFile, UploadText, UploadFromStream, or UploadByteArray, because these methods replace all of the contents of a blob.

![Azure Storage Resources](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)
<!-- keep by customization: end -->