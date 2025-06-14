# Microsoft Teams Meeting Assistant - Technical Documentation

### ✅ GOAL

Build a **Microsoft Teams App** that:

1. **Records and transcribes** meetings in real-time.
2. **Extracts tasks**, assignments, and summaries using **AI**.
3. **Stores data** (dialogues, tasks, summaries) in a structured format.
4. Allows **progress tracking** of assigned tasks within Teams.

### 🔧 TOOLS & TECHNOLOGIES

#### 🧠 AI & NLP

* **Microsoft Azure OpenAI Service** – for summarization, task extraction.
* **Azure Speech-to-Text** – real-time transcription from Teams audio.
* **Microsoft Graph API** – to interact with Teams data and user info.
* **Azure Text Analytics** (optional) – sentiment analysis, entity recognition.

#### 🏗 Backend & Storage

* **Azure Functions / Logic Apps** – to process tasks asynchronously.
* **Azure Blob Storage or OneDrive/SharePoint** – to store transcript files or documents.
* **Azure Cosmos DB / SQL Server / Excel on OneDrive** – structured data storage.
* **Power Automate** – optionally for workflow integration.

#### 💻 Frontend (Teams App)

* **Microsoft Teams Toolkit for Visual Studio / VS Code**
* **React.js + Fluent UI** – for adaptive cards & UI inside Teams.
* **Bot Framework / Message Extensions** – for conversational UI.

#### 🔐 Security & Auth

* **Azure AD Authentication** – secure access and user info.

***

### 🧭 ROADMAP – PHASE BY PHASE

***

#### **PHASE 1: Planning & Design**

**Deliverables:**

* Define functional requirements:
  * Record meeting
  * Transcribe and store conversations
  * Extract tasks
  * Summarize discussion
  * Track task progress

**Design:**

* System architecture
* User roles & access (e.g., Admin, Manager, Team Member)
* UI/UX flows in Teams
* Decide storage format (e.g., Excel file per meeting vs. centralized DB)

***

#### **PHASE 2: Meeting Capture & Transcription**

**Tasks:**

* Register a **Teams Bot** with Microsoft Bot Framework.
* Use **Graph API** to:
  * Access meeting info
  * Capture meeting participants
  * Join meeting as a bot (if needed)

**Transcription:**

* Use **Azure Cognitive Services – Speech-to-Text**
  * Real-time transcription of the meeting audio.
  * Output stored in `.txt`, `.json`, or directly into database.
  * Add speaker diarization (who said what).

**Output Example:**

```json
jsonCopyEdit[
  { "timestamp": "10:02", "speaker": "Alice", "text": "John, please handle the budget review by Friday." },
  { "timestamp": "10:03", "speaker": "John", "text": "Sure, I’ll take care of that." }
]
```

***

#### **PHASE 3: AI Summarization & Task Extraction**

**Steps:**

*   Send transcript to **Azure OpenAI** or **ChatGPT API** with prompt:

    ```
    arduinoCopyEdit"Summarize the meeting and extract any tasks assigned. Return in structured JSON format."
    ```

**Sample Output:**

```json
jsonCopyEdit{
  "summary": "Discussion on Q2 budget, new product launch and task distribution.",
  "tasks": [
    { "task": "Budget review", "assignedBy": "Alice", "assignedTo": "John", "dueDate": "Friday", "status": "Not Started" }
  ]
}
```

**Store this in:**

* **Excel file** (via Microsoft Graph API to OneDrive)
* or in **Azure SQL/Cosmos DB**

***

#### **PHASE 4: Task Management UI in Teams**

**Create a Teams Tab or Messaging Extension:**

* **UI to show:**
  * Meeting Summary
  * Assigned Tasks (filtered by user)
  * Task status (Editable: In Progress, Done)

**Use:**

* **React + Fluent UI**
* **Graph API** for user identity
* **Adaptive Cards** for lightweight UI

***

#### **PHASE 5: Notifications & Progress Tracking**

