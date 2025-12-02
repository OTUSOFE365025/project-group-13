## Step 1: Ensure Clarity of Drivers

| Category | Details |
|----------|---------|
| Design purpose | The purpose of this iteration is to study QA-1, QA-2, and QA-5 and make design decisions that address them. |
| Primary functional requirements | Mainly working on UC-9: An event gets rescheduled. The data source system reflects the change due to the data being synced every set interval. |
| Quality attribute scenarios | **Scenario ID / Importance to the Customer / Difficulty of Implementation According to the Architect**<br>QA-1: Low / Med<br>QA-2: High / Med<br>QA-5: High / High |
| Constraints | CON-1, CON-3, and a little bit of CON-4 will be considered in this iteration. |

## Step 2: Establish Iteration Goal by Selecting Drivers

In this iteration, we will focus on the following architectural drivers:

- **UC-9**: An event gets rescheduled. The data source system reflects the change due to the data being synced every set interval.
- **QA-1**: Maintainability - Developers push updates without taking the system offline.
- **QA-2**: Reliability - After maintenance, system restores user data and queued queries automatically.
- **QA-5**: Interoperability - System pulls and syncs data correctly from multiple university databases.
- **CON-1**: The system must back up data daily and be able to restore it after downtime.
- **CON-3**: The system must automatically synchronize data at set intervals and retry if a sync fails.
- **CON-4**: The platform must scale to handle up to 5 000 concurrent users.

## Step 3: Choose One or More Elements of the System to Refine

We will refine the deployment pattern diagram, and then create a sequence diagram to clarify how we will sync external system data.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

Design decisions made during this iteration:

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| Introduce active redundancy within the deployment pattern diagram | We can introduce active redundancy of the database to address QA-1, QA-2, CON-1, and CON-4. |
| Create a sequence diagram for database syncing to external systems | This addresses UC-9, QA-5, and CON-3. |

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| Load Balancer will utilize active redundancy within the deployment pattern diagram. | To address the associated drivers, load balancer will be replicated and the replicated load balancer will be put into use when the original load balancer is down. |
| Database Server will utilize active redundancy within the deployment pattern diagram. | To address the associated drivers, database server will be replicated and this replicated database server will be put into use when the original database is down. Changes made to one database should propagate to the other one. |
| UC-9 will be represented using a sequence diagram. | Syncing will work by using the source/replica replication strategy. This involves having local nodes of the database that will contain external information, and these will be updated whenever the source is updated. So our sequence diagram will capture event rescheduling on an external system. The reason for having a local version of external system data is that it would be faster to query a local database than to request a query on an external one. |

Design decision results will be laid out in the next step.

## Step 6: Sketch Views and Record Design Decisions

![alt text](<ADD Iteration 3 Diagrams/UpdatedLoadBalancedCluster.png>)

### Module Elements

The table below outlines the elements of each module of the updated load balanced cluster deployment pattern.

| Element | Responsibility |
|---------|---------------|
| Client | The client application that initiates requests to the system. |
| Firewall (Client-side) | Security layer that protects the internal network from external threats and filters incoming traffic. |
| <<replicated>> Load Balancer | Distributes incoming client requests across multiple application server instances. Added active redundancy, meaning that there will be a copy of the original load balancer that is put into use when the original is down. |
| Instance n: Application Server | Individual server instances that host server-side application logic and process client requests, facilitating data retrieval. |
| Firewall (Database-side) | Security layer that protects the database server and controls access between application servers and the database. |
| <<replicated>> Database Server | Hosts the database system and manages data storage, retrieval, and persistence. Added active redundancy, meaning there will be a copy of the original database to be put into use when the original is down. |
| Node n: Local External Data | There will be an n amount of database nodes that contain external data. These are responsible for receiving updates from the source and storing external data. |
| External Systems | Hosts the data that external systems can provide. |

**Note**: We did not outline the relationship between Nodes N and Database because it is more of an explicit way of representing how we can sync data between external systems and local systems. Nodes N would be a part of the Database Server.

### Module Relationships

The table below outlines details about the relationships between modules.

| Relationship | Description |
|--------------|-------------|
| Between Client and Firewall (Client-side) | Client requests pass through the firewall for security inspection before entering the internal network. |
| Between Firewall (Client-side) and Load Balancer | Filtered traffic is forwarded to the load balancer for distribution. |
| Between Load Balancer and Application Server Instances | Load balancer distributes requests across multiple application server instances. |
| Between Application Server Instances and Firewall (Database-side) | Application servers communicate with the database through a firewall for secure database access. |
| Between Firewall (Database-side) and Database Server | Authorized database queries and commands are passed through the firewall to the database server. |
| Nodes N and External Systems | Updated information will update from external systems to the nodes. |
