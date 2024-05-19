# Logical Clocks

A system of logical clocks provides a way to order events in a distributed system based on their causal relationships. The system consists of:
- **Time Domain $\( T \)$**: A partially ordered set of timestamps.
- **Logical Clock $\( C \)$**: A function mapping each event $\( e \)$ to a timestamp in $\( T \)$, denoted $\( C(e) \)$.

The key property of this system is the clock consistency condition:
- If event $\( e_i \)$ causally precedes event $\( e_j \)$ (denoted $\( e_i \rightarrow e_j \)$ ), then $\( C(e_i) < C(e_j) \)$.

When this condition holds in both directions, meaning $\( e_i \rightarrow e_j \iff C(e_i) < C(e_j) \)$, the system is said to be strongly consistent.

### Implementation of Logical Clocks

To implement logical clocks, each process in the system maintains:
- **Local Logical Clock $(\( lc_i \))$**: Tracks the progress of the process.
- **Global Logical Clock $(\( gc_i \))$**: Reflects the process's view of the global time.

The implementation follows two rules:
- **R1**: Update the local clock before executing any event (send, receive, or internal).
- **R2**: Update the global clock upon receiving a message, using the timestamp from the sender to ensure global consistency.

### Scalar Time

Proposed by Lamport in 1978, scalar time aims to totally order events in a distributed system:
- **Time Domain**: Set of non-negative integers.
- **Local Clock $(\( C_i \))$**: An integer representing both the local and global logical time.

#### Rules:
- **R1**: Before any event, increment $\( C_i \)$ by a positive value $\( d \)$ (typically $\( d = 1 \)$ ).
- **R2**: On message receipt, update $\( C_i \)$ to the maximum of its current value and the received timestamp, then increment $\( C_i \)$.

#### Properties:
- **Consistency**: Ensures $\( e_i \rightarrow e_j \implies C(e_i) < C(e_j) \)$.
- **Total Ordering**: Orders events with identical timestamps using process identifiers.
- **Event Counting**: When $\( d = 1 \)$, the timestamp indicates the number of events preceding the current event.
- **No Strong Consistency**: Scalar clocks do not guarantee $\( C(e_i) < C(e_j) \implies e_i \rightarrow e_j \)$.

### Vector Time

Developed by Fidge, Mattern, and Schmuck, vector time uses vectors to maintain more detailed causality information:
- **Time Domain**: Set of $\( n \)$-dimensional non-negative integer vectors.
- **Vector Clock ($\( vt_i \)$ )**: Each process $\( p_i \)$ maintains a vector where $\( vt_i[i] \)$ is its local logical clock, and $\( vt_i[j] \)$ represents its knowledge of $\( p_j \)$'s logical time.

#### Rules:
- **R1**: Increment the local logical time $\( vt_i[i] \)$ before executing an event.
- **R2**: On message receipt, update the vector clock element-wise to the maximum of the current and received vectors, then increment the local logical time.

#### Properties:
- **Isomorphism**: There is a direct mapping between event causality and vector timestamps: $\( e_i \rightarrow e_j \iff vt_i < vt_j \)$.
- **Strong Consistency**: Allows determination of causal relationships between events by comparing their vector timestamps.
- **Event Counting**: With $\( d = 1 \)$, the vector's \$( i \)$-th component counts the number of events at $\( p_i \)$, and the sum of all components minus one gives the total number of causally preceding events.

### Conclusion

Logical clocks, whether scalar or vector, are essential for managing the order and causality of events in distributed systems. Scalar clocks provide a simple total ordering mechanism, while vector clocks offer a detailed and strongly consistent representation of causality. Each approach has its specific use cases and benefits in the context of distributed computing.

---

# Election Algorithms

#### General Concept
- **Coordinator**: A unique process designated to manage tasks and resources.
- **Priority Number**: Each process has a unique priority. The process with the highest priority becomes the coordinator.

#### Bully Algorithm
- **Messages**:
  - **Election Message**: Announces an election.
  - **OK Message**: Responds to an election message.
  - **Coordinator Message**: Announces the new coordinator.

