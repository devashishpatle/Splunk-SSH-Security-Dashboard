# Splunk-SSH-Security-Dashboard
A Splunk-based SSH Security Dashboard for monitoring and analyzing SSH authentication activity, including successful logins, failed login attempts, brute-force attacks, suspicious IP addresses, targeted usernames, and authentication patterns.

**📊 How to Open the Dashboard in Splunk**

**Step 1:** Start Splunk Enterprise

Open Splunk in your browser:

http://localhost:8000

Login with your Splunk username and password.

**Step 2:** Open Search & Reporting

From the Splunk home page:

Search & Reporting

**Step 3:** Open Dashboards

From the left-side menu, click:

Dashboards

# SPL-Queries-&-Dashboard-Panels

**1. Total SSH Event Count:**

**Description:**
Displays the total number of SSH events collected in Splunk. This provides an overall view of the volume of SSH activity.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count

**2. SSH Event Type Distribution**

**Description:**
Categorizes SSH activity by event type. It helps analysts understand how many events are successful logins, failed logins, multiple authentication attempts, and unauthenticated connections.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by event_type
