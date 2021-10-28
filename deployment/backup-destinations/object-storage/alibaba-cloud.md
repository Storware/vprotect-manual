# Alibaba Cloud OSS

## Overview

Albaba Cloud is a S3-compatible backup provider. Configuration as backup destination is similiar to AWS S3.

## Example

After logging in, go to the Object Storage Service and create new bucket.

![](../../../.gitbook/assets/alibaba-overview.png)

Provide necessary details for your bucket and enable versioning.

![](../../../.gitbook/assets/allibaba-new-bucket.png)

Next, go to Manage AccessKey Management and create new AccessKey

![](../../.gitbook/assets/../../../.gitbook/assets/alibaba-key.png)

Now go to the Backup destination tab on the vProtect dashboard and change the sub-tab to object storage. Provide the bucket name and key credentials, and then configure the remaining options according to your requirements:

![](../../.gitbook/assets/../../../.gitbook/assets/alibaba-example.png)