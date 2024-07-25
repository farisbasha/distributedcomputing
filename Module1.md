# Distributed Systems:

- Independent entities cooperating to solve big problems.
- Key points:
  1. Even if one entity fails, work can still happen.
  2. Entities communicate through messages over a network, not sharing memory or time.
  3. Appears as one big entity to users.
  4. it can be loosely coupled, or tight coupled depends upon the architecture

# Distributed Systems Features:

- No shared clock
- No Shared Memory
- Geographical separation.( we can run node in any country,city etc , distance doesn't matter )
- Autonomy and heterogeneity: Entities are loosely connected, with different speeds and operating systems, cooperating by offering services or solving problems together.

# Relation to Computer System Components:

<img width="400" alt="Screenshot 2024-03-11 at 7 32 33 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/ae93a6af-138e-4667-b616-2fa61f4f1762">

# Distributed Systems (Advantages):

- Resource sharing.
- Access to geographically remote data and resources.
- Enhanced reliability: Ensures availability, integrity, and fault-tolerance.
- Increased performance/cost ratio: Achieved through resource sharing and remote data access.
- Scalability.
- Modularity and incremental expandability.

# Primitives for Distributed Communication

**Blocking/Non-blocking, Synchronous/Asynchronous Primitives**:

- **Blocking**: Control returns after the operation completes.
  - Example: A blocking Send() or Receive() halts the process until the action is finished.
- **Non-blocking**: Control returns immediately after invocation, allowing the process to proceed without waiting for the operation to complete.
  - Example: A non-blocking Send() allows the process to continue while the data is still being transmitted.
- **Synchronous**: Send and Receive operations handshake, with each step waiting for the previous one to complete.

  - Example: A synchronous Send() completes only after the corresponding Receive() has been invoked and completed.
  - <img width="150" alt="Screenshot 2024-05-18 at 8 55 21 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/0e9b1301-30fe-40cb-967b-d1db84649a03">

- **Asynchronous**: Actions occur independently and do not wait for each other.
  - Example: An asynchronous Send() returns control to the process after the data is copied out of the user buffer, without waiting for the Receive() to complete.
  - <img width="158" alt="Screenshot 2024-05-18 at 8 55 33 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/571b008d-8260-47bb-bb54-fde3b31b9cb3">

**Buffered vs. Unbuffered Options**:

- **Buffered**:
  - Data is first copied from the user buffer to a kernel buffer, then to the network.
  - Buffered Receive ensures data has a storage place in the kernel if it arrives before the Receive() is invoked.
- **Unbuffered**:
  - Data is copied directly from the user buffer to the network.

**Send()**:

- Parameters: destination, user buffer.

**Receive()**:

- Parameters: source, user buffer.

**Completion Checking for Non-blocking Primitives**:

1. **Polling**: Continuously check if the handle is flagged.
2. **Wait Call**: Blocks until one of the handles is posted.

- **Wait Call Behavior**:
  - Blocks if the primitive hasn't completed.
  - Returns immediately if the primitive has completed.
  - Communication subsystem sets handle value and signals process on completion.

**Processor Synchrony**:

- In distributed systems, complete synchrony is impractical; synchronization occurs at larger code steps.
- **Barrier Synchronization**: Ensures no processor proceeds to the next code step until all have completed the previous one.

**Libraries and Standards**:

- Tools and guidelines to facilitate communication between distributed entities, ensuring consistency and understanding.
- Examples include MPI (Message Passing Interface) and PVM (Parallel Virtual Machine).

---

# Four versions of the Send primitive

1. ### Synchronous blocking (Send/Recv),
2. ### Synchronous non-blocking(Send/Recv),
3. ### Asynchronous blocking(Send),
4. ### Asynchronous non-blocking(Recieve)

    <img width="496" alt="Screenshot 2024-05-18 at 9 01 53 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/1e9b890d-4c78-41c6-8f50-9633db25e4be">

    <img width="478" alt="Screenshot 2024-05-18 at 9 02 18 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/c1525c08-89ba-4743-a50a-87256e069a1e">

# **Design Issues and Challenges:**

1. **Communication:**

   - Designing communication methods like remote procedure call (RPC) and message-oriented communication.

2. **Processes:**

   - Managing processes and threads, code migration, and software design.

3. **Naming:**

   - Creating robust naming schemes for locating resources and processes transparently.

4. **Synchronization:**

   - Ensuring coordination among processes, including mutual exclusion and clock synchronization.

5. **Data Storage and Access:**

   - Developing efficient data storage and access schemes across the network.

6. **Consistency and Replication:**

   - Replicating data for scalability and fast access while ensuring consistency.

7. **Fault Tolerance:**

   - Maintaining operation despite failures through resilience mechanisms.

8. **Security:**

   - Addressing security aspects like cryptography and access control.

9. **API and Transparency:**

   - Providing transparent access to system resources and locations.

   - **Transparency**:
     - Hides implementation details from users.
     - Types of transparency include:
       - **Access Transparency**: Uniform access to resources despite differences in data representation.
       - **Location Transparency**: Resources' locations are transparent to users.
       - **Migration Transparency**: Relocating resources without altering names.
       - **Relocation Transparency**: Relocating resources during access.
       - **Replication Transparency**: Users unaware of data replication.
       - **Concurrency Transparency**: Masks concurrent resource use.
       - **Failure Transparency**: Ensures reliability and fault tolerance.

10. **Scalability and Modularity:**
    - Distributing algorithms, data, and services for scalability using techniques like replication and caching.

# **Algorithmic Challenges in Distributed Computing:**

1. **Execution Models and Frameworks:**

   - Interleaving and partial order models for system executions and algorithm design.

2. **Dynamic Distributed Graph Algorithms:**
   - Modeling distributed systems as graphs and designing algorithms for communication, data dissemination, and object management.
3. **Time and Global State:**
   - Challenges in providing accurate physical time and logical time for distributed systems.
4. **Synchronization/Coordination Mechanisms:**
   - Essential for overcoming limited observation of system state and enabling impactful actions.
5. **Group Communication and Message Delivery:**
   - Efficient algorithms for dynamic group management and ordered message delivery.
6. **Monitoring Distributed Events and Predicates:**
   - On-line algorithms for monitoring global system state conditions.
7. **Program Design and Verification Tools:**
   - Designing tools for methodical program design and verification.
8. **Debugging Distributed Programs:**
   - Challenges in debugging due to concurrency and uncertainty.
9. **Data Replication, Consistency, and Caching:**
   - Managing replicated data for fast access while ensuring consistency and addressing placement challenges.

---

# Applications of Distributed Computing

**1. Mobile Systems**

- **Wireless Communication**: Shared broadcast medium.
- **Challenges**: Routing, location management, channel allocation, localization, mobility management.
- **Architectures**:
  - Base-station (cellular) approach
  - Ad-hoc network approach

**2. Sensor Networks**

- **Function**: Sense physical parameters.
- **Characteristics**: Mobile/static, wireless/wired communication.
- **Challenges**: Self-configuration, position and time estimation.

**3. Ubiquitous or Pervasive Computing**

- **Examples**: Intelligent homes, smart workplaces.
- **Features**: Self-organizing, network-centric, resource-constrained, connected to powerful networks.

**4. Peer-to-Peer (P2P) Computing**

- **Definition**: Peer-level interactions without hierarchy.
- **Characteristics**: Self-organizing, no central directories.

**5. Publish-Subscribe, Content Distribution, and Multimedia**

- **Dynamic Info Distribution**: Efficient publishing, subscribing, and filtering.
- **Multimedia Challenges**: Large data sizes, compression, synchronization.

**6. Distributed Agents**

- **Functions**: Collect, process, exchange information.
- **Interactions**: Cooperative or competitive.
- **Challenges**: Coordination, mobility control, software design.

**7. Distributed Data Mining**

- **Scenarios**: Private, sensitive data (banking); massive datasets (weather prediction).
- **Challenges**: Data cannot be centralized; needs distributed processing.

**8. Grid Computing**

- **Definition**: Virtual supercomputer using network-connected machines.
- **Challenges**: Job scheduling, quality of service, security.

**9. Security in Distributed Systems**

- **Challenges**: Confidentiality, authentication, availability.
- **Goal**: Efficient, scalable security solutions.

---

# Model of Distributed Computations

**Overview**:

- **Structure**: Set of processors connected by a communication network.
- **Communication**: Via message passing; no shared global memory.
- **Delay**: Finite but unpredictable communication delays.
- **Clock**: No global clock; processes have no instantaneous clock access.
- **Challenges**: Out-of-order messages, message loss, duplication, processor/link failures.
- **Graph Model**: Processes are vertices; communication channels are directed edges.

**Distributed Programs**:

- **Composition**: Consist of `n` asynchronous processes (p1, p2, ..., pn) on different processors.
- **Channels**: `Cij` is the channel from process `pi` to `pj`.
- **Messages**: `mij` denotes a message from `pi` to `pj`.
- **Execution**: Asynchronous, with processes executing actions and sending messages independently.

**Global State**:

- **Components**: States of processes and communication channels.
- **Process State**: Defined by local memory state.
- **Channel State**: Defined by messages in transit.

**Execution Model**:

- **Process Execution**: Sequential execution of actions.
- **Types of Events**:
  - Internal events
  - Message send events
  - Message receive events
- **Event Effects**: Change states of processes and channels.
  - Internal event: Changes state of the process.
  - Send/Receive event: Changes state of both the process and the channel.

**Event Ordering**:

- Events at a process are linearly ordered by occurrence.

<img width="398" alt="Screenshot 2024-05-18 at 9 09 06 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7e79c901-a339-489c-9dcf-4bb7e539d536">

---

# Causal Precedence Relation in Distributed Computations

**Execution and Events**:

- Distributed application execution produces a set of events `H = ∪i hi`.
- <img width="602" alt="Screenshot 2024-05-18 at 9 11 53 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/534ad231-b0ee-467b-ba2d-f3e08164488e">
- **Causal Dependencies**: Defined by a binary relation `→` on `H`.

**Causal Precedence (→)**:

- **Irreflexive Partial Order**: Induces a partial order on events.
- **Lamport's "Happens Before"**: If `ei → ej`, then `ej` depends on `ei`.
  - **Direct/Transitive Dependency**: `ej` is dependent on `ei` through a path of messages and process executions.
  - **Flow of Information**: All information at `ei` is accessible at `ej`.

**Implications of Causal Precedence**:

- If `ei → ej`, then `ei` affects `ej`, and `ej` is aware of `ei`.
- If `ei` and `ej` are not related by `→`:
  - `ej` does not depend on `ei`.
  - `ej` is not aware of `ei` or subsequent events on the same process.

**Concurrency (||)**:

- **Definition**: If `ei → ej` and `ej → ei`, then `ei` and `ej` are concurrent, denoted as `ei || ej`.
- **Non-transitivity**: `||` is not a transitive relation.
- **Event Relationship**: For any events `ei` and `ej`:
  - Either `ei → ej`
  - Or `ej → ei`
  - Or `ei || ej`

---

# Logical vs. Physical Concurrency in Distributed Computations

**Logical Concurrency**:

- **Definition**: Two events are logically concurrent if they do not causally affect each other.
- **Independence**: These events can occur independently without influencing each other's outcome.
- **Example**: Events `ei` and `ej` are logically concurrent if neither `ei → ej` (ei happens before ej) nor `ej → ei` (ej happens before ei). This means `ei` does not happen before `ej` and `ej` does not happen before `ei`.

**Physical Concurrency**:

- **Definition**: Events occur at the same instant in physical time.
- **Synchronization**: Actual simultaneous occurrence in the physical timeline.

**Distinguishing Features**:

- **Temporal Independence**: Logically concurrent events do not have to occur simultaneously in physical time.
- **Outcome Consistency**: The outcome of computation is unaffected by whether logically concurrent events happen at the same physical instant or not.
- **Processor Speed and Delays**: Variations in processor speed and message delays could make logically concurrent events overlap in physical time.

**Key Point**:

- Logically concurrent events remain consistent in their outcome regardless of their physical timing, highlighting their independence in logical ordering.

---

# Models of Communication Networks

**FIFO (First-In, First-Out)**:

- **Description**: Channels act as queues, preserving message order.
- **Property**: Messages are received in the order they were sent.

**Non-FIFO**:

- **Description**: Channels act like sets, where messages can be received in any order.
- **Property**: Message order is not preserved.

**Causal Ordering**:

- **Description**: Based on Lamport’s “happens before” relation.
- **Property**: Causally related messages are delivered in an order consistent with their causal relationships.
- **Usefulness**:
  - Simplifies distributed algorithm design by providing built-in synchronization.
  - Ensures consistency in replicated database systems by maintaining the order of updates.

**Examples**:

- **Replicated Databases**: Ensures all replicas receive updates in the same order, maintaining consistency without extra checks.

---

# Global State of a Distributed System

**Definition**:

- **Global State**: Collection of local states of processes and communication channels.
- **Local State**: Defined by process internals; channel state by message transit.
- **State Transitions**: Events change process and channel states.
- <img width="584" alt="Screenshot 2024-05-18 at 9 23 54 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/875408cf-6f20-4d26-acb5-4cfced609940">

**Consistency**:

- **Consistent Global State**: All received messages are recorded as sent, maintaining causality.
- **Inconsistent Global State**: Violation of causality; some messages received without being sent.

# Cuts in a Distributed Computation

**Definition**:

- **Cut**: Zigzag line joining arbitrary points on each process line in a space–time diagram.
- **PAST and FUTURE**: Divides events into PAST (left) and FUTURE (right) of the cut.
- **Notation**: $\( PAST(C) \)$ and $\( FUTURE(C) \)$ denote events in the PAST and FUTURE of cut $\( C \)$.
- **Global State Representation**: Every cut corresponds to a global state graphically.

**Consistency**:

- **Consistent Cut**: Every received message in the PAST was sent in the PAST.
  - All messages crossing from PAST to FUTURE are in transit.
- **Inconsistent Cut**: Message crosses from FUTURE to PAST, violating causality.

**Example**:

 <img width="737" alt="Screenshot 2024-05-18 at 9 28 28 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7f81e0a9-7534-4b11-bf98-5edee9a20349">
 
- **C1**: Inconsistent cut.
- **C2**: Consistent cut.

**Past and Future Cones**:

- **Definition**: Events in PAST of $\( e_j \)$ are those that causally affect $\( e_j \)$.
- **Notation**: $\( Past(e_j) \)$ denotes events in the past of $\( e_j \)$ in a computation $\( (H, \rightarrow) \)$.
- <img width="721" alt="Screenshot 2024-05-18 at 9 28 50 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/99fa1adf-f118-4eb8-8d46-ddc93bc7a3bd">

---
