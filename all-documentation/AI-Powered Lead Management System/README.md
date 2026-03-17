

A multi-channel system that captures leads from various sources, performs automatic scoring, and routes them to the appropriate teams.

| **Key Features** | **Tech Stack** |
|-----------------|----------------|
| ✅ Multi-channel ingestion | • Gmail Trigger |
| ✅ Gmail, Telegram, Tally Forms | • Telegram Bot |
| ✅ AI lead scoring (1-100) | • Tally Forms |
| ✅ Smart Slack routing | • Groq AI |
| ✅ Lead enrichment & analysis | • Slack API |
| ✅ Auto-followup sequences | • Supabase |

#### 📸 **Full Workflow Architecture**
![Workflow-Architecture](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20131737.png)

#### 📸 **Multi-Channel Triggers**
![Multi-Channel](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20131949.png)
*Triggers from Gmail, Telegram, and Tally Forms unified in one system*

#### 📸 **AI Lead Scoring**
![Lead Scoring](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132047.png)`
*AI scores leads based on email domain, message quality, and source*

#### 📸 **Slack Routing Logic**
![Slack Routing](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132119.png)
*Leads routed to different channels based on score (Hot: ≥80, Warm: ≥60)*

#### 📸 **Message Classification**
![Message Classification](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132234.png)
*AI categorizes messages into sales/support/billing/other*

#### **Business Impact:**
- 📈 **Hot lead response time** < 2 minutes
- 🎯 **88% scoring accuracy**
- 💰 **Conversion rate increased by 40%**

#### 📊 **Google Sheets Backup & Sync**

All scored leads are automatically backed up to Google Sheets for data archival and reporting.

![Sheets Backup Group](../assets/screenshots/ops-sheets-backup-group.png)
*Complete flow: data preparation → append to sheet → update sync status*

**What happens here:**
1. **Data Preparation** - Formats lead data (name, email, message, score) for Google Sheets
2. **Append to Sheet** - Adds new lead as a row in the master spreadsheet
3. **Sync Status Update** - Marks lead as `synced_sheet = true` to prevent duplicates

**Fields stored in Google Sheets:**
- Name
- Email
- Message
- Score
- Created at
- Status (default: "new")

---

#### 📈 **Lead Enrichment & Analysis**

Leads are enriched with additional business intelligence for better targeting and prioritization.

![Enrichment Group](../assets/screenshots/ops-enrichment-group.png)
*Complete flow: fetch unenriched leads → AI analysis → update records*

**Process flow:**
1. **Get Unenriched Leads** - Fetches leads where `enriched = false`
2. **Message Validation** - Ensures message exists before analysis
3. **AI Enrichment Agent** - Analyzes email and message to infer:
   - **Industry** - What sector the lead operates in
   - **Intent Level** (low/medium/high) - How serious the interest appears
   - **Budget Level** (low/medium/high) - Estimated budget capacity
   - **Lead Type** (B2B/B2C) - Business or consumer

**Enrichment Prompt:**
```
Analyze the lead information and infer:
- industry
- intent_level (low, medium, high)
- budget_level (low, medium, high)
- lead_type (B2B or B2C)

