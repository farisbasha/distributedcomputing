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


- Each computer has memory and a processor, connected by a network.
  - Distributed software, or middleware, facilitates collaboration for common goals.
  - Layered architecture simplifies system design complexity.
  - Middleware enables distributed system functionality while handling diversity transparently.

# Distributed Systems (Advantages):
  - Resource sharing.
  - Access to geographically remote data and resources.
  - Enhanced reliability: Ensures availability, integrity, and fault-tolerance.
  - Increased performance/cost ratio: Achieved through resource sharing and remote data access.
  - Scalability.
  - Modularity and incremental expandability.

#  **Primitives for Distributed Communication** 
  - **Blocking/Non-blocking, Synchronous/Asynchronous Primitives**: 
      - **Blocking**: Actions that halt until a response is received. Like waiting in line until it's your turn.
      - **Non-blocking**: Actions that don't wait and allow other tasks to proceed immediately. Like sending a message and continuing with other tasks without waiting for a reply.
      - **Synchronous**: Actions that occur in sequence, with each step waiting for the previous one to complete. Like following a recipe step by step.
      - **Asynchronous**: Actions that can occur independently and don't necessarily wait for each other to finish. Like cooking multiple dishes simultaneously.
  
  - **Processor Synchrony**: 
      - Refers to how well processors or entities within a distributed system are synchronized in terms of timing and execution. Think of it like a synchronized dance where everyone moves in perfect harmony.
  
  - **Libraries and Standards**: 
      - Tools and guidelines provided to make communication between distributed entities easier and more consistent. It's like using a common language or set of rules so everyone understands each other better.
   
  - # **Processor Synchrony**:
  - Refers to all processors executing in synchronized lock-step with their clocks aligned.
  - In distributed systems, complete synchrony is impractical. Instead, synchronization occurs at larger code steps.
  - Implemented using barrier synchronization, ensuring no processor proceeds to the next code step until all have completed the previous one.

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
