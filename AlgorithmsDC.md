# Module 2

## Chandy Lamport
**Overview:**
- The Chandy-Lamport algorithm is designed to capture a consistent global state in a distributed system using control messages called markers.
- This algorithm works in systems with FIFO (First-In-First-Out) communication channels.

### **Algorithm**
**Marker Sending Rule for Process  $\( p_i \)$ :**
1. Process  $\( p_i \)$  records its local state.
2. For each outgoing channel  $\( C \)$  that hasn't received a marker yet,  $\( p_i \)$  sends a marker along  $\( C \)$  before sending any further messages on  $\( C \)$ .
**Marker Receiving Rule for Process  $\( p_j \)$ :**
- Upon receiving a marker along channel  $\( C \)$ :
  - If  $\( p_j \)$  has not yet recorded its state:
    -  $\( p_j \)$  records the state of  $\( C \)$  as empty.
    -  $\( p_j \)$  then records its own state and follows the Marker Sending Rule.
  - If  $\( p_j \)$  has already recorded its state:
    -  $\( p_j \)$  records the state of  $\( C \)$  as the set of messages received after  $\( p_j \)$  recorded its state and before receiving the marker.



---


# Module 3

## Lamport's Distributed Mutual Exclusion Algorithm:

- **Overview**: Utilizes permission-based approach and timestamps for mutual exclusion in distributed systems.
- **Complexity**: Requires 3(N-1) messages per critical section execution.


<img width="470" alt="Screenshot 2024-05-19 at 10 44 48 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/d806570e-607b-487c-bf30-de885477f53b">

---

## Ricart–Agrawala Algorithm :

- **Overview**: Proposed by Ricart and Agrawala, it's a mutual exclusion algorithm for distributed systems, building upon Lamport's Algorithm.
- **Algorithm**: Uses REQUEST and REPLY messages for permission-based mutual exclusion. Requests are timestamped.
- **Message Complexity**: Requires 2(N – 1) messages per critical section execution, including (N – 1) request messages and (N – 1) reply messages.

 <img width="636" alt="Screenshot 2024-04-15 at 10 35 29 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/37592cc4-efac-476c-8356-602e3ae900b8">

---

## **Maekawa’s Algorithm**:

- **Overview**: Quorum-based mutual exclusion algorithm, distinct from permission-based approaches like Lamport’s and Ricart-Agrawala.
- **Quorum Concept**: Sites request permission from a subset called quorum, not every site.
- **Messages**: Utilizes REQUEST, REPLY, and RELEASE messages.
- Low message complexity: O(sqrt(N)) messages per invocation.
  
 <img width="794" alt="Screenshot 2024-04-15 at 11 03 10 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/67b4fde7-7146-4a33-a62c-48b9e181f53e">

---


## **Suzuki–Kasami Algorithm**:

- **Overview**: Token-based mutual exclusion algorithm, derived from Ricart–Agrawala, utilizing REQUEST and REPLY messages.
- **Token-Based Approach**: Access to critical section granted via possession of a unique token, unlike non-token algorithms.
- **Data Structures**:
  - RN[1…N]: Array to track largest sequence numbers received from each site.
  - LN[1…N]: Array used by the token, with LN[j] representing the recently executed request's sequence number from site Sj.
  - Queue Q: Maintains IDs of sites waiting for the token.
- **Message Complexity**: Requires either 0 or a maximum of N messages per critical section execution, depending on token ownership.

<img width="507" alt="Screenshot 2024-04-15 at 10 48 26 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/e401c510-4ad4-43b9-b864-c4eff377e50c">

---

# Module 4

##  Lamport’s Bakery Algorithm

Lamport’s Bakery Algorithm is a classic algorithm for achieving mutual exclusion in a system of \( n \) processes that share memory. The algorithm is designed to ensure that no two processes are in the critical section simultaneously.A unique lexicographic order is defined on the tuple <token, pid> maintained for resolving conflict
- The algorithm uses a combination of timestamps and process IDs to ensure a unique ordering of processes.
- The timestamps are chosen in a way that mimics the behavior of customers taking a number in a bakery, thus the name "Bakery Algorithm".


<img width="417" alt="Screenshot 2024-05-20 at 2 01 00 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/b3d4c9da-b6ff-4f53-a1ef-75dc7f23b01b">

---

# Module 5

## Consensus Algorithm for Crash Failures (Synchronous System)



<img width="464" alt="Screenshot 2024-05-20 at 9 12 15 PM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/13119b83-04ec-468c-9c35-984916708715">
