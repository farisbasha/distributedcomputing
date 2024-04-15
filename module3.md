# 1. Explain the three approaches used for implementing distributed mutual exclusion? (3)


| Feature                                     | Token-Based Algorithm                                        | Non-Token-Based Approach                                 | Quorum-Based Approach                                    |
|---------------------------------------------|--------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|
| Unique Token Sharing                        | Yes                                                          | No                                                       | No                                                       |
| Method of Granting Access                   | Possession of Token                                          | Communication and Agreement Among Sites                  | Requesting Permission from Subset of Sites               |
| Ordering Requests                          | Sequence Numbers                                             | Timestamps                                               | -                                                        |
| Handling Request Conflicts                  | Based on Sequence Numbers                                    | Based on Timestamps                                      | -                                                        |
| Handling Concurrent Requests               | Token Ownership Determines Access                            | Timestamp Comparison Determines Access                    | -                                                        |
| Mutual Exclusion Guarantee                | Token Uniqueness Ensures Mutual Exclusion                   | Timestamps Ensure Mutual Exclusion                       | Quorum Structure Ensures Mutual Exclusion                |
| Examples                                   | Suzuki–Kasami Algorithm                                      | Ricart–Agrawala Algorithm                               | Maekawa’s Algorithm                                      |

