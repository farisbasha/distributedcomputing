# **Distributed Shared Memory (DSM)**:


Distributed Shared Memory (DSM) provides an abstraction that allows programmers to perceive a distributed system's memory as a single monolithic memory, akin to traditional von Neumann architecture. Key points include:

- **Simplified Access**: Programmers use simple read and write primitives to access data across the network.
- **Ease of Use**: Eliminates the need for send and receive communication primitives, simplifying synchronization and consistency management.
- **Memory Allocation**: Part of each computer’s memory is allocated to shared space, while the remainder is private memory.


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


#  Lamport’s Bakery Algorithm

Lamport’s Bakery Algorithm is a classic algorithm for achieving mutual exclusion in a system of \( n \) processes that share memory. The algorithm is designed to ensure that no two processes are in the critical section simultaneously.
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
