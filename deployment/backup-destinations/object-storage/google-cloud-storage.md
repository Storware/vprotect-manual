# Google Cloud Storage

**Google Cloud Storage** allows data to be stored and accessed on Google Cloud Platform infrastructure. It combines the performance and scalability of Google's cloud with advanced security and sharing capabilities.

## How to use **GCS** as a backup destination for **vProtect**:

1. Create a project: Click [here](https://cloud.google.com/resource-manager/docs/creating-managing-projects) for more info about **Creating and Managing Projects**.
2. Create a bucket: Click [here](https://cloud.google.com/storage/docs/creating-buckets) for more info about **Creating Storage Buckets**.

![](../../../.gitbook/assets/object-storage-google-bucket.jpg)

1. Enable versioning in your bucket: Click [here](https://cloud.google.com/storage/docs/using-object-versioning#gsutil) for more info about **Enabling Object Versioning**. `gsutil versioning set on gs://[BUCKET_NAME]`
2. Generate a service account key: Click [here](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) for more info about **Creating service account keys**. The service account key should have the **Role** set to **Storage Admin and Service Usage Consumer**.

![](../../../.gitbook/assets/object-storage-google-service-account.jpg)

![](../../../.gitbook/assets/object-storage-google-service-account-2.jpg)

![](../../../.gitbook/assets/object-storage-google-service-account-3.jpg)

You can leave the third tab - Grant users access to this service account \(optional\).  
To generate an account key, click on the "three-dot" button next to your service account and then click on "create key".  
You should then see the window below - click on create to download the JSON file. You'll need its content in the last step.

![](../../../.gitbook/assets/object-storage-google-service-account-4.jpg)

1. After the key is created, open your vProtect server page \(you can also use **CLI**\), click on **BACKUP DESTINATIONS**, then on the **Create Backup Destination** button, and then select **Google Cloud Storage** from the drop-down list. In addition to the [standard properties](../), you need to specify:
2. The **Bucket name** specified during bucket creation.  
3. The **Service account key** - paste the content of the service account key .json file created before.

![](../../../.gitbook/assets/object-storage-google-bucket-browse.jpg)

![](../../../.gitbook/assets/object-storage-google-create-backup-destination.jpg)

Now you can store vProtect backups on Google Cloud Storage.

