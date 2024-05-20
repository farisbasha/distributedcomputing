
# **Distributed Mutual Exclusion Algorithms**

- **Mutual Exclusion**: Ensures serialized access to shared resources in distributed systems.
- **Challenges**: Shared variables or local kernel can't be used; relies on message passing.
- **Key Approaches**:
  1. **Token-based**: Unique token grants access to CS; mutual exclusion ensured by the token's uniqueness.
  2. **Non-token-based**: Multiple message rounds determine CS access based on local assertions.
  3. **Quorum-based**: Requests CS access from a subset (quorum) of sites; ensures mutual exclusion through intersecting quorums.

| Feature                                     | Token-Based Algorithm                                        | Non-Token-Based Approach                                 | Quorum-Based Approach                                    |
|---------------------------------------------|--------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|
| Unique Token Sharing                        | Yes                                                          | No                                                       | No                                                       |
| Method of Granting Access                   | Possession of Token                                          | Communication and Agreement Among Sites                  | Requesting Permission from Subset of Sites               |
| Ordering Requests                          | Sequence Numbers                                             | Timestamps                                               | -                                                        |
| Handling Request Conflicts                  | Based on Sequence Numbers                                    | Based on Timestamps                                      | -                                                        |
| Handling Concurrent Requests               | Token Ownership Determines Access                            | Timestamp Comparison Determines Access                    | -                                                        |
| Mutual Exclusion Guarantee                | Token Uniqueness Ensures Mutual Exclusion                   | Timestamps Ensure Mutual Exclusion                       | Quorum Structure Ensures Mutual Exclusion                |
| Examples                                   | Suzuki–Kasami Algorithm                                      | Ricart–Agrawala Algorithm                               | Maekawa’s Algorithm                                      |




**System Model**:
- **Components**: N sites (S1, S2, ..., SN) with one process per site.
- **States**: Requesting CS, executing CS, idle.
- **Token-based idle state**: Holding token but not executing CS.
- **Request Queue**: Sites queue and serve CS requests sequentially.

**Requirements**:
1. **Safety Property**:
   - Ensures that at any instant, only one process can execute the critical section (CS).
   - Essential for maintaining the integrity of shared resources.

2. **Liveness Property**:
   - Guarantees the absence of deadlock and starvation.
   - Ensures that two or more sites do not endlessly wait for messages that will never arrive.
   - Every requesting site should have the opportunity to execute the CS in finite time.
   - A site must not wait indefinitely to execute the CS while other sites are repeatedly executing it.

3. **Fairness**:
   - Ensures each process gets a fair chance to execute the CS.
   - Generally means that CS execution requests are executed in the order of their arrival in the system.
  

**Performance Metrics**:
- **Message Complexity**: Number of messages per CS execution.
- **Synchronization Delay**: Time between CS exits and next CS entry.
- **Response Time**: Wait time for CS execution post request.
- **System Throughput**: Rate of CS execution, calculated as $\text{throughput} = 1 / (\text{SD} + E)$, where SD is synchronization delay, E is average CS execution time.

**Load Performance**:
- **Low Load**: Rare simultaneous CS requests.
- **High Load**: Constant pending CS requests at sites.

---

# Strategies for handling deadlocks in a distributed environment

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
  

---

# Issues Deadlock detection

- Deadlock detection comprises two main tasks: identifying existing deadlocks and resolving them.
- Detecting deadlocks involves maintaining and searching the wait-for graph (WFG) for cycles.
- In distributed systems, the effectiveness of cycle detection depends on the WFG representation across sites.
- Deadlock detection algorithms must meet two criteria: progress (detect all deadlocks in finite time) and safety (avoid reporting false deadlocks).
- In distributed systems without global memory or clock, ensuring algorithm correctness is challenging due to potential inconsistency across sites.
- This inconsistency can lead to the detection of non-existent cycles, termed phantom deadlocks.

---



# Lamport's Distributed Mutual Exclusion Algorithm**:

- **Overview**: Utilizes permission-based approach and timestamps for mutual exclusion in distributed systems.
- **Algorithm**: Requests, replies, and releases are exchanged using three message types. Sites maintain timestamp-ordered request queues.
- **Complexity**: Requires 3(N-1) messages per critical section execution.
- **Drawbacks**: Unreliable if a process fails; high message complexity.
- **Performance**: Synchronization delay equals maximum message transmission time. Can be optimized to 2(N-1) messages.
- **Advantages**: Simplicity, fairness, scalability.
- **Disadvantages**: Message overhead, delayed execution, limited fault tolerance.

<img width="470" alt="Screenshot 2024-05-19 at 10 44 48 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/d806570e-607b-487c-bf30-de885477f53b">

<img width="727" alt="Screenshot 2024-04-15 at 10 27 25 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/e8590641-c0d6-4b4a-bfb6-626b3124a286">


# Ricart–Agrawala Algorithm**:

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




# **Suzuki–Kasami Algorithm**:

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


# **Maekawa’s Algorithm**:

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
# Deadlock handling strategies in a distributed environment
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

---

# Models of Deadlocks

1. **Single Resource Model**:
   - **Description**: Each process can request only one unit of a resource at a time.
   - **Detection**: A cycle in the Wait-for-Graph (WFG) indicates a deadlock.
   - **Diagram**: Shows processes forming a cycle, indicating a deadlock.
   - <img width="187" alt="Screenshot 2024-05-19 at 9 47 15 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/56846528-afe5-4acb-befc-17af3ea12247">


2. **AND Model**:
   - **Description**: A process can request multiple resources simultaneously and becomes active only when all requested resources are granted.
   - **Detection**: A cycle in the WFG indicates a deadlock. However, a process can be deadlocked without being part of a cycle if it is dependent on another deadlocked process.
   - **Diagram**: Shows a cycle indicating a deadlock.
   - <img width="159" alt="Screenshot 2024-05-19 at 9 47 35 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/09c3771b-deef-4f1c-949d-d9a3c95a24e3">


