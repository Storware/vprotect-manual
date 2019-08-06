# AWS S3 or S3-compatible

vProtect can store backups in AWS S3 or S3-compatible backup provider. In most cases you just need to prepare a bucket \(with versioning enabled if possible\), and generate access/secret key for vProtect. vProtect can be installed in AWS \(if EC2 backup is used\), but in most cases S3 is going to be used just as a cloud backup provider for on-prem environments.

Typical use cases are:

* when AWS is used - choose a single bucket with **versioning enabled** - all backup objects will have names in `/container_name/path/to/backup` format, where `container_name` typically is VM name with identifier 
* when 3-rd party is used - you need to verify:
  * which strategy is supported by the vendor - i.e. Scality requires single bucket without versioning 
  * when recording timestamp of the object should occur - i.e. Scality does it after data is stored \(unlike AWS\)

vProtect also is able to encrypt backups before sending backups \(client-side encryption: SSE-C\). For performance improvements we also recommend to use AWS Direct Connect to access S3. Otherwise backups would be send over Internet which could result in poor performance.

## Permissions

Depending on the mode selected you may different set of permissions. For single bucket you need to use access keys of a user that has full control over the specific bucket - here is example IAM policy:

```text
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1565000046353",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::BACKUP_DESTINATION_BUCKET/*"
    }
  ]
}
```

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

