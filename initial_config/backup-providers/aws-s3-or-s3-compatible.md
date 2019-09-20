# AWS S3 or S3-compatible

vProtect can store backups in AWS S3 or S3-compatible backup provider. In most cases you just need to prepare a bucket \(with versioning enabled if possible\), and generate access/secret key for vProtect. vProtect can be installed in AWS \(if EC2 backup is used\), but in most cases S3 is going to be used just as a cloud backup provider for on-prem environments.

Typical use cases are:

* when AWS is used - choose a single bucket with **versioning enabled** - all backup objects will have names in `/container_name/path/to/backup` format, where `container_name` typically is VM name with identifier 
* when 3-rd party is used - you need to verify:
  * which strategy is supported by the vendor - i.e. Scality requires single bucket without versioning 
  * when recording timestamp of the object should occur - i.e. Scality does it after data is stored \(unlike AWS\)

vProtect also is able to **encrypt** backups before sending backups \(client-side encryption: SSE-C\). Once enabled, new data being stored is going to be encrypted with keys generated and kept by vProtect. For performance improvements we also recommend to use AWS Direct Connect to access S3. Otherwise backups would be send over Internet which could result in poor performance.

**Notice:** S3 has a **limit of 5TB** per object. This means that depending on the virtualization platform and backup format used by export/import mode you may have limit of 5TB per VM \(if it is Proxmox VMA or Citrix XVA image-based backup\) or per VM disk \(in most cases\). Bigger files are currently not supported.

## Permissions

Depending on the mode selected you may different set of permissions. For single bucket you need to use access keys of a user that has ability to control objects within the bucket over the specific bucket - here is example IAM policy:

```text
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1568968204280",
      "Action": [
        "s3:DeleteObject",
        "s3:DeleteObjectTagging",
        "s3:DeleteObjectVersion",
        "s3:DeleteObjectVersionTagging",
        "s3:GetBucketTagging",
        "s3:GetBucketVersioning",
        "s3:GetObject",
        "s3:GetObjectRetention",
        "s3:GetObjectTagging",
        "s3:GetObjectVersion",
        "s3:GetObjectVersionTagging",
        "s3:ListBucket",
        "s3:ListBucketVersions",
        "s3:PutObject",
        "s3:PutObjectTagging",
        "s3:PutObjectVersionTagging",
        "s3:RestoreObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::BACKUP_DESTINATION_BUCKET/*"
    }
  ]
}
```

**Notice:** It recommended to periodically rotate your access/secret keys. More information can be found here: [https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/](https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/). After changing key in AWS, remember to update it in vProtect as well.

## Parameters

* `API URL (optional)` - optional if Amazon S3 is used, otherwise provide API URL for your S3-compatible backup provider
* `Access key` - your S3 access key
* `Secret key` - your S3 secret key
* `Backup mode` - mode that specifies if backups should be stored in one bucket or in separate buckets for each VM

  * Single bucket for all VMs - stores all backups in a single bucket \(default for Amazon S3\)
  * Single bucket \(without versioning support\) for all VMs - same as above, but 3rd-party solutions may not support object versioning - choose that to store object versions as separate objects
  * One bucket per VM \(**deprecated**\) - stores all backups in separate buckets \(note Amazon S3 may have limits on the number of buckets\)

* `Record backup time after store` - some 3rd party solutions record object version after data transfer, enable this options only if you experience `Backup not found` issues.
* `Bucket name (for SINGLE_BUCKET only)` - single bucket name to be used for backups
* `Bucket name prefix (for ONE_BUCKET_PER_VM only)` - prefix for bucket names used in amazon \(must be globally unique\), buckets will have names starting with this prefix - one for each VM 
* `Encryption` - enables build-in encryption of the data at rest. Once enabled, new data being stored is going to be encrypted. 

## Bucket replication

Even though S3 is a highly available service, you may want to be prepared in case of a region failure. We recommend to follow this guide[ https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html](%20https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html) to setup bucket replication, so that your data is replicated to the other region in the worst case. Remember to point vProtect to the replicated bucket in case of a disaster.

## Costs

When storing backups in S3 additional charges will occur for stored backups. Retention setting in vProtect can limit the storage costs of stored backups. Currently vProtect doesn't support Glacier \(but will appear in future releases\).

Please visit [https://aws.amazon.com/s3/pricing/](https://aws.amazon.com/s3/pricing/) to check current AWS S3 pricing.

