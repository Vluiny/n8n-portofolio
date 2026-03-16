Saya sangat setuju! Pendekatan yang lebih sederhana justru lebih baik. Ini versi yang **lebih ringkas** dengan format: **Penjelasan singkat → Screenshot di bawahnya**.

---

```markdown
# 🤖 AI-Powered Email Support Automation - Complete Documentation

## 📋 Table of Contents
1. [System Architecture](#system-architecture)
2. [Trigger & Email Fetching](#trigger--email-fetching)
3. [Filtering & Spam Detection](#filtering--spam-detection)
4. [AI Classification](#ai-classification)
5. [Priority Scoring Logic](#priority-scoring-logic)
6. [Customer Database](#customer-database)
7. [Support Cases Management](#support-cases-management)
8. [AI Response Generation](#ai-response-generation)
9. [SLA & Escalation](#sla--escalation)
10. [Logging & Monitoring](#logging--monitoring)
11. [Setup Guide](#setup-guide)

---

## 🏗️ System Architecture

Complete system flow from Gmail trigger to AI-powered response.

![Full Workflow](../assets/screenshots/email-support-full.png)

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

![Gmail Trigger](../assets/screenshots/email-support-gmail-trigger.png)

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

![Filter Node](../assets/screenshots/email-support-filter.png)

**Filter Criteria (must meet ALL):**
- From cannot be empty
- Subject cannot be empty
- From does not contain "mailer-daemon"
- Size estimate > 50 bytes

Suspicious emails are flagged using a Text Classifier:

![Text Classifier](../assets/screenshots/email-support-text-classifier.png)

**Categories:**
- **SUS** - Phishing/spam attempts → sent to manual review
- **NORMAL** - Legitimate emails → proceed to AI classification

---

## 🧠 AI Classification

The core intelligence: AI analyzes email content and returns structured data.

![AI Classification Node](../assets/screenshots/email-support-ai-classification.png)

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

![Confidence Check](../assets/screenshots/email-support-confidence-check.png)

- **Confidence > 60** → Continue to database
- **Confidence ≤ 60** → Send to manual review

---

## 📊 Priority Scoring Logic

Custom JavaScript calculates priority scores and determines escalation levels.

![Priority Scoring Node](../assets/screenshots/email-support-scoring.png)

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

![Customer Database Flow](../assets/screenshots/email-support-customer-flow.png)

**Process:**
1. Check if customer email exists
2. If **new** → create record with first_seen_at
3. If **existing** → update last_seen_at and increment total_cases

---

## 📋 Support Cases Management

Each email becomes a support case with full tracking.

![Create Support Case](../assets/screenshots/email-support-create-case.png)

**Case Fields:**
- Customer information
- AI classification results
- Priority score & escalation level
- Status tracking (NEW, WAITING_CUSTOMER, etc.)
- Gmail thread ID for conversation history

Inbound messages are recorded separately:

![Create Support Message](../assets/screenshots/email-support-create-message.png)

---

## 💬 AI Response Generation

The system generates professional, context-aware replies.

![Response Generation](../assets/screenshots/email-support-response-ai.png)

**Response Format:**
```json
{
  "subject": "Re: Your support request",
  "body": "Hello,\n\nThank you for contacting us...\n\nBest regards,\nAutoFlow Dynamics"
}
```

The reply is sent via Gmail maintaining the same thread:

![Reply Node](../assets/screenshots/email-support-reply.png)

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

![Execution Logs](../assets/screenshots/email-support-execution-logs.png)

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

---

⭐ **Thank you for reviewing this documentation!**
```
