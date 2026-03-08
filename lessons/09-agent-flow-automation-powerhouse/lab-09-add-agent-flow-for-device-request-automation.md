# Lab 09 – Add an Agent Flow for Automation and Enhance Topic Capabilities

## Lab Objective

In this lab, we enhance the Request device topic by adding an agent flow that automates the device request process end to end.

The flow will:

- Accept the selected device ID from the adaptive card
- Accept the user display name
- Accept optional additional comments
- Retrieve the selected SharePoint item from the Devices list
- Send an email notification to the manager
- Return the selected device model back to the agent
- Display a confirmation message to the user
- Redirect the conversation to the End of Conversation topic

This lab turns the topic into a fully operational workflow rather than only a conversational experience.

---

## Use Case

As a manager of an employee  
I want to receive device requests  
So that I can review the device requested by the employee  

---

## Prerequisites

- Devices SharePoint list created in the Contoso IT SharePoint site (Lesson 00)
- Contoso Helpdesk Agent available (Lesson 06)
- Available devices topic created (Lesson 07)
- Request device topic with adaptive card created (Lesson 08)
- Access to SharePoint and Office 365 Outlook connector connections

---

## 9.1 Create an Agent Flow

We will create an agent flow that retrieves the selected device item from SharePoint and sends an email containing the request details.

### Create the Flow from the Request Device Topic

1. Open the Contoso Helpdesk Agent
2. Open the Topics tab
3. Open the Request device topic
4. Scroll to the Ask with adaptive card node
5. Add a new node below it
6. Select Add a tool
7. In the Basic tools tab, select New Agent flow

The Agent Flow Designer opens with two default actions:

- Trigger: When an agent calls the flow
- Action: Respond to the agent

---

## Configure Trigger Inputs

Select the trigger node and add three inputs.

### Input 1 – DeviceSharePointId

1. Select Add an input
2. Choose Text
3. Name the input:

DeviceSharePointId

### Input 2 – User

1. Select Add an input
2. Choose Text
3. Name the input:

User

### Input 3 – AdditionalComments

1. Select Add an input
2. Choose Text
3. Name the input:

AdditionalComments

Make this field optional:

1. Select the ellipsis next to AdditionalComments
2. Select Make the field optional

---

## Add SharePoint Get Item Action

1. Select the plus icon below the trigger
2. Search for:

Get item

3. Select the SharePoint connector action Get item

### Rename the Action

Rename the action to:

Get Device

### Configure Inputs

- Site Address: Select the Contoso IT SharePoint site
- List Name: Select the Devices list

### Configure the Id Field Using Dynamic Content

1. Select the dynamic content icon beside the Id field
2. Search for:

id

3. Select:

DeviceSharePointId

4. Select Add

### Limit Columns by View

1. Select Show all
2. In Limit Columns by View, select:

All Items

---

## Add Send Email Action

1. Select the plus icon below the Get Device action
2. Search for:

send an email

3. Select the Office 365 Outlook connector action Send an email (V2)

If needed, create a connection by signing in.

### Rename the Action

Rename the action to:

Send an email to manager

### Configure Core Inputs

- To: Select yourself for testing
- Subject:

Request type: new device

- Body:

Hi,

New device requested from

Manufacturer:
Model:
Link to item in SharePoint
Additional comments from:

This is an automated email from Contoso Helpdesk Agent

---

## Insert Dynamic Content into Email Body

### Insert User Name

After the line:

New device requested from

Insert dynamic content:

User

from the trigger.

### Insert Manufacturer

Next to:

Manufacturer:

Insert dynamic content:

Manufacturer

from the Get Device action.

### Insert Model

Next to:

Model:

Insert dynamic content:

Model

from the Get Device action.

---

## Convert SharePoint Link to a Hyperlink

We will make the "Link to item in SharePoint" text clickable.

1. Switch the email body to HTML mode using the code editor toggle
2. Before the text "Link to item in SharePoint", insert:

<a href="

3. Insert dynamic content for:

Link to item

from the Get Device action

4. After the dynamic content, insert:

">

5. After the visible text, insert:

</a>

6. Toggle back from code view if needed

---

## Insert Additional Comments with Conditional Logic

We want the email to show:

- The user’s comment if provided
- `None` if no comment was submitted

### Insert User Name for the Additional Comments Section

In the line:

Additional comments from:

Insert dynamic content:

User

from the trigger

### Insert Expression for Comment Value

After the colon, insert an expression using the Function tab:

if(empty())

Then build the full expression by inserting the AdditionalComments trigger input as the parameter.

The logic should be:

- If AdditionalComments is empty → return `'None'`
- Otherwise → return AdditionalComments

Build the expression so it evaluates to:

if(empty(AdditionalComments), 'None', AdditionalComments)

Add it to the Body field.

### Format Final Line

Format the final line:

This is an automated email from Contoso Helpdesk Agent

in italics.

---

## Configure the Respond to the Agent Action

We now return the selected device model back to the topic.

1. Scroll up to the Respond to the agent action
2. Select it
3. Add a Text output

