Business Case 
In 2025, a university in Oshawa Ontario called Ontario Tech students wanted to change its student services infrastructure to better its accessibility and ux across all academic and administrative services.

One important aspect to achieve this goal is 24/7 conversational access to data and services. 

The students wanted to deploy an AI-Powered Digital Assistant Platform that supports natural language processing and multi-channel interaction. 

The platform integrates all the existing existing university legacy systems including the Learning Management System (canvas lms), student info databases, admin services, and email servers. Within the university ecosystem, these systems are organized hierarchically, where critical data sources such as the student information and course catalog provide authoritative information. 

Many stakeholders depend on timely access to institutional data, so one priority for the university was to provide reliable, accurate responses to queries from students, faculty, and administrators. 

Problems may require system maintainers to perform troubleshooting, model retraining, or infrastructure scaling. Another priority for the university was to collect interaction data to monitor platform performance and continuously improve the AI models based on user feedback and query patterns.

In the initial deployment plans, the university wanted to support up to 5,000 concurrent users across multiple interaction modes including web, mobile, and voice-assistant devices. Besides natural language processing, the platform supports standard integration protocols such as REST and GraphQL APIs, which provide three basic categories of operations: Query operations, Update operation and  Notification operations.

Here are some of the main goals of the AIDAP.

User Experience Management. The goal is to provide intuitive and personalized interactions. The platform must support text and voice modes, multi-language queries, and maintain conversation context. Response times must average under 2 seconds under normal load, with 99.5% monthly availability.

Data Integration Management. This includes connecting to and synchronizing with external university systems such as LMS, registration databases, calendars, and email servers. The system must handle data source failures gracefully with retry and recovery mechanisms, maintain data integrity and consistency, and support configurable synchronization intervals.

Security and Access Control. The goal here is to protect user data and comply with institutional privacy policies. This includes implementing secure authentication through single sign-on (SSO), ensuring role-based access control for students, lecturers, administrators, and maintainers, and guaranteeing that sensitive data are visible only to authorized users.

AI Model Management. This goal focuses on maintaining and improving the AI capabilities that interpret natural language queries and generate relevant responses. The platform must support integration of multiple llm model versions. It should also externally log performance metrics for accuracy and latency, and enable continuous learning from past interactions to improve response relevance.
