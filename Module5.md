# Consensus and Agreement Algorithms

### Problem Definition
Reaching agreement among processes in a distributed system is essential for many applications. This agreement is necessary for coordination and executing tasks that require a collective decision, such as the commit decision in database transactions. The challenge lies in designing algorithms that enable processes to agree under different system and failure models.

### Assumptions for Agreement Algorithms

1. #### **Failure Models**
   Failure models describe the ways in which processes can fail in a system. These models impact the design and feasibility of consensus algorithms.

   1. **Fail-Stop Model**: A process can crash unexpectedly during execution, halting its operations and possibly leaving messages unsent or partially sent.
   2. **Send/Receive Omission Model**: A process may fail to send or receive messages. For example, a message intended for a process may never be sent or may be lost in transit.
   3. **Byzantine Failure Model**: Processes may exhibit arbitrary or malicious behavior, including sending false information or tampering with messages..
   
2. #### **Communication Model**:
   Communication models determine how processes exchange messages and the timing guarantees of message delivery.

   - **Synchronous Systems**: Non-arrival of a message can be detected.
   - **Asynchronous Systems**: Non-arrival of a message is indistinguishable from a delay.
   

3. #### Network Connectivity
- **Full Logical Connectivity**: Every process can communicate directly with any other process in the system. This assumption simplifies the design of consensus algorithms by ensuring that message paths are always available.

4. #### Sender Identification
- **Known Sender**: A process that receives a message always knows the identity of the sender. Even if a message is tampered with, the recipient can identify who sent it. This helps in detecting and handling malicious behavior.

5. #### Channel Reliability
- **Reliable Channels**: Assumes that the communication channels themselves do not fail or lose messages; only the processes can fail. This assumption focuses the complexity of the consensus problem on process failures rather than communication failures.

6. #### Message Authentication
- **Unauthenticated Messages**: The study assumes that messages are not authenticated. This means that:
  - A faulty process can forge messages and attribute them to other processes.
- **Authenticated Messages**: Using techniques like digital signatures, message authentication makes it easier to detect forgeries and tampering, reducing the potential damage caused by faulty processes.

7. #### Agreement Variable
- **Boolean or Multi-valued**: The variable on which agreement is reached can be either boolean (e.g., true/false) or have multiple values. For simplicity, some algorithms are presented using a boolean variable.


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

# File Service Architecture

In a distributed file service architecture, clear separation of concerns is achieved by structuring the system into three components: the client module, the directory service, and the flat file service.

<img width="435" alt="Screenshot 2024-05-20 at 10 22 43 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/fae267e7-9e20-4cc6-861c-853415a9c2ab">


1. **Client Module**:
   - **Role**: This component operates on each client computer.
   - **Functionality**: It integrates and extends the operations of the flat file service and the directory service, providing a unified application programming interface (API) to user-level programs.
   
2. **Directory Service**:
   - **Role**: Provides a mapping between human-readable file names and unique file identifiers (UFIDs).
   - **Functionality**: Clients use the directory service to obtain a file's UFID by providing its text name. Access control checks are performed during this mapping to ensure proper permissions.

3. **Flat File Service**:
   - **Role**: Manages operations related to the actual content of files.
   - **Functionality**: Uses UFIDs to reference files in all operations. It generates a new UFID when a file is created and returns it to the requester. UFIDs are unique across the distributed system.
   - Flat File Service Model Operations:
      - Read(FileId, i, n) -> Data: Reads up to n items from a file starting at item ‘i’ and returns it in Data.
      - Write(FileId, i, Data): Write a sequence of Data to a file, starting at item I and extending the file if necessary.
      - Create() -> FileId: Creates a new file with length 0 and assigns it a UFID.
      - Delete(FileId): The file is removed from the file store.
      - GetAttributes(FileId) -> Attr: Returns the file’s file characteristics.
      - SetAttributes(FileId, Attr): Sets the attributes of the file.

### Access Control

- **Mechanism**: An access check is conducted whenever a file name is converted to a UFID. Each client request includes a user identity, and the server performs access checks for every file operation to ensure security.

### File Groups

- **Definition**: A file group is a collection of files located on a specific server.
- **Characteristics**:
  - Servers may hold multiple file groups.
  - File groups can be moved between servers, although individual files cannot change groups.
  - They facilitate the allocation of files across different servers.
- **Identifier Uniqueness**: File group identifiers must be unique across the distributed system. This is achieved by generating identifiers using a combination of a 32-bit IP address and a 16-bit date-derived integer, ensuring global uniqueness even if systems merge.


---

# Sun Network File System (NFS)

**Overview**:
- **NFS**: A distributed file system protocol developed by Sun Microsystems.
- **Function**: Allows users on client computers to access files on remote servers using remote procedure calls (RPC).

<img width="480" alt="Screenshot 2024-05-20 at 10 29 13 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7f3ca9c8-2841-45c2-94b4-8703da6a0a7e">


**Virtual File System (VFS)**:
- **VFS Module**: Added to the UNIX kernel to manage both local and remote files.
- **File Handles**: Identifiers used in NFS.
- **Components**:
  - **File System Identifier**: Unique number for each file system.
  - **i-node Number**: Locates files and stores attributes; reused after file deletion.
  - **i-node Generation Number**: Incremented with each reuse of i-node numbers.

**Client Integration**:
- **NFS Client Module**: Works with the VFS on client machines.
- **Local Files**: Accessed through the UNIX file system.
- **Remote Files**: Accessed via NFS requests to the server.
- **Operation**: Similar to the UNIX file system, including caching file blocks in local memory.

