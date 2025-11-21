# Iteration 1

## Step 1: Clarity of Drivers

Specifically, we add a use case diagram as we were missing this in phase 1.

| Category | Details |
|----------|---------|
| **Design purpose** | The purpose is to create an architectural design satisfying the drivers of the AIDAP system. |
| **Primary functional requirements** | Taking into account all drivers. |
| **Quality attribute scenarios** | See table below |
| **Constraints** | All constraints from CON-1 to CON-4 will be included as they will be crucial to the primary functional requirements. |

### Quality Attribute Scenarios

| Scenario ID | Importance to the Customer | Difficulty of Implementation According to the Architect |
|-------------|---------------------------|--------------------------------------------------------|
| QA-1 | Low | Med |
| QA-2 | High | Med |
| QA-3 | High | Med |
| QA-4 | Med | High |
| QA-5 | High | Med |

## Step 2: Establish Iteration Goal by Selecting Drivers

As this is the first iteration, our goal should be to create a preliminary architectural design. This means that we select all drivers to implement them into the design. Below are most drivers of interest:

- **QA-1:** Maintainability
- **QA-2:** Reliability
- **QA-3:** Security
- **QA-4:** Scalability
- **QA-5:** Interoperability
- **CON-1:** The system must back up data daily and be able to restore it after downtime.
- **CON-2:** Only authorized lecturers and administrators can modify or post academic data.
- **CON-3:** The system must automatically synchronize data at set intervals and retry if a sync fails.
- **CON-4:** The platform must scale to handle up to 5,000 concurrent users.
- **CRN-1:** Different university systems may use incompatible or evolving APIs. (linked to RD2)
- **CRN-2:** Real-time data retrieval could cause slow response times. (linked to RS10 and R6)
- **CRN-3:** System may need rapid maintenance in early stages that will violate RS11, so it won't be up 99.5% of each month. (linked to RS11)

### Context Diagram


## Step 3: Choose One or More Elements of the System to Refine

In this iteration, we are creating the preliminary software architecture, so we would be trying to refine the AIDAP system by decomposing the parts and analysing each component's relationships.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

In this iteration, we will be going through a selection of design options that satisfy the architectural drivers that encapsulate the entire system.

### Design Decisions

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| **Logically structuring the system using the Rich Internet reference architecture** | We ultimately decide on using the Rich Internet Application reference architecture because it provides a rich user experience without needing the user to install anything. Additionally, we need it to run like a web application, so a Rich Internet reference architecture would be a blend of a Web Application reference architecture and a Rich Client Application reference architecture. We are also able to store some of the users requests on their local storage if they appear to fail (UC-4). Users would also be able to view recent responses cache using the local storage (UC-5). |

### Discarded Alternatives

| Alternative | Rationale |
|-------------|-----------|
| **Web Application reference architecture** | This reference architecture would not fit our requirements because we need to have a local cache of some sort (UC-5). Although the web application part of it does match up with some of our requirements, we ultimately need a rich client interface for good user experience. |
| **Rich Client Application reference architecture** | This reference architecture requires the software to be installed onto the client's machine, and that is not what we are looking for. We are looking for a rich client interface (RS3, RS12), but it shouldn't be installed onto the client's machine. In R7, we want the application to be cloud native, meaning we generally use the server to process user requests instead of needing to use the user's resources. |
| **Mobile Application reference architecture** | The Mobile Application reference architecture is used for mobile applications. Our application will need a mobile application as stated in RS9, but currently it will be overcomplicated as a preliminary architectural design to include mobile application support. Considerations for supporting a mobile application could make the design something like a server that follows Service Application reference architecture with a facade between normal web client and mobile user to communicate which device the user is on. Additionally, there would be a data layer just like there will be in this iteration. |
| **Service Application reference architecture** | The Service Application reference architecture is meant for external systems to utilize and is not meant for direct human usage. Additionally, the server components are pretty closely coupled to the client. |

### Additional Design Decisions

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| **Physically structure the application using the load-balanced cluster deployment pattern** | Adhering to R7, we need to use a load balancer to distribute users across multiple cloud servers. This would also address QA-2 and QA-4 in reliability and scalability respectively. Performance would also be increased as load would be distributed across a number of servers that increase and decrease in quantity depending on the server load. |
| **The React framework with SSR could be used to provide good UX along with Kubernetes to manage the cloud containers of the server** | This is a suggestion of using the React framework that fits into the architectural design. Additionally, React follows a component based architecture, which makes reusable code and improves maintainability. Kubernetes also has load balancing functionalities that fit the scaling of cloud containers. The Database Server and fine details not outlined. Storage of recent responses persisting onto the local storage on the Client. |

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| **Regarding the Rich Internet Application reference architecture, a modification will need to be made, which is the addition of the AI model** | Adding the AI model aligns with R5 and the associated RS1, RS4, RS5, RS10, RL6, RM3, and RM4. |
| **Addition of external systems to the deployment pattern** | External systems will be located within the data layer as we are only referring to external system data (R3). |

Design decision results will be laid out in the next step.