**Add Features:**

* **Daily/Weekly Reminders** via Teams bot
* **User can update task status** via chat or card buttons
* **Notifications to assigner** on status changes

***

#### **PHASE 6: Export & Reports**

* Option to export:
  * Tasks to Excel
  * Summary to PDF or OneNote
* Dashboard (Power BI or inside Teams tab) for:
  * Meeting trends
  * Task completion rates
  * Participation metrics

***

### 🧪 TESTING & DEPLOYMENT

#### Testing

* Unit test all backend services
* Test Teams app on developer tenant
* Test Azure transcription with real recordings
* Validate AI summary accuracy with human oversight

#### Deployment

* Host backend on Azure App Services or Azure Functions
* Publish Teams app through **Teams Admin Center**

***

### 🔐 SECURITY & COMPLIANCE

* Handle PII carefully (names, voices).
* Store data with encryption (Azure Key Vault).
* Enable role-based access controls (RBAC).
* Ensure compliance with Microsoft 365 data retention policies.

***

### 📚 OPTIONAL ENHANCEMENTS

* **Voice commands** during meeting ("Assistant, create a task...")
* **Integration with Planner or Outlook Tasks**
* **Multilingual support** (using Azure translation services)
* **Real-time transcription highlights during meeting**

***

### 📦 FINAL DELIVERABLES

| Module                 | Technology                      |
| ---------------------- | ------------------------------- |
| Teams Bot/App          | Bot Framework + Graph API       |
| Transcription          | Azure Speech-to-Text            |
| Summarization/AI Tasks | Azure OpenAI / ChatGPT API      |
| Data Storage           | Excel + Graph API / Azure SQL   |
| UI in Teams            | React + Fluent UI + Adaptive UI |
| Notifications          | Bot Framework + Power Automate  |

***



**Table of Contents**

1. Functional Requirements
2. Architecture Diagram
3. Data Flow Diagram
4. High-Level Design (HLD)
5. Low-Level Design (LLD)
6. Tools & Technologies
7. Step-by-Step Instructions
8. Code Samples
9. Deployment Instructions

***

### 1. Functional Requirements

#### Core Features:

* Join scheduled Microsoft Teams meetings automatically.
* Transcribe meeting conversation in real-time.
* Identify speaker using diarization.
* Extract task assignments and discussion summaries using AI.
* Store data in structured format (Excel, Azure SQL, etc.).
* Display data in a Teams tab or chat bot.
* Track task progress.
* Notify users of pending or updated tasks.

#### Functional Requirements:

* F1: Real-time meeting transcription
* F2: Speaker identification
* F3: Summary extraction (AI-based)
* F4: Task extraction (AI-based)
* F5: Task assignment management
* F6: Dashboard for viewing summaries and tasks
* F7: Notifications and reminders
* F8: Export capabilities (Excel, PDF)

***

### 2. Sample Architecture Diagram

**Components:**

* Microsoft Teams Client
* Teams Bot (Bot Framework)
* Azure Cognitive Services (Speech-to-Text)
* Azure OpenAI / ChatGPT
* Azure Function App
* Azure SQL Database / OneDrive Excel
* Azure Blob Storage (optional)

```
 +---------------------------+
 |   Microsoft Teams Client |
 +------------+--------------+
              |
              v
     +--------+--------+
     | Teams Bot (Node) |
     +--------+--------+
              |
     +--------v---------+
     | Azure Function   |
     +--------+---------+
              |
     +--------v---------+
     | Azure Speech     |
     +--------+---------+
              |
     +--------v---------+
     | Azure OpenAI     |
     +--------+---------+
              |
     +--------v---------+
     | Azure SQL/Excel  |
     +------------------+
```

***

### 3. Data Flow Diagram

```
User -> Teams Bot -> Azure Speech -> Transcript
Transcript -> Azure OpenAI -> Summary, Tasks
Summary + Tasks -> Azure SQL / Excel
User -> Teams Tab UI -> View + Update Tasks
Azure Function -> Bot -> Notifications
```

