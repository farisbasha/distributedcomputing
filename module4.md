# **Distributed Shared Memory (DSM)**:

<img width="635" alt="Screenshot 2024-04-16 at 12 16 27 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/40c33590-e151-4add-8368-c42c81240055">



| **Advantages of DSM**                              | **Disadvantages of DSM**                                    |
|----------------------------------------------------|-----------------------------------------------------------|
| 1. Simplifies network communication for programmers | 1. Programmers must understand replica consistency models    |
| 2. Provides a single address space                 | 2. Overheads comparable to message passing                 |
| 3. Exploits locality of reference                  | 3. Standard implementations may have higher overhead       |
| 4. Cost-effective compared to dedicated systems    | 4. Loss of control over memory management                  |
| 5. No bottleneck from single memory access bus     | 5. Restricted efficiency compared to tailored solutions    |
| 6. Offers a large virtual main memory              | 6. Potential for increased complexity in debugging and     |
| 7. Provides program portability                    |    troubleshooting                                           |


 
 
 #### Main issues in designing a DSM system:

1. **Semantics of Concurrent Access**:
   - Clear specification of semantics for concurrent access to shared objects is crucial for programmer understanding and coding.
  
2. **Implementation of Access Semantics**:
   - Decision on replication strategy (partial or full) and type (read or write) affects system efficiency.
   
3. **Location Selection for Replication**:
   - Optimizing replication locations to enhance system efficiency.
   
4. **Accessing Remote Data**:
   - Determining locations of remote data when full replication is not used.
   
5. **Communication Efficiency**:
   - Minimizing communication delays and message overhead during implementation of access semantics.
  

# **Q) Describe Shared memory Mutual exclusion**

- **Definition**: Mutual exclusion prevents race conditions by ensuring that only one process can enter its critical section at a time.
  
- **Single Computer System**:
  - Shared memory and resources facilitate easy access to shared variables.
  - Mutual exclusion achieved through shared variables like semaphores.

- **Distributed System**:
  - Absence of shared memory and common physical clock complicates mutual exclusion.
  - Approach based on message passing used to address mutual exclusion.
  - Lack of complete system state information due to absence of shared memory and common clock.
 
  

# **Checkpointing and Rollback Recovery**

1. **Definition**:
   - Rollback recovery treats a distributed system application as a collection of processes communicating over a network.
   - It involves periodically saving the state of a process during failure-free execution to enable restart from a saved state upon failure, reducing lost work.
   - Saved states are called checkpoints, and restarting from them is termed rollback recovery.

2. **Challenges in Distributed Systems**:
   - Rollback recovery in distributed systems is complex due to message-induced inter-process dependencies.
   - Failure of one process may force others to rollback, leading to rollback propagation, also known as the domino effect.

3. **Causes of Rollback Propagation**:
   - When a sender rolls back before sending a message, the receiver must also rollback to maintain consistency.
   - Failure to rollback would imply message receipt without sending, violating correct execution.

4. **Preventing Rollback Propagation**:
   - **Coordinated Checkpointing**: Processes coordinate checkpoints to achieve system-wide consistent states, preventing rollback propagation.
   - **Communication-Induced Checkpointing**: Checkpoints are taken based on information received from other processes, ensuring system-wide consistency.
   - **Log-based Rollback Recovery**: Combines checkpointing with logging of nondeterministic events, allowing deterministic recreation of pre-failure states.

5. **Log-based Rollback Recovery**:
   - Relies on the piecewise deterministic (PWD) assumption, where all non-deterministic events are identifiable and can be logged.
   - By logging and replaying non-deterministic events, processes can deterministically recreate pre-failure states, even without checkpoints.
   - Enables recovery beyond the most recent set of consistent checkpoints.



# **Q9. Different Types of Messages:**

<img width="409" alt="Screenshot 2024-04-16 at 7 07 01 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7a7a4557-b455-4236-94b7-eff1195da206">

1. **In-transit Messages:**
   - Messages sent but not yet received are termed in-transit messages.
   - They do not cause inconsistency if part of the global system state.
   - For reliable channels, in-transit messages must be included in the system state, ensuring their eventual delivery.
   - In lossy channels, in-transit messages may be omitted from the system state.

2. **Lost Messages:**
   - Messages whose send operation is not undone but receive is undone are called lost messages.
   - Occurs when the receiver rolls back before receiving the message, while the sender does not.
   - Lost messages introduce inconsistencies and must be addressed during recovery.

3. **Delayed Messages:**
   - Messages whose receive event is not recorded due to receiver unavailability or late arrival post-rollback are termed delayed messages.
   - They pose challenges as their reception status is uncertain during recovery.

4. **Orphan Messages:**
   - Messages with receive recorded but send event not recorded are termed orphan messages.
   - They occur when a rollback undoes the message send, leaving the receive event intact.
   - Orphan messages are problematic and must be managed during recovery to maintain consistency.

# **Q10. Issues in Failure Recovery:**
- Restoring the system to a consistent state involves rolling back processes to valid checkpoints.
- Orphan messages arise when a process receives a message after rolling back, causing inconsistencies.
- Rollback of other processes may be required to eliminate orphan messages, ensuring a consistent global state.
- Messages received during the failure may be in abnormal states, including delayed, lost, or orphaned.
- Differentiating between normal, delayed, lost, and orphan messages is crucial during recovery.
- Lost messages can be handled by logging and replaying sent messages during recovery but may lead to duplicates.
- Techniques for managing delayed, lost, and orphan messages are essential for effective failure recovery.

