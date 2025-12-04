# Shortened ATAM Process Similar to Lab 5

## Architectural Decisions

- **AD1:** Active redundancy availability tactic
- **AD2:** Source/replica replication strategy for external systems
- **AD3:** Users are balanced across a multitude of cloud containers via a load balancer
- **AD4:** Deploying cloud containers for the servers so we can scale easily
- **AD5:** Utilising a rich internet reference architecture to represent components

## Risks

- **R1:** Change of information on the replica side will not work
- **R2:** Inadequate logging will lead to disorganisation and difficulty solving errors
- **R3:** Data syncing issues leading to large uncoordinated data between replicas
- **R4:** Introduces a single point of failure

## Non-risks

- **N1:** Active replicas mitigate the risk of a single point of failure in both the load balancer and the database
- **N2:** Source/Replica replication allows us to keep local versions of external data for faster average retrieval times
- **N3:** The load balancer solves the risk of having overloaded systems during peak hours
- **N4:** Multiple application servers that scale according to server load can also contribute to better maintainability, availability, and performance
- **N5:** Following a proven reference architecture mitigates risk

## Sensitivities

- **S1:** Setting up a load balancer can be complex
- **S2:** Concern over security issues arising because of having more ground to cover
- **S3:** Concern over response and network latency due to increased memory from having a replica database and load balancer along with having local versions of external data
- **S4:** Load balancing algorithm affects performance

## Tradeoffs

- **T1:** Performance (-) vs. Availability (+) - Higher overhead latency due to the extra step of load balancer distributing across application servers
- **T2:** Performance (-) vs. Maintainability (+) - Developers can push updates without downing the system
- **T3:** Performance (-) vs. Interoperability (+) - Storing local versions of external data prevents the need of directly requesting from source every time it is needed, and this increases the amount of storage we need, but it can also decrease latency during peak hours
- **T4:** Performance (-) vs. Availability (+) - Higher overhead latency due to replicated load balancer and database

## Analysing Scenario

| Analysing Scenario | |
|--------------------|---|
| **Scenario** | Client is able to access the system and use the AIDAP with reliable information. |
| **Attributes** | Interoperability, Maintainability, and Availability |
| **Stimulus** | Client accesses and utilizes system |
| **Environment** | |
| **Response** | System responds according to defined requirements |

| Architecture Decision | Sensitivity | Tradeoffs | Risk | Nonrisk |
|-----------------------|-------------|-----------|------|---------|
| AD1 | S2, S3 | T4 | R2 | N1 |
| AD2 | S2 | T3 | R1, R2, R3 | N2 |
| AD3 | S1, S4 | T1 | R4 | N3 |
| AD4 | S2 | T2 | | N4 |
| AD5 | | | | N5 |

## Conclusion

To conclude, the ATAM process brings up relevant risks and concerns that are not addressed across the current 3 iterations, such as data integrity, data syncing failures and retries, changing external data from AIDAP, state management, and what data we should keep on local nodes. Availability should be added as a QA-6, as the tactic of active redundancy contributes towards that. This would have the scenario of "Load distribution and scalable cloud containers help keep the system available during peak hours." These will need to be addressed in future iterations.

![alt text](<UtilityTree.webp>)