***

### 4. High-Level Design (HLD)

#### Modules:

* Teams Bot
* Meeting Capture Service
* AI Processor
* Task Database
* UI Dashboard in Teams
* Notification Engine

#### Interactions:

* Bot interacts with Graph API to get meeting info.
* Audio sent to Azure Speech.
* Text sent to OpenAI for analysis.
* Output stored and shown in UI.

***

### 5. Low-Level Design (LLD)

#### Teams Bot:

* Commands: `"show tasks"`, `"update task <id>"`
* Graph API integration for user info

#### Azure Function:

* Triggered post-meeting
* Handles transcript parsing, OpenAI call, and database insert

#### Database Schema (SQL):

```sql
Meetings(id, date, summary)
Tasks(id, meeting_id, assigned_by, assigned_to, task, due_date, status)
```

#### Excel Format:

* Sheet1: Summary
* Sheet2: Tasks with columns \[Assigned By, Assigned To, Task, Due Date, Status]

***

### 6. Tools & Technologies

* Microsoft Teams Toolkit
* Bot Framework SDK (Node.js)
* Azure Cognitive Services
* Azure OpenAI
* Azure SQL
* Azure Blob/OneDrive/Excel
* Fluent UI (Teams tab)
* Microsoft Graph API

***

### 7. Step-by-Step Instructions

#### Step 1: Setup Azure Services

* Create Speech-to-Text and OpenAI resource
* Create Azure Function App
* Create SQL DB or Excel on OneDrive

#### Step 2: Create the Bot

* Use Bot Framework to create Node.js bot
* Integrate Graph API for meetings access
* Register on Azure and Teams Admin Center

#### Step 3: Real-Time Transcription

* Bot joins Teams call or reads uploaded audio
* Audio sent to Azure Speech
* Store transcript in Blob or DB

#### Step 4: AI Processing

* Send transcript to OpenAI API

```python
prompt = f"Summarize the following meeting: {transcript}. Extract tasks in JSON."
```

* Store result in DB or Excel

#### Step 5: Teams Tab UI

* Use React + Fluent UI
* Display summary, tasks, allow status update

#### Step 6: Task Notifications

* Azure Function checks for due/updated tasks
* Sends notifications to Teams bot

#### Step 7: Deployment

* Use Teams Toolkit to deploy app
* Distribute via Teams Admin

***

### 8. Code Samples

#### Bot Sample (Node.js)

```javascript
const { TeamsActivityHandler } = require('botbuilder');
class MyBot extends TeamsActivityHandler {
  async onMessage(context, next) {
    const text = context.activity.text;
    if (text.includes('show tasks')) {
      // fetch tasks from DB
    }
    await next();
  }
}
```

#### Azure Function (Python)

```python
def main(req):
    transcript = req.get_json()
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "system", "content": "Extract tasks from transcript."},
                  {"role": "user", "content": transcript}]
    )
    json_output = json.loads(response.choices[0].message.content)
    # store in DB
```

#### Adaptive Card Example

```json
{
  "type": "AdaptiveCard",
  "body": [
    {"type": "TextBlock", "text": "Assigned Tasks"},
    {"type": "Input.Text", "id": "taskUpdate", "placeholder": "Update status..."}
  ],
  "actions": [{"type": "Action.Submit", "title": "Submit"}],
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "version": "1.3"
}
```

***

### 9. Demo Sample Walkthrough

#### Example Scenario:

* Meeting held on June 14
* Transcript generated via Azure Speech
* Transcript sent to Azure OpenAI
* AI returns:

```json
{
  "summary": "Budget and marketing discussion.",
  "tasks": [
    {"task": "Prepare Q3 forecast", "assignedTo": "Jane", "assignedBy": "Mark", "dueDate": "June 20"}
  ]
}
```

* Bot sends Jane a task card
* Jane updates status via Teams tab

***

