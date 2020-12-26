# CloudFront Exam Study (Just the Important Stuff)

## Key Terms

1. Edge Location: A physical location where content is cached.  Separate from a Region or AZ.
1. Origin: A source or origination point of content (S3 bucket, EC2 instance, Elastic Load Balancer, Route53)
1. Distribution: A name or label for a CDN consisting of a number of Edge Locations.
1. Web distribution:  
1. RTMP: Streaming Adobe Flash media files



## Exam Tips:

1. Understand Edge Location is different and/or separate from a Region or AZ

1. Understand the Origin and that an Origin can be non-AWS
  * You can have **multiple origins** for a single distribution

1. Distributions (Web, RTMP)

1. Edge locations are not just READ, you can also write to edge Locations (i.e.
  PUT an object to an Edge location)

1. Objects are cached for the life of the TTL (time to live)

1. You can restrict access to content by enabling Restrict Viewer Access
and using/specifying signed URLs or signed cookies.

1. You can clear cached objects on-demand (independently of the TTL) but you
will be charged for it.



## Lab:  Creating a CDN

1. Create buckets in Sydney and Seoul

1. Upload objects to each bucket

1. Add Everyone Open/Download permissions to each object

4.  
