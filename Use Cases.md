Use Case | Description
--- | ---
UC-1: External Systems Integration | On user prompt, system will draw upon pre-existing university data systems and provide the appropriate response based on the data.
UC-2: Registering for an Event | Student registers for an upcoming event. The system should notify the student prior to the event.
UC-3: Controlling Notification Preferences | The student will be able to modify preferences for notification. Additionally, they will be able to change the language of the application.
UC-4: Data Recovery | System goes down due to scheduled maintenance. System recovers any pending user queries after downtime.
UC-5: Offline Cache | The system is down, but the user is able to view their previous responses.
UC-6: Secure Login | Students are provided secure single sign on through the institution credentials.
UC-7: Setting Deadline Reminders | A lecturer sets a reminder about a deadline, the system will log it and notify students at the appropriate time.
UC-8: Monitoring Dashboard | The system will provide a dashboard to monitor latency, errors, and uptime to the system maintainer. If the system goes down, the dashboard will show that the system is down. If the system is undergoing peak load and has high latency, the dashboard will also reflect that. Errors will also be logged and accounted for so that the system maintainer can take action from the dashboard's data.
UC-9: Event Date Change | An event gets rescheduled. The data source system reflects the change due to the data being synced every set interval.
UC-10: Load Balancing | User activity increases during peak hours, so the system will distribute incoming traffic and computation across multiple cloud instances to maintain performance. The system maintainer monitors load distribution through the dashboard.
