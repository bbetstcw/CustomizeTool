## Create a backup vault for a VM

A backup vault is an entity that stores all the backups and recovery points that have been created over time. The backup vault also contains the backup policies that will be applied to the virtual machines being backed up.

This image shows the relationships between the various Azure Backup entities:
![Azure Backup entities and relationships](./media/backup-create-vault-for-vms/vault-policy-vm.png)

To create a backup vault:

1. Sign in to the [Azure Classic Management Portal](http://manage.windowsazure.cn/).

2. to create a backup vault, click **New** > **Data Services** > **Recovery Services** > **Backup Vault** > **Quick Create** (see the image below).

    ![Create backup vault](./media/backup-create-vault-for-vms/backup_vaultcreate.png)

3. For **Name**, enter a friendly name to identify the vault. The name needs to be unique for the Azure subscription. Type a name that contains between 2 and 50 characters. It must start with a letter, and can contain only letters, numbers, and hyphens.

4. In **Region**, select the geographic region for the vault. The vault must be in the same region as the virtual machines that you want to protect. If you have virtual machines in multiple regions, you must create a backup vault in each region. There is no need to specify storage accounts to store the backup data--the backup vault and the Azure Backup service handle this automatically.

5. In **Subscription** select the subscription you want to associate with the backup vault. There will be multiple choices only if your organizational account is associated with multiple Azure subscriptions.

5. Click **Create Vault**. It can take a while for the backup vault to be created. Monitor the status notifications at the bottom of the portal.

    ![Create vault toast notification](./media/backup-create-vault-for-vms/creating-vault.png)

6. A message will confirm that the vault has been successfully created. It will be listed on the **recovery services** page as **Active**. Make sure to choose the appropriate storage redundancy option right after the vault has been created. Read more about [setting the storage redundancy option in the backup vault](/documentation/articles/backup-configure-vault/#azure-backup---storage-redundancy-options).

    ![List of backup vaults](./media/backup-create-vault-for-vms/backup_vaultslist.png)

7. Click the backup vault to go to the **Quick Start** page, where the instructions for backing up Azure virtual machines are shown.

    ![Virtual machine backup instructions on the Dashboard page](./media/backup-create-vault-for-vms/vmbackup-instructions.png)
