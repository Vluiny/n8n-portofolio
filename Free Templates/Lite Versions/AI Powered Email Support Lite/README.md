# 🧩 n8n Free Templates - AI Powered Email Support Lite

A collection of **free, simplified n8n workflows** designed to demonstrate practical automation use cases.

These templates are created as **lightweight versions** of more advanced systems, making them easier to understand, modify, and reuse.

---

## 🚀 Purpose

This repository section is intended to:

- Share **ready-to-use workflow templates**
- Help others learn **automation system design**
- Provide a **starting point** for building real-world workflows
- Showcase simplified versions of more complex systems

---

## 📂 Available Templates

### 📧 AI Powered Email Support Lite
A simplified email support automation system that:

- 🛡️ **Detects spam/phishing** using Gemini AI
- 📊 **Classifies emails** by intent (HOT_LEAD, SUPPORT, COMPLAINT, etc.)
- 🎭 **Analyzes sentiment** (Positive, Neutral, Negative)
- ⚡ **Determines urgency** (Low, Medium, High)
- ✍️ **Generates auto-replies** using Llama AI
- 👥 **Tracks customers** (new vs returning)
- 💾 **Logs all cases** in Supabase database

---

## ⚙️ What to Expect

Each template is:

- ✅ **Simplified** (focused on core AI & email processing)
- ✅ **Readable** (clean structure & node naming)
- ✅ **Modular** (easy to extend)
- ✅ **Multi-AI** (Gemini + Groq Llama)
- ❌ Not a full production system
- ❌ No conversation history tracking
- ❌ No team collaboration features

---

## 🛠️ Tech Stack

Tools used in this template:

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **Gmail API** | Email trigger & sender |
| **Google Gemini** | Spam/phishing detection |
| **Groq (Llama 3.3)** | Email classification & response generation |
| **Supabase** | Customer & case database |

---

## 📊 Workflow Structure

```
GMAIL TRIGGER (every minute)
    ↓
Get Email Content
    ↓
SPAM DETECTION (Gemini)
    ├── SUSPICIOUS → Manual Check / Ignore
    └── LEGITIMATE
         ↓
    AI CLASSIFICATION (Llama)
    (Intent + Urgency + Sentiment + Budget)
         ↓
    Parse JSON Response
         ↓
    CHECK CUSTOMER DATABASE
         ├── New Customer → Create Record
         └── Returning → Update Last Seen
              ↓
    CREATE SUPPORT CASE
         ↓
    AI RESPONSE GENERATION (Llama)
         ↓
    Parse & Format Email
         ↓
    SEND AUTO-REPLY
         ↓
    UPDATE CASE STATUS
```

---

## 🔧 How the AI Works

### Spam Detection (Gemini)
The AI identifies suspicious emails based on:
- Suspicious sender patterns
- Urgent or threatening language
- Requests for sensitive data
- Suspicious links or attachments
- Poor grammar/spelling

### Email Classification (Llama)
The AI analyzes legitimate emails and returns:

```json
{
  "intent": "HOT_LEAD",
  "urgency": "MEDIUM",
  "sentiment": "POSITIVE",
  "budget_detected": true,
  "priority_signal": "PRIORITY_2",
  "confidence_score": 92
}
```

**Intent categories:**
- `HOT_LEAD` - Ready to buy
- `GENERAL_INQUIRY` - Question about services
- `SUPPORT_REQUEST` - Technical help needed
- `COMPLAINT` - Customer dissatisfaction
- `EXISTING_CLIENT` - Current customer
- `PARTNERSHIP_REQUEST` - Business collaboration

**Sentiment levels:**
- `POSITIVE` - Enthusiastic, interested
- `NEUTRAL` - Professional, factual
- `NEGATIVE` - Frustrated, unhappy
- `VERY_NEGATIVE` - Angry, urgent

---

## 📌 How to Use

1. **Import** the `.json` workflow into n8n
2. **Connect your credentials** (Gmail, Supabase, Groq, Gemini)
3. **Create database tables** using the SQL below
4. **Update email addresses** in Manual Check node
5. **Activate** the workflow

### Database Setup

```sql
-- Customers table
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  email TEXT UNIQUE,
  first_seen_at TIMESTAMP,
  last_seen_at TIMESTAMP,
  total_cases INTEGER DEFAULT 0
);

-- Support cases table
CREATE TABLE support_cases (
  id SERIAL PRIMARY KEY,
  customer_id INTEGER REFERENCES customers(id),
  customer_email TEXT,
  intent TEXT,
  urgency TEXT,
  sentiment TEXT,
  budget_detected BOOLEAN,
  priority_signal TEXT,
  confidence_score INTEGER,
  status TEXT DEFAULT 'NEW',
  last_action_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Required Credentials

| Node | Credential Type | Where to Get |
|------|-----------------|--------------|
| Gmail (all) | Gmail OAuth2 | Google Cloud Console |
| Supabase | Supabase API | Project Settings → API |
| Groq (x2) | Groq API | console.groq.com |
| Gemini | Google Gemini API | aistudio.google.com |

---

## ⚠️ Notes

- This template uses **Groq's Llama 3.3 70B** (free tier: ~30 requests/min)
- **Gemini 2.5 Flash** also has generous free tier limits
- Update `YOUR_EMAIL` in the Manual Check node to receive suspicious email alerts
- All AI prompts can be customized by editing the system messages
- For production use, add error handling and retry logic

---

## 🔗 Full Portfolio

For more advanced and complete systems:

👉 https://github.com/Vluiny/n8n-portofolio

---

## 📞 Contact

- **LinkedIn**: https://linkedin.com/in/restu-pambudi-6b190a386
- **Email**: restupambudi.dev@gmail.com

---

## 📝 License

MIT License - feel free to use, modify, and learn from this template.

---

⭐ **Like this template?** Star the repository to show support!
