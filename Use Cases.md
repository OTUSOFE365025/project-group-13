Use Case | Description
--- | ---
UC-1: External Systems Integration | When asked for the relevant information, system should draw upon pre-existing university data systems and provide the appropriate response based on the data.
--- | ---
UC-2: Registering for an Event | Student registers for an upcoming event. The system should notify the student prior to the event.
--- | ---
UC-3: Controlling Notification Preferences | The student should be able to modify preferences for notification. Additionally, they should be able to change the language of the application.
--- | ---
UC-4: Data Recovery | System goes down due to scheduled maintenance and student was querying for a due date on an assignment. The system should save this prompt so that whenever the system is available again, the student should be able to retry their prompt.
--- | ---
UC-5: Offline Cache | When the system is down, the user should be able to view their previous responses.
--- | ---
UC-6: Secure Login | Students should be provided secure sign on through institution credentials.
--- | ---
UC-7: Setting Deadline Reminders | When a lecturer sets a reminder about a deadline, the system should log it and notify students at the appropriate time.
--- | ---
UC-8: Monitoring Dashboard | The system should provide a dashboard to monitor latency, errors, and uptime to the system maintainer. If the system goes down, the dashboard should show that the system is down. If the system is undergoing peak load and has high latency, the dashboard should also reflect that. Errors should also be logged and accounted for so that the system maintainer can take action from the dashboard's data.
--- | ---
UC-9: Event Date Change | If an event gets rescheduled, the data source system should reflect the change due to the data being synced every set interval.
