# Distributed Mutual Exclusion Algorithms

Mutual exclusion is a fundamental problem in distributed computing systems. It ensures that concurrent access of processes to a shared resource or data is serialized, executed in a mutually exclusive manner. In a distributed system, shared variables (semaphores) or a local kernel cannot be used to implement mutual exclusion. Message passing is the sole means for implementing distributed mutual exclusion. 

## Approaches for Implementing Distributed Mutual Exclusion:

1. **Token-based Approach**:
   - In this approach, a unique token (also known as the PRIVILEGE message) is shared among the sites.
   - Mutual exclusion is ensured because the token is unique.
   - Algorithms based on this approach differ in the way a site carries out the search for the token.

2. **Non-token-based Approach**:
   - In this approach, two or more successive rounds of messages are exchanged among the sites to determine which site will enter the CS next.
   - Mutual exclusion is enforced because the assertion becomes true only at one site at any given time.

3. **Quorum-based Approach**:
   - Each site requests permission to execute the CS from a subset of sites (quorum).
   - Quorums are formed so that when two sites concurrently request access to the CS, at least one site receives both requests and ensures only one request executes the CS at any time.

## System Model:
- The system consists of N sites, S1, S2, ..., SN.
- A single process is running on each site.
- A site can be in one of three states: requesting the CS, executing the CS, or neither requesting nor executing the CS (idle).
- Safety, Liveness, and Fairness properties are essential for mutual exclusion algorithms.

## Performance Metrics:
- **Message Complexity**: Number of messages required per CS execution by a site.
- **Synchronization Delay**: Time required before the next site enters the CS after a site leaves.
- **Response Time**: Time interval a request waits for its CS execution to be over.
- **System Throughput**: Rate at which the system executes requests for the CS.
  - $\text{System throughput} = \frac{1}{\text{SD} + E}$

   Where:
   - $\text{SD}$ represents the synchronization delay.
   - $E$ represents the average critical section execution time.

## Low and High Load Performance:
- Performance of mutual exclusion algorithms is studied under low and high load conditions, determined by the arrival rate of CS execution requests.
- Under low load conditions, there is seldom more than one request for the CS present in the system simultaneously.
- Under heavy load conditions, there is always a pending request for the CS at a site.
