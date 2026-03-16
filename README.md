# 🚀 n8n Production Workflow Portfolio

> *"Showcasing enterprise-grade automation workflows without exposing the code"*

Halo! Saya adalah **automation specialist** yang membangun sistem kompleks menggunakan [n8n](https://n8n.io/). Repository ini adalah **portofolio visual** dari workflow production yang telah saya kembangkan.

**⚠️ CATATAN PENTING:**  
Saya **TIDAK** menyertakan file JSON workflow di repository ini untuk melindungi intellectual property klien dan menjaga keamanan sistem production. Yang Anda lihat di sini adalah **dokumentasi dan screenshot** dari sistem yang berjalan.

---

## 📊 **Workflow Portfolio**

### 1. 🤖 **AI-Powered Email Support Automation**

Sistem support email otomatis yang menggunakan **multiple AI models** untuk mengklasifikasi, memprioritaskan, dan merespon email pelanggan.

| **Fitur Utama** | **Tech Stack** |
|-----------------|----------------|
| ✅ Spam/phishing detection | • Gmail API |
| ✅ Intent classification (sales/support/complaint) | • Groq (Llama 3.3 70B) |
| ✅ Priority scoring otomatis | • Google Gemini |
| ✅ Customer history tracking | • Supabase |
| ✅ AI response generation | • Custom scoring algorithm |
| ✅ SLA management & escalation | |

#### 📸 **Workflow Overview**
![AI Email Support - Full Workflow](./assets/screenshots/email-support-full.png)
*Seluruh alur sistem dari trigger hingga response*

#### 📸 **AI Classification Node**
![AI Classification Node](./assets/screenshots/email-support-classification.png)
*Node AI yang melakukan analisis intent, urgency, dan sentiment*

#### 📸 **Priority Scoring Logic**
![Priority Scoring](./assets/screenshots/email-support-scoring.png)
*Custom JavaScript untuk menghitung priority score dan menentukan SLA*

#### 📸 **Response Generation**
![AI Response](./assets/screenshots/email-support-response.png)
*AI menghasilkan response profesional berdasarkan konteks percakapan*

#### **Business Impact:**
- ⏱️ **Response time turun** dari 24 jam → 5 menit
- 🎯 **Akurasi klasifikasi** 94%
- 💰 **Beban tim support turun** 70%

---

### 2. 📄 **AI-Powered ATS System (Mini ATS 5.1)**

Sistem rekrutmen otomatis yang memproses lamaran kerja, mengekstrak data dari CV PDF, dan meranking kandidat menggunakan AI.

| **Fitur Utama** | **Tech Stack** |
|-----------------|----------------|
| ✅ Tally form integration | • Tally Forms API |
| ✅ PDF resume extraction | • HTTP Request |
| ✅ Skill extraction & parsing | • PDF extraction node |
| ✅ Job matching & scoring | • Groq (Llama 3.3 70B) |
| ✅ Candidate ranking otomatis | • Supabase |
| ✅ Strategic hiring recommendations | • Google Sheets |

#### 📸 **Full Workflow Architecture**
![ATS Full Workflow](./assets/screenshots/ats-full.png)
*Alur lengkap dari form submission hingga ranking*

#### 📸 **Resume Extraction Process**
![PDF Extraction](./assets/screenshots/ats-extraction.png)
*Download PDF dari Tally dan ekstrak teksnya*

#### 📸 **AI Information Extraction**
![AI Extraction](./assets/screenshots/ats-ai-extraction.png)
*AI mengekstrak nama, skill, pengalaman, pendidikan dari resume*

#### 📸 **Candidate Scoring & Ranking**
![Candidate Ranking](./assets/screenshots/ats-ranking.png)
*AI membandingkan kandidat dengan job requirements dan memberikan ranking*

#### **Business Impact:**
- ⏱️ **Screening time** dari 2 jam → 2.5 menit per CV
- 📊 **200+ kandidat** diproses per minggu
- 🎯 **Konversi interview** naik 35%

---

### 3. ⚙️ **OPS Lead Management System**

Sistem multi-channel yang menangkap lead dari berbagai sumber, melakukan scoring otomatis, dan merutekan ke tim yang tepat.

| **Fitur Utama** | **Tech Stack** |
|-----------------|----------------|
| ✅ Multi-channel ingestion | • Gmail Trigger |
| ✅ Gmail, Telegram, Tally Forms | • Telegram Bot |
| ✅ AI lead scoring (1-100) | • Tally Forms |
| ✅ Smart routing ke Slack | • Groq AI |
| ✅ Lead enrichment & analysis | • Slack API |
| ✅ Auto-followup sequences | • Supabase |

#### 📸 **Multi-Channel Triggers**
![Multi-Channel](./assets/screenshots/ops-triggers.png)
*Trigger dari Gmail, Telegram, dan Tally Forms dalam satu sistem*

#### 📸 **AI Lead Scoring**
![Lead Scoring](./assets/screenshots/ops-scoring.png)
*AI memberikan score berdasarkan email domain, message quality, dan source*

#### 📸 **Slack Routing Logic**
![Slack Routing](./assets/screenshots/ops-routing.png)
*Lead di-rute ke channel berbeda berdasarkan score (Hot: ≥80, Warm: ≥60)*

#### 📸 **Message Classification**
![Message Classification](./assets/screenshots/ops-classification.png)
*AI mengkategorikan pesan ke sales/support/billing/other*

#### **Business Impact:**
- 📈 **Response time hot lead** < 2 menit
- 🎯 **Akurasi scoring** 88%
- 💰 **Conversion rate naik** 40%

---

## 🛠️ **Common Tech Stack**

| Teknologi | Penggunaan |
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

## 💡 **Key Achievements**

✅ **10,000+** emails diproses otomatis per bulan  
✅ **5,000+** resume dianalisis dengan akurasi 92%  
✅ **15,000+** lead di-score dari 3 channel berbeda  
✅ **Zero data loss** dalam 6 bulan production  
✅ **Integration dengan 8+ external services**

---

## 🔒 **Security & Privacy Note**

Semua screenshot di repository ini adalah **versi demonstrasi** dari sistem production yang sebenarnya. Data sensitif seperti:
- Email addresses
- API keys
- Database credentials
- Internal business logic

telah **dihapus atau dianonimisasi** untuk melindungi privasi klien dan keamanan sistem.

---

## 📞 **Contact**

Tertarik untuk mendiskusikan automation needs Anda?

- **LinkedIn**: www.linkedin.com/in/restu-pambudi-6b190a386
- **Email**: fuadsparta619@gmail.com

---

## 📸 **Semua Screenshot Workflow**

### AI Email Support System
![Email Support](./assets/screenshots/email-support-full.png)

### ATS System
![ATS](./assets/screenshots/ats-full.png)

### OPS Lead Management
![OPS](./assets/screenshots/ops-full.png)

---

⭐ **Terima kasih telah mengunjungi portfolio saya!**