Output name:

ModelValue

### Set the Output Value

For the output value, insert dynamic content:

Model

from the Get Device action

Save draft.

---

## Rename and Describe the Flow

1. Open the Overview tab
2. Select Edit

Set:

Flow name:

Send device request email

Description:

This flow starts when an agent manually triggers it and provides device and user details. It retrieves device information from a SharePoint list using the provided device ID. After successfully getting the device details, it sends an email to a manager with the request information, and sends a value back to the agent.

Save changes.

---

## Publish the Agent Flow

1. Go to the Designer tab
2. Select Publish
3. Wait for the confirmation message

The agent flow is now available for use inside topics.

---

## 9.2 Add the Agent Flow to the Request Device Topic

1. Return to Agents
2. Open Contoso Helpdesk Agent
3. Open Topics
4. Open the Request device topic
5. Scroll to the Ask with adaptive card node
6. Add a new node below it
7. Select Add a tool
8. Choose the published flow:

Send device request email

Its description should also be visible in the picker.

---

## Map Topic Variables to Flow Inputs

### DeviceSharePointId

1. Select the ellipsis for the DeviceSharePointId input
2. Choose variable
3. Select:

deviceSelectionId

This comes from the adaptive card output.

### User

1. Select the ellipsis for the User input
2. Choose variable
3. Open the System tab
4. Select:

User.DisplayName

### AdditionalComments

1. Expand Advanced inputs
2. Select the ellipsis for AdditionalComments
3. Choose Formula
4. Expand the formula editor
5. Enter:

If(IsBlank(Topic.commentsId), "", Topic.commentsId)

6. Select Insert

This ensures:

- Empty string if no comment was provided
- Comment value if one exists

Save the topic.

---

## 9.3 Enhance the Topic with Confirmation and Conversation Closure

We will now add:

- A Send a message node
- A Topic management node

### Add Send a Message

1. Add a node below the agent flow
2. Select Send a message

Enter the message in parts.

Start with:

Thanks

Insert system variable:

User.DisplayName

Continue with:

. Your selected device,

Insert custom variable:

ModelValue

Finish with:

, has been submitted and will be reviewed by your manager.

The final message should confirm both the user and selected model.

---

## Add Topic Management

1. Add a node below the Send a message node
2. Select Topic management
3. Choose Go to another topic
4. Select:

End of Conversation

Save the topic.

---

## 9.4 Test the Agent

### Enable Cross-Topic Tracking

1. Open the test pane
2. Start a new session
3. Open the ellipsis menu
4. Enable:

Track between topics

This allows you to observe topic redirection in real time.

---

## Scenario 1 – Request a Device and Enter a Comment

### Test Steps

1. Enter:

I need a laptop

2. When the agent asks whether you want to request a device, enter:

yes please

3. The agent should redirect to the Request device topic and display the adaptive card
4. Select:

Surface Laptop 13

5. In the comments field, enter:

I need 16GB of RAM please

6. Select Submit Request
7. If prompted, allow connection access

### Expected Result

- The agent flow runs
- The agent displays a confirmation message including:
  - User display name
  - Selected model
- The conversation redirects to End of Conversation
- An email is received in your inbox
- The email contains:
  - Manufacturer
  - Model
  - Hyperlink to the SharePoint item
  - Additional comment text

Select the SharePoint hyperlink and confirm the item opens correctly.

---

## Scenario 2 – Request a Device Without Entering a Comment

### Test Steps

1. Refresh the test pane
2. Enter:

I need a laptop

3. Reply:

yes please

4. In the adaptive card, select:

Surface Laptop 15

5. Leave the comments field blank
6. Submit the request

### Expected Result

- The flow runs successfully
- Confirmation message is shown
- Email is received
- Additional comments field in the email displays:

None

This validates the conditional logic inside the flow.

---

## Scenario 3 – Do Not Request a Device

### Test Steps

1. Refresh the test pane
2. Enter:

I need a laptop

3. When the agent asks whether you want to request a device, respond:

No

### Expected Result

- The Available devices topic does not redirect to Request device
- The agent invokes the Goodbye topic
- The conversation follows the Goodbye topic flow

---

## Validation Checklist

- Agent flow created with trigger inputs:
  - DeviceSharePointId
  - User
  - AdditionalComments
- AdditionalComments marked optional
- SharePoint Get item action added and configured
- Email action added and configured
- Hyperlink added in email body
- Conditional expression added for comments
- Respond to the agent output configured with ModelValue
- Agent flow renamed, described, and published
- Flow added to Request device topic
- Topic inputs mapped correctly
- Confirmation message added
- Topic redirects to End of Conversation
- All three test scenarios validated

---

## Result

The Request device topic now supports full automation:

- User selects a device through an adaptive card
- The topic calls an agent flow
- The flow retrieves device details from SharePoint
- A notification email is sent to the manager
- The selected model is returned to the agent
- The user receives confirmation
- The conversation ends cleanly

This completes the transformation of the topic from an interactive request form into a complete business automation workflow.
