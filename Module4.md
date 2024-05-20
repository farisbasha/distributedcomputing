# **Distributed Shared Memory (DSM)**:


Distributed Shared Memory (DSM) provides an abstraction that allows programmers to perceive a distributed system's memory as a single monolithic memory, akin to traditional von Neumann architecture. Key points include:

- **Simplified Access**: Programmers use simple read and write primitives to access data across the network.
- **Ease of Use**: Eliminates the need for send and receive communication primitives, simplifying synchronization and consistency management.
- **Memory Allocation**: Part of each computer’s memory is allocated to shared space, while the remainder is private memory.


<img width="635" alt="Screenshot 2024-04-16 at 12 16 27 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/40c33590-e151-4add-8368-c42c81240055">

<img width="301" alt="Screenshot 2024-05-20 at 2 00 09 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7b5ad6fb-ff23-413e-92ce-dc1b8c3a1e2b">



| **Advantages of DSM**                              | **Disadvantages of DSM**                                    |
|----------------------------------------------------|-----------------------------------------------------------|
| 1. Simplifies network communication for programmers | 1. Programmers must understand replica consistency models    |
| 2. Provides a single address space                 | 2. Overheads comparable to message passing                 |
| 3. Exploits locality of reference                  | 3. Standard implementations may have higher overhead       |
| 4. Cost-effective compared to dedicated systems    | 4. Loss of control over memory management                  |
| 5. No bottleneck from single memory access bus     | 5. Restricted efficiency compared to tailored solutions    |
| 6. Offers a large virtual main memory              | 6. Potential for increased complexity in debugging and     |
| 7. Provides program portability                    |    troubleshooting                                           |


#  Lamport’s Bakery Algorithm

Lamport’s Bakery Algorithm is a classic algorithm for achieving mutual exclusion in a system of \( n \) processes that share memory. The algorithm is designed to ensure that no two processes are in the critical section simultaneously.A unique lexicographic order is defined on the tuple <token, pid> maintained for resolving conflict
- The algorithm uses a combination of timestamps and process IDs to ensure a unique ordering of processes.
- The timestamps are chosen in a way that mimics the behavior of customers taking a number in a bakery, thus the name "Bakery Algorithm".
- This algorithm is particularly notable for being one of the first to provide a simple and effective solution to the mutual exclusion problem in distributed systems.

#### 3 Requirements of Critical Section

- **Mutual Exclusion**: Only one process can be in the critical section (CS) at a time.
- **Bounded Waiting**: There is a limit on the number of times other processes can enter their critical sections after a process has made a request to enter and before the requesting process is granted access.
- **Progress**: If no process is in the critical section, and some processes wish to enter, then only those processes that are not in the remainder section will compete to enter the critical section, and one of them will be granted access.

#### Algorithm Breakdown

1. **Initialization**:
   - `choosing[i]`: Boolean array indicating if process $\( P_i \)$ is choosing a timestamp.
   - `timestamp[i]`: Integer array holding the timestamp (or token) for each process.

2. **Entry Section**:
   - Each process $\( P_i \)$ follows these steps to enter the critical section:
     - **Choosing a Timestamp**:
       - Set `choosing[i]` to `true`.
       - Assign `timestamp[i]` to the maximum value in the `timestamp` array plus one.
       - Set `choosing[i]` to `false`.
     - **Wait for Other Processes**:
       - Ensure that any other process $\( P_j \)$ that is in the middle of choosing a timestamp has finished.
       - Wait until all processes with a smaller timestamp (or the same timestamp but a lower process ID) have finished their critical section.

3. **Critical Section**:
   - The process $\( P_i \)$ enters and executes its critical section.

4. **Exit Section**:
   - After completing the critical section, the process resets its timestamp to 0.

5. **Remainder Section**:
   - The process executes the remainder of its code outside the critical section.

<img width="417" alt="Screenshot 2024-05-20 at 2 01 00 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/b3d4c9da-b6ff-4f53-a1ef-75dc7f23b01b">


---

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

