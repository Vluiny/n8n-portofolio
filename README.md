# 🚀 n8n Production Workflow Portfolio


Hello! I'm an **automation specialist** who builds complex systems using [n8n](https://n8n.io/). This repository is a **visual portfolio** of production workflows I have developed.

**⚠️ IMPORTANT NOTE:**  
I do **NOT** include workflow JSON files in this repository to protect client intellectual property and maintain production system security. What you see here is **documentation and screenshots** of live, running systems.

---

## 📊 **Workflow Portfolio**

### 1. 🤖 **AI-Powered Email Support Automation**

An automated email support system that uses **multiple AI models** to classify, prioritize, and respond to customer emails.

| **Key Features** | **Tech Stack** |
|-----------------|----------------|
| ✅ Spam/phishing detection | • Gmail API |
| ✅ Intent classification (sales/support/complaint) | • Groq (Llama 3.3 70B) |
| ✅ Automatic priority scoring | • Google Gemini |
| ✅ Customer history tracking | • Supabase |
| ✅ AI response generation | • Custom scoring algorithm |
| ✅ SLA management & escalation | |

#### 📸 **Workflow Overview**
![AI Email Support - Full Workflow](https://github.com/Vluiny/n8n-portofolio/blob/45294978e39f41f150a4173507a30fe7e2e25b7d/Screenshot/Screenshot%202026-03-16%20173427.png)
*Complete system flow from trigger to response*

#### 📸 **AI Classification Node**
![AI Classification Node](https://github.com/Vluiny/n8n-portofolio/blob/a6a2058b5482464c65c6708006bada1333a140d5/Screenshot/Screenshot%202026-03-16%20173241.png)
*AI node performing intent, urgency, and sentiment analysis*

#### 📸 **Priority Scoring Logic**
![Priority Scoring](https://github.com/Vluiny/n8n-portofolio/blob/a6a2058b5482464c65c6708006bada1333a140d5/Screenshot/Screenshot%202026-03-16%20173257.png)
*Custom JavaScript calculating priority scores and determining SLA*

#### 📸 **Response Generation**
![AI Response](https://github.com/Vluiny/n8n-portofolio/blob/45294978e39f41f150a4173507a30fe7e2e25b7d/Screenshot/Screenshot%202026-03-16%20173348.png)
*AI generates professional responses based on conversation context*

#### **Business Impact:**
- ⏱️ **Response time reduced** from 24 hours → 5 minutes
- 🎯 **94% classification accuracy**
- 💰 **Support team workload reduced by 70%**

[→ View Full Documentation]((https://github.com/Vluiny/n8n-portofolio/tree/2f421181e650443bea93a879114e090312afadcb/all-documentation/ai-email-support))

---

### 2. 📄 **AI-Powered ATS System (Mini ATS 5.1)**

An automated recruitment system that processes job applications, extracts data from PDF resumes, and ranks candidates using AI.

| **Key Features** | **Tech Stack** |
|-----------------|----------------|
| ✅ Tally form integration | • Tally Forms API |
| ✅ PDF resume extraction | • HTTP Request |
| ✅ Skill extraction & parsing | • PDF extraction node |
| ✅ Job matching & scoring | • Groq (Llama 3.3 70B) |
| ✅ Automatic candidate ranking | • Supabase |
| ✅ Strategic hiring recommendations | • Google Sheets |

#### 📸 **Full Workflow Architecture**
![ATS Full Workflow](assets/screenshots/ats-full.png)
*Complete flow from form submission to ranking*

#### 📸 **Resume Extraction Process**
![PDF Extraction](assets/screenshots/ats-extraction.png)
*Download PDF from Tally and extract text content*

#### 📸 **AI Information Extraction**
![AI Extraction](assets/screenshots/ats-ai-extraction.png)
*AI extracts name, skills, experience, and education from resumes*

#### 📸 **Candidate Scoring & Ranking**
![Candidate Ranking](assets/screenshots/ats-ranking.png)
*AI compares candidates against job requirements and provides rankings*

#### **Business Impact:**
- ⏱️ **Screening time reduced** from 2 hours → 2.5 minutes per CV
- 📊 **200+ candidates** processed weekly
- 🎯 **Interview conversion increased by 35%**

[→ View Full Documentation](mini-ats/README.md)

---

### 3. ⚙️ **OPS Lead Management System**

A multi-channel system that captures leads from various sources, performs automatic scoring, and routes them to the appropriate teams.

| **Key Features** | **Tech Stack** |
|-----------------|----------------|
| ✅ Multi-channel ingestion | • Gmail Trigger |
| ✅ Gmail, Telegram, Tally Forms | • Telegram Bot |
| ✅ AI lead scoring (1-100) | • Tally Forms |
| ✅ Smart Slack routing | • Groq AI |
| ✅ Lead enrichment & analysis | • Slack API |
| ✅ Auto-followup sequences | • Supabase |

#### 📸 **Multi-Channel Triggers**
![Multi-Channel](assets/screenshots/ops-triggers.png)
*Triggers from Gmail, Telegram, and Tally Forms unified in one system*

#### 📸 **AI Lead Scoring**
![Lead Scoring](assets/screenshots/ops-scoring.png)
*AI scores leads based on email domain, message quality, and source*

#### 📸 **Slack Routing Logic**
![Slack Routing](assets/screenshots/ops-routing.png)
*Leads routed to different channels based on score (Hot: ≥80, Warm: ≥60)*

#### 📸 **Message Classification**
![Message Classification](assets/screenshots/ops-classification.png)
*AI categorizes messages into sales/support/billing/other*

#### **Business Impact:**
- 📈 **Hot lead response time** < 2 minutes
- 🎯 **88% scoring accuracy**
- 💰 **Conversion rate increased by 40%**

[→ View Full Documentation](lead-ops/README.md)

---

## 🛠️ **Common Tech Stack**

| Technology | Usage |
|-----------|------------|
| **n8n** | Workflow automation platform |
| **Groq (Llama 3.3 70B)** | Primary AI model |
| **Google Gemini** | Secondary AI (fallback) |
| **Supabase** | PostgreSQL database |
| **Gmail API** | Email processing |
| **Telegram Bot API** | Chat integration |
| **Tally Forms API** | Form submissions |
| **Slack API** | Team notifications |
| **Google Sheets** | Data backup & reporting |

---

## 🔒 **Security & Privacy Note**

All screenshots in this repository are **demonstration versions** of actual production systems. Sensitive data such as:
- Email addresses
- API keys
- Database credentials
- Internal business logic

have been **removed or anonymized** to protect client privacy and system security.

---

## 📞 **Contact**

Interested in discussing your automation needs?

- **LinkedIn**: https://linkedin.com/in/restu-pambudi-6b190a386
- **Email**: fuadsparta619@gmail.com

---

## 📸 **All Workflow Screenshots**

### AI Email Support System
![Email Support](assets/screenshots/email-support-full.png)

### ATS System
![ATS](assets/screenshots/ats-full.png)

### OPS Lead Management
![OPS](assets/screenshots/ops-full.png)

---

⭐ **Thank you for visiting my portfolio!**
