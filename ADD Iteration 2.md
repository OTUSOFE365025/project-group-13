# Step 1: Review Input Drivers

| Category | Details |
|----------|---------|
| Design purpose | The purpose is to create sequence diagrams to clarify the processes between layers using the architectural drivers. |
| Primary functional requirements | We are going to specifically look at one large scenario of UC-1, UC-6, and UC-7. |
| Quality attribute scenarios | |

| Scenario ID | Importance to the Customer | Difficulty of Implementation According to the Architect |
|-------------|---------------------------|-------------------------------------------------------|
| QA-3 | High | Med |
| QA-5 | High | Med |

| Category | Details |
|----------|---------|
| Constraints | CON-2 would be addressed because setting a deadline reminder in the form of an announcement would be done only by authorized lecturers. |


# Step 2: Establish Iteration Goal by Selecting Drivers

For this iteration, we need to select our use cases and define sequence diagrams for each. They are as follows:

- **UC-1:** External Systems Integration
- **UC-6:** Secure Login
- **UC-7:** Setting Deadline Reminders

# Step 3: Choose One or More Elements of the System to Refine

We will refine the specific relationships between the layers of the architectural design. This would result in a sequence diagram that outlines a lecturer signing securely signing in and setting deadline reminders that will integrate into external systems as an announcement in the LMS.
## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

In this iteration, we are creating the preliminary software architecture, so we would be trying to refine the AIDAP system by decomposing the parts and analysing each component's relationships.

### Design Decisions

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| **Use school SSO for login** | Secure login aligns with QA-3 Security. Using school SSO avoids storing passwords, ensures only verified institution users can access the system, and satisfies CON-2 because only authorized lecturers can set reminders. |

### Discarded Alternatives

