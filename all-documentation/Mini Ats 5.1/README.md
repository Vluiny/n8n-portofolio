
## 🤖 AI Information Extraction

An AI agent extracts structured information from the raw CV text.

![AI Extraction Agent](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224153.png)

**System Prompt:**
```
You are an information extraction engine.
Extract structured data from the provided resume text.

Return ONLY valid JSON:
{
  "name": "",
  "email": "",
  "skills": "",
  "years_experience": 0,
  "education": "",
  "previous_role": ""
}
```

**Output Example:**
```json
{
  "name": "John Doe",
  "email": "john.doe@email.com",
  "skills": "Python, JavaScript, Project Management, Team Leadership",
  "years_experience": 5,
  "education": "Bachelor of Computer Science",
  "previous_role": "Senior Developer"
}
```

A Code node validates and parses the AI output:

![Validation Node](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224252.png)

---

## ✅ Data Validation

An IF node ensures all required fields are present before proceeding.

![Validation Check](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224315.png)

**Required Fields:**
- Name
- Email
- Skills

If any field is missing, the application is flagged for manual review.

---

## 🎯 AI Candidate Scoring

A second AI agent compares the candidate against job requirements and provides scores.

![AI Scoring Agent](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224350.png)

**System Prompt:**
```
You are a resume evaluation engine.
Compare candidate data with job requirements.

Scoring Rules:
- skill_match_score: 0-100 (how well skills match required_skills)
- experience_match_score: 0-100 (compare with minimum_years_experience)
- overall_score: weighted average (50% skills, 30% experience, 20% education)
- justification: explain why the score was given

Return ONLY valid JSON:
{
  "skill_match_score": 85.5,
  "experience_match_score": 90,
  "overall_score": 87.75,
  "justification": "Candidate has all required skills plus leadership experience..."
}
```

Another Code node parses and processes the scoring output:

![Scoring Parser](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224414.png)

**Additional fields added:**
- `is_qualified`: true if overall_score ≥ 70
- `confidence_score`: normalized average of all scores

---

## 💾 Database Storage

All candidate data and AI scores are stored in Supabase.

![Insert Candidate](https://github.com/Vluiny/n8n-portofolio/raw/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20224432.png)

**Table: candidates**
| Field | Description |
|-------|-------------|
| `job_id` | Foreign key to job_positions |
| `name` | Candidate name |
| `email` | Candidate email |
| `skills` | Extracted skills |
| `years_experience` | Extracted experience |
| `education` | Extracted education |
| `previous_role` | Extracted previous role |
| `skill_match_score` | AI-generated score (0-100) |
| `experience_match_score` | AI-generated score (0-100) |
| `overall_score` | Weighted average |
| `justification` | AI explanation |
| `status` | "Active" by default |
| `created_at` | Timestamp |

---

## 📊 Business Impact

Based on production usage:

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Screening time per CV | 2 hours | 2.5 minutes | **98% faster** |
| Candidates processed weekly | 20 | 200+ | **10x more** |
| Interview conversion | Baseline | +35% | **Significant increase** |
| Screening consistency | Subjective | Objective | **Bias reduced** |

---

## ⚙️ Setup Guide

### Prerequisites
- n8n instance
- Tally Forms account
- Supabase project
- Groq API key (for Llama 3.3 70B)

### Database Setup

**Table: job_positions**
```sql
CREATE TABLE job_positions (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  title TEXT,
  required_skills TEXT,
  minimum_years_experience INTEGER,
  preferred_education TEXT,
  additional_criteria TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Table: candidates**
```sql
CREATE TABLE candidates (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  job_id UUID REFERENCES job_positions(id),
  name TEXT,
  email TEXT,
  skills TEXT,
  years_experience INTEGER,
  education TEXT,
  previous_role TEXT,
  skill_match_score DECIMAL,
  experience_match_score DECIMAL,
  overall_score DECIMAL,
  justification TEXT,
  status TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Tally Form Setup
Create a form with:
- Name field (short text)
- Email field (email)
- Job position dropdown (match with job_positions titles)
- CV upload field (file upload)

### n8n Setup Steps
1. Import workflow JSON
2. Configure credentials:
   - Tally Forms API
   - Supabase API
   - Groq API
3. Update table names if different
4. Test with a sample application
5. Activate workflow

---

## 🔒 Security Notes

- CV files are processed in memory, not stored permanently
- No sensitive data exposed in workflow
- All database connections encrypted
- AI processing uses secure API calls

---

## 📞 Contact

For questions or custom development:
- **LinkedIn**: [linkedin.com/in/restu-pambudi-6b190a386](https://linkedin.com/in/restu-pambudi-6b190a386)
- **Email**: fuadsparta619@gmail.com

---

## 🎯 Key Features Summary

✅ **Fully automated** - No manual intervention needed
✅ **AI-powered** - Uses Llama 3.3 70B for extraction and scoring
✅ **Scalable** - Handles hundreds of applications
✅ **Objective** - Consistent scoring eliminates bias
✅ **Transparent** - Justification provided for every score
✅ **Production-ready** - Used in real recruitment processes

---

⭐ **Thank you for reviewing this documentation!**

---
| Teks "System Prompt:" | Dihilangkan koma yang salah |

Sekarang semua gambar akan muncul dengan benar di README-mu! Ada lagi yang perlu diperbaiki?