4. **Preventing Rollback Propagation ( Domino Effect )**:
   - **Coordinated Checkpointing**: Processes coordinate checkpoints to achieve system-wide consistent states, preventing rollback propagation.
   - **Communication-Induced Checkpointing**: Checkpoints are taken based on information received from other processes, ensuring system-wide consistency.
   - **Log-based Rollback Recovery**: Combines checkpointing with logging of nondeterministic events, allowing deterministic recreation of pre-failure states.



#### Local Checkpoint
- **Definition**: A snapshot of a process's state at a specific point in time.
- **Storage**: Saved on stable storage to be available even after a crash.
- **Rollback**: Processes can roll back to any checkpoint to restore their state.
- **Notation**: $C_{i,k}$ represents the $k$-th checkpoint of process $P_i$.

#### Consistent System States
- **Global State**: A collection of individual states of all processes and communication channels.
- **Consistency**: 
  - **Consistent State**: All received messages have corresponding send events.
  - **Inconsistent State**: Shows receipt of messages without corresponding send events (e.g., due to failures).
- **Goal of Rollback-Recovery**: Restore the system to a consistent state that could have occurred during a failure-free execution.

#### Example
<img width="420" alt="Screenshot 2024-05-20 at 2 03 40 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/93474fe0-e825-4191-b6f0-bc8f414674f9">

- **Consistent State**: Message $m_1$ is sent but not received.
- **Inconsistent State**: Process $P_2$ received message $m_2$, but $P_1$ does not show it as sent.

--- 

# Different Types of Messages:**


<img width="684" alt="Screenshot 2024-05-20 at 2 12 21 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/9747c5ee-ef48-479f-96ac-d093d1edf502">

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


# Issues in Failure Recovery:**
- Restoring the system to a consistent state involves rolling back processes to valid checkpoints.
- Orphan messages arise when a process receives a message after rolling back, causing inconsistencies.
- Rollback of other processes may be required to eliminate orphan messages, ensuring a consistent global state.
- Messages received during the failure may be in abnormal states, including delayed, lost, or orphaned.
- Differentiating between normal, delayed, lost, and orphan messages is crucial during recovery.
- Lost messages can be handled by logging and replaying sent messages during recovery but may lead to duplicates.
- Techniques for managing delayed, lost, and orphan messages are essential for effective failure recovery.


---

# Checkpointing in Distributed Systems

## Uncoordinated Checkpointing
- **Autonomy**: Processes decide independently when to take checkpoints, reducing runtime overhead.
- **Advantages**: 
  - No need for synchronization.
  - Processes can choose optimal times for checkpoints.
- **Disadvantages**:
  - Risk of the **domino effect**, causing extensive rollback and lost work.
  - Slow recovery, needing multiple iterations to find a consistent state.
  - Requires maintaining multiple checkpoints and periodic garbage collection.
  - Not suitable for frequent output commits due to the need for global coordination.

## Coordinated Checkpointing
- **Synchronization**: Processes coordinate checkpoints to form a consistent global state.
- **Advantages**:
  - Simplified recovery, avoiding the domino effect.
  - Only one checkpoint per process, reducing storage needs.
- **Disadvantages**:
  - High latency and overhead for each checkpoint.
  - Blocking communications during the checkpoint process can hinder performance.

### Blocking Coordinated Checkpointing
- **Process**:
  - Processes block communications, take checkpoints, and send acknowledgments.
  - Coordinator commits checkpoints and processes resume execution.
- **Issue**: Computation is blocked, leading to delays.

### Non-blocking Checkpoint Coordination
- **FIFO Channels**: Uses checkpoint requests to coordinate non-blocking checkpoints.
  - Example: Chandy-Lamport snapshot algorithm.
- **Non-FIFO Channels**: 
  - Uses piggybacked markers or checkpoint indices on messages.
- **Scalability**: 
  - Involves only processes that have communicated since the last checkpoint.



## Communication-Induced Checkpointing

### Overview
- **Purpose**: Avoid the domino effect while allowing some independent checkpointing.
- **Benefits**: Reduces or eliminates useless checkpoints.