**Steps**:
1. A process starts an election by sending an election message to higher priority processes.
2. If no OK messages are received within time \( T \), it declares itself the coordinator and sends a coordinator message to lower priority processes.
3. If it receives OK messages, higher priority processes continue the election.
4. If the coordinator fails to respond within \( T \), it is considered failed.
5. The initiating process sends an election message to higher priority processes and waits for responses.
6. If no responses, it elects itself as coordinator and informs lower priority processes.
7. A recovered process with higher priority can become the new coordinator.
8. The process with the highest priority always wins, hence "bully algorithm."

#### Ring-Based Election Algorithm
- **System Structure**: Processes are arranged in a logical ring with unidirectional communication.

**Steps**:
1. Any process starts an election by marking itself as a participant and sending an election message with its identifier to its clockwise neighbor.
2. Each process compares the received identifier with its own.
   - If the received identifier is higher, it forwards the message.
   - If lower, it replaces the identifier with its own and forwards it.
3. When a process receives its own identifier, it declares itself as the coordinator.
4. The coordinator sends an elected message to inform all processes.

Both algorithms ensure that a unique coordinator is elected to maintain system coordination.



---

# System Model

1. **Processes and Channels:**
   - The system consists of `n` processes \( p_1, p_2, \ldots, p_n \) connected by communication channels.
   - There is no shared memory or global clock; processes communicate via message passing.
   - \( C_{ij} \) denotes the channel from process \( p_i \) to \( p_j \), with its state denoted by \( SC_{ij} \).

2. **Events:**
   - Processes perform internal events, message send events, and message receive events.
   - For a message \( m_{ij} \) sent from \( p_i \) to \( p_j \), \( \text{send}(m_{ij}) \) and \( \text{rec}(m_{ij}) \) denote its send and receive events.

3. **Process State:**
   - The state of process \( p_i \) at any instant, denoted by \( LS_i \), results from the sequence of events executed by \( p_i \).
   - An event \( e \) is part of \( LS_i \) if it is in the sequence leading to \( LS_i \).

4. **Channel State:**
   - For a channel \( C_{ij} \), the set of messages in transit is defined as \( \text{transit}(LS_i, LS_j) = \{ m_{ij} \mid \text{send}(m_{ij}) \in LS_i \land \text{rec}(m_{ij}) \notin LS_j \} \).

5. **Models of Communication:**
   - **FIFO Model:** Channels act as first-in-first-out queues, preserving message order.
   - **Non-FIFO Model:** Channels are sets where messages are added and removed in random order.
   - **Causal Delivery:** Ensures that if \( \text{send}(m_{ij}) \rightarrow \text{send}(m_{kj}) \), then \( \text{rec}(m_{ij}) \rightarrow \text{rec}(m_{kj}) \).

### Consistent Global State

- The global state \( GS \) of a distributed system is a collection of local states of processes and channels: \( GS = \{ \bigcup_i LS_i , \bigcup_{i,j} SC_{ij} \} \).

- **Consistency Conditions:**
  - **C1:** If \( \text{send}(m_{ij}) \in LS_i \), then \( m_{ij} \) must be in \( SC_{ij} \) or \( \text{rec}(m_{ij}) \in LS_j \).
  - **C2:** If \( \text{send}(m_{ij}) \notin LS_i \), then \( m_{ij} \) is neither in \( SC_{ij} \) nor \( \text{rec}(m_{ij}) \in LS_j \).

- **Interpretation in Terms of Cuts:**
  - A cut in a space-time diagram divides it into past and future.
  - A consistent global state corresponds to a cut where every message received in the past was also sent in the past, known as a consistent cut.

### Issues in Recording a Global State

- **No Global Physical Clock:** A distributed system lacks a global clock, creating two main issues:
  1. **Distinguishing Messages:**
     - Messages sent before recording a snapshot must be included in the snapshot.
     - Messages sent after recording a snapshot must not be included.
  2. **Timing of Snapshots:**
     - A process must record its snapshot before processing a message sent by another process after that process recorded its snapshot.

In summary, the system model describes processes and channels, event types, and how process states are determined. A consistent global state ensures that the state of channels and processes are in sync according to specific conditions. Recording a consistent global state involves addressing challenges due to the lack of a global clock.

---

### Chandy-Lamport Algorithm for Distributed Snapshot

