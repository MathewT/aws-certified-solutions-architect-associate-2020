# S3 Exam Notes (Just the important stuff.)

[S3 F.A.Q. Read this a lot!](https://aws.amazon.com/s3/faqs/)

[S3 Documentation](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html)


1. Files are between 0 byte and 5 terabytes.
1. Buckets names must be unique globally, across regions and availability zones.
1. **Read after Write consistency** for PUTS of *new* objects.  New objects are
immediately available for read after first PUT.  
1. **Eventual consistency** for overwrite PUTS and DELETEs (Updates to objects via PUT
or DELETE can take some time to propagate.)
1. Bucket URL format: https://s3-region.amazonaws.com/bucket-name
1. Upon successful file upload to a bucket, AWS returns HTTP 200 return code
1. S3 is a simple key/value store.  
    * The *key* is the path/name of the object.
    * The *value* is the actual bytes of the data/file being stored.
    * Version ID is important for tracking value/data version.  
    * Metadata
    * Access Control Lists
1. Securing Data with ACLs and Bucket policies
1. Largest size file you can transfer to S3 in a single PUT operation is 5GB
1. S3 stores data in alphabetical order (lexigraphical order).
    + Make sure object names are not all beginning with same letters, may want to
 **salt the beginning of object names to prevent hot spots**.
    + Use sha256 characters as a good object name salt
    + Or use some random salt
1. Torrent.  S3 supports Bit Torrent protocol
1. By default, you're allowed 100 buckets.   Additional buckets require a request to AWS Support.


## Access Control Lists (ACLs)

1. Apply ACL to individual objects or to entire bucket

see Security and Encryption

## Bucket Policies

see Security and Encryption

## Versioning

1. Enabled at the bucket level.
2. Once enabled, it *cannot be turned off or disabled, only suspended*
3. Versioning allows for MFA delete capability for additional security
4. Review S3 versioning....

## Cross Region Replication

1. Auto replicate to a bucket in a different AWS region
1. Must replicate to a different region, not the same region as the source bucket
1. Cross region replication, **requires versioning on both buckets**
1. Source for replication can be a sub-folder within a the source bucket
1. Specify the destination storage class
1. Create/IAM Role... 
1. **Note:** Objects already existing in the source bucket prior to enabling replication are **not** 
automatically replicated to the target bucket.  Replication only applies to new objects created 
in the source bucket **after** replication is enabled.
1. **Note:** All versions of the source file are replicated to the destination bucket.
1. **Note:** Object permissions are replicated from the source bucket to the destination bucket.
1. **Note:** Removing a delete marker to restore in the source bucket **does not** automatically 
replicate to the destination bucket.  The object is not automatically un-deleted in the destination 
bucket.
1. **Note:** Removing individual object *versions* in the source bucket **does not** automatically 
replicate to the destination bucket.  Still charged for those object versions in the destination bucket.
1. Deleting the object itself **does** replicate to the target bucket.
1. Buckets can only be replicated to one other cross-region bucket.
1. You cannot **daisy chain** replication buckets at this time.

## S3 Storage Tiers or Classes (Tiered Storage)
  ### S3 Standard
  * Fast retrieval
  * Designed for 99.99% availability
  * Amazon Guarantee is 99.9% availability
  * 11 9s durability
  * Most expensive
  * No minimum object size
  * Minimum storage duration of **30 days before Lifecycle transfer to IA**
  * Minimum storage duration of **1 day before Lifecycle transfer to Glacier or Delete**
  * No special retrieval fee
  * Objects are stored in multiple devices within a facility and across mulitple facilities
  * Designed to sustain the loss of 2 facilities concurrently
  ### S3 IA (Infrequently Accessed)
  * Fast retrieval
  * **Minimum object size is 128KB**
  * **Miniumum storage duration is 30 days**
  * For objects that are accessed less frequently but requires rapid retrieval
  * 11 9s durability
  * Example:  Payroll data which is not accessed frequently, accessed at end of year, 
    needed quickly when requested access when needed.
  * Lower fee than S3 **but you are charged a retrieval fee per GB**
  ### Reduced Redundancy Storage (RRS)
  * Fast retrieval
  * 99.99% durability
  * 99.99% availability over a given year
  * Used to store data that can be lost or easily regenerated (e.g. image
thumbnails)
  ### Glacier
  * Separate and independent from S3 but integrates tightly into S3
  * 11 9s durability
  * Very slow retrieval
  * Very cheap, $0.01 per GB per month
  * Archival only
  * **3-5 hours to retrieve data (definitely on the exam**
  * **Minimum storage duration is 90 days**

![Storage Tiers](https://github.com/MathewT/aws-certified-solutions-architect-associate-2020/blob/master/S3/s3-tiers.JPG)

## S3 Charges
1. Storage
1. Requests for objects
1. Transfer
  + Data transferred into S3 is free
  + Data moved around (e.g. cross region replication) can be charged
1. Storage Management Pricing
  + Data management tags for objects, not free, charge per tag basis
1. Transfer Acceleration
  + Enable faster transfers of files from end users to S3
  + Uses CloudFront edge locations
  + Data arrives at an Edge Location and is transferred (uploaded) to S3 via optimized path
  + Edge Locations are much more closer to end users than a geo-located bucket :sparkles:
  

## Lifecyle Management Rules (Automatically move objects to lower cost storage tiers)

1. S3 Standard to IA Transition:
  * 128Kb minimum size
  * 30 days after object creation
1. Transition to Glacier:
  * From S3: 1 day after object creation
  * From IA: 30 days after transition to IA (61 days after object creation and transition to IA)
1. Transition to Delete:
  * 1 day after object creation (S3 Standard) OR 1 day after transition to Glacier (61 days after
    object creation)
1. Can be used in conjunction with versioning.
1. Versioning **not** required for Lifecycle management but is required for replication
1. Lifecycle Management can be applied to current versions and previous versions of objects
1. Lifecycle Management can be applied to the entire bucket or a sub-folder in the bucket
1. Reduced Redundancy Storage is **not** a Lifecycle Management option