3. **OR Model**:
   - **Description**: A process can request multiple resources and proceeds if any one of them is granted.
   - **Detection**: A knot in the WFG indicates a deadlock. Presence of a cycle does not necessarily imply a deadlock.
   - **Conditions for Deadlock**:
     1. Each process in the set is blocked.
     2. The dependent set for each process is a subset of the set.
     3. No grant message is in transit between any processes in the set.
   - **Diagram**: Shows the structure and dependencies indicating a potential deadlock.
   - <img width="140" alt="Screenshot 2024-05-19 at 9 47 59 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/83c7f513-67a2-4d73-929e-8f74b486e486">


4. **AND-OR Model**:
   - **Description**: Combines AND and OR models; requests can be a mix of both.
   - **Detection**: No direct WFG method; detection relies on repeated OR-model tests, which is inefficient.

5. **P-out-of-Q Model**:
   - **Description**: A process can request any \( p \) out of \( q \) resources. This model is a compact variation of the AND-OR model.
   - **Detection**: Can be expressed using AND-OR logic.
   - **Diagram**: Illustrates how resources can be requested and allocated.
   - <img width="159" alt="Screenshot 2024-05-19 at 9 48 18 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/d81c2cf2-ee67-4e1e-85d8-046ef74b8294">


6. **Unrestricted Model**:
   - **Description**: No specific assumptions on resource requests, only that deadlocks are stable.
   - **Detection**: This model separates the concerns of deadlock properties from system computations.
   - **Diagram**: Not specific, focuses on general principles.


---

# Wait-for Graph (WFG)

#### Definition
- **Wait-for Graph (WFG)**: A directed graph where nodes represent processes and directed edges indicate that one process is waiting for another to release a resource.

#### Deadlock Detection
- **Deadlock**: Occurs if there is a directed cycle or knot in the WFG, indicating mutual waiting among processes with no progress possible.

#### Example Figure 10.1 Analysis
<img width="488" alt="Screenshot 2024-05-19 at 9 42 48 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/3cf84ff0-eb50-4d66-bb44-47f6cd28a1de">

- **Processes and Dependencies**:
  - **Site 1**: $P_{11}$ waits for $P_{21}$ (same site) and $P_{32}$ (Site 2).
  - **Site 2**: $P_{32}$ waits for $P_{33}$ (Site 3).
  - **Site 1**: $P_{21}$ waits for $P_{24}$ (Site 4).
  - **Site 4**: $P_{54}$ waits for $P_{44}$; $P_{44}$ waits for $P_{24}$.

  If $P_{33}$ (Site 3) waits for $P_{24}$ (Site 4), a cycle forms, causing deadlock.

### Summary Based on Figure 10.1 in the Context of Different Deadlock Models

#### The Single-Resource Model
- In this model, a process can have at most one outstanding request for a single resource.
- A cycle in the WFG directly indicates a deadlock.
- **Figure 10.1 Example**: The cycle $P_{11} \rightarrow P_{21} \rightarrow P_{24} \rightarrow P_{54} \rightarrow P_{11}$ shows a deadlock.

#### The AND Model
- A process can request multiple resources simultaneously and the request is satisfied only when all requested resources are granted.
- The presence of a cycle in the WFG indicates a deadlock.
- **Figure 10.1 Example**: Process $P_{11}$ has two outstanding requests. The cycle $P_{11} \rightarrow P_{21} \rightarrow P_{24} \rightarrow P_{54} \rightarrow P_{11}$ corresponds to a deadlock situation.
- Even processes not part of the cycle can be deadlocked if they are dependent on the cycle. For example, $P_{44}$ is deadlocked due to its dependency on $P_{24}$.

#### The OR Model
- A process can make requests for multiple resources simultaneously, and the request is satisfied if any one of the requested resources is granted.
- The presence of a cycle does not necessarily indicate a deadlock. Instead, the presence of a knot in the WFG indicates a deadlock.
- **Figure 10.1 Example**: If all nodes are OR nodes, $P_{11}$ is not deadlocked if $P_{33}$ releases its resources, allowing $P_{32}$ and then $P_{11}$ to proceed.
- A blocked process is deadlocked if it is in a knot or if it can only reach processes that are part of a knot.

#### The AND-OR Model
- This model combines the AND and OR models, allowing complex resource requests.
- A request can specify any combination of AND and OR.
- Deadlock detection involves testing for OR-model deadlocks repeatedly, though this is inefficient.

#### The P-out-of-Q Model
- A variation of the AND-OR model where a request can specify obtaining any $k$ out of $n$ available resources.
- Expressive power is similar to the AND-OR model but with a more compact request formation.

#### The Unrestricted Model
- No assumptions are made about the structure of resource requests.
- The only assumption is that the deadlock is stable.
- These algorithms are theoretically valuable but may not be practical due to lack of assumptions about the underlying distributed system computations.

### Specific Analysis for Figure 10.1
- **Single-Resource and AND Models**: The cycle $P_{11} \rightarrow P_{21} \rightarrow P_{24} \rightarrow P_{54} \rightarrow P_{11}$ confirms a deadlock.
- **OR Model**: The presence of a cycle does not necessarily indicate a deadlock. Deadlock detection requires identifying a knot, not just a cycle.
- **Impact on $P_{44}$**: Regardless of the cycle or knot, $P_{44}$ is deadlocked due to its dependency on $P_{24}$.

Thus, Figure 10.1 shows a deadlock in both the single-resource and AND models due to the presence of a cycle. In the OR model, further analysis would be needed to confirm a deadlock by checking for knots.
