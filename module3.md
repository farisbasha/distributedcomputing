# 1. Explain the three approaches used for implementing distributed mutual exclusion? (3)


| Feature                                     | Token-Based Algorithm                                        | Non-Token-Based Approach                                 | Quorum-Based Approach                                    |
|---------------------------------------------|--------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|
| Unique Token Sharing                        | Yes                                                          | No                                                       | No                                                       |
| Method of Granting Access                   | Possession of Token                                          | Communication and Agreement Among Sites                  | Requesting Permission from Subset of Sites               |
| Ordering Requests                          | Sequence Numbers                                             | Timestamps                                               | -                                                        |
| Handling Request Conflicts                  | Based on Sequence Numbers                                    | Based on Timestamps                                      | -                                                        |
| Handling Concurrent Requests               | Token Ownership Determines Access                            | Timestamp Comparison Determines Access                    | -                                                        |
| Mutual Exclusion Guarantee                | Token Uniqueness Ensures Mutual Exclusion                   | Timestamps Ensure Mutual Exclusion                       | Quorum Structure Ensures Mutual Exclusion                |
| Examples                                   | Suzuki–Kasami Algorithm                                      | Ricart–Agrawala Algorithm                               | Maekawa’s Algorithm                                      |



# 2. List out the s strategies for handling deadlocks in a distributed environment

Sure, here's an overview of deadlock prevention, avoidance, and detection:

1. **Deadlock Prevention**:
   - Deadlock prevention involves designing systems in a way that eliminates one or more of the necessary conditions for deadlock to occur. The necessary conditions for deadlock are:
      - Mutual Exclusion: Resources cannot be shared; each resource is either allocated to exactly one process or is available.
      - Hold and Wait: Processes hold resources already allocated to them while waiting for additional resources.
      - No Preemption: Resources cannot be forcibly taken away from a process; they must be released voluntarily.
      - Circular Wait: There must be a circular chain of two or more processes, each of which is waiting for a resource held by the next process in the chain.
   - By ensuring that one or more of these conditions cannot be met, deadlock can be prevented. Techniques include resource allocation graphs, lock ordering, and ensuring that processes request and release resources in a specific order.

2. **Deadlock Avoidance**:
   - Deadlock avoidance aims to dynamically allocate resources to processes in a way that ensures deadlock never occurs. It requires knowledge of future resource requests by processes, which is often impractical or impossible.
   - One common technique for deadlock avoidance is the Banker's Algorithm. This algorithm simulates resource allocation to determine whether granting a resource request would potentially lead to deadlock. If granting the request could potentially result in deadlock, the request is delayed until it can be safely granted.

3. **Deadlock Detection**:
   - Deadlock detection involves periodically checking the system's state to determine whether a deadlock has occurred. If a deadlock is detected, appropriate actions can be taken to recover from it.
   - Detection can be done using various algorithms, such as the resource allocation graph algorithm. This algorithm analyzes the resource allocation graph to identify cycles, indicating potential deadlocks.
   - Once a deadlock is detected, recovery strategies such as process termination, resource preemption, or rollback can be applied to resolve the deadlock and restore system functionality.
  

# 3.Issues Deadlock detection

- Deadlock detection comprises two main tasks: identifying existing deadlocks and resolving them.
- Detecting deadlocks involves maintaining and searching the wait-for graph (WFG) for cycles.
- In distributed systems, the effectiveness of cycle detection depends on the WFG representation across sites.
- Deadlock detection algorithms must meet two criteria: progress (detect all deadlocks in finite time) and safety (avoid reporting false deadlocks).
- In distributed systems without global memory or clock, ensuring algorithm correctness is challenging due to potential inconsistency across sites.
- This inconsistency can lead to the detection of non-existent cycles, termed phantom deadlocks.


# 4.Lamports Algorithm

**Lamport's Distributed Mutual Exclusion Algorithm**:

