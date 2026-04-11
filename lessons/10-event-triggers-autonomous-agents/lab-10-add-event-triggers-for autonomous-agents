# Lab 10 – Add Event Triggers for Autonomous Agent Behavior

## Objective

Enable the IT Help Desk agent to respond automatically to new SharePoint tickets by:

- Triggering without user input  
- Processing ticket details  
- Sending acknowledgment emails using a tool  
- Completing the workflow autonomously  

---

## Prerequisites

- IT Help Desk agent created (previous labs)  
- SharePoint Tickets list available  
- Generative orchestration enabled  
- Copilot Studio environment with event triggers enabled  
- Outlook connector access  

---

## 10.1 Enable Generative AI and Create SharePoint Trigger

### Enable Generative AI

1. Open the agent in Copilot Studio  
2. Go to **Settings → Orchestration**  
3. Enable **Use generative AI orchestration** (if not already enabled)  
4. Save or close settings  

---

### Create Event Trigger

1. Navigate to **Overview → Triggers**  
2. Click **+ Add trigger**  
3. Search and select:

   **When an item is created (SharePoint)**  

---

### Configure Trigger

- **Trigger name:**  
  New Support Ticket Created in SharePoint  

- **Site Address:**  
  Contoso IT SharePoint site  

- **List Name:**  
  Tickets  

---

### Add Trigger Instructions

New Support Ticket Created in SharePoint: {Body}

Use the 'Acknowledge SharePoint Ticket' tool to generate the email body automatically and respond.

IMPORTANT: Do not wait for any user input. Work completely autonomously.


4. Click **Create trigger**  
5. Close the dialog  

---

## 10.2 Edit Trigger in Power Automate

1. In Triggers section, click **... → Edit in Power Automate**  
2. Ensure **New Designer** is enabled  
3. Select:
   **Sends a prompt to the specified copilot for processing**  

---

### Add Custom Expression

1. Remove existing body content  
2. Insert the following expression:

concat(
'Submitted By Name: ', first(triggerOutputs()?['body/value'])?['Author/DisplayName'],
'\nSubmitted By Email: ', first(triggerOutputs()?['body/value'])?['Author/Email'],
'\nTitle: ', first(triggerOutputs()?['body/value'])?['Title'],
'\nIssue Description: ', first(triggerOutputs()?['body/value'])?['Description'],
'\nPriority: ', first(triggerOutputs()?['body/value'])?['Priority/Value'],
'\nTicket ID : ', first(triggerOutputs()?['body/value'])?['ID']
)


3. Click **Add**  
4. Click **Publish**  

---

## 10.3 Create Email Acknowledgment Tool

1. Go to **Tools tab**  
2. Click **+ Add a tool → Connector**  
3. Select:
   **Send an email (V2) – Office 365 Outlook**  

---

### Configure Tool

- **Name:**  
  Acknowledge SharePoint ticket  

- **Description:**  
  This tool sends an email acknowledgment that a ticket has been received  

---

### Configure Inputs

#### To
- Description: Email address of ticket submitter  
- Identify as: Email  

#### Body
- Description: Acknowledgment confirming ticket receipt and response timeline  

4. Click **Save**  

---

## 10.4 Test the Trigger

### Start Testing

1. Navigate to **Overview tab**  
2. Click **Test Trigger**  

---

### Create Test Ticket

1. Open SharePoint Tickets list  
2. Click **+ Add new item**  
3. Enter:

- Title: Unable to connect to VPN  
- Description: Unable to connect to corporate WIFI network after recent update  
- Priority: Normal  

4. Save the item  

---

### Execute Trigger

1. Return to Copilot Studio  
2. Refresh Test Trigger panel  
3. Wait for trigger event  
4. Click **Start testing**  
5. Click **Allow** when prompted  

---

### Validate Execution

Verify:

- Trigger is activated  
- Agent receives payload  
- Email tool is invoked automatically  
- Workflow completes successfully  

---

### Verify Email

1. Check submitter’s inbox  
2. Confirm acknowledgment email is received  

---

### Review Activity Logs

1. Open **Activity tab** in Copilot Studio  
2. Validate:

- Trigger execution  
- Tool execution  
- End-to-end workflow  

---

## Result

The agent now:

- Detects new SharePoint tickets automatically  
- Processes ticket details  
- Sends acknowledgment emails  
- Operates without user interaction  

This completes the implementation of event-driven automation using triggers.
