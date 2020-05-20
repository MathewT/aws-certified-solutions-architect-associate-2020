# S3

:sparkles: [S3 F.A.Q. Read this a lot!](https://aws.amazon.com/s3/faqs/) :sparkles:

:sparkles: [S3 Documentation](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) :sparkles:

:heart:  :dog: :heart

1. Files are between 0 byte and 5 terabytes.
1. Buckets names must be unique globally, across regions and availability zones.
1. **Read after Write consistency** for PUTS of *new* objects.  New objects are
        immediately available for read after first PUT.  
1. **Eventual consistency** for overwrite PUTS and DELETEs (Updates to objects via PUT
or DELETE can take some time to propagate.)
1. Bucket URL format: https://[s3-region].amazonaws.com/bucket-name
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
 + make sure object names are not all beginning with same letters, may want to
 **salt the beginning of object names to prevent hot spots**.
 + Use sha256 characters as a good object name salt
 + Or use some random salt
1. Torrent.  S3 supports Bit Torrent protocol
1. By default, you're allowed 100 buckets.   Additional buckets require a request to AWS Support.

