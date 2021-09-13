# Oracle Cloud Infrastructure Object Storage

## Overview

_The Oracle Cloud Infrastructure Object Storage service is an internet-scale, high-performance storage platform that offers reliable and cost-efficient data durability. The Object Storage service can store an unlimited amount of unstructured data of any content type, including analytic data and rich content._

### Example

Log in to the Oracle cloud dashboard, expand the left side menu and go to the Object Storage tab.

![](../../../.gitbook/assets/object-storage-oracle-cloud-object-storage.jpg)

Now let's create a new bucket.

![](../../../.gitbook/assets/object-storage-oracle-cloud-object-storage-bucket.jpg)

We do not require specific bucket settings for vProtect. The bucket name will be needed when we want to create a backup destination in vProtect.

![](../../../.gitbook/assets/object-storage-oracle-cloud-object-storage-bucket2.jpg)

After creating the bucket, you'll see a list of buckets. Click on the name to view the details of the object. Remember the "namespace", we also need it when creating a backup destination.

![](../../../.gitbook/assets/object-storage-oracle-cloud-object-storage-bucket3.jpg)

Now we need to create a user that we will use to authenticate our backup destination. Please go to the Users tab under the Identity tab in the menu on the left.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user.jpg)

Now create a new user.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user2.jpg)

Fill in the required fields.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user3.jpg)

Then go to the Groups page, which you can also find under the identity tab in the left side menu.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-group.jpg)

Now click on the existing group "Administrators".

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-group2.jpg)

Now click on "Add User to Group" and choose the user you created previously.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-group3.jpg)

Go back to the Users page and go to the details page of our user.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-secrets.jpg)

Scroll down and open the "Customer Secret Keys" tab. Click on "Generate Secret Key".

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-secrets2.jpg)

Enter any name.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-secrets3.jpg)

As you see in the note below, copy and save the secret key because you can only do this now.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-secrets4.jpg)

After generating the secret key, you can view the access key, just move the mouse over it.

![](../../../.gitbook/assets/object-storage-oracle-cloud-user-secrets5.jpg)

Now we can go to the vProtect Dashboard. Open the "Backup Destination" tab from the left side menu, then the sub-tab "Object Storage" and choose "Amazon S3 / S3-compatible" as the new type of backup destination.

![](../../../.gitbook/assets/backup-destinations-object-storage%20%284%29%20%284%29%20%282%29%20%281%29%20%284%29.jpg)

First let's focus on the "S3-Compatible" section.  
To generate an API URL, you will need this site: [https://docs.cloud.oracle.com/en-us/iaas/api/\#/en/s3objectstorage/20160918/](https://docs.cloud.oracle.com/en-us/iaas/api/#/en/s3objectstorage/20160918/)  
As I mentioned earlier, you will need an object storage namespace \(choose the API URL from the list according to your region\).  
Then provide your bucket name and region, and finally switch on "Record time after backup" and "Path style access enabled".  
Configure the rest of the settings as desired.

![](../../../.gitbook/assets/backup-destinations-object-storage-oracle.jpg)

