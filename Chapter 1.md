# Foundations of data system

Chapter 1 introduces the terminology and approach that we’re going to use throughout this book. It examines what we actually mean by words like reliability, scalability, and maintainability, and how we can try to achieve these goals.

Successful data systems:

- databases
- caches
- Search indexes: search by keyword or filter
- stream processing: send a message to another process, to be handled asynchronously. 
- Batch processing: periodically crunch a large amount of accumulated data. 

There are various approaches to caching, several ways of building search indexes, and so on. When building an application, we still need to figure out which tools and which approaches are the most appropriate for the task at hand.

## Reliability

Making things work correctly, even when faults occur. 

**Hardware faults**

- Random and independent from each other

**Software faults**

- Harder to anticipate

**Human error**

- Humans are known to be unreliable
- Methods
  - Well- designed abstractions, APIs and admin interfaces. 
  - Used a sandbox to experiment
  - Test thoroughly at all levels
  - Automated testing
  - Set up detailed and clear monitoring (e.g. telemetry in Rocket lift off)

**How important is Reliability**

More mundane applications are also expected to work reliably. 

## Scalability

The term to describe a system’s ability to cope with increased load. 

Not a one-dimensional label

**load parameters**

Twitter:

- Post a tweet: insert new tweet into a global collection of tweet (4.6k write/sec, peak 12k write/sec). Not very difficult
- Home timeline, user can view posted by the people they follow (300k read/sec)

Solution:

- Post: simply insert the new tweet into a global collection
- Look up all the people they follow, find all the tweets for each of those users, and merge them (sorted by time)
- Maintain a cache for each user’s home timeline
- Insert the result to the cache

**Describe performance**

- Throughput: the number of records we can process per second
- Response time: the time between a client sending a request and receiving a response. 

**Latency and response time**

- Response time: service time + network delays (what the client sees)
- Latency: the duration that a request is waiting to be handled
- Average is not a very good metric. Percentile/median(p50) is better.
- Look at higher percentiles, like p95, p99
- The measure of response time should be independent from the request handling. 
- Averaging percentiles is meaningless. The right way is to add histograms. 

**Approaches for coping with load**

- In  reality, good architectures usually involve a pragmatic mixture of approaches. 
- Common practice: first think about scaling up, then make it distributed. 

## Maintainability

Making life better for the engineering and operations teams who need to work with it. 

- Operability: make the software running smoothly and visible. 
  - Monitoring 
  - Keeping software update to date
  - Establishing good tools for deployment
  - security
  - Make operation predictable
  - Preserving the organization’s knowledge about the system
  - Avoiding dependency on individual machine
  - Automation and integration with standard tool. 
  - documentation
  - Self-healing
- simplicity
  - The best tool: abstraction
  - Hide implementation detail behind a clean, simple-to-understand facade. 
- Evolvability
  - How to deal with: agile working patterns. 
  - Test driven development
  - refactoring



