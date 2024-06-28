# UС-002: Detect deletion of important files from the Document folder

## TOC

- [UС-002: Detect deletion of important files from the Document folder](#uс-002-detect-deletion-of-important-files-from-the-document-folder)
  - [TOC](#toc)
  - [Playbook Overview](#playbook-overview)
  - [Known issues](#known-issues)
    - [Source justifying the exception](#source-justifying-the-exception)
  - [Phase 1 - Offense Analysis](#phase-1---offense-analysis)
    - [Task 1 - Event Analysis](#task-1---event-analysis)
    - [Task 2 - Check Violation](#task-2---check-violation)
    - [Task 3 - Search remote communication](#task-3---search-remote-communication)
    - [Task 4 - Reputation check](#task-4---reputation-check)
    - [Task 5 - Create a message from SOC Analyst to SOC Analyst](#task-5---create-a-message-from-soc-analyst-to-soc-analyst)
  - [Phase 2 - Eliminating the consequences of the incident](#phase-2---eliminating-the-consequences-of-the-incident)
    - [Task 1 - Create an appeal from SOC Analyst to customer's IS](#task-1---create-an-appeal-from-soc-analyst-to-customers-is)
    - [Task 2 - Close offense](#task-2---close-offense)

## Playbook Overview

This playbook contains an incident response plan with the identifier UC-002.

## Known issues

### Source justifying the exception

If the action is performed by a user with the Username "ddraguncov", such actions are considered legitimate and are not subject to further analysis.

## Phase 1 - Offense Analysis

### Task 1 - Event Analysis

- **Task Name** : Event Analysis
- **Performer** : SOC L1
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Open Offense, change its status to Protected
2. Assign Offense to your account.
3. Go to the list of events and define the values of the fields:

- [ ] Log Source
- [ ] Source IP
- [ ] Destination IP
- [ ] Username
- [ ] Count of events

* **Incident Fields** : Values of the fields from p.3
* **Comments** : -

### Task 2 - Check Violation

- **Task Name** : Check Violation
- **Performer** : SOC L1
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1.  Check that the Username is not an exception, namely, it is not "ddraguncov"
2.  **Scenario 1 - Username is an exception to this incident**:

- [ ] Add note:

```
The action from the user "ddraguncov" was approved within the project #328-17
```

- [ ] Go to close Offense with the Close Reason = Non-issue

**Scenario 2 - Actual triggering**:

- [ ] Go to the next task

* **Incident Fields** : The processing scenario is selected
* **Comments** : --

### Task 3 - Search remote communication

- **Task Name** : Search by Destionation IP
- **Performer** : SOC L1
- **Required** : Mandatery
- **Due Date** : No
- **Instructions** :

1. Go to Activity log.
2. Add the filter Destination IP contains any of <Specify Destination IP>.
3. Perform a search for the last 3 hours grouped by event name.
4. Determine if there are any events that indicate a previous remote connection to the host.

5. **Scenario 1 - No suspicious connections detected**:

- [ ] Go to the next task

**Scenario 2 - Suspicious connections are detected**:

- [ ] Add note:

```
The historical search for events at this IP address showed:
<Write the time of event-1> - <Event name-1> - <Source IP-1> - <Destination-1> - <Log Source-1>
<Write the time of event-2> - <Event name-2> - <Source IP-2> - <Destination-2> - <Log Source-2>...
```

- [ ] Go to the next task

* **Incident Fields** : The processing scenario is selected
* **Comments** : --

### Task 4 - Reputation check

- **Task Name** : Reputation check
- **Performer** : SOC L1
- **Required** : Optional (pass condition: scenario 1 in the previous task)
- **Due Date** : No
- **Instructions** :

1. Analyze the Source IP reputation from the remote connection event on the IBM X-Force resource (https://exchange.xforce.ibmcloud.com/)
2. Analyze the reputation of the Source IP from the remote connection event using Virus Total (https://www.virustotal.com/).
3. Perform a reputation check using Cisco Talos (https://talosintelligence.com/reputation_center/).
4. Add a note if the reputation is negative:

```
The reputation of IP <ip_address> is negative according to the database <Enter database name>.
<Link to the report
```

- **Incident Fields** : The reputation of the Source IP is determined
- **Comments** : -

### Task 5 - Create a message from SOC Analyst to SOC Analyst

- **Task Name** : Create a message from SOC Analyst to SOC Analyst
- **Performer** : SOC L1\SOC Analyst
- **Required** : Optional (pass condition: false positive)
- **Due Date** : No
- **Instructions** :

1. Create a request with parameters according to the current workflow for the Customer. The request should contain the context of the detected potential incident and a clear statement of work for the Customer (clarification of legitimacy for the detected activity, blocking of the OS, host isolation, etc.)
2. Change the status of Offense to Follow Up, add a note with the number of the request.

- **Incident Fields** : Note
- **Comments** : --

## Phase 2 - Eliminating the consequences of the incident

### Task 1 - Create an appeal from SOC Analyst to customer's IS

- **Task Name** : Create an appeal from SOC Analyst to customer's IS
- **Performer** : SOC Analyst\Customer's IS
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Create a request to the customer's IS with information about the incident and a request for response, such as blocking the host, terminating the remote connection to the host, recovering deleted data, etc.
2. Change the Offense status to Follow Up, add a note with the request number.

- **Incident Fields** : Note
- **Comments** : --

### Task 2 - Close offense

- **Task Name** : Close offense
- **Performer** : SOC-L1\SOC Analyst
- **Required** : Mandatory
- **Due Date** : No
- **Instructions** :

1. Ensure that the status of all incident-related requests is Completed.
2. Close the Offense according to the Close Reason in the scenario\close as Resolved.

- **Incident Fields** : Closed Offense.
- **Comments** : --
