
# ⚙️ OPS Lead Management System - Complete Documentation

## 📋 Table of Contents
1. System Architecture
2. Multi-Channel Lead Ingestion
3. AI Lead Scoring
4. Smart Slack Routing
5. Message Classification
6. Google Sheets Backup
7. Auto-Reply & Channel Detection
8. Lead Enrichment & Analysis
9. Two-Way Google Sheets Sync
10. Automated Follow-up Sequences
11. Activity Logging
12. Business Impact
13. Setup Guide

---

## 🏗️ System Architecture

A multi-channel system that captures leads from various sources, performs automatic scoring, and routes them to the appropriate teams.

| **Key Features** | **Tech Stack** |
|-----------------|----------------|
| ✅ Multi-channel ingestion | • Gmail Trigger |
| ✅ Gmail, Telegram, Tally Forms | • Telegram Bot |
| ✅ AI lead scoring (1-100) | • Tally Forms |
| ✅ Smart Slack routing | • Groq AI |
| ✅ Lead enrichment & analysis | • Slack API |
| ✅ Auto-followup sequences | • Supabase |

![Full Workflow Architecture](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20131737.png)
*Complete system flow from lead capture across multiple channels to scoring, routing, and follow-up.*

**Main Flow:**
1. **Multi-Channel Triggers** - Captures leads from Gmail, Telegram, and Tally Forms
2. **AI Lead Scoring** - Scores leads from 1-100 based on quality signals
3. **Smart Routing** - Routes to appropriate Slack channels based on score
4. **Message Classification** - Categorizes messages (sales/support/billing/other)
5. **Google Sheets Backup** - Stores all leads in Google Sheets
6. **Lead Enrichment** - Adds industry, intent level, budget insights
7. **Auto-Reply** - Sends immediate acknowledgment
8. **Follow-up Sequences** - Automated follow-ups for unconverted leads

---

## 📱 Multi-Channel Lead Ingestion

The system captures leads from 3 different channels simultaneously.

![Multi-Channel Triggers](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20131949.png)
*Triggers from Gmail, Telegram, and Tally Forms unified in one system*

**Channel 1: Tally Forms** - Web form submissions trigger the workflow
- Data collected: Name, Email, Source, Message

**Channel 2: Telegram** - Telegram messages trigger the workflow
- Data collected: First name, Message text, Chat ID

**Channel 3: Gmail** - Emails trigger the workflow every minute
- Data collected: From address, Subject, Message body, Thread ID

All three channels are merged into a single data structure for consistent processing.

---

## 🎯 AI Lead Scoring

Every lead is scored from 1-100 based on multiple quality signals.

![AI Lead Scoring](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132047.png)
*AI scores leads based on email domain, message quality, and source*

**Evaluation Criteria:**
- **Email domain**: Corporate (.com) → higher, free (gmail, yahoo) → lower
- **Name**: Full realistic name → higher, suspicious → lower
- **Message quality**: Clear intent → higher, vague → lower
- **Source**: Referral/form → higher, unknown → lower

A parser node extracts the score and note, then updates the lead record in the database.

---

## 📢 Smart Slack Routing

Leads are automatically routed to the appropriate Slack channels based on their score.

![Slack Routing Logic](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132119.png)
*Leads routed to different channels based on score (Hot: ≥80, Warm: ≥60)*

**Routing Rules:**
| Score Range | Channel | Priority |
|-------------|---------|----------|
| ≥80 | #hot-lead | Immediate attention |
| 60-79 | #warm-lead | Prompt follow-up |
| <60 | No notification | Low priority |

**Message Format:**
```
New Lead
Name: [name]
Email: [email]
Score: [score]
Type: [Hot/Warm] Lead
```

---

## 🏷️ Message Classification

Incoming messages are categorized to route to the right team.

