# Production Reliability Testing

##### Injection Points
We have several key “building block” components that are used within Netflix. They help us to isolate failure and define fallbacks (Hystrix), communicate with dependencies (Ribbon), cache data (EVCache), or persist data (Astyanax). Each of these layers make perfect inflection points to inject failure. These layers interface with the FIT context to determine if this request should be impacted. The failure behavior is provided to that layer, which determines how to emulate that failure in a realistic fashion: sleep for a delay period, return a 500, throw an exception, etc.

##### Failure Scenarios
Whether we are recreating a past outage, or proactively testing the loss of a dependency, we need to know what could fail in order to build a simulation. We use an internal system that traces requests through the entirety of the Netflix ecosystem to find all of the injection points along the path. We then use these to create failure scenarios, which are sets of injection points which should or should not fail. One such example is our critical services scenario, the minimum set of our services required to stream. Another may be the loss of an individual service, including its persistence and caching layers.

##### Automated Testing
Failure testing tools are only as valuable as their usage. Our device testing teams have developed automation which: enables failure, launches Netflix on a device, browses through several lists, selects a video, and begins streaming. We began by validating this process works if only our critical services are available. Currently we are extending this to identify every dependency touched during this process, and systematically failing each one individually. As this is running continuously, it helps us to identify vulnerabilities when introduced.

##### Resiliency Strategies
FIT has proven useful to bridge the gap between isolated testing and large scale chaos exercises, and make such testing self service. It is one of many tools we have to help us build more resilient systems. The scope of the problem extends beyond just failure testing, we need a range of techniques and tools: designing for failure, better detection and faster diagnosis, regular automated testing, bulkheading, etc. If this sounds interesting to you, we’re looking for great engineers to join our reliability, cloud architecture, and API teams!


#### Scenarios
1. One of the app servers dies (power cable yanked out).
2. One of the nodes in the messaging cluster dies.
2. All of the app servers leave the load-balancing pool.
3. One of the app servers gets wiped clean and needs to be fully rebuilt from scratch.
4. Database dies (power cable yanked out and/or process is killed ungracefully).
5. Database is fully corrupt and needs full restore from backup.
6. Offsite database replica is needed to investigate/restore/replay single transactions.
7. Connectivity to third-party sites is cut off entirely.

Expectations for how the system would behave if these scenarios occurred in production, 
and how they could confirm these expectations with logs, graphs, and alerts. Once armed with these scenarios, they worked on 
how to make these failures either:

- not matter at all (transparently recover and continue on with processing),
- matter only temporarily (gracefully degrade with no data loss and provide constructive feedback to the user),
- or matter only to a minimal subset of users (including an audit log for reconstructing and recovering quickly and possibly automatically).

After these mechanisms were written and tested in development, the time came to test them in production. The Etsy team was cognizant of how much activity the system was seeing; the support and product groups were on hand to help with any necessary communication; and team members went through each of the scenarios, gathering answers to questions such as:

- Were they successful in transparently recovering, through redundancy, replication, queuing, etc.?
- How long did each process take—in the case of rebuilding a node automatically from scratch, recovering a database, etc.?
- Could they confirm that no data was lost during the entire exercise?
- Were there any surprises?

#### Limitations

The goal of fault injection and GameDay exercises is to increase confidence in an otherwise complicated or complex system's ability to stay resilient, but they have limitations.

First, the exercises aren't meant to discover how engineering teams handle working under time pressure with escalating and sometimes disorienting scenarios. That needs to come from the postmortems of actual incidents, not from handling faults that have been planned and designed for.

The faults and failure modes are contrived. They reflect the fault designer's imagination and therefore can't be comprehensive enough to guarantee the system's safety. While any increase in confidence in the system's resiliency is positive, it's still just that: an increase, not a completion of perfect confidence. Any complex system can (and will) fail in surprising ways, no matter how many different types of faults you inject and recover from.

Some have suggested that continually introducing failures automatically is a more efficient way to gain confidence in the adaptability of the system than manually running GameDay exercises as an engineering-team event. Both approaches have the same limitation mentioned here, that they increase confidence but can't be used to achieve sufficient safety coverage.