- **Overview**: Utilizes permission-based approach and timestamps for mutual exclusion in distributed systems.
- **Algorithm**: Requests, replies, and releases are exchanged using three message types. Sites maintain timestamp-ordered request queues.
- **Complexity**: Requires 3(N-1) messages per critical section execution.
- **Drawbacks**: Unreliable if a process fails; high message complexity.
- **Performance**: Synchronization delay equals maximum message transmission time. Can be optimized to 2(N-1) messages.
- **Advantages**: Simplicity, fairness, scalability.
- **Disadvantages**: Message overhead, delayed execution, limited fault tolerance.
```
// Variables:
//   tsi: Timestamp of Site Si
//   request_queuei: Queue of Site Si ordered by timestamps

// To enter Critical section:
When Si wants to enter critical section:
    Send REQUEST(tsi, i) message to all other sites
    Add REQUEST(tsi, i) to request_queuei

When Sj receives REQUEST(tsi, i) from Si:
    Send REPLY(tsj, j) message to Si
    Add REQUEST(tsi, i) to request_queuej

// To execute the critical section:
When Si wants to execute critical section:
    If all sites have sent REPLY messages with timestamps greater than (tsi, i) and request_queuei is at the top:
        Enter critical section
        Execute critical section code
        Send RELEASE message to all other sites
        Remove REQUEST(tsi, i) from request_queuei

// To release the critical section:
When Si exits the critical section:
    Remove REQUEST(tsi, i) from request_queuei
    Send RELEASE message to all other sites
```
<img width="727" alt="Screenshot 2024-04-15 at 10 27 25 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/e8590641-c0d6-4b4a-bfb6-626b3124a286">


# 5.Ricart-Agarwala Algo
**Ricart–Agrawala Algorithm**:

- **Overview**: Proposed by Ricart and Agrawala, it's a mutual exclusion algorithm for distributed systems, building upon Lamport's Algorithm.
- **Algorithm**: Uses REQUEST and REPLY messages for permission-based mutual exclusion. Requests are timestamped.
- **Entry**: Site Si sends REQUEST to all sites. Upon receiving a REQUEST, a site Sj sends REPLY if it's not requesting or executing the critical section, or if Si's timestamp is smaller.
- **Execution**: Si enters the critical section upon receiving REPLY messages from all other sites.
- **Exit**: Si sends REPLY to deferred requests upon exiting the critical section.
- **Message Complexity**: Requires 2(N – 1) messages per critical section execution, including (N – 1) request messages and (N – 1) reply messages.
- **Advantages**:
  - Low message complexity.
  - Scalability for large systems.
  - Non-blocking operation allows nodes to continue normal operations.
- **Drawbacks**:
  - Unreliable: Failure of any node can halt the system's progress, leading to potential starvation.
- **Performance**:
  - Synchronization delay equals maximum message transmission time.
  - Requires 2(N – 1) messages per critical section execution.
 <img width="636" alt="Screenshot 2024-04-15 at 10 35 29 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/37592cc4-efac-476c-8356-602e3ae900b8">
<img width="606" alt="Screenshot 2024-04-15 at 10 38 19 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/af91f924-39e1-4848-9dbc-286a60142c95">


# 6.Suzuki-Kasami

**Suzuki–Kasami Algorithm**:

- **Overview**: Token-based mutual exclusion algorithm, derived from Ricart–Agrawala, utilizing REQUEST and REPLY messages.
- **Token-Based Approach**: Access to critical section granted via possession of a unique token, unlike non-token algorithms.
- **Sequence Numbers**: Each critical section request contains a sequence number to differentiate between old and current requests.
- **Data Structures**:
  - RN[1…N]: Array to track largest sequence numbers received from each site.
  - LN[1…N]: Array used by the token, with LN[j] representing the recently executed request's sequence number from site Sj.
  - Queue Q: Maintains IDs of sites waiting for the token.
- **Algorithm**:
  - **Entry**: Si increments its sequence number and sends REQUEST(i, sn) to request the token. Sj updates RNj[i] and sends the token if conditions are met.
  - **Execution**: Si enters critical section upon acquiring the token.
  - **Release**: Si updates LN[i], updates Q, and sends the token if the queue is non-empty.
- **Message Complexity**: Requires either 0 or a maximum of N messages per critical section execution, depending on token ownership.
- **Drawbacks**:
  - Non-symmetric: Sites retain the token without requesting critical section access, violating symmetry principles.
- **Performance**:
  - Synchronization delay is 0 if the site holds the idle token. Otherwise, maximum delay equals message transmission time, with a maximum of N messages per critical section invocation.

