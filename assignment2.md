### **Agreement in Message-Passing Synchronous Systems with Failures**
- In a failure-free system, consensus is easily achieved by collecting information, making a decision, and disseminating it.
- Processes broadcast values and compute the same function on received values (e.g., majority, max, min).
- Algorithms involve token circulation on a logical ring, tree-based broadcast-convergecast-broadcast, or direct communication.
- Synchronous systems enable consensus in a constant number of rounds, with common knowledge of the decision value possible with an additional round.
- However, in failure-prone systems, reaching agreement becomes more challenging.

#### **Consensus Algorithm for Crash Failures (Synchronous System)**
- Designed for n processes with up to f (f < n) potential crash failures.
- Utilizes a consensus variable x, where each process has an initial value xi.
- Proceeds over f+1 rounds, with each process broadcasting its current value.
- After each round, processes take the minimum value among those received and their own, ensuring consensus.

```python
# Global constants
integer: f  # maximum number of crash failures tolerated

# Local variables
Integer: x ← local value

# Algorithm
(1) Process Pi (1 ≤ i ≤ n) executes consensus algorithm for up to f crash failures:
    (1a) for round from 1 to f + 1 do
    (1b) if the current value of x has not been broadcast then
    (1c) broadcast(x)
    (1d) yi ← value (if any) received from process j in this round
    (1e) x ← min ∀j (x, yj)
    (1f) output x as the consensus value
```

#### **Explanation and Analysis**
- **Agreement Condition:** In f + 1 rounds, at least one round is failure-free, ensuring consistent values among non-failed processes.
- **Validity Condition:** Processes broadcast agreed-upon values, ensuring no fictitious values are sent.
- **Termination Condition:** Algorithm completes within a finite number of rounds.
- **Complexity:** Requires f + 1 rounds with O(n^2) messages each, resulting in O((f + 1) * n^2) total messages.

#### **Lower Bound on Number of Rounds**
- **Minimum Rounds:** At least f + 1 rounds needed, where f < n.
- **Worst-case Scenario:** One process may fail per round, but with f + 1 rounds, there's at least one failure-free round, ensuring agreement among non-failed processes.


# Consensus algorithms for Byzantine failures (synchronous system) 



In a system of *n* processes, the Byzantine agreement problem (as well as other variants of the agreement problem) can be solved in a synchronous system only if the number of Byzantine processes *f* is such that $\( f \leq \lfloor \frac{n-1}{3} \rfloor \)$
. 

We informally justify this result using two steps:
<img width="1044" alt="Screenshot 2024-04-16 at 11 57 29 AM" src="https://github.com/farisbasha/distributedcomputing/assets/72191505/ee998796-feb0-421c-be32-1c81ffdd9963">

- With *n = 3* processes, the Byzantine agreement problem cannot be solved if the number of Byzantine processes *f = 1*. The argument uses the illustration in Figure , which shows a commander *Pc* and two lieutenant processes *Pa* and *Pb*. 



  In both scenarios, *Pa* gets different values from the other two processes. This shows the impossibility of agreement when *n = 3* and *f = 1*.

- With *n* processes and *f ≥ n/3* processes, the Byzantine agreement problem cannot be solved. The correctness argument of this result can be shown using reduction.



- With *n = 3* processes and *f = 1*, Byzantine agreement problem unsolvable due to inability to distinguish scenarios, leading to incorrect decisions.
- Byzantine agreement problem also unsolvable with *f ≥ n/3* processes.
- Reduction argument: If *Z(n≤3f,f)* is solvable, then *Z(3,1)* is also solvable, but since *Z(3,1)* is not solvable, *Z(n≤3f,f)* is also unsolvable.



