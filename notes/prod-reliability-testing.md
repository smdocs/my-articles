# Production Reliability Testing

####Scenarios
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

####Limitations

The goal of fault injection and GameDay exercises is to increase confidence in an otherwise complicated or complex system's ability to stay resilient, but they have limitations.

First, the exercises aren't meant to discover how engineering teams handle working under time pressure with escalating and sometimes disorienting scenarios. That needs to come from the postmortems of actual incidents, not from handling faults that have been planned and designed for.

The faults and failure modes are contrived. They reflect the fault designer's imagination and therefore can't be comprehensive enough to guarantee the system's safety. While any increase in confidence in the system's resiliency is positive, it's still just that: an increase, not a completion of perfect confidence. Any complex system can (and will) fail in surprising ways, no matter how many different types of faults you inject and recover from.

Some have suggested that continually introducing failures automatically is a more efficient way to gain confidence in the adaptability of the system than manually running GameDay exercises as an engineering-team event. Both approaches have the same limitation mentioned here, that they increase confidence but can't be used to achieve sufficient safety coverage.


