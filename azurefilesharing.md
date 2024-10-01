Create a storage account on azure >> resource group create new >> give storage name
1. Create a Storage Account
>>Sign in to the Azure portal.
>>In the left-hand menu, select Storage accounts.
>>Click + Create to create a new storage account.
>>Fill in the required details such as the subscription, resource group, and storage account name.
>>Under Performance, select either Standard or Premium based on your needs.
>>Under Replication, choose the desired redundancy option.
>>Click Review + create and then Create.
2. Create a File Share
>>Navigate to your newly created storage account.
>>In the storage account menu, select File shares under Data storage.
>>Click + File share.
>>Enter a name for your file share and set the quota (size limit).
>>Click Create
3.In the Azure portal, go to your file share and click Connect.
>>connect to a virtual machine first 
>>Select the Linux tab and follow the instructions to mount the file share using the provided commands.

systemctl daemon reload 
ls 
df -h 
