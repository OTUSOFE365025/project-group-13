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
![alt text](<ADD Iteration 1 Diagrams/ContextDiagram.png>)

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


#### Step 6: Sketch Views and Record Design Decisions

## Component Descriptions

The table below describes each component, layer, and block of the diagram.

| Element | Responsibility |
|---------|---------------|
| Client | The container that houses components within the client machine. |
| Browser | Web browser environment hosting the application. |
| Plug-in execution container | Container for executing plug-ins and extensions. |
| Rich UI Engine | Handles rich user interface rendering and interactions. |
| Presentation | Manages the presentation layer and UI components. |
| LocalStorage | Client-side storage for recent chat response data persistence. |
| Service Interfaces | Defines the contracts and APIs for services like the AI model and reaching business logic. |
| Message Types | Defines message structures and data formats for communication. |
| Application Facade | Provides a simplified interface to business layer complexity. |
| Business Workflow | Manages business process flows and orchestration. |
| Business Logic | Contains core business rules and processing logic. |
| Business Entities | Represents business domain objects and models. |
| Data Access | Handles database queries and data operations. |
| Helpers and Utilities | Provides common utility functions and helper methods to access the database. |
| Service Agents | Manages communication with external services like registration, LMS, and calendars. |
| Security | Handles authentication, authorization, and security policies. |
| Operational Management | Manages monitoring, logging, and operational metrics. |
| Communication | Manages inter-component and inter-system communication. |
| AI Model | Provides artificial intelligence and machine learning capabilities that should provide responses to the user via their response passed through Service Interfaces, formatted with Message Types, and based on relevant information like the external systems and the database. |
| Database | External data storage system. |
| External Systems | Third-party systems and integrations like registration, LMS, and calendars. |

## Load Balanced Cluster Deployment Pattern

The table below outlines the elements of each module of the load balanced cluster deployment pattern.

| Element | Responsibility |
|---------|---------------|
| Client | The client application that initiates requests to the system. |
| Firewall (Client-side) | Security layer that protects the internal network from external threats and filters incoming traffic. |
| Load Balancer | Distributes incoming client requests across multiple application server instances. |
| Instance n: Application Server | Individual server instances that host server-side application logic and process client requests, facilitating data retrieval. |
| Firewall (Database-side) | Security layer that protects the database server and controls access between application servers and the database. |
| Database Server | Hosts the database system and manages data storage, retrieval, and persistence. |
| External Systems | Hosts the data that external systems can provide. |

## Module Relationships

The table below outlines details about the relationships between modules.

| Relationship | Description |
|-------------|-------------|
| Between Client and Firewall (Client-side) | Client requests pass through the firewall for security inspection before entering the internal network. |
| Between Firewall (Client-side) and Load Balancer | Filtered traffic is forwarded to the load balancer for distribution. |
| Between Load Balancer and Application Server Instances | Load balancer distributes requests across multiple application server instances. |
| Between Application Server Instances and Firewall (Database-side) | Application servers communicate with the database through a firewall for secure database access. |
| Between Firewall (Database-side) and Database Server | Authorized database queries and commands are passed through the firewall to the database server. |
| Between Firewall (Database-side) and External Systems | Authorized database queries and commands are passed through the firewall to the external systems endpoints. |



# Step 7: Perform Analysis of Current Design and Review Iteration

## Design Decisions Made During Iteration

### Use Cases

| ID | Status | Analysis |
|----|--------|----------|
| UC-1 | Completely Addressed | The external systems are integrated and well represented in the diagrams we have made in this iteration. It is also addressed that if the external system exposes API endpoints with some sort of authorization, we can draw data through those. |
| UC-2 | Completely Addressed | This ties into the external system integration, assuming we can obtain the event data from the LMS, we should be able to integrate an export feature for upcoming events in the form of a formatted .CSV file that should be easily compatible with something like google calendar. |
| UC-3 | Completely Addressed | This should be addressed in the business layer of the Rich Internet Application diagram. We can control the logic to change user preferences and save these in the database. |
| UC-4 | Partially Addressed | This one is not fully clear. This can be addressed in the database if there isn't a response for a question asked by the user. |
| UC-5 | Completely Addressed | This should be completely addressed via caching their most recent chat history in local storage on the client side. |
| UC-6 | Completely Addressed | The security component of the Rich Internet Application diagram should check valid credentials for each page and single sign-on is addressed using an external system provided by the school to verify credentials. |
| UC-7 | Completely Addressed | This use case can be satisfied through some sort of business logic in the business layer of the Rich Internet Application diagram created to notify students at the set time. |
| UC-8 | Completely Addressed | Selected reference architecture should fully cover the dashboard with analytics and error logs for the client, which should be a verified System Maintainer. |
| UC-9 | Not Addressed | Not addressed in this iteration. It is crucial to identify all the components associated with this use case. |
| UC-10 | Completely Addressed | The load balancing cluster diagram should address this very well, as we use a load balancer to distribute user load across a multitude of cloud containers that increase or decrease according to traffic. |

### Quality Attributes

| ID | Status | Analysis |
|----|--------|----------|
| QA-1 | Completely Addressed | Can be addressed by updating individual cloud containers. |
| QA-2 | Completely Addressed | This is addressed through the multiple cloud containers. If one goes down, we can have the other containers take on and distribute the load. In terms of restoring user data, we can do the same as UC-4 with just using the business logic to verify user query vs. their paired responses. If one query is missing a response, we can pick up on it and get the AI model to process it. |
| QA-3 | Completely Addressed | This should be addressed externally through the SSO of the school system. Mainly authentication should be carried out on each page but with a carried token either in local storage or protocoled cookies. |
| QA-4 | Completely Addressed | Cloud containers are scaled up and down according to traffic. Additionally, a load balancer is put into play to distribute load across the multiple cloud containers. |
| QA-5 | Completely Addressed | External systems should have a way to communicate with other related school systems. This would allow the business layer to extract data from the external systems. |

### Constraints

| ID | Status | Analysis |
|----|--------|----------|
| CON-1 | Not Addressed | This is not addressed within this iteration. The process would have to be outlined. |
| CON-2 | Not Addressed | This is not addressed specifically within this iteration. Also needs the process to be outlined explicitly. |
| CON-3 | Not Addressed | This is not addressed specifically within this iteration. Also needs the process to be outlined explicitly. |
| CON-4 | Completely Addressed | This is addressed through the performance the load balancer introduces. At peak times, the load balancer will pull its weight. |

### Concerns

| ID | Status | Analysis |
|----|--------|----------|
| CRN-1 | Not Addressed | Not addressed, although you could have some sort of facade or interface to interpret. |
| CRN-2 | Not Addressed | This is not addressed. |
| CRN-3 | Completely Addressed | This is addressed as we can replace individual cloud containers and have the load balancer redistribute across the leftover containers. |