**Overview:**
- The Chandy-Lamport algorithm is designed to capture a consistent global state in a distributed system using control messages called markers.
- This algorithm works in systems with FIFO (First-In-First-Out) communication channels.

### **Algorithm**
**Marker Sending Rule for Process \( p_i \):**
1. Process \( p_i \) records its local state.
2. For each outgoing channel \( C \) that hasn't received a marker yet, \( p_i \) sends a marker along \( C \) before sending any further messages on \( C \).

**Marker Receiving Rule for Process \( p_j \):**
- Upon receiving a marker along channel \( C \):
  - If \( p_j \) has not yet recorded its state:
    - \( p_j \) records the state of \( C \) as empty.
    - \( p_j \) then records its own state and follows the Marker Sending Rule.
  - If \( p_j \) has already recorded its state:
    - \( p_j \) records the state of \( C \) as the set of messages received after \( p_j \) recorded its state and before receiving the marker.


### **Operation of the Algorithm:**
- The algorithm can be initiated by any process, which records its state and sends markers on all its outgoing channels.
- Each process, upon receiving a marker for the first time, records its state and the state of the incoming channel as described.
- The algorithm terminates when each process has received a marker on all of its incoming channels.
- After termination, all local snapshots are disseminated to all processes, allowing them to determine the global state.

**Correctness:**
- **Condition C2:** The FIFO property ensures that no messages sent after a marker are included in the channel state.
- **Condition C1:** 
  - If a process \( p_j \) receives a message \( m_{ij} \) that precedes a marker, and it hasn't taken its snapshot, \( p_j \) includes \( m_{ij} \) in its snapshot.
  - If \( p_j \) has already taken its snapshot, \( m_{ij} \) is recorded in the state of the channel \( C_{ij} \).

**Complexity:**
- **Messages:** The algorithm requires \( O(e) \) messages, where \( e \) is the number of edges in the network.
- **Time:** The algorithm requires \( O(d) \) time, where \( d \) is the diameter of the network.

**Summary:**
The Chandy-Lamport algorithm effectively captures a consistent global snapshot in a distributed system by ensuring that each process records its state and the state of its incoming channels accurately, maintaining consistency according to the FIFO channel properties. This algorithm efficiently handles the challenges of distributed systems where there is no global clock or shared memory.


---

# Termination Detection
A fundamental problem in distributed systems is to determine if a distributed computation has terminated.

## Termination Detection by Weight Throwing

#### Basic Idea:
- A process called the **controlling agent** oversees the computation.
- Initially, all processes are idle with zero weight, and the controlling agent has a weight of 1.
- The computation starts when the controlling agent sends a basic message to one of the processes.

**Key Points:**
- Each active process and each message in transit are assigned a non-zero weight \( W \) such that \( 0 < W \leq 1 \).
- The total sum of weights across all active processes and messages in transit is always 1.
- When a process sends a message, it includes part of its weight with the message.
- When a process receives a message, it adds the weight from the message to its own weight.
- When a process becomes passive (idle), it sends its weight back to the controlling agent through a control message.
- The controlling agent detects termination when its weight reaches 1.

**Notation:**
- \( W \): Weight on a process or controlling agent.
- \( B(DW) \): A basic message \( B \) sent as part of the computation, with \( DW \) being the weight assigned to it.
- \( C(DW) \): A control message \( C \) sent from a process to the controlling agent, with \( DW \) being the weight assigned to it.

#### Rules of the Algorithm:

1. **Rule 1 (Weight Splitting and Sending):**
   - An active process or the controlling agent sends a basic message to a process \( P \) by splitting its weight \( W \) into \( W_1 \) and \( W_2 \) such that \( W_1 + W_2 = W \) and \( W_1 > 0 \), \( W_2 > 0 \).
   - It assigns its weight to \( W_1 \) and sends \( B(DW := W_2) \) to \( P \).

2. **Rule 2 (Weight Addition on Receipt):**
   - On receiving the basic message \( B(DW) \), process \( P \) adds \( DW \) to its weight \( W \) (i.e., \( W := W + DW \)).
   - If \( P \) was idle, it becomes active.

3. **Rule 3 (Becoming Idle and Sending Control Message):**
   - A process switches from the active state to the idle state by sending a control message \( C(DW := W) \) to the controlling agent and sets its weight \( W := 0 \).

