# Consensus and Agreement Algorithms

### Problem Definition

Reaching agreement among processes in a distributed system is essential for many applications. This agreement is necessary for coordination and executing tasks that require a collective decision, such as the commit decision in database transactions. The challenge lies in designing algorithms that enable processes to agree under different system and failure models.

### Assumptions for Agreement Algorithms

1. #### **Failure Models**
   Failure models describe the ways in which processes can fail in a system. These models impact the design and feasibility of consensus algorithms.

   1. **Fail-Stop Model**: A process can crash unexpectedly during execution, halting its operations and possibly leaving messages unsent or partially sent.
   2. **Send/Receive Omission Model**: A process may fail to send or receive messages. For example, a message intended for a process may never be sent or may be lost in transit.
   3. **Byzantine Failure Model**: Processes may exhibit arbitrary or malicious behavior, including sending false information or tampering with messages..
   
2. **Communication Model**:
   Communication models determine how processes exchange messages and the timing guarantees of message delivery.

   - **Synchronous Systems**: Non-arrival of a message can be detected.
   - **Asynchronous Systems**: Non-arrival of a message is indistinguishable from a delay.
   
3. **Network Connectivity**: 
   - Full logical connectivity allows direct message passing between any two processes.
   
4. **Sender Identification**: 
   - The identity of the sender is always known, even if the message content is tampered with.

5. **Channel Reliability**: 
   - Channels are reliable; only processes may fail.
   
6. **Message Authentication**: 
   - The study assumes unauthenticated (oral) messages, where message integrity cannot be verified.

7. **Agreement Variable**: 
   - Simplifying assumption using boolean or multi-valued variables.

### Byzantine Generals Problem

A classic example of agreement in the presence of Byzantine faults involves generals planning an attack. 
Consider four generals surrounding a fort, needing to agree on a common time to attack. This scenario models:
- **Processes**: Generals
- **Messages**: Communication between generals
- **Failures**: One or more generals could be traitors (Byzantine faults) giving misleading information.

The challenge is to ensure that all loyal generals agree on the same attack time despite the presence of traitors. This example illustrates the difficulties of reaching consensus in distributed systems, especially under the Byzantine failure model.


### Key Points

- **Synchronous vs. Asynchronous Systems**: Detecting message failures is feasible in synchronous systems but not in asynchronous systems.
- **Failure Model Impact**: The type of failure model significantly affects the feasibility and complexity of consensus algorithms.
- **Message Authentication**: Authentication simplifies consensus by enabling detection of tampered messages.

Designing robust consensus algorithms requires addressing these complexities to ensure processes can reliably agree despite failures and potential malicious behavior.


---

# The Byzantine Agreement and Other Problems

### The Byzantine Agreement Problem

The Byzantine agreement problem is a fundamental challenge in distributed systems, where processes must reach a common decision despite the presence of faulty or malicious actors. This problem is defined formally to ensure that even in the presence of failures, the non-faulty processes can still reach agreement. Here are the key aspects of the Byzantine agreement problem:

1. **Source Process**: A designated process starts with an initial value.
2. **Agreement Condition**: All non-faulty processes must agree on the same value.
3. **Validity Condition**: If the source process is non-faulty, the value agreed upon by all non-faulty processes must match the source's initial value.
4. **Termination Condition**: Each non-faulty process must eventually decide on a value.

The validity condition ensures that the solution is meaningful and not trivial, such as agreeing on a constant value regardless of the source’s initial value. If the source is faulty, the correct processes may agree on any value, as the behavior of faulty processes is not relevant to the final decision.

### Other Flavors of Byzantine Agreement Problems

In addition to the Byzantine agreement problem, there are other related problems in distributed systems, such as the consensus problem and the interactive consistency problem.


### The Consensus Problem

The consensus problem is a variation where each process starts with its own initial value. The requirements are:

1. **Agreement**: All non-faulty processes must agree on the same value.
2. **Validity**: If all non-faulty processes have the same initial value, the agreed-upon value must be that initial value.
3. **Termination**: Each non-faulty process must eventually decide on a value.

### The Interactive Consistency Problem

The interactive consistency problem requires each process to start with an initial value, and all non-faulty processes must agree on a set of values, one for each process. The conditions are:

1. **Agreement**: All non-faulty processes must agree on the same array of values $A[v_1, \ldots, v_n]$.
2. **Validity**: If a non-faulty process $i$ has an initial value $v_i$, then $v_i$ must be the $i$-th element in the agreed array. If a process $j$ is faulty, the agreed value for $A[j]$ can be any value.
3. **Termination**: Each non-faulty process must eventually decide on the array $A$.

