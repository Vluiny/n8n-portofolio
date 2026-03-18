# 📄 AI-Powered ATS System (Mini ATS 5.1) - Complete Documentation

This project is a simulation of a production-ready ATS system, built to demonstrate real-world automation architecture and AI-assisted candidate evaluation.

---

## 📋 Table of Contents
1. System Architecture
2. Trigger & Form Integration
3. PDF Resume Extraction
4. Job Requirement Lookup
5. AI Information Extraction
6. Data Validation
7. AI Candidate Scoring
8. Database Storage
9. Business Impact
10. Setup Guide

## 🏗️ System Architecture

Complete flow from job application form submission to candidate ranking.

![ATS Full Workflow](https://github.com/Vluiny/n8n-portofolio/blob/1913d4e5bc1c190f6c3798e023c557cff5b2a95b/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20214935.png)

**Main Flow:**
1. **Tally Form** collects applicant data and CV
2. **PDF Extraction** pulls text from resume
3. **Job Requirements** fetched from database
4. **AI Extraction** pulls structured data from CV
5. **Validation** ensures data quality
6. **AI Scoring** compares candidate with job requirements
7. **Database** stores all candidate data with scores

---

## 📝 Trigger & Form Integration

Applications are submitted via Tally Forms and trigger the workflow automatically.

![Tally Trigger](https://github.com/Vluiny/n8n-portofolio/blob/33ced338cb6025e316235c834c92dbba6f315354/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20223857.png)

**Data Collected:**
- Name
- Email
- Job position selected
- CV file URL

The Edit Fields node structures this data for the workflow:

![Edit Fields](https://github.com/Vluiny/n8n-portofolio/blob/33ced338cb6025e316235c834c92dbba6f315354/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20223923.png)

---

## 📄 PDF Resume Extraction

The CV is downloaded from the Tally URL and text is extracted.

![HTTP Request](https://github.com/Vluiny/n8n-portofolio/blob/33ced338cb6025e316235c834c92dbba6f315354/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20223943.png)
*HTTP Request node downloads the PDF file*

![Extract from File](https://github.com/Vluiny/n8n-portofolio/blob/33ced338cb6025e316235c834c92dbba6f315354/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-16%20223952.png)
*Extract from File node converts PDF to plain text*

---

## 🔍 Job Requirement Lookup

The system fetches job requirements from Supabase based on the position selected.

![Get Job Row](https://github.com/Vluiny/n8n-portofolio/blob/17777f05a054222ee93352bc99e9aacaa353e8b6/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20114853.png)

**Job Data Retrieved:**
- Required skills
- Minimum years of experience
- Preferred education
- Additional criteria

## 🤖 AI Information Extraction

An AI agent extracts structured information from the raw CV text.

![AI Extraction Agent](https://github.com/Vluiny/n8n-portofolio/blob/17777f05a054222ee93352bc99e9aacaa353e8b6/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20114923.png)

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

## 🎯 AI Candidate Scoring

A second AI agent compares the candidate against job requirements and provides scores.

![AI Scoring Agent](https://github.com/Vluiny/n8n-portofolio/blob/17777f05a054222ee93352bc99e9aacaa353e8b6/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20115002.png)

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
---

## 💾 Database Storage

All candidate data and AI scores are stored in Supabase.

![Insert Candidate](https://github.com/Vluiny/n8n-portofolio/blob/17777f05a054222ee93352bc99e9aacaa353e8b6/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20115027.png)

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

## 📊 Potential Business Impact (Simulation-Based)

This system is designed to potentially:

| Metric | Expected Impact |
|--------|----------------|
| Screening time per CV | Reduced significantly through automation |
| Candidate processing capacity | Increased due to automated workflows |
| Screening consistency | More standardized using AI scoring |
| Decision support | Structured candidate comparison |

---

---

## ⏰ Scheduled Ranking & Top 3 Selection

Every 3 days, the system automatically re-evaluates all candidates and selects the top 3 for each job position.

### Schedule Trigger
![Schedule Trigger](https://github.com/Vluiny/n8n-portofolio/blob/917b0b509081c631bb6dab76353598e509bfa829/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20121818.png)
*Workflow runs every 3 days at 08:00 AM*

Configuration:
```json
{
  "rule": {
    "interval": [
      {
        "daysInterval": 3,
        "triggerAtHour": 8
      }
    ]
  }
}
```

---

### Data Ingestion & Filtering
![Data Ingestion & Filtering](https://github.com/Vluiny/n8n-portofolio/blob/917b0b509081c631bb6dab76353598e509bfa829/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20121856.png)
*The initial phase involving job position retrieval and potential candidate screening.*

**Core Processes:**
* **Fetch Jobs & Candidates:** Retrieves active job positions and applicant data from the database.
* **Score Filter:** Filters candidates with a minimum overall score of 70.
* **Sort & Limit:** Identifies and selects the Top 15 candidates for advanced strategic analysis.

---

### AI Strategic Decision Engine
![AI Strategic Decision Engine](https://github.com/Vluiny/n8n-portofolio/blob/917b0b509081c631bb6dab76353598e509bfa829/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20121948.png`)
*LLM analyzes candidates based on predefined criteria and prompts.*

**Core Processes:**
* **AI Strategic Ranking:** LLM analyzes candidates based on complex strategic criteria and job requirements.
* **JSON Parsing:** Converts AI reasoning into structured, machine-readable data.
* **Data Enrichment:** Re-fetches and maps complete profile details for the final Top 3 candidates.

---

### Database Synchronization
![Database Synchronization](https://github.com/Vluiny/n8n-portofolio/blob/917b0b509081c631bb6dab76353598e509bfa829/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20122122.png)
*Handles legacy data cleanup and synchronizes the latest ranking results with the system.*

**Core Processes:**
* **Old Data Cleanup:** Removes ranking data from previous cycles to maintain data integrity.
* **Insert New Top 3:** Records the newly selected elite candidates into the `top_candidate` table.
* **Stats Update:** Updates real-time statistics in the `job_positions` table (e.g., candidate count & timestamp).

---

### Analytics, Stability & Logging
![Analytics, Stability & Logging](https://github.com/Vluiny/n8n-portofolio/blob/917b0b509081c631bb6dab76353598e509bfa829/all-documentation/Mini%20Ats%205.1/Screenshot/Screenshot%202026-03-17%20122058.png)
*The final stage for monitoring ranking stability and determining recruitment urgency.*

**Core Processes:**
* **Stability Check:** Compares current results with previous cycles to identify ranking shifts.
* **Urgency & Confidence:** Calculates hiring priority and the system's confidence level in the ranking.
* **Final Logging:** Records all activities into the `ranking_log` for audit trails and performance tracking.
---

## 🎯 **Business Impact of Scheduled Ranking**

| Feature | Benefit |
|---------|---------|
| **Every 3 days ranking** | Always up-to-date candidate evaluation |
| **Top 3 selection** | Focus only on the best candidates |
| **Strategic AI reasoning** | Not just scores, but business context |
| **Stability tracking** | Know if top candidates are consistent |
| **Hiring recommendation** | Conservative, Balanced, or Aggressive |
| **Confidence scoring** | Know how reliable the ranking is |

---

## 🔄 **Complete Ranking Flow Summary**

```
Schedule Trigger (Every 3 days)
    ↓
Get All Jobs
    ↓
For Each Job:
    ↓
Get Candidates → Filter (Score ≥70) → Sort → Top 15
    ↓
AI Strategic Ranking → Top 3 Selection
    ↓
Get Complete Candidate Data
    ↓
Clean Old Data → Insert New Top 3
    ↓
Update Job Stats
    ↓
Log Ranking Activity
    ↓
Check Stability → Calculate Urgency → Determine Confidence
    ↓
Save Complete Log
```

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
- **Email**: restupambudi2234@gmail.com

---

# 🔄 Complete Flow Summary

```
[SCHEDULED RANKING TRIGGER — Every 3 Days]
              ↓
    ┌─────────────────┐
    │  Get All Jobs   │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │ Get Candidates  │
    │  Per Job Position│
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │   Filter:       │
    │ Score ≥ 70 Only │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │ Sort & Select   │
    │   Top 15        │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │   AI Strategic  │
    │  Ranking Engine │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │  Parse JSON &   │
    │  Get Candidate  │
    │  Complete Data  │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │   Clean Old     │
    │  Ranking Data   │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │ Insert New Top 3│
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │ Update Job Stats│
    │(Count & Timestamp│
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │   Log Ranking   │
    │    Activity     │
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │Check Stability &│
    │   Calculate     │
    │Urgency & Confidence
    └─────────────────┘
              ↓
    ┌─────────────────┐
    │ Save Complete   │
    │  Ranking Log    │
    └─────────────────┘
              ↓
         [END CYCLE]
```

## 📊 Flow Breakdown

| Phase | Process | Description |
|-------|---------|-------------|
| **1** | Schedule Trigger | Every 3 days at 08:00 AM |
| **2** | Data Collection | Fetch all jobs and their candidates |
| **3** | Filtering | Only candidates with score ≥ 70 proceed |
| **4** | Sorting | Top 15 candidates by overall_score |
| **5** | AI Analysis | LLM determines strategic top 3 |
| **6** | Data Enrichment | Fetch complete candidate profiles |
| **7** | Cleanup | Remove previous ranking data |
| **8** | Storage | Insert new top 3 candidates |
| **9** | Statistics | Update job position metrics |
| **10** | Logging | Record all activities |
| **11** | Analysis | Check stability, urgency, confidence |
| **12** | Final Save | Complete ranking log entry |

---

## 🎯 Key Decision Points

```
                    ┌─────────────────┐
                    │  Score ≥ 70?    │
                    └─────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │   PROCEED   │        │   DISCARD   │
         │  to Top 15  │        │  from Elite │
         └─────────────┘        └─────────────┘

                    ┌─────────────────┐
                    │  AI Strategic   │
                    │    Ranking      │
                    └─────────────────┘
                           ↓
                ┌──────────┴──────────┐
         ┌──────▼──────┐        ┌──────▼──────┐
         │   TOP 3     │        │   NOT IN    │
         │  Selected   │        │    TOP 3    │
         └─────────────┘        └─────────────┘

                    ┌─────────────────┐
                    │  Stability Check│
                    └─────────────────┘
                           ↓
         ┌──────────────┬──┴──┬──────────────┐
    ┌────▼────┐   ┌─────▼─────┐   ┌────▼────┐
    │  First  │   │  Stable   │   │ Shifting │
    │  Cycle  │   │           │   │         │
    └─────────┘   └───────────┘   └─────────┘
```

---

## ⏱️ Timeline Visualization

```
Day 0: Application Submitted
    ↓ (Real-time)
┌─────────────────────────────────────┐
│  AI Extraction → Scoring → Storage  │
└─────────────────────────────────────┘
    ↓
Day 3: First Ranking Cycle
    ↓
┌─────────────────────────────────────┐
│  Top 15 Selection → AI Ranking      │
│  → Top 3 → Database → Logging       │
└─────────────────────────────────────┘
    ↓
Day 6: Second Ranking Cycle
    ↓
┌─────────────────────────────────────┐
│  Compare with Previous → Stability  │
│  Check → Update Rankings → Log      │
└─────────────────────────────────────┘
    ↓
Day 9, 12, 15... (Continues Every 3 Days)
```

---

## 🔄 Data Flow Relationships

```
job_positions
      ↑
      │ (1 job has many candidates)
      │
candidates ──→ temp_top_candidate (temporary storage during ranking)
      ↑                    ↑
      │                    │
      └───┬────────────────┘
          │ (top 3 candidates)
    ┌─────▼─────┐
    │top_candidate│
    └───────────┘
          ↓
    ┌─────▼─────┐
    │ranking_log│
    └───────────┘
```

---

## 📈 Metrics Tracked Per Cycle

| Metric | Source | Purpose |
|--------|--------|---------|
| `total_candidates` | Database | Overall volume |
| `elite_candidates` | Filter (≥70) | Quality pool size |
| `top_3_ids` | AI Ranking | Selected candidates |
| `ranking_summary` | AI Output | Strategic overview |
| `stability_status` | Comparison | First/Stable/Shifting |
| `hiring_urgency` | Calculation | High/Medium/Low |
| `confidence_level` | Algorithm | High/Medium/Low |
| `cycle_number` | Increment | Tracking history |

---

✅ **Complete Ranking Flow** - From trigger to final log
✅ **Every 3 Days** - Regular, automated evaluation
✅ **AI-Powered Decisions** - Strategic candidate selection
✅ **Full Audit Trail** - All activities logged
✅ **Stability Tracking** - Monitor ranking consistency
---

⭐ **Thank you for reviewing this documentation!**
```

