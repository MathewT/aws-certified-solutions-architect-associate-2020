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

[AWS S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)

  ### S3 Standard
  * Fast retrieval
  * Designed for 99.99% availability
  * Backed with the Amazon S3 Service Level Agreement for availability
  * Amazon Guarantee is 99.9% availability across multiple AZs
  * 11 9s durability
  * Most expensive
  * No minimum object size
  * S3 Lifecycle management for automatic migration of objects to other S3 Storage Classes  
  * Minimum storage duration of **30 days before Lifecycle transfer to IA**
  * Minimum storage duration of **1 day before Lifecycle transfer to Glacier or Delete**
  * No special retrieval fee
  * Objects are stored in multiple devices within a facility and across mulitple facilities
  * Designed to sustain the loss of 2 facilities concurrently
  * Low latency and high throughput performance
  * Resilient against events that impact an entire Availability Zone
  * Supports SSL for data in transit and encryption of data at rest
  
### Amazon S3 Intelligent-Tiering (S3 Intelligent-Tiering)
> Amazon S3 Intelligent-Tiering (S3 Intelligent-Tiering) is the only cloud storage class that delivers automatic cost savings by moving objects between four access tiers when access patterns change. The S3 Intelligent-Tiering storage class is designed to optimize costs by automatically moving data to the most cost-effective access tier, without operational overhead. It works by storing objects in four access tiers: two low latency access tiers optimized for frequent and infrequent access, and two optional archive access tiers designed for asynchronous access that are optimized for rare access.

> S3 Intelligent-Tiering works by storing objects in four access tiers: two low latency access tiers optimized for frequent and infrequent access, and two opt-in archive access tiers designed for asynchronous access that are optimized for rare access. Objects uploaded or transitioned to S3 Intelligent-Tiering are automatically stored in the Frequent Access tier. S3 Intelligent-Tiering works by monitoring access patterns and then moving the objects that have not been accessed in 30 consecutive days to the Infrequent Access tier. Once you have activated one or both of the archive access tiers, S3 Intelligent-Tiering will automatically move objects that haven’t been accessed for 90 consecutive days to the Archive Access tier and then after 180 consecutive days of no access to the Deep Archive Access tier. If the objects are accessed later, S3 Intelligent-Tiering moves the objects back to the Frequent Access tier. 

> There are no retrieval fees when using the S3 Intelligent-Tiering storage class, and no additional tiering fees when objects are moved between access tiers within S3 Intelligent-Tiering. It is the ideal storage class for data sets with unknown storage access patterns, like new applications, or unpredictable access patterns, like data lakes. 

**Key Features:**
* Automatically optimizes storage costs for data with changing access patterns
* Stores objects in four access tiers, optimized for frequent, infrequent, archive, and deep archive access
* Frequent and Infrequent Access tiers have same low latency and high throughput performance of S3 Standard
* Activate optional automatic archive capabilities for objects that become rarely accessed
* Archive access and deep Archive access tiers have same performance as Glacier and Glacier Deep Archive
* Designed for durability of 99.999999999% of objects across multiple Availability Zones
* Designed for 99.9% availability over a given year
* Backed with the Amazon S3 Service Level Agreement for availability
* Small monthly monitoring and auto-tiering fee
* No operational overhead, no retrieval fees, no additional tiering fees apply when objects are moved between access tiers within the S3 Intelligent-Tiering storage class


### S3 IA (Infrequently Accessed)
> S3 Standard-IA is for data that is accessed less frequently, but requires rapid access when needed. S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval fee. This combination of low cost and high performance make S3 Standard-IA ideal for long-term storage, backups, and as a data store for disaster recovery files. S3 Storage Classes can be configured at the object level and a single bucket can contain objects stored across S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA, and S3 One Zone-IA. You can also use S3 Lifecycle policies to automatically transition objects between storage classes without any application changes.

**Key Features:**
* Fast retrieval
* **Minimum object size is 128KB**
* **Miniumum storage duration is 30 days**
* For objects that are accessed less frequently but requires rapid retrieval
* 11 9s durability
* Example:  Payroll data which is not accessed frequently, accessed at end of year, 
needed quickly when requested access when needed.
* Lower fee than S3 **but you are charged a retrieval fee per GB**

### Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA) (Formerly Reduced Redundancy)

> S3 One Zone-IA is for data that is accessed less frequently, but requires rapid access when needed. Unlike other S3 Storage Classes which store data in a minimum of three Availability Zones (AZs), S3 One Zone-IA stores data in a single AZ and costs 20% less than S3 Standard-IA. S3 One Zone-IA is ideal for customers who want a lower-cost option for infrequently accessed data but do not require the availability and resilience of S3 Standard or S3 Standard-IA. It’s a good choice for storing secondary backup copies of on-premises data or easily re-creatable data. You can also use it as cost-effective storage for data that is replicated from another AWS Region using S3 Cross-Region Replication.

> S3 One Zone-IA offers the same high durability†, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval fee. **S3 Storage Classes can be configured at the object level, and a single bucket can contain objects stored across S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA, and S3 One Zone-IA.** You can also use S3 Lifecycle policies to automatically transition objects between storage classes without any application changes.

**Key Features:**
* Same low latency and high throughput performance of S3 Standard
* Designed for durability of 99.999999999% of objects in a single Availability Zone†
* Designed for 99.5% availability over a given year
* Backed with the Amazon S3 Service Level Agreement for availability
* Supports SSL for data in transit and encryption of data at rest
* S3 Lifecycle management for automatic migration of objects to other S3 Storage Classes
* Used to store data that can be lost or easily regenerated (e.g. image
thumbnails)

† Because S3 One Zone-IA stores data in a single AWS Availability Zone, data stored in this storage class will be lost in the event of Availability Zone destruction.


### Amazon S3 Glacier (S3 Glacier)
> S3 Glacier is a secure, durable, and low-cost storage class for data archiving. You can reliably store any amount of data at costs that are competitive with or cheaper than on-premises solutions. To keep costs low yet suitable for varying needs, S3 Glacier provides three retrieval options that range from a few minutes to hours. You can upload objects directly to S3 Glacier, or use S3 Lifecycle policies to transfer data between any of the S3 Storage Classes for active data (S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA, and S3 One Zone-IA) and S3 Glacier. For more information, visit the Amazon S3 Glacier page »

**Key Features:** 
* Designed for durability of 99.999999999% of objects across multiple Availability Zones
* Data is resilient in the event of one entire Availability Zone destruction
* Supports SSL for data in transit and encryption of data at rest
* Low-cost design is ideal for long-term archive
* Very cheap, $0.01 per GB per month
* Configurable retrieval times, from minutes to hours
* Separate and independent from S3 but integrates tightly into S3
* S3 PUT API for direct uploads to S3 Glacier, and S3 Lifecycle management for automatic migration of objects
* **Minimum storage duration is 90 days**
* **3-5 hours to retrieve data (definitely on the exam)**


### Amazon S3 Glacier Deep Archive (S3 Glacier Deep Archive)
> S3 Glacier Deep Archive is Amazon S3’s lowest-cost storage class and supports long-term retention and digital preservation for data that may be accessed once or twice in a year. It is designed for customers — particularly those in highly-regulated industries, such as the Financial Services, Healthcare, and Public Sectors — that retain data sets for 7-10 years or longer to meet regulatory compliance requirements. S3 Glacier Deep Archive can also be used for backup and disaster recovery use cases, and is a cost-effective and easy-to-manage alternative to magnetic tape systems, whether they are on-premises libraries or off-premises services. S3 Glacier Deep Archive complements Amazon S3 Glacier, which is ideal for archives where data is regularly retrieved and some of the data may be needed in minutes. All objects stored in S3 Glacier Deep Archive are replicated and stored across at least three geographically-dispersed Availability Zones, protected by 99.999999999% of durability, and can be restored within 12 hours.

**Key Features:**
* Designed for durability of 99.999999999% of objects across multiple Availability Zones
* Lowest cost storage class designed for long-term retention of data that will be retained for 7-10 years
* Ideal alternative to magnetic tape libraries
* Retrieval time within 12 hours
* S3 PUT API for direct uploads to S3 Glacier Deep Archive, and S3 Lifecycle management for automatic migration of objects

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
