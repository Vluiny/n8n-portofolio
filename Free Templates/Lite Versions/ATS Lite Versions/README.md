# 🧩 n8n Free Templates - Mini ATS (Lite Version)

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

### 📄 Mini ATS (Lite Version)
A simplified Applicant Tracking System that:

- 📥 **Collects candidate data** via form integration (Tally Forms)
- 📄 **Extracts candidate information** (name, email, skills, experience)
- 🗄️ **Stores data automatically** in Supabase database
- 🧠 **Applies basic qualification logic** (experience-based tagging)
- 🏷️ **Tags candidates** (Qualified / Basic)

---

## ⚙️ What to Expect

Each template is:

- ✅ **Simplified** (focused on core pipeline)
- ✅ **Readable** (clean structure & node naming)
- ✅ **Modular** (easy to extend)
- ✅ **Database-ready** (Supabase integration)
- ❌ Not a full production ATS system
- ❌ No AI scoring or ranking
- ❌ No multi-job matching

---

## 🛠️ Tech Stack

Tools used in this template:

| Tool | Purpose |
|------|---------|
| **n8n** | Workflow automation engine |
| **Tally Forms API** | Candidate data input |
| **Supabase** | Candidate database |
| **Function Node** | Basic qualification logic |

---

## 📊 Workflow Structure

```
FORM TRIGGER (Tally Forms)
    ↓
Receive Candidate Submission
    ↓
EXTRACT DATA
    (Name, Email, Skills, Experience)
    ↓
INSERT CANDIDATE
    (Store in Supabase)
    ↓
QUALIFICATION CHECK
    ├── Experience > 2 years → "Qualified"
    └── Experience ≤ 2 years → "Basic"
         ↓
UPDATE CANDIDATE
    (Add qualification tag)
```

---

## 🔧 How the AI Works

### Basic Qualification Logic
The workflow applies simple experience-based tagging:

| Experience | Tag |
|------------|-----|
| > 2 years | `Qualified` |
| ≤ 2 years | `Basic` |

**Note:** This is intentionally simplified. The full version includes:
- AI-powered skill matching
- Job-specific scoring
- Candidate ranking algorithms

---

## 📌 How to Use

1. **Import** the `.json` workflow into n8n
2. **Connect your credentials** (Supabase, Tally Forms)
3. **Create database tables** using the SQL below
4. **Configure your form** to match the data structure
5. **Activate** the workflow

### Database Setup

```sql
-- Candidates table
CREATE TABLE candidates (
  id SERIAL PRIMARY KEY,
  name TEXT,
  email TEXT UNIQUE,
  skills TEXT,
  experience INTEGER,
  qualification TEXT,
  applied_at TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Required Credentials

| Node | Credential Type | Where to Get |
|------|-----------------|--------------|
| Supabase | Supabase API | Project Settings → API |
| Tally Forms | Tally API | tally.so/account |

---

## ⚠️ Notes

- This is a **simplified demonstration** — not production-ready
- For production use, add:
  - Error handling
  - Data validation
  - Duplicate checking
  - Email notifications to candidates
- Customize the qualification logic in the Function Node
- The full version with AI scoring is available separately

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
