# Ceph Rados Gateway

## Overview

Ceph Object Gateway supports a RESTful API that is compatible with the basic data access model of the Amazon S3 API. Ceph Object Gateway is an object storage interface built on top of librados to provide applications with a RESTful gateway to Ceph Storage Clusters. Ceph Object Storage supports two interfaces:

* **S3-compatible**: Provides object storage functionality with an interface that is compatible with a large subset of the Amazon S3 RESTful API.
* **Swift-compatible**: Provides object storage functionality with an interface that is compatible with a large subset of the OpenStack Swift API.

### Example

Log in to ceph dashboard. Open Object gateway and then go to "Buckets"

![](../../../.gitbook/assets/object-storage-ceph-s3-bucket.jpg)

Then click on "Create" button

![](../../../.gitbook/assets/object-storage-ceph-s3-bucket2.jpg)

Fill requirement fields

![](../../../.gitbook/assets/object-storage-ceph-s3-bucket3.jpg)

Now create a dedicated access account for the backup destination. Open Users tab under object gateway menu

![](../../../.gitbook/assets/object-storage-ceph-s3-user.jpg)

Fill out the username field, you can leave the other settings as default

![](../../../.gitbook/assets/object-storage-ceph-s3-user4.jpg)

To see account key and secret key expand user details and open keys tab, click on the key and then on the show button

![](../../../.gitbook/assets/object-storage-ceph-s3-user2.jpg)

The access key and secret key will be needed to create a backup destination on vProtect

![](../../../.gitbook/assets/object-storage-ceph-s3-user3.jpg)

Now we can go to the vProtect Dashboard. Open "Backup Destination" tab from the left side menu and choose "Amazon S3 / S3-compatible" as a new type of backup destination

![](../../../.gitbook/assets/object-storage-oracle-cloud-backup-destination%20%281%29.jpg)

By default, Ceph provides S3 under port 8000. Also remember to enable the "record backup time after store" option

![](../../../.gitbook/assets/object-storage-ceph-s3-backup-destination2.jpg)

![](../../../.gitbook/assets/object-storage-ceph-s3-backup-destination3.jpg)

