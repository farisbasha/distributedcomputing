### Logical Clocks

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
- **R1**: Before any event, increment $\( C_i \)$ by a positive value $\( d \)$ (typically $\( d = 1 \)$).
- **R2**: On message receipt, update $\( C_i \)$ to the maximum of its current value and the received timestamp, then increment $\( C_i \)$.

#### Properties:
- **Consistency**: Ensures $\( e_i \rightarrow e_j \implies C(e_i) < C(e_j) \)$.
- **Total Ordering**: Orders events with identical timestamps using process identifiers.
- **Event Counting**: When $\( d = 1 \)$, the timestamp indicates the number of events preceding the current event.
- **No Strong Consistency**: Scalar clocks do not guarantee $\( C(e_i) < C(e_j) \implies e_i \rightarrow e_j \)$.

### Vector Time

Developed by Fidge, Mattern, and Schmuck, vector time uses vectors to maintain more detailed causality information:
- **Time Domain**: Set of $\( n \)$-dimensional non-negative integer vectors.
- **Vector Clock ($\( vt_i \)$)**: Each process $\( p_i \)$ maintains a vector where $\( vt_i[i] \)$ is its local logical clock, and $\( vt_i[j] \)$ represents its knowledge of $\( p_j \)$'s logical time.

#### Rules:
- **R1**: Increment the local logical time $\( vt_i[i] \)$ before executing an event.
- **R2**: On message receipt, update the vector clock element-wise to the maximum of the current and received vectors, then increment the local logical time.

#### Properties:
- **Isomorphism**: There is a direct mapping between event causality and vector timestamps: $\( e_i \rightarrow e_j \iff vt_i < vt_j \)$.
- **Strong Consistency**: Allows determination of causal relationships between events by comparing their vector timestamps.
- **Event Counting**: With $\( d = 1 \)$, the vector's \$( i \)$-th component counts the number of events at $\( p_i \)$, and the sum of all components minus one gives the total number of causally preceding events.

### Conclusion

Logical clocks, whether scalar or vector, are essential for managing the order and causality of events in distributed systems. Scalar clocks provide a simple total ordering mechanism, while vector clocks offer a detailed and strongly consistent representation of causality. Each approach has its specific use cases and benefits in the context of distributed computing.