### Types of Checkpoints
- **Autonomous Checkpoints**: Taken independently by processes.
- **Forced Checkpoints**: Taken due to communication and piggybacked information.

### Process
- **Piggybacking Information**: Protocol-related data is added to each application message.
- **Decision Making**: The receiver uses the piggybacked information to determine if a forced checkpoint is needed to maintain a consistent global state.
- **Forced Checkpoints**: Must be taken before processing the message content, adding some latency and overhead.

### Advantages
- **No Coordination Messages**: Unlike coordinated checkpointing, there are no special messages exchanged.
- **Reduced Overhead**: Focus on minimizing the number of forced checkpoints.

### Types of Communication-Induced Checkpointing
1. **Model-Based Checkpointing**:
   - **Goal**: Prevent patterns that lead to inconsistent states.
   - **Mechanism**: Processes detect potential inconsistencies and independently take forced checkpoints.
   - **Operation**: No control messages exchanged; decisions based on locally available information.

2. **Index-Based Checkpointing**:
   - **Goal**: Ensure checkpoints with the same index across processes form a consistent state.
   - **Mechanism**: Assigns monotonically increasing indexes to checkpoints.
   - **Piggybacking**: Indexes are piggybacked on messages to help receivers decide when to take a forced checkpoint.
   - **Protocol Example**: A process takes a checkpoint upon receiving a message with a higher index than its local checkpoint index.


## Log-based Rollback Recovery

Log-based rollback recovery leverages deterministic and non-deterministic events to maintain system consistency during failures.

### Deterministic and Non-deterministic Events
- **Non-deterministic Events**: Events like message receipts or internal events within a process.
- **Deterministic Events**: Regular execution intervals that can be logged.

### Logging and Recovery
- Processes log determinants of all non-deterministic events to stable storage during normal operation.
- On failure, systems use these logs to restore consistent states.

### No-Orphans Consistency Condition
- **Depend(e)**: Processes affected by a non-deterministic event $e$. According to lamport happened before relation
- **Log(e)**: Processes that have logged $e$'s determinant in volatile memory.
- **Stable(e)**: True if $e$'s determinant is logged in stable storage.
- **Always-No-Orphans Condition**: $\( \forall e: \neg \text{Stable}(e) \Rightarrow \text{Depend}(e) \subseteq \text{Log}(e) \)$.

### Pessimistic Logging
- Assumes failures can occur after any non-deterministic event.
- **Synchronous Logging**: $\( \forall e: \neg \text{Stable}(e) \Rightarrow \text{Depend}(e) = 0 \)$.
- Requires determinants to be logged immediately to prevent dependencies on unlogged events.
- <img width="561" alt="Screenshot 2024-05-20 at 2 14 59 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/350e8b55-35d5-4863-aebe-2ca22f7a417f">
- Suppose processes 𝑃1 and 𝑃2 fail as shown, restart from checkpoints B and
C, and roll forward using their determinant logs to deliver again the same
sequence of messages as in the pre-failure execution
- Once the recovery is complete, both processes will be consistent with the
state of 𝑃0 that includes the receipt of message 𝑚7 from 𝑃1

### Optimistic Logging
- Logs determinants asynchronously.
- <img width="704" alt="Screenshot 2024-05-20 at 2 15 34 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/cf0e0366-6f85-4f58-a305-19fc36359c2e">
- Assumes logging completes before failures occur.
- Does not enforce the always-no-orphans condition.
- Requires tracking causal dependencies and may need multiple checkpoints.

### Causal Logging

<img width="723" alt="Screenshot 2024-05-20 at 2 15 55 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/0d2b5cd4-70f7-4dcc-a944-b47b99f766c1">

- Merges advantages of both pessimistic and optimistic logging with a more complex recovery protocol.
- Allows independent output commits and prevents orphans.
- Ensures the always-no-orphans property.
- Maintains information on events that have causally affected a process's state.

Each logging strategy balances trade-offs between performance, complexity, and reliability to suit different system needs.