4. **Rule 4 (Controlling Agent Receiving Control Message):**
   - Upon receiving a control message \( C(DW) \), the controlling agent adds \( DW \) to its weight \( W \) (i.e., \( W := W + DW \)).
   - If \( W = 1 \), the controlling agent concludes that the computation has terminated.

#### Correctness of the Algorithm:

- **Sets and Weights:**
  - \( A \): Set of weights on all active processes.
  - \( B \): Set of weights on all basic messages in transit.
  - \( C \): Set of weights on all control messages in transit.
  - \( W_c \): Weight on the controlling agent.

- **Invariant:** The sum of weights in \( A \), \( B \), \( C \), and \( W_c \) is always 1.

#### Characteristics:
- **Termination Detection:** The controlling agent determines termination when it regains the full weight of 1.
- **No Freezing:** The algorithm does not indefinitely delay the underlying computation.
- **No New Channels:** The algorithm does not require the addition of new communication channels.

By maintaining the sum of weights and ensuring proper weight transfer, the algorithm accurately detects the global termination of a distributed computation.


## Termination Detection Using Distributed Snapshots

#### Informal Description:
- **Key Idea**: The last process to become idle triggers the detection of termination.
- **Process Idle Transition**: When a process becomes idle, it sends a snapshot request to all other processes.
- **Snapshot Agreement**: Processes agree to take a snapshot if they acknowledge the requester became idle before them.
- **Global Snapshot**: A request is successful if all processes take a snapshot, indicating the global state and computation termination.

#### Formal Description:
- **Logical Clocks**: Each process has a logical clock \(x\), starting at zero and incrementing each time the process becomes idle.
- **Message Types**:
  - **Basic Message**: \(B(x)\) - sent during regular operations.
  - **Control Message**: \(R(x, i)\) - sent to request snapshots.
- **Clock Synchronization**: Processes update their clocks to the maximum value seen.
- **Variable \(k\)**: Tracks the maximum \((x, k)\) values.

#### Algorithm Rules:
1. **R1**: Active process \(i\) sends \(B(x)\) to process \(j\).
2. **R2**: Upon receiving \(B(x')\), process \(i\) increments \(x\) and activates if idle.
3. **R3**: When process \(i\) goes idle, it increments \(x\), sets \(k = i\), sends \(R(x, k)\) to all, and takes a local snapshot.
4. **R4**: Upon receiving \(R(x', k')\):
   - If \((x', k') > (x, k)\) and idle, update \((x, k)\) and take a snapshot.
   - If \((x', k') \leq (x, k)\) and idle, do nothing.
   - If active, update \(x\) to \(\max(x', x)\).

The last process to become idle has the largest clock value, ensuring that all processes take a snapshot for it, confirming computation termination.



## Spanning-Tree-Based Termination Detection Algorithm

- **Structure:**
  - \( N \) processes \( P_i \) (\(0 \leq i \leq N\)) are nodes in a connected undirected graph.
  - A fixed spanning tree with \( P_0 \) as the root handles termination detection.

- **Termination Detection:**
  - **Leaf Nodes:** Report termination to their parents.
  - **Parent Nodes:** Report to their parents after all children and themselves have terminated.
  - **Root Node:** Concludes termination when it and all its children are idle.

- **Signal Waves:**
  - **First Wave (Inward):** Tokens from leaves to root indicate idle subtrees.
  - **Second Wave (Outward):** If no termination, the root sends repeat signals outward.

- **Token Propagation:**
  - **Initial Tokens:** Given to leaf processes.
  - **Reporting:** Idle leaf processes send tokens to parents, indicating idle subtrees.
  - **Subtree Confirmation:** Tokens propagate to root, signaling idle states.

- **Challenges and Solution:**
  - **Reactivation Issue:** Processes might become active again after sending a token.
  - **Coloring Method:** Use white and black colors to track message passing and reactivation.
    - **Black Token:** Indicates message passing in the subtree.
    - **Repeat Signals:** Root restarts the detection if a black token is received.

- **Performance:**
  - **Best Case:** \( O(N) \) messages.
  - **Worst Case:** \( O(N \times M) \) messages, where \( M \) is the number of computation messages.