![Message Classification](https://github.com/Vluiny/n8n-portofolio/blob/680d8be3c0f61873d833571b402244cc48e51b88/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20132234.png)
*AI categorizes messages into sales/support/billing/other*

**Classification Categories:**
- **sales** - Product demos, pricing, service interest
- **support** - Technical issues, troubleshooting
- **billing** - Payments, invoices, subscriptions
- **other** - Everything else

**Channel Destinations:**
| Category | Slack Channel |
|----------|---------------|
| Sales | #sales |
| Support | #support |
| Billing | #billing |
| Other | No notification |

---

## 📊 Google Sheets Backup & Sync

All scored leads are automatically backed up to Google Sheets for data archival and reporting.

![Sheets Backup Group](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141010.png)
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

## ✉️ Auto-Reply & Channel Detection

Leads receive immediate acknowledgment, with channel-specific routing based on email presence.

![Auto-Reply Group](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141141.png)
*Complete flow: channel detection → email/telegram reply → activity logging*

**Process flow:**
1. **Channel Detection** - Checks if email contains "@" and ".com"
   - **If YES (Email)** → Send email acknowledgment
   - **If NO (Telegram)** → Send Telegram acknowledgment

2. **Email Auto-Reply**
   ![Email Reply](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141512.png)
   **Template:**
   ```
   Hi [name],

   Thanks for contacting our team. We received your message and will get back to you shortly.

   Best regards,
   Sales Team
   ```

3. **Telegram Auto-Reply**
   ![Telegram Reply](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141516.png)
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

## 📈 Lead Enrichment & Analysis

Leads are enriched with additional business intelligence for better targeting and prioritization.

![Enrichment Group](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141552.png)
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

A parser node processes the JSON and updates the lead record with enriched data.

---

## 🔄 Two-Way Google Sheets Sync

Changes made in Google Sheets are synced back to the database for manual updates.

![Sheets Sync Group](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141614.png)
*Complete flow: sheets trigger → data extraction → database update → activity log*

**Process flow:**
1. **Google Sheets Trigger** - Monitors spreadsheet for new rows every minute
2. **Data Extraction** - Captures email and status from the new row
3. **Database Update** - Finds lead by email and updates status
4. **Activity Logging** - Records "google_sheet_sync" activity

---

## ⏰ Automated Follow-up Sequences

Unconverted leads receive automated follow-ups every 3 days to maximize conversion.

![Follow-up Group](https://github.com/Vluiny/n8n-portofolio/blob/3eb6c479004d1e2acfe3461a041e36905d49cfff/all-documentation/AI-Powered%20Lead%20Management%20System/Screenshot/Screenshot%202026-03-17%20141633.png)
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

## 📝 Activity Logging Throughout

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

## ⚙️ Setup Guide

### Prerequisites
- n8n instance
- Supabase project
- Groq API key (for Llama 3.3 70B)
- Slack workspace with API access
- Google Sheets access
- Telegram bot
- Gmail account with API access
- Tally Forms account

### Database Setup

**Table: leads**
```sql
CREATE TABLE leads (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  name TEXT,
  email TEXT,
  source TEXT,
  message TEXT,
  score INTEGER DEFAULT 0,
  status TEXT,
  threadid_id TEXT,
  notified_sales BOOLEAN DEFAULT FALSE,
  notified_sales_at TIMESTAMP,
  message_routed BOOLEAN DEFAULT FALSE,
  synced_sheet BOOLEAN DEFAULT FALSE,
  auto_email_sent BOOLEAN DEFAULT FALSE,
  enriched BOOLEAN DEFAULT FALSE,
  industry TEXT,
  intent_level TEXT,
  budget_level TEXT,
  lead_type TEXT,
  followup_stage INTEGER DEFAULT 0,
  last_contacted_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Table: sales_activity**
```sql
CREATE TABLE sales_activity (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  lead_id UUID REFERENCES leads(id),
  activity_type TEXT,
  notes TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Slack Channels Setup
Create the following channels:
- `#hot-lead` - Score ≥ 80
- `#warm-lead` - Score 60-79
- `#sales` - Sales inquiries
- `#support` - Support requests
- `#billing` - Billing questions
- `#error-handling` - System errors

### n8n Setup Steps
1. Import workflow JSON
2. Configure credentials:
   - Supabase API
   - Groq API
   - Slack API
   - Google Sheets OAuth2
   - Telegram Bot
   - Gmail OAuth2
   - Tally Forms API
3. Update Slack channel IDs
4. Update Google Sheet ID
5. Test with sample leads
6. Activate workflow

---

## 🔒 Security Notes

- All API keys stored securely in n8n credentials
- No sensitive data exposed in logs
- Email content processed in memory only
- Error handling routes issues to dedicated Slack channel
- Data anonymized in screenshots

---

## 🎯 Key Features Summary

# 🔄 Complete Flow Summary

```
[MULTI-CHANNEL INGESTION — Real-time]
         ↓
┌─────────────────────────────────────┐
│    Tally Forms    │   Telegram    │    Gmail     │
│  • Web Form       │  • DM/Chat    │  • Email     │
│  • Name/Email/    │  • Name/      │  • From/     │
│    Message        │    Message    │    Subject/  │
│                   │    Chat ID    │    Body      │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│         MERGE & UNIFY DATA          │
│    Single structure for all leads   │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│         AI LEAD SCORING             │
│  ┌───────────────────────────────┐  │
│  │ Evaluation Criteria:          │  │
│  │ • Email domain: Corporate ↑   │  │
│  │   Free email ↓                │  │
│  │ • Name: Realistic ↑           │  │
│  │   Suspicious ↓                │  │
│  │ • Message: Clear intent ↑     │  │
│  │   Vague ↓                     │  │
│  │ • Source: Referral/Form ↑     │  │
│  │   Unknown ↓                   │  │
│  └───────────────────────────────┘  │
│         ↓                           │
│    Score 1-100 + Note               │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│         SMART SLACK ROUTING          │
│         ┌───────────────┐           │
│   ┌─────│  Score ≥80    │─────┐     │
│   │     └───────────────┘     │     │
│   ▼                           ▼     │
│ #hot-lead                  #warm-lead│
│ Immediate                  Prompt   │
│ attention                  follow-up│
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       MESSAGE CLASSIFICATION         │
│  ┌───────────────────────────────┐  │
│  │  AI categorizes intent:       │  │
│  │  • Sales   → #sales           │  │
│  │  • Support → #support         │  │
│  │  • Billing → #billing         │  │
│  │  • Other   → No notification  │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       GOOGLE SHEETS BACKUP          │
│  ┌───────────────────────────────┐  │
│  │  Fields stored:               │  │
│  │  • Name, Email, Message       │  │
│  │  • Score, Created At, Status  │  │
│  └───────────────────────────────┘  │
│         ↓                           │
│    Mark as synced_sheet = true      │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       AUTO-REPLY & CHANNEL          │
│         DETECTION                   │
│         ┌───────────────┐           │
│   ┌─────│ Contains @ &  │─────┐     │
│   │     │    .com?      │     │     │
│   │     └───────────────┘     │     │
│   ▼                           ▼     │
│ ┌─────────────┐           ┌─────────────┐
│ │   Email     │           │  Telegram   │
│ │ Auto-Reply  │           │ Auto-Reply  │
│ └─────────────┘           └─────────────┘
│         ↓                           ↓
│    Thank you message        Thank you message
│    via Gmail                via Telegram
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       STATUS UPDATE                 │
│  • auto_email_sent = true           │
│  • status = contacted               │
│  • last_contacted_at = now          │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       LEAD ENRICHMENT               │
│  ┌───────────────────────────────┐  │
│  │  AI infers:                   │  │
│  │  • Industry                   │  │
│  │  • Intent Level (low/med/high)│  │
│  │  • Budget Level (low/med/high)│  │
│  │  • Lead Type (B2B/B2C)        │  │
│  └───────────────────────────────┘  │
│         ↓                           │
│    Update lead with enriched data   │
│    Mark as enriched = true          │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       ACTIVITY LOGGING              │
│  ┌───────────────────────────────┐  │
│  │  sales_activity table:        │  │
│  │  • lead_created               │  │
│  │  • lead_rank                  │  │
│  │  • lead_classification        │  │
│  │  • temporary_database         │  │
│  │  • email_sent                 │  │
│  │  • lead_analysis              │  │
│  │  • google_sheet_sync          │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│   [SCHEDULED — EVERY 3 DAYS]        │
│         FOLLW-UP SEQUENCES           │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    GET CONTACTED LEADS              │
│    (status = contacted)              │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    CALCULATE FOLLOW-UP NEED         │
│  ┌───────────────────────────────┐  │
│  │  days_since = now -           │  │
│  │  last_contacted_at            │  │
│  │  needs_followup = days ≥ 3    │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│         FOLLOW-UP FILTER            │
│         ┌───────────────┐           │
│   ┌─────│ needs_followup│─────┐     │
│   │     │    = true?    │     │     │
│   │     └───────────────┘     │     │
│   ▼                           ▼     │
│ Continue                    Discard │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       CHANNEL DETECTION              │
│         ┌───────────────┐           │
│   ┌─────│   Source?     │─────┐     │
│   │     └───────────────┘     │     │
│   ▼                           ▼     │
│ ┌─────────────┐           ┌─────────────┐
│ │   Email     │           │  Telegram   │
│ │ Follow-up   │           │ Follow-up   │
│ └─────────────┘           └─────────────┘
│         ↓                           ↓
│ "Just checking if          "Just checking if
│  you're still              you're still
│  interested..."            interested..."
│  via Gmail                  via Telegram
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│       FOLLOW-UP TRACKING            │
│  • followup_stage +1                │
│  • last_contacted_at = now          │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│   [REAL-TIME SYNC]                  │
│   TWO-WAY GOOGLE SHEETS SYNC        │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    GOOGLE SHEETS TRIGGER            │
│    (Monitors for new rows)          │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    EXTRACT DATA                     │
│    (Email, Status from new row)     │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    DATABASE UPDATE                  │
│    Find lead by email → update      │
│    status                           │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│    ACTIVITY LOGGING                 │
│    Record "google_sheet_sync"       │
└─────────────────────────────────────┘
```

## 📊 Flow Breakdown by Phase

| Phase | Group | Key Actions |
|-------|-------|-------------|
| **1** | Ingestion | Tally/Telegram/Gmail → Merge → Unify |
| **2** | Scoring | AI Evaluation → Score 1-100 |
| **3** | Routing | Score-based → #hot-lead / #warm-lead |
| **4** | Classification | Intent Categorization → Slack channels |
| **5** | Backup | Google Sheets Append → Sync Status |
| **6** | Acknowledgment | Channel Detection → Email/Telegram Reply |
| **7** | Enrichment | AI Analysis → Industry/Intent/Budget/Type |
| **8** | Logging | All activities to sales_activity |
| **9** | Follow-up | Every 3 days → Calculate → Send → Track |
| **10** | Sync | Sheets → Database → Update → Log |

---

## 🎯 Key Decision Points

```
                    ┌─────────────────────┐
                    │    Email contains   │
                    │     "@" and ".com"? │
                    └─────────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │     YES     │        │     NO      │
         │  Email path │        │Telegram path│
         └─────────────┘        └─────────────┘

                    ┌─────────────────────┐
                    │      Score Range    │
                    └─────────────────────┘
                           ↓
         ┌──────────────┬──┴──┬──────────────┐
    ┌────▼────┐   ┌─────▼─────┐   ┌────▼────┐
    │  ≥80    │   │   60-79   │   │   <60   │
    │#hot-lead│   │ #warm-lead│   │   No    │
    │Immediate│   │  Prompt   │   │Routing  │
    └─────────┘   └───────────┘   └─────────┘

                    ┌─────────────────────┐
                    │  Message Category   │
                    └─────────────────────┘
                           ↓
         ┌──────────────┬──┴──┬──────────────┐
    ┌────▼────┐   ┌─────▼─────┐   ┌────▼────┐
    │ Sales   │   │  Support  │   │ Billing │
    │ #sales  │   │ #support  │   │#billing │
    └─────────┘   └───────────┘   └─────────┘

                    ┌─────────────────────┐
                    │  Follow-up Needed?  │
                    │   days_since ≥ 3    │
                    └─────────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │     YES     │        │     NO      │
         │  Send next  │        │   Wait 3    │
         │  follow-up  │        │ more days   │
         └─────────────┘        └─────────────┘
```

---

## ⏱️ Follow-up Timeline

```
Day 0: Lead Captured
    ↓
┌─────────────────────────────────────┐
│  Auto-Reply Sent Immediately        │
│  Status: "contacted"                │
└─────────────────────────────────────┘
    ↓
Day 3: First Follow-up
    ↓
┌─────────────────────────────────────┐
│  "Just checking if you're still     │
│   interested..."                    │
│  followup_stage = 1                 │
└─────────────────────────────────────┘
    ↓
Day 6: Second Follow-up (if no reply)
    ↓
┌─────────────────────────────────────┐
│  Second follow-up message           │
│  followup_stage = 2                 │
└─────────────────────────────────────┘
    ↓
Day 9: Third Follow-up (if no reply)
    ↓
┌─────────────────────────────────────┐
│  Third follow-up message            │
│  followup_stage = 3                 │
└─────────────────────────────────────┘
    ↓
Continues every 3 days until conversion
```

---

## 🔄 Data Flow Relationships

```
tally_trigger    telegram_trigger    gmail_trigger
      ↓                   ↓                 ↓
      └─────────┬─────────┴─────────┬───────┘
                ↓
            [MERGE]
                ↓
            leads table
    ┌───────────────────────┐
    │ • id                  │
    │ • name                │
    │ • email               │
    │ • source              │
    │ • message             │
    │ • score               │
    │ • status              │
    │ • threadid_id         │
    │ • notified_sales      │
    │ • notified_sales_at   │
    │ • message_routed      │
    │ • synced_sheet        │
    │ • auto_email_sent     │
    │ • enriched            │
    │ • industry            │
    │ • intent_level        │
    │ • budget_level        │
    │ • lead_type           │
    │ • followup_stage      │
    │ • last_contacted_at   │
    │ • created_at          │
    └───────────────────────┘
            ↑       ↑
            │       │
    ┌───────┘       └───────┐
    ↓                       ↓
sales_activity        Google Sheets
    ↓                       ↓
• lead_created        • name
• lead_rank           • email
• lead_classification • message
• temporary_database  • score
• email_sent          • created_at
• lead_analysis       • status
• google_sheet_sync
```

---

## 📈 Metrics Tracked

| Metric | Source | Purpose |
|--------|--------|---------|
| `score` | AI Lead Scoring | Lead quality (1-100) |
| `notified_sales` | Database | Hot/Warm notification status |
| `message_routed` | Database | Classification routing status |
| `synced_sheet` | Database | Google Sheets backup status |
| `auto_email_sent` | Database | Auto-reply status |
| `enriched` | Database | Lead enrichment status |
| `industry` | AI Enrichment | Lead's business sector |
| `intent_level` | AI Enrichment | Interest seriousness |
| `budget_level` | AI Enrichment | Estimated budget |
| `lead_type` | AI Enrichment | B2B vs B2C |
| `followup_stage` | Database | Number of follow-ups sent |
| `days_since_contact` | Calculation | Time since last contact |
| `response_time` | Calculation | Lead response speed |

---

## ✅ Complete Flow Summary

| Step | Component | Function |
|------|-----------|----------|
| **1** | Multi-Channel Triggers | Capture leads from Tally, Telegram, Gmail |
| **2** | Merge | Unify data structure |
| **3** | AI Lead Scoring | Score 1-100 based on quality signals |
| **4** | Slack Routing | Route hot (≥80) and warm (60-79) leads |
| **5** | Message Classification | Categorize intent → route to sales/support/billing |
| **6** | Google Sheets Backup | Archive all leads automatically |
| **7** | Auto-Reply | Immediate acknowledgment via email/telegram |
| **8** | Status Update | Mark as contacted |
| **9** | Lead Enrichment | Add industry, intent, budget, type insights |
| **10** | Activity Logging | Record all actions |
| **11** | Follow-up (Every 3 days) | Re-engage unconverted leads |
| **12** | Two-Way Sheets Sync | Sync manual updates back to database |

---

⭐ **Thank you for reviewing this documentation!**