**Access Control and Authentication**:
- **Stateless Server**: The server doesn't keep files open for clients.
- **Access Check**: Performed on each request to verify user permissions.

**NFS Server Interface**:
- **Unified Service**: Handles file and directory operations.
- **Create Operation**: Combines file creation and insertion into directories.

**Mount Services**:
- **Function**: Makes remote directories accessible locally.
- **Syntax**: `mount(remotehost, remotedirectory, localdirectory)`
- **Types**:
  - **Soft Mount**: Time-bound, failure message if the request times out.
  - **Hard Mount**: No time limit, retries until successful.
  - **Auto Mount**: Performed on demand.

**Server Caching**:
- **Read Operations**: No consistency issues.
- **Write Operations**:
  - **Write-through Caching**: Data written to disk before replying to client.
  - **Commit Operation**: Data stored in memory, written to disk upon commit.

**Client Caching**:
- **Operations Cached**: Read, write, getattr, lookup, readdir.
- **Validation**: Timestamp-based method with two timestamps:
  - **Tc**: Last validation time.
  - **Tm**: Last modification time on the server.

---

# Andrew File System (AFS)

<img width="515" alt="Screenshot 2024-05-20 at 10 40 12 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/7636274f-47b6-468b-970a-a53596bd752c">

**Overview**:
- **AFS**: A distributed file system that uses trusted servers to manage file access.
- **Local Cache**: Enhances performance by reducing server load and speeding up data access.

**Implementation**:
- **Components**: Two software components, Vice and Venus.
  - **Vice**: Server software running as a user-level UNIX process on server machines.
  - **Venus**: Client software running as a user-level process on client machines.

**File Handling**:
- **Volumes**: Collections of files managed by Vice on a server.
- **File Types**:
  - **Local Files**: Handled by the local UNIX file system.
  - **Shared Files**: Stored on servers and cached on local disks of workstations.

**Operation**:
1. **First Request**: Data is fetched from the server and cached locally.
2. **Subsequent Requests**: Data is served from the local cache.

**Stateful Servers**:
- **Callbacks**: Servers notify clients of file updates via callback promises.
  - **Callback Promise**: A guarantee issued by the server to inform the client of any modifications.
  - **Token States**: Valid or cancelled.
  - **Validation**: Tokens are checked for validity before accessing cached files.

**Cache Consistency**:
- **Callback Mechanism**:
  - When a file is modified, the server sends callbacks to all clients with a copy.
  - Clients mark the token as cancelled upon receiving a callback.
- **Revalidation**:
  - On restart, Venus requests cache validation from the server.
  - Valid tokens are reinstated if the timestamp is current; otherwise, tokens are cancelled.

**Design Characteristics**:
- **Whole-file Serving**: Entire files or large chunks are sent to clients.
- **Whole-file Caching**: Files are cached locally and persist across reboots.

**File Operations in AFS**:
1. **Open System Call**: 
   - If the file is not in the local cache, a request is sent to the server.
   - The file is copied to the local UNIX file system and opened.
2. **Subsequent Operations**: 
   - Performed on the local copy.
3. **Close System Call**: 
   - If the file is modified, the local changes are sent back to the server.
   - The server updates the file and its timestamps.
   - The local copy remains in the cache for future use.
  
---

### Google File System (GFS)

<img width="625" alt="Screenshot 2024-05-20 at 10 44 11 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/a624118b-7d4a-4f7a-b7b2-9f56e9de534c">

**Overview**:
- **Developed by Google Inc.**: To meet growing data processing needs.
- **Purpose**: Fault tolerance, dependability, scalability, availability, and performance.
- **Components**: Built from inexpensive commodity hardware.

**Structure**:
- **Node Cluster**: Includes a single master and several chunk servers.
- **Chunk Servers**: Store data in large 64 MB chunks, replicated at least three times.
- **Clients**: Access chunk servers directly for data, reducing network overhead.

**Master Server**:
- **Responsibilities**:
  - Manages metadata, including namespace, access control, and data mapping.
  - Communicates with chunk servers via heartbeat messages to monitor status.
- **Operation Log**: Keeps a record of cluster activities.
- **Metadata**: Tracks chunk locations and their corresponding files.

**Chunk Servers**:
- **Function**: Store file chunks as Linux files.
- **Replication**: Each chunk is stored on multiple servers for fault tolerance.
- **Direct Access**: Clients interact directly with chunk servers to fetch data.

**Scalability**:
- **Cluster Size**: Can exceed 1,000 nodes and 300 TB of storage, accessed by hundreds of clients simultaneously.

**Features**:
- **Namespace Management**: Organizes files hierarchically with path names.
- **Fault Tolerance**: Replicates data across multiple servers.
- **Efficient Recovery**: Automated and efficient data recovery processes.
- **High Availability**: Large chunk size reduces client-master interactions.
- **High Throughput**: Supports concurrent operations on many nodes.
- **Critical Data Replication**: Ensures data is accessible even during node failures.

**Advantages**:
- **High Accessibility**: Data remains available despite node failures.
- **Reliable Storage**: Detects and duplicates corrupted data.
- **Efficient for Large Data**: High throughput and fault tolerance for large datasets.

**Disadvantages**:
- **Inefficient for Small Files**: Not optimized for handling small files.
- **Potential Master Bottleneck**: The master server can become a performance bottleneck.
- **Limited Random Access**: Not designed for frequent random writes; better for write-once, read-many workloads.