### Summary

- **Byzantine Agreement Problem**: Focuses on a single source process with an initial value and requires all non-faulty processes to agree on that value if the source is non-faulty. This includes agreement, validity, and termination conditions.
- **Consensus Problem**: Involves each process having its own initial value and requires all non-faulty processes to agree on a single value that is one of the initial values, especially if they all start with the same value.
- **Interactive Consistency Problem**: Requires each process to start with an initial value and for all non-faulty processes to agree on a set of values, one for each process. The conditions include agreement on the same array of values, validity where each process’s value is correctly reflected in the array if the process is non-faulty, and termination where each non-faulty process eventually decides on the array.

---


### Consensus Algorithm for Crash Failures (Synchronous System)

Algorithm 14.1 describes a consensus algorithm for $n$ processes, where up to $f$ processes ($f < n$) may fail in the fail-stop model. The consensus variable $x$ is integer-valued, and each process has an initial value $x_i$. The algorithm proceeds as follows:

1. **Rounds**: The algorithm runs for $f + 1$ rounds.
2. **Broadcast**: In each round, if a process has not broadcast its value $x$ before, it does so.
3. **Update**: Each process collects values received from others in the round and updates $x$ to the minimum of these values and its own current value.
4. **Consensus**: After $f + 1$ rounds, $x$ is guaranteed to be the consensus value.

### Algorithm

<img width="464" alt="Screenshot 2024-05-20 at 9 12 15 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/13119b83-04ec-468c-9c35-984916708715">


### Properties

- **Agreement**: All non-faulty processes agree on the same value because in at least one round, all processes that have not failed broadcast and receive the same minimum value.
- **Validity**: Processes do not send fictitious values, ensuring that the agreed value is legitimate.
- **Termination**: Each non-faulty process eventually decides on a value after $f + 1$ rounds.

### Complexity

- **Rounds**: $f + 1$ rounds.
- **Messages**: Up to $O(n^2)$ messages per round, resulting in a total of $O((f + 1) \cdot n^2)$ messages.

### Early-Stopping Variant

An early-stopping consensus algorithm can terminate sooner if fewer than $f$ failures occur, finishing in $f' + 1$ rounds where $f'$ is the actual number of failures.

### Lower Bound

At least $f + 1$ rounds are necessary because, in the worst case, one process may fail in each round. The additional round ensures at least one round without any failures, allowing reliable message delivery and computation of the consensus value.


---

# Distributed File System (DFS)

A distributed file system (DFS) is designed to manage files in a client/server architecture, providing efficient and secure file storage, retrieval, and management across a network. Key aspects of a DFS include:

### Key Components and Modules
1. **Directory Module**: Maps file names to file IDs.
2. **File Module**: Associates file IDs with specific files.
3. **Access Control Module**: Verifies permissions for requested operations.
4. **File Access Module**: Handles reading and writing of file data and attributes.
5. **Block Module**: Manages disk blocks, including access and allocation.
6. **Device Module**: Manages disk I/O operations and buffering.

### File Attributes
Files in a DFS contain both data and attributes. Attributes provide metadata about the file, such as:
- **File Length**
- **Creation, Read, Write, and Attribute Timestamps**
- **Reference Count**
- **Owner**
- **File Type**
- **Access Control List**

The shaded attributes are managed by the file system and typically not updated by user programs.

### Requirements for Distributed File Systems
1. **Transparency**:
   - **Access Transparency**: Uniform access to local and remote files.
   - **Location Transparency**: Uniform file name space, allowing file relocation without changing pathnames.
   - **Mobility Transparency**: Files can be moved without requiring changes to client programs or system administration tables.
   - **Performance Transparency**: Stable performance despite varying service loads.
   - **Scaling Transparency**: Incremental growth to handle increasing loads and network sizes.

2. **Concurrent File Updates**: Ensuring changes by one client do not interfere with other clients.

3. **File Replication**: Multiple copies of a file at different locations for load sharing and fault tolerance, often supported through caching.

4. **Heterogeneity**: Compatibility with different operating systems and hardware, ensuring openness.

5. **Fault Tolerance**: Continued operation despite client/server failures, often achieved through replication and robust communication semantics.

6. **Consistency**: One-copy update semantics ensuring uniform updates across all file replicas.

7. **Security**: Authentication, access control, digital signatures, and encryption to protect file access and data integrity.

8. **Efficiency**: Performance comparable to conventional file systems with powerful and general facilities.


---