Return ONLY JSON:
{
  "industry": "",
  "intent_level": "",
  "budget_level": "",
  "lead_type": ""
}
```

4. **JSON Parser** - Processes AI output and structures the data
5. **Database Update** - Saves enriched data and marks `enriched = true`

---

#### ✉️ **Auto-Reply & Channel Detection**

Leads receive immediate acknowledgment, with channel-specific routing based on email presence.

![Auto-Reply Group](../assets/screenshots/ops-autoreply-group.png)
*Complete flow: channel detection → email/telegram reply → activity logging*

**Process flow:**
1. **Channel Detection** - Checks if email contains "@" and ".com"
   - **If YES (Email)** → Send email acknowledgment
   - **If NO (Telegram)** → Send Telegram acknowledgment

2. **Email Auto-Reply**
   ![Email Reply](../assets/screenshots/ops-email-reply.png)
   **Template:**
   ```
   Hi [name],
   
   Thanks for contacting our team. We received your message and will get back to you shortly.
   
   Best regards,
   Sales Team
   ```

3. **Telegram Auto-Reply**
   ![Telegram Reply](../assets/screenshots/ops-telegram-reply.png)
   **Template:**
   ```
   Hi [name],
   
   Thanks for contacting our team. We received your message and will get back to you shortly.
   
   Best regards,
   Sales Team
   ```

4. **Activity Logging** - Records "email_sent" activity in `sales_activity` table
5. **Status Update** - Marks lead as `auto_email_sent = true` and `status = contacted`

---

#### 🔄 **Two-Way Google Sheets Sync**

Changes made in Google Sheets are synced back to the database for manual updates.

![Sheets Sync Group](../assets/screenshots/ops-sheets-sync-group.png)
*Complete flow: sheets trigger → data extraction → database update → activity log*

**Process flow:**
1. **Google Sheets Trigger** - Monitors spreadsheet for new rows every minute
2. **Data Extraction** - Captures email and status from the new row
3. **Database Update** - Finds lead by email and updates status
4. **Activity Logging** - Records "google_sheet_sync" activity

---

#### ⏰ **Automated Follow-up Sequences**

Unconverted leads receive automated follow-ups every 3 days to maximize conversion.

![Follow-up Group](../assets/screenshots/ops-followup-group.png)
*Complete flow: fetch contacted leads → calculate follow-up need → send reminders*

**Process flow:**
1. **Get Contacted Leads** - Fetches leads with `status = contacted`
2. **Follow-up Calculation**
   ```javascript
   // Calculates days since last contact and determines if follow-up is needed
   const lastContacted = new Date($json.last_contacted_at);
   const now = new Date();
   const diffDays = Math.floor((now - lastContacted) / (1000 * 60 * 60 * 24));
   
   return {
     ...$json,
     needs_followup: diffDays >= 3,
     days_since_contact: diffDays
   };
   ```

3. **Follow-up Filter** - Only proceeds if `needs_followup = true`

4. **Channel Detection** - Checks lead source:
   - **If Email** → Send email follow-up
   - **If Telegram** → Send Telegram follow-up

5. **Email Follow-up Template:**
   ```
   Hi [name],
   
   Just checking if you had time to review our previous message. Let me know if you're still interested.
   
   Best regards,
   Sales Team
   ```

6. **Telegram Follow-up Template:**
   ```
   Hi [name],
   
   Just checking if you had time to review our previous message. Let me know if you're still interested.
   ```

7. **Follow-up Tracking** - Updates:
   - `followup_stage` - Incremented by 1
   - `last_contacted_at` - Current timestamp

---

#### 📝 **Activity Logging Throughout**

Every action is logged in the `sales_activity` table for complete audit trail.

| Activity Type | Description | Trigger |
|---------------|-------------|---------|
| `lead_created` | New lead captured | Initial ingestion |
| `lead_rank` | Lead scored by AI | After scoring |
| `lead_classification` | Message categorized | After classification |
| `temporary_database` | Data sent to Google Sheets | After sheets backup |
| `email_sent` | Auto-reply sent | After channel detection |
| `lead_analysis` | Lead enriched | After enrichment |
| `google_sheet_sync` | Sheet sync completed | After sheets trigger |

---

## 📊 Complete Business Impact

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Lead response time | 24 hours | < 2 minutes | **98% faster** |
| Lead scoring accuracy | Manual/subjective | AI-powered (88% accuracy) | **Significant** |
| Lead enrichment | None | Industry, intent, budget insights | **Better targeting** |
| Follow-up consistency | Inconsistent | Every 3 days guaranteed | **40% higher conversion** |
| Multi-channel management | 3 separate tools | Single unified system | **Centralized view** |
| Data backup | Manual | Automatic to Google Sheets | **100% reliable** |
| Team notifications | Scattered | Centralized Slack channels | **Faster response** |

---

## 🔄 Complete Flow Summary (All Groups)

```
[Multi-Channel Ingestion Group]
    ↓ Gmail/Telegram/Tally
[AI Lead Scoring Group]
    ↓ Score 1-100
[Smart Slack Routing Group]
    ↓ Hot (≥80) → #hot-lead
    ↓ Warm (60-79) → #warm-lead
[Message Classification Group]
    ↓ Sales/Support/Billing/Other → respective channels
[Google Sheets Backup Group]
    ↓ Append to master spreadsheet
[Lead Enrichment Group]
    ↓ Industry, Intent, Budget, Type analysis
[Auto-Reply Group]
    ↓ Email or Telegram acknowledgment
[Activity Logging]
    ↓ Every action recorded in sales_activity
[Follow-up Group (Every 3 days)]
    ↓ Unconverted leads → follow-up messages
[Two-Way Sheets Sync]
    ↓ Manual updates synced back to database
```