<img width="507" alt="Screenshot 2024-04-15 at 10 48 26 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/e401c510-4ad4-43b9-b864-c4eff377e50c">


# 7.Maekawa Algo

**Maekawa’s Algorithm**:

- **Overview**: Quorum-based mutual exclusion algorithm, distinct from permission-based approaches like Lamport’s and Ricart-Agrawala.
- **Quorum Concept**: Sites request permission from a subset called quorum, not every site.
- **Messages**: Utilizes REQUEST, REPLY, and RELEASE messages.
- **Request Set Construction**:
  - Quorums must satisfy:
    - Common sites between any two quorums.
    - Each site belongs to exactly K quorums.
    - N = K(K - 1) + 1, and |Ri| = √N.
- **Algorithm**:
  - **Entry**: Si sends REQUEST(i) to sites in its quorum. Sj replies if no recent RELEASE, else queues the request.
  - **Execution**: Si enters if all sites in its quorum replied.
  - **Release**: Si sends RELEASE(i) to its quorum. Sj sends REPLY to next queued site or updates status.
- **Message Complexity**: Requires 3√N messages per critical section execution, involving √N requests, replies, and releases.
- **Drawbacks**:
  - Deadlock-prone due to locking by other sites, without prioritizing requests by timestamp.
- **Performance**:
  - Synchronization delay equals twice message propagation delay time.
  - Requires 3√N messages per critical section execution.
- **Advantages**:
  - Low message complexity: O(sqrt(N)) messages per invocation.
  - Scalability: Suitable for large-scale distributed systems.
  - Fairness: Ensures fairness by allowing one process per group to enter the critical section eventually.
- **Disadvantages**:
  - High overhead: Maintaining group membership information can lead to increased network traffic and latency.
  - Limited flexibility: Requires predefined number of groups, less adaptable than dynamically adjustable algorithms.
  - Delayed execution: Process may need to wait for messages from other groups, causing delays in critical section execution.
 <img width="794" alt="Screenshot 2024-04-15 at 11 03 10 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/67b4fde7-7146-4a33-a62c-48b9e181f53e">

- 
# 7. Explain in detail about deadlock handling strategies in a distributed environment
**System Model**:
- Distributed system composed of processors connected by a communication network.
- Communication delay is finite but unpredictable.
- Comprised of n asynchronous processes communicating via message passing.
- Each process runs on a separate processor.
- No common global memory; communication solely via message passing.
- No physical global clock; communication may be asynchronous and unordered.
- System modeled as a directed graph with processes as vertices and communication channels as edges.
  
**Assumptions**:
- Only reusable resources in the system.
- Processes allowed exclusive access to resources.
- Single copy of each resource.
- Processes can be in running (active) or blocked state, waiting for resources.

**Deadlock Handling Strategies**:
- **Prevention**: Acquire all resources before execution or preempt processes holding needed resources.
- **Avoidance**: Grant resource if resulting system state is safe; impractical in distributed systems.
- **Detection**: Examine process-resource interactions for cyclic wait; best approach in distributed systems.

Handling deadlocks in distributed systems is complex due to limited knowledge of system state and unpredictable communication delays. Deadlock prevention and avoidance are often impractical, making deadlock detection the preferred strategy.


# 8.Explain / compare various models of deadlocks. / Explain any different models of deadlock

1. **Single-Resource Model**:
   - Processes can have at most one outstanding request for one unit of a resource.
   - Presence of a cycle in the WFG indicates deadlock.

2. **AND Model**:
   - Processes can request multiple resources simultaneously, fulfilled only when all requested resources are granted.
   - Cycle in WFG indicates deadlock.

3. **OR Model**:
   - Processes can request multiple resources simultaneously, fulfilled if any requested resource is granted.
   - Cycle in WFG does not necessarily imply deadlock.

4. **AND-OR Model**:
   - Generalization of OR and AND models.
   - Requests may specify combinations of AND and OR in resource requests.
   - Includes P-out-of-Q model, allowing requests for any k resources out of n.

5. **Unrestricted Model**:
   - No assumptions about resource request structure.
   - Assumes deadlock stability.
   - Separates concerns about problem properties and underlying distributed system computations.

