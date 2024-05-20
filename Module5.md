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
