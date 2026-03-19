# 🧩 n8n Free Templates - AI Lead Management Lite

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

### 🤖 AI-Powered Lead Management System (Lite Version)
A simplified lead management system that:

- 📊 **Scores leads automatically** using AI (1-100 scale)
- 📨 **Classifies customer messages** into Sales/Support/Billing/Other
- 🔔 **Routes notifications** to different Slack channels
- 📧 **Captures emails** as leads via Gmail
- 💾 **Stores data** in Supabase database

---

## ⚙️ What to Expect

Each template is:

- ✅ **Simplified** (focused on core AI & routing logic)
- ✅ **Readable** (clean structure & node naming)
- ✅ **Modular** (easy to extend)
- ✅ **Multi-channel** (Email + Scheduled processing)

---

## 🛠️ Tech Stack

Tools used in this template:

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **Supabase** | Database for lead storage |
| **Groq (Llama 3.3)** | AI for lead scoring & classification |
| **Slack** | Team notifications & routing |
| **Gmail API** | Email capture trigger |
| **Schedule Trigger** | Automated batch processing |

---

## 📊 Workflow Structure

```
SCHEDULE TRIGGER (every minute)
├── Lead Scoring Branch
│   ├── Get unscored leads (score = 0)
│   ├── AI Agent → Calculate score (1-100)
│   ├── Parse response
│   └── Update database with score
│
└── Message Classification Branch
    ├── Get unrouted messages
    ├── AI Agent → Classify (sales/support/billing/other)
    ├── Switch by category
    └── Send to respective Slack channel

GMAIL TRIGGER (real-time)
    ├── Capture incoming emails
    ├── Extract sender & message
    ├── Create lead in database
    └── Send Slack notification
```

---

## 🔧 How the AI Works

### Lead Scoring Criteria
The AI evaluates leads based on:

- **Email domain**: Corporate vs free email (gmail/yahoo/outlook)
- **Name quality**: Complete vs suspicious/incomplete
- **Message quality**: Clear intent vs vague
- **Source**: Website form vs unknown source

### Message Classification
The AI categorizes messages into:
- **sales** - Demo requests, pricing questions, service interest
- **support** - Technical issues, troubleshooting
- **billing** - Payments, invoices, subscription questions
- **other** - Everything else

---

## 📌 How to Use

1. **Import** the `.json` workflow into n8n
2. **Connect your credentials** (Supabase, Groq, Slack, Gmail)
3. **Create the database table** using the SQL below
4. **Configure Slack channels** for each category
5. **Activate** the workflow

### Database Setup
```sql
CREATE TABLE leads (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT,
  source TEXT,
  message TEXT,
  score INTEGER DEFAULT 0,
  status TEXT DEFAULT 'new',
  message_routed BOOLEAN DEFAULT false,
  threadid_id TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Required Credentials
| Node | Credential Type | Where to Get |
|------|-----------------|--------------|
| Supabase | Supabase API | Project Settings → API |
| Groq | Groq API | console.groq.com |
| Slack | Slack API | api.slack.com/apps |
| Gmail | Gmail OAuth2 | Google Cloud Console |

---

## 🎯 Sample Output

### Slack Notification (Sales Channel)
```
New Customer Message

Name: John Doe
Email: john@company.com
Message: Saya tertarik dengan demo produk untuk tim sales kami.

status : new
```

### Lead in Database
```json
{
  "name": "John Doe",
  "email": "john@company.com",
  "source": "Website Form",
  "message": "Demo produk untuk tim sales",
  "score": 85,
  "status": "update",
  "message_routed": true
}
```

---

## ⚠️ Notes

- This template uses **Groq's Llama 3.3 70B** model (free tier available)
- The AI scoring logic can be customized by editing the system message
- For production use, add error handling and retry logic
- Slack channels need to be created before running the workflow

---

## 🔗 Full Portfolio

For more advanced and complete systems:

👉 https://github.com/username/n8n-portfolio

---

## 📞 Contact

- **LinkedIn**: https://linkedin.com/in/restu-pambudi-6b190a386
- **Email**: restupambudi.dev@gmail.com

---

## 📝 License

MIT License - feel free to use, modify, and learn from this template.

---

⭐ **Like this template?** Star the repository to show support!
