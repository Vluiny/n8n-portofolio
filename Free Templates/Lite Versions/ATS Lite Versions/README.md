# 📄 Mini ATS (Lite Version) — n8n Workflow

A simplified version of an AI-powered Applicant Tracking System built using **n8n**.

This workflow demonstrates how candidate data can be automatically processed, stored, and lightly evaluated using automation and basic AI logic.

---

## 🚀 Overview

This is a **lite / simplified version** of a more advanced ATS system.

It focuses on the **core pipeline**:
1. Collect candidate data
2. Extract relevant information
3. Store it in a database
4. Apply basic qualification logic

Designed for:
- Demonstration purposes
- Learning automation architecture
- Sharing a clean, understandable template

---

## ⚙️ Workflow Features

- 📥 **Form Trigger Integration** (e.g., Tally Forms)
- 📄 **Basic Candidate Data Extraction**
- 🗄️ **Automatic Database Insertion (Supabase)**
- 🧠 **Simple Qualification Logic**
- 🏷️ **Candidate Tagging (e.g., Qualified / Basic)**

---

## 🧩 Workflow Structure

1. **Trigger**
   - Receives candidate submission from form

2. **Data Extraction**
   - Extracts:
     - Name
     - Email
     - Skills
     - Experience

3. **Insert Candidate**
   - Stores data into database

4. **Qualification Check (Optional Logic)**
   - Example:
     - If experience > 2 years → `Qualified`
     - Else → `Basic`

---

## 🛠️ Tech Stack

- **n8n** — Workflow automation
- **Supabase** — Database
- **Tally Forms API** — Data input
- **Basic AI / Function Node** — Data processing

---

## 🎯 Purpose

This version is intentionally simplified to:

- Show the **core structure** of an ATS system  
- Keep the workflow **easy to understand**  
- Provide a **starting point** for further development  

---

## ⚠️ Notes

- This is **not a full production ATS**
- Advanced features such as:
  - AI scoring
  - Candidate ranking
  - Multi-job matching  
  are available in the full version

---

## 🔗 Full Version

For a more advanced system with AI scoring and ranking:

👉 https://github.com/Vluiny/n8n-portofolio

---

## 📌 Usage

You can:
- Import this workflow into n8n
- Connect your own database
- Modify the qualification logic
- Extend it into a full ATS system

---

## 📞 Contact

- LinkedIn: https://linkedin.com/in/restu-pambudi-6b190a386
- Email : restupambudi.dev@gmail.com

---

⭐ If you find this useful, feel free to explore the full portfolio.
