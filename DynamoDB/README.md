# DynamoDB

## Use Cases

* Hyper scale data
* Hyper ephemeral compute
* OLTP; reading and wrting small bits of data at high speed
* Caching; store the results of complex, frequently accessed data from other databases or operations
* Simple data models that don't require complex querying
* Key-value or wide column data store
* Wide column data store is a distributed hash table in which the value is a B-tree.  B-tree allows the lookup of a particular item or a range of items, like a phone book.  E.g. find Presley, Elvis or find all Smith.  So a wide column database is like a bookshelf of phone books, each for a different city.  The bookshelf is the hash table and the B-tree is the phone book.  

## Data Modeler 

[AWS DynamoDB Data Modeler](https://rh-web-bucket.s3.amazonaws.com/index.html#)

## Read Capacity Units (RCUs)

* One RCU = 1 strongly-consistent read per second or 2 eventually consistent reads per second up to 4KB in size 

## Write Capacity Units (WCUs)
* One WCU = 1 write per second up to 1KB in size

## Pricing Model

* Read and Write throughput are configured separately
* Throughput can be dynamically scaled up and down as needed

## Basics

### Table

A grouping of records that conceptually belong together.

### Item

A single record in a DynamoDB table.

### Attributes

Typed data values that make up a DynamoDB item.

  * Scalars - string, number, binary, boolean, null
  * Complex - lists, maps
  * Sets - compound type that represents multiple unique values
    * string sets, number sets, binary sets

### Primary Key

1. Every item must include a primary key.  
1. Can be single value (partition key) or composite of two values (partition key and sort key).  
1. Simple primary key
    1. Partition key
    1. Fetch single record only
1. Composite key
    1. Partition key and sort key
    1. Query all items matching a partition key and further narrow down by sort key
    1. Great for handling relations between items
1. Each item in a table is uniquely identifiable by its primary key.
1. Primary key selection and design is the most important part of DynamoDB modelling. 
1. Almost all data access is via primary keys.

### Secondary Indexes

1. Used when additional flexibility is needed
1. Secondary access pattern
1. Must specify a primary key on creation
1. Specify primary key (single or composite) like for main table
1. AWS will copy all items from the main table into the secondary index in the reshaped form.  You can then query the secondary index.
1. Two kinds of secondary indexes:
    1. Local secondary index (LSI)
        1. Uses the same partition key as the table partition key but a different sort key
        1. The partition key can act like a top-level property and different sort keys for granular filters
        1. Must be created when the table is created
        1. Eventual consistency by default, can be strongly consistent but at a higher cost
    1. Global secondary index (GSI)
        1. Choose any attributes as partition key and sort key
        1. Must provisional additional throughput (RCUs, WCUs) for GSI
        1. GSI throughput is separate from main table
        1. Data replicated from main table to GSI is eventually consistent only



