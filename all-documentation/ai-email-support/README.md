# 🤖 AI-Powered Email Support Automation - Complete Documentation

This project is a simulation of a production-ready AI-powered email automation system, built to demonstrate workflow automation, AI-assisted email processing, and intelligent response handling across various use cases.

**Data Tabels**
1. System Architecture
2. Trigger & Email Fetching
3. Filtering & Spam Detection
4. AI Classification
5. Priority Scoring Logic
6. Customer Database
7. Support Cases Management
8. AI Response Generation
9. SLA & Escalation
10. Logging & Monitoring
11. Setup Guide

---

## 🏗️ System Architecture

Complete system flow from Gmail trigger to AI-powered response.

![Full Workflow](https://github.com/Vluiny/n8n-portofolio/blob/03cbd6e6bcfe7d0234534d44404143782c3511af/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20173427.png)

**Main Flow:**
1. **Gmail Trigger** monitors new emails every minute
2. **Filter** removes spam and irrelevant messages
3. **AI Classification** determines intent, urgency, sentiment
4. **Customer Database** tracks all interactions
5. **Support Cases** are created and managed
6. **AI Response** generates professional replies
7. **All Activities** are logged for monitoring

---

## 📧 Trigger & Email Fetching

Gmail Trigger polls for new emails every minute, then fetches complete message details.

![Gmail Trigger](https://github.com/Vluiny/n8n-portofolio/blob/97622761ce8291b7a7565c77da8216d402290452/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-17%20112547.png)

**Configuration:**
```json
{
  "pollTimes": {
    "item": [{ "mode": "everyMinute" }]
  }
}
```

The system retrieves: `From`, `To`, `Subject`, `Body (text)`, `ThreadId`, and `MessageId`.

---

## 🚫 Filtering & Spam Detection

Emails are filtered to ensure only legitimate business emails are processed.

![Filter Node](https://github.com/Vluiny/n8n-portofolio/blob/4ae4eb6fc6e35d7a2b567da4a3ec396fd62f2588/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20205321.png)

**Filter Criteria (must meet ALL):**
- From cannot be empty
- Subject cannot be empty
- From does not contain "mailer-daemon"
- Size estimate > 50 bytes

Suspicious emails are flagged using a Text Classifier:

![Text Classifier](https://github.com/Vluiny/n8n-portofolio/blob/4ae4eb6fc6e35d7a2b567da4a3ec396fd62f2588/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20205154.png)

**Categories:**
- **SUS** - Phishing/spam attempts → sent to manual review
- **NORMAL** - Legitimate emails → proceed to AI classification

---

## 🧠 AI Classification

The core intelligence: AI analyzes email content and returns structured data.

![AI Classification Node](https://github.com/Vluiny/n8n-portofolio/blob/97622761ce8291b7a7565c77da8216d402290452/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-17%20112647.png)

**AI Output Example:**
```json
{
  "intent": "SUPPORT_REQUEST",
  "urgency": "MEDIUM",
  "sentiment": "NEUTRAL",
  "budget_detected": false,
  "priority_signal": "PRIORITY_3",
  "confidence_score": 85
}
```

A confidence check ensures quality:

![Confidence Check](https://github.com/Vluiny/n8n-portofolio/blob/4ae4eb6fc6e35d7a2b567da4a3ec396fd62f2588/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20205340.png)

- **Confidence > 60** → Continue to database
- **Confidence ≤ 60** → Send to manual review

---

## 📊 Priority Scoring Logic

Custom JavaScript calculates priority scores and determines escalation levels.

![Priority Scoring Node](https://github.com/Vluiny/n8n-portofolio/blob/dc43b2f1f00ca99b2b99efda61bb6d1a65d6c830/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20210158.png)

**Scoring Logic:**
```javascript
let score = 0;
if (urgency === "high") score += 40;
else if (urgency === "medium") score += 20;
if (sentiment === "angry") score += 30;
else if (sentiment === "negative") score += 15;
if (budget) score += 25;
```

**Escalation Levels:**
| Score | Level | SLA |
|-------|-------|-----|
| ≥90 | critical | 1 hour |
| ≥60 | high | 4 hours |
| ≥30 | medium | 24 hours |
| <30 | low | 72 hours |

---

## 👥 Customer Database

Customer records are managed in Supabase.

![Customer Database Flow](https://github.com/Vluiny/n8n-portofolio/blob/e9c5dad3f8e3ed87f5c0b03fd9bd98e5abc3db0f/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20205648.png)

**Process:**
1. Check if customer email exists
2. If **new** → create record with first_seen_at
3. If **existing** → update last_seen_at and increment total_cases

---

## 📋 Support Cases Management

Each email becomes a support case with full tracking.

![Create Support Case](https://github.com/Vluiny/n8n-portofolio/blob/dc43b2f1f00ca99b2b99efda61bb6d1a65d6c830/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20210653.png)

**Case Fields:**
- Customer information
- AI classification results
- Priority score & escalation level
- Status tracking (NEW, WAITING_CUSTOMER, etc.)
- Gmail thread ID for conversation history

Inbound messages are recorded separately:

![Create Support Message](https://github.com/Vluiny/n8n-portofolio/blob/dc43b2f1f00ca99b2b99efda61bb6d1a65d6c830/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20210653.png)

---

## 💬 AI Response Generation

The system generates professional, context-aware replies.

![Response Generation](https://github.com/Vluiny/n8n-portofolio/blob/f2644ff4744acdf1f2c447abeed85f94ff0b77f4/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20211039.png)

**Response Format:**
```json
{
  "subject": "Re: Your support request",
  "body": "Hello,\n\nThank you for contacting us...\n\nBest regards,\nAutoFlow Dynamics"
}
```

The reply is sent via Gmail maintaining the same thread:

![Reply Node](https://github.com/Vluiny/n8n-portofolio/blob/9d8de110e403b88fab8c45daf3cb4e5015ebe72d/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20211209.png)

Outbound messages are also logged in the database.

---

## ⏱️ SLA & Escalation

**SLA Deadlines:**
- **critical**: 1 hour response required
- **high**: 4 hours response required
- **medium**: 24 hours response required
- **low**: 72 hours response required

Cases are flagged for human attention when:
- Score ≥ 90 (critical)
- Urgency is "high"
- Sentiment is "angry"
- Confidence score is low

---

## 📝 Logging & Monitoring

All AI executions are logged for audit and improvement.

![Execution Logs](https://github.com/Vluiny/n8n-portofolio/blob/9d8de110e403b88fab8c45daf3cb4e5015ebe72d/all-documentation/ai-email-support/Screenshot/Screenshot%202026-03-16%20205748.png)

**Logged Data:**
- Case ID reference
- AI model used
- Escalation level
- Timestamp

Activity tracking includes:
- Lead creation
- Email classification
- Response generation
- Case updates

---

## ⚙️ Setup Guide

### Prerequisites
- n8n instance
- Gmail account with API access
- Supabase project
- Groq API key (for Llama 3.3 70B)
- Google Gemini API key (optional)

### Database Tables

**customers**
```sql
CREATE TABLE customers (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  email TEXT UNIQUE,
  first_seen_at TIMESTAMP,
  last_seen_at TIMESTAMP,
  total_cases INTEGER DEFAULT 0
);
```

**support_cases**
```sql
CREATE TABLE support_cases (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  customer_id UUID REFERENCES customers(id),
  intent TEXT, urgency TEXT, sentiment TEXT,
  priority_score INTEGER, escalation_level TEXT,
  status TEXT, gmail_thread_id TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Quick Setup Steps
1. Import workflow JSON
2. Configure credentials (Gmail, Supabase, Groq)
3. Update `HumanCheck@gmail.com` with your team email
4. Test with a sample email
5. Activate workflow

---

## 🔒 Security Notes

- No API keys stored in workflow
- All connections encrypted
- Suspicious emails routed to human review
- Full audit trail maintained

---

## 📞 Contact

For questions or custom development:
- **LinkedIn**: [linkedin.com/in/restu-pambudi-6b190a386](https://linkedin.com/in/restu-pambudi-6b190a386)
- **Email**: fuadsparta619@gmail.com

# 🔄 Complete Flow Summary

```
[GMAIL TRIGGER — Every Minute]
         ↓
┌─────────────────────────┐
│   Get Email Details     │
│ (From, To, Subject, Body│
│  ThreadId, MessageId)   │
└─────────────────────────┘
         ↓
┌─────────────────────────┐
│    Filter & Spam Check  │
│  ┌─────────────────────┐│
│  │ • From not empty    ││
│  │ • Subject not empty ││
│  │ • No mailer-daemon  ││
│  │ • Size > 50 bytes   ││
│  └─────────────────────┘│
└─────────────────────────┘
         ↓
    ┌────┴────┐
    ▼         ▼
┌─────────┐ ┌─────────┐
│  SUS    │ │ NORMAL  │
│(Spam)   │ │(Legit)  │
└─────────┘ └─────────┘
    ↓            ↓
┌─────────┐ ┌─────────┐
│ Manual  │ │   AI    │
│ Check   │ │Classification
└─────────┘ └─────────┘
                  ↓
         ┌─────────────────┐
         │  Intent, Urgency│
         │  Sentiment, Score│
         └─────────────────┘
                  ↓
         ┌─────────────────┐
         │ Confidence Check│
         │    > 60?        │
         └─────────────────┘
                  ↓
           ┌──────┴──────┐
           ▼              ▼
    ┌──────────┐    ┌──────────┐
    │ Continue │    │  Manual  │
    │          │    │  Check   │
    └──────────┘    └──────────┘
           ↓
    ┌─────────────────────┐
    │ Priority Score Calc │
    │ • Urgency: +5-40    │
    │ • Sentiment: +5-30  │
    │ • Budget: +25       │
    │ • Follow-up: +5 each│
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │ Escalation Level    │
    │ • ≥90: critical     │
    │ • ≥60: high         │
    │ • ≥30: medium       │
    │ • <30: low          │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Customer Database  │
    │  ┌───────────────┐  │
    │  │ Check if      │  │
    │  │ exists        │  │
    │  └───────────────┘  │
    │         ↓           │
    │  ┌───────────────┐  │
    │  │New? → Create  │  │
    │  │Old? → Update  │  │
    │  └───────────────┘  │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Support Case       │
    │  Created            │
    │ • Link to customer  │
    │ • Store AI results  │
    │ • Priority score    │
    │ • Status: NEW       │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Inbound Message    │
    │  Logged             │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Internal Analysis  │
    │  AI evaluates:      │
    │ • Risk level        │
    │ • Case complexity   │
    │ • Recommended action│
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  AI Response Gen    │
    │ • Professional reply│
    │ • Context-aware     │
    │ • Tone-adjusted     │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Outbound Message   │
    │  Logged             │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Reply via Gmail    │
    │ (Same thread)       │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  Case Updated       │
    │ • Status: WAITING   │
    │ • Last action time  │
    │ • Internal analysis │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  AI Execution Log   │
    │ • Model used        │
    │ • Escalation level  │
    │ • Timestamp         │
    └─────────────────────┘
           ↓
    ┌─────────────────────┐
    │  SLA Timer Starts   │
    │ • critical: 1h      │
    │ • high: 4h          │
    │ • medium: 24h       │
    │ • low: 72h          │
    └─────────────────────┘
```

## 📊 Flow Breakdown by Phase

| Phase | Process Group | Key Actions |
|-------|---------------|-------------|
| **1** | Email Intake | Gmail Trigger → Get Message → Basic Filter |
| **2** | Security | Spam Detection → SUS/NORMAL split |
| **3** | AI Analysis | Classification → Confidence Check |
| **4** | Prioritization | Score Calculation → Escalation Level |
| **5** | Data Management | Customer DB → Support Case → Message Log |
| **6** | Internal Review | Risk Analysis → Complexity Assessment |
| **7** | Response | AI Generation → Outbound Log → Gmail Reply |
| **8** | Finalization | Case Update → Execution Log → SLA Start |

---

## 🎯 Key Decision Points

```
                    ┌─────────────────────┐
                    │   Filter Criteria   │
                    │   ALL must pass?    │
                    └─────────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │   PROCEED   │        │   DISCARD   │
         │ to Spam Check│       │(Mailer-daemon│
         └─────────────┘        │   or empty)  │
                                └──────────────┘

                    ┌─────────────────────┐
                    │   Text Classifier   │
                    └─────────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │     SUS     │        │   NORMAL    │
         │(Phishing/Spam)       │ (Legitimate)│
         └─────────────┘        └─────────────┘
               ↓                        ↓
         ┌──────▼──────┐        ┌──────▼──────┐
         │   Manual    │        │     AI      │
         │   Review    │        │Classification│
         └─────────────┘        └─────────────┘

                    ┌─────────────────────┐
                    │  Confidence Score   │
                    │      > 60?          │
                    └─────────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │  AUTOMATED  │        │   MANUAL    │
         │  Processing │        │   REVIEW    │
         └─────────────┘        └─────────────┘
```

---

## ⏱️ SLA Timeline

```
Email Received (T=0)
    ↓
[Processing Time: ~30 seconds]
    ↓
AI Response Sent (T+30s)
    ↓
┌─────────────────────────────────────────────────┐
│              SLA COUNTDOWN BEGINS                │
├─────────────────────────────────────────────────┤
│                                                  │
│  CRITICAL (Score ≥90)    → 1 hour to resolve    │
│  HIGH    (Score ≥60)    → 4 hours to resolve    │
│  MEDIUM  (Score ≥30)    → 24 hours to resolve   │
│  LOW     (Score <30)    → 72 hours to resolve   │
│                                                  │
└─────────────────────────────────────────────────┘
    ↓
If no customer reply within SLA:
    ↓
┌─────────────────────────────────────────────────┐
│         ESCALATION TRIGGERED                     │
│    • Alert sent to support team                  │
│    • Case priority increased                     │
│    • Manual intervention required                │
└─────────────────────────────────────────────────┘
```

---

## 🔄 Data Flow Relationships

```
gmail_trigger
    ↓
gmail_message
    ↓
┌─────────────────────────────────────┐
│         customers                   │
│  ┌─────────────┐                    │
│  │ • id        │                    │
│  │ • email     │◄─────┐             │
│  │ • first_seen│      │             │
│  │ • last_seen │      │             │
│  │ • total_cases│     │             │
│  └─────────────┘      │             │
│         ↑             │             │
│         │             │             │
│  ┌─────────────┐      │             │
│  │ support_cases│     │             │
│  │ • id        │      │             │
│  │ • customer_id│─────┘             │
│  │ • intent    │                    │
│  │ • urgency   │                    │
│  │ • sentiment │                    │
│  │ • priority_score│                 │
│  │ • escalation_level│               │
│  │ • status    │                    │
│  │ • gmail_thread_id│                │
│  └─────────────┘                    │
│         ↑                           │
│         │                           │
│  ┌─────────────┐                    │
│  │support_messages│                  │
│  │ • case_id   │────┘               │
│  │ • direction │                    │
│  │ • content   │                    │
│  │ • gmail_message_id│               │
│  └─────────────┘                    │
│         ↑                           │
│         │                           │
│  ┌─────────────┐                    │
│  │ai_execution_logs│                 │
│  │ • case_id   │────┘               │
│  │ • model_used│                    │
│  │ • escalation_level│               │
│  └─────────────┘                    │
└─────────────────────────────────────┘
```

---

## 📈 Metrics Tracked

| Metric | Source | Purpose |
|--------|--------|---------|
| `confidence_score` | AI Output | Classification reliability |
| `priority_score` | Calculation | Urgency level |
| `escalation_level` | Algorithm | critical/high/medium/low |
| `response_time` | Timestamp | SLA compliance |
| `customer_history` | Database | Previous interactions |
| `total_cases` | Database | Customer support volume |
| `sentiment_trend` | AI Analysis | Customer satisfaction |
| `intent_distribution` | AI Analysis | Support vs Sales vs Complaints |

---

## ✅ Complete Flow Summary

| Step | Component | Function |
|------|-----------|----------|
| **1** | Gmail Trigger | Polls for new emails every minute |
| **2** | Get Message | Retrieves full email details |
| **3** | Filter | Removes invalid/mailer-daemon emails |
| **4** | Spam Classifier | Detects phishing/suspicious emails |
| **5** | AI Classification | Determines intent, urgency, sentiment |
| **6** | Confidence Check | Routes low-confidence to manual review |
| **7** | Priority Scoring | Calculates numeric priority score |
| **8** | Escalation | Assigns critical/high/medium/low level |
| **9** | Customer DB | Creates/updates customer record |
| **10** | Support Case | Creates case with AI results |
| **11** | Message Log | Records inbound message |
| **12** | Internal Analysis | AI evaluates risk & complexity |
| **13** | Response Generation | AI creates contextual reply |
| **14** | Outbound Log | Records AI-generated reply |
| **15** | Gmail Reply | Sends response in same thread |
| **16** | Case Update | Updates status to WAITING_CUSTOMER |
| **17** | Execution Log | Records AI model and decisions |
| **18** | SLA Start | Begins countdown based on escalation |

---

⭐ **Thank you for reviewing this documentation!**
```