| Alternative | Rationale |
|-------------|-----------|
| **Custom username/password login stored in our own DB (no SSO)** | Increases security risk (we'd have to store and protect passwords), duplicates what the university already has, and makes it harder to enforce institution-level access rules. It weakens QA-3 Security compared to using school SSO and doesn't align as well with UC-6. |
| **Let any logged-in user (including students) create reminders** | Breaks CON-2, because students would then be able to post or modify academic data. It also conflicts with UC-7, which assumes lecturers are the ones setting deadlines and announcements. |
| **Send reminders only via email from our own system, with no integration to LMS / calendar** | This ignores the requirement that the system should integrate with external academic systems and calendars. It only partially satisfies UC-7 and does not properly address UC-1 or QA-5. |
| **Direct database access into the LMS tables instead of using official APIs** | Any LMS schema change would break our system. This hurts QA-5 Interoperability and makes integration harder to maintain. Using official LMS APIs is more stable and aligned with UC-1. |

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decisions and Location | Rationale |
|-------------------------------|-----------|
| **Place institution SSO inside the Security component of the Rich Internet Application architecture** | Keeps all login and token validation logic in a single, secure location. This satisfies UC-6 Secure Login and supports QA-3 Security by delegating authentication to the university's SSO instead of storing passwords ourselves. |
| **Add a Role and Authorization sub-component in the Business Logic layer** | After SSO login, this component checks whether the user is a lecturer, student, or admin before allowing operations like creating reminders. This directly enforces CON-2 and supports UC-7 by ensuring only lecturers can post academic data. |

## Step 6: Sketch Views and Record Design Decisions

| Element | Responsibility |
|---------|----------------|
| **Lecturer** | Initiates the interaction by opening the system, logging in, and creating a deadline reminder for a course. |
| **AIDAP Client (Web UI)** | Front-end running in the browser; sends login requests via SSO, collects reminder details from the lecturer, and calls backend APIs. |
| **Browser** | Hosts the AIDAP client and handles redirects to and from the institution's SSO login page. |
| **Security / SSO Handler** | Communicates with the university SSO provider, validates SSO tokens, and returns an authenticated user identity (for UC-6 Secure Login). |
| **Application Facade** | Main entry point to the backend; receives "create reminder" requests from the client and routes them to the appropriate business services. |
| **Role / Authorization Service** | Checks the user's role (lecturer vs student/admin) after login and only allows lecturers to create or modify deadline reminders, enforcing CON-2. |
| **Reminder Service** | Validates reminder details (course, deadline time, message), stores them, and passes them to the scheduler for future execution, realizing UC-7 Setting Deadline Reminders. |
| **Scheduler / Job Queue** | Schedules reminders to trigger at the configured time and initiates follow-up calls to external systems when the reminder becomes due. |
| **External LMS / Calendar Agent** | Encapsulates API calls to the LMS and/or calendar system, creating announcements/events for the relevant course, supporting UC-1 External Systems Integration. |
| **Database** | Persists user roles/permissions, course mappings, and reminder records (time, message, status, target course). |
| **External Systems (LMS / Calendar)** | Receive announcements or events from AIDAP (via the External Agent) and expose them to students through existing university platforms. |
| **Operational Management / Logging** | Logs key actions (login, reminder creation, LMS call success/failure) to support monitoring and troubleshooting of the overall flow. |

## Step 7: Perform Analysis of Current Design and Review Iteration

| | Not Addressed | Partially Addressed | Completely Addressed | Design Decisions made during Iteration |
|---|:---:|:---:|:---:|---|
| **UC-1** | | ✓ | | API-based External LMS/Calendar Agent integrates with LMS/calendar via official APIs. Detailed data mapping and error-handling policies still to be refined. So Partially Addressed. |
| **UC-2** | ✓ | | | Not in scope for this iteration; no new behaviour or components added. |
| **UC-3** | ✓ | | | Not considered in this iteration. Future work could connect preferences to Reminder Service. |
| **UC-4** | ✓ | | | No specific recovery mechanisms or backup flows were refined here. |
| **UC-5** | ✓ | | | Not impacted by this iteration's login/reminder sequence work. |
| **UC-6** | | | ✓ | Use of school SSO inside the Security component ensures only verified institutional users can log in. We do not store passwords; token validation is centralized, which strongly supports secure login. |
| **UC-7** | | | ✓ | Reminder Service + Scheduler/Job Queue fully support creating and scheduling reminders by lecturers. Sequence diagram shows end-to-end flow from lecturer to LMS announcement. |
| **UC-8** | ✓ | | | Only basic logging mentioned; no detailed monitoring dashboard design added. |
| **UC-9** | ✓ | | | Not covered in the current sequence; would need additional flows in future iterations. |
| **UC-10** | ✓ | | | Handled in Iteration 1; this iteration did not refine deployment/load balancing behaviour. |
| **QA-1** | ✓ | | | Not a primary focus in this iteration, though introducing clear interfaces. |
| **QA-2** | ✓ | | | No explicit failover or retry logic analysed here; that will require further work. |
| **QA-3** | | | ✓ | School SSO + Role/Authorization Service ensure only authenticated and authorized users perform sensitive actions (e.g., setting reminders). This strongly addresses security for the selected scenario. |
| **QA-4** | ✓ | | | Not revisited in this iteration; still mainly handled by Iteration 1's load-balanced cluster. |
| **QA-5** | | ✓ | | Use of a dedicated External Calendar Agent and API-based integration layer improves interoperability, but details for multiple different APIs are not fully specified yet. |
| **CON-1** | ✓ | | | No backup/restore mechanisms refined in this iteration. |
| **CON-2** | | | ✓ | Role/Authorization Service checks lecturer role before allowing reminder creation/posting, satisfying this constraint. |
| **CON-3** | ✓ | | | Sync interval/retry logic for external systems not yet defined. |
| **CON-4** | ✓ | | | Not analysed here; still tied to deployment decisions from Iteration 1. |
| **CRN-1** | ✓ | | | Risks around changing external APIs, performance, and early maintenance weren't revisited in this iteration. |
| **CRN-2** | ✓ | | | Risks around changing external APIs, performance, and early maintenance weren't revisited in this iteration. |
| **CRN-3** | ✓ | | | Risks around changing external APIs, performance, and early maintenance weren't revisited in this iteration. |
