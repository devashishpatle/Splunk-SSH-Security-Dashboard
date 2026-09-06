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

**Step 4:** Create the Dashboard

Create New Dashboard

**Step 5:** After clicking Create, you will see your empty dashboard.

Click:
Add Panel

You will get the panel menu

You will see options such as:

Events

Statistics Table

Line Chart

.

.

.

For your project, select the visualization according to the panel.

# SPL-Queries-&-Dashboard-Panels

**1. Total SSH Events:**

**Description:**
Counts all SSH events present in the Splunk index/source. This gives the analyst an overall view of SSH activity.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count

**2. Failed SSH Login Attempts**

**Description:**
Counts failed SSH authentication attempts. A large number of failures may indicate password guessing or unauthorized access attempts.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Failed SSH Login"
| stats count

**3. Successful SSH Login Attempts**

**Description:**
Displays the total number of successful SSH authentication events. This helps analysts understand how many SSH authentication attempts resulted in successful access.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Successful SSH Login"
| stats count

**4. Multiple Authentication Attempts**

**Description:**
Counts events involving multiple failed authentication attempts. These events can be useful for detecting repeated login activity.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Multiple Failed Authentication Attempts"
| stats count

**5. SSH Event Distribution**

**Description:**
Groups SSH events by their event type. This provides a high-level overview of successful logins, failed logins, and other authentication activity.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by event_type
| sort - count

**6. Most Active IP Addresses**

**Description:**
Identifies the top 10 source IP addresses generating the highest number of SSH events. High-volume source IPs can be investigated for suspicious or automated activity.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by id.orig_h
| sort - count
| head 10

**7. Most Targeted Usernames**

**Description:**
Identifies the usernames most frequently involved in SSH authentication activity. This can help identify accounts that are being repeatedly targeted.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by username
| sort - count
| head 10

**8. Failed Login Attempts by IP**

**Description:**
Shows the source IP addresses responsible for the highest number of failed SSH login attempts. This is an important investigation panel for identifying potentially malicious sources.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Failed SSH Login"
| stats count by id.orig_h
| sort - count
| head 10

**9. Top 5 Active IP Addresses**

**Description:**
Displays the five source IP addresses with the highest number of SSH events. This provides a quick summary of the most active sources.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by id.orig_h
| sort - count
| head 5

**10. Bottom 5 Less Active IP Addresses**

**Description:**
Displays the five IP addresses with the lowest number of SSH events. This is useful for comparing source activity levels.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by id.orig_h
| sort count
| head 5

**11. Brute-Force Attack Detection**

**Description:**
Correlates source IP addresses and usernames with repeated authentication attempts. A high number of attempts may indicate a possible SSH brute-force attack.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Multiple Failed Authentication Attempts"
| stats sum(auth_attempts) as total_attempts count by id.orig_h username
| sort - total_attempts

**12. Successful Login After Multiple Attempts**

**Description:**
Identifies successful SSH logins that required more than one authentication attempt. These events can be investigated for potentially suspicious authentication behavior.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
auth_success=true auth_attempts>1
| table ts id.orig_h username auth_attempts event_type

**13. IP Address + Username Analysis**

**Description:**
Correlates source IP addresses with targeted usernames. This helps analysts determine which accounts are being targeted by specific IP addresses.

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
| stats count by id.orig_h username
| sort - count
| head 20

**14. Failed SSH Login Details**

**Description:**
Provides detailed information about failed SSH authentication events.

The table contains:

Timestamp

Username

Source IP

Destination IP

Authentication attempts

**SPL Query:**
source="ssh_logs_new.json" host="Welcome" sourcetype="_json"
event_type="Failed SSH Login"
| table ts username id.orig_h id.resp_h auth_attempts
| sort - ts
