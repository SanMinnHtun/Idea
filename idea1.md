# 🏥 MediMind AI — Personal Health Intelligence Platform

> **An AI-powered healthcare web application featuring real-time symptom analysis, health data tracking, and a conversational medical chatbot — all running locally on your machine.**

---

## 📋 Table of Contents

1. [Project Overview](#-project-overview)
2. [Key Features](#-key-features)
3. [System Architecture](#-system-architecture)
4. [Technology Stack](#-technology-stack)
5. [Workflow — How It Works](#-workflow--how-it-works)
   - [Feature 1: AI Symptom Analyzer](#feature-1-ai-symptom-analyzer)
   - [Feature 2: Health Data Tracker](#feature-2-health-data-tracker)
   - [Feature 3: AI Chatbot](#feature-3-ai-chatbot)
6. [AI Integration (Ollama)](#-ai-integration-ollama)
7. [Data Flow Diagram](#-data-flow-diagram)
8. [Setup & Installation Guide](#-setup--installation-guide)
9. [Project File Structure](#-project-file-structure)
10. [Privacy & Security](#-privacy--security)
11. [Future Roadmap](#-future-roadmap)
12. [Disclaimer](#-disclaimer)

---

## 🌟 Project Overview

**MediMind AI** is a browser-based healthcare assistant that combines **artificial intelligence** with **health data visualization** to provide users with:

- Intelligent symptom analysis powered by a **local AI language model (Ollama)**
- A personal health dashboard with **real-time trend charts**
- A **24/7 AI health chatbot** for health Q&A

The entire application runs **100% locally** — no data is sent to external servers, ensuring complete **patient privacy**.

```
┌─────────────────────────────────────────────────────────┐
│                    USER's BROWSER                        │
│                                                         │
│   ┌──────────┐  ┌──────────────┐  ┌─────────────────┐  │
│   │ Symptom  │  │   Health     │  │   AI Chatbot    │  │
│   │ Analyzer │  │   Tracker    │  │   (MediMind)    │  │
│   └────┬─────┘  └──────┬───────┘  └────────┬────────┘  │
│        │               │                    │           │
│        └───────────────┼────────────────────┘           │
│                        │                                │
│            ┌───────────▼───────────┐                   │
│            │    Ollama Local AI    │                   │
│            │   (llama3.2 model)    │                   │
│            │   localhost:11434     │                   │
│            └───────────────────────┘                   │
└─────────────────────────────────────────────────────────┘
```

---

## 🎯 Key Features

| Feature | Description | AI Used |
|---|---|---|
| 🔍 **Symptom Analyzer** | Enter symptoms → AI diagnoses possible conditions | ✅ Ollama LLM |
| 📊 **Health Tracker** | Log vitals, view charts, get AI insights | ✅ Rule-based AI |
| 💬 **AI Chatbot** | Ask any health question, get expert answers | ✅ Knowledge base |
| 📈 **Health Score** | Dynamic score (0–100) based on your vitals | ✅ Weighted algorithm |
| 🔔 **Activity Feed** | Log of all health events and checks | — |
| 💾 **Data Persistence** | All data saved locally (no server needed) | — |

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MEDIMIND AI ARCHITECTURE                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                  FRONTEND (Browser)                  │   │
│  │                                                     │   │
│  │   index.html ──── Single Page Application (SPA)    │   │
│  │       │                                            │   │
│  │       ├── css/styles.css   (Design System)         │   │
│  │       │                                            │   │
│  │       ├── js/app.js        (Navigation + State)    │   │
│  │       ├── js/symptom-engine.js  (AI Connector)     │   │
│  │       ├── js/health-tracker.js  (Vitals + Score)   │   │
│  │       ├── js/charts.js          (Visualizations)   │   │
│  │       └── js/chatbot-engine.js  (Health Chatbot)   │   │
│  └──────────────────────────┬──────────────────────────┘   │
│                             │  HTTP REST API call           │
│                             ▼                               │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              OLLAMA (Local AI Engine)                │   │
│  │                                                     │   │
│  │   Running at: http://localhost:11434                │   │
│  │   Model: llama3.2 (or mistral, phi3, gemma2...)    │   │
│  │                                                     │   │
│  │   Endpoint: POST /api/generate                     │   │
│  │   - Receives: Medical prompt with symptoms         │   │
│  │   - Returns: JSON with diagnosis + recommendations │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │            LOCAL STORAGE (Browser)                   │   │
│  │   Stores: Vitals readings, health history           │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | HTML5, CSS3, Vanilla JavaScript | App structure, UI, logic |
| **Design** | Custom CSS with Glassmorphism | Dark mode premium UI |
| **Charts** | Chart.js (CDN) | Health data visualization |
| **AI Engine** | Ollama (local LLM runner) | Runs AI model locally |
| **AI Model** | Llama 3.2 (Meta) | Medical symptom analysis |
| **Storage** | Browser localStorage | Persists health data |
| **Server** | Python http.server | Serves files locally |
| **Font** | Google Fonts (Inter) | Typography |

---

## 🔄 Workflow — How It Works

---

### Feature 1: 🔍 AI Symptom Analyzer

This is the core AI-powered feature. Here is the step-by-step workflow:

```
STEP 1: User Input
──────────────────
User selects body part (Head, Chest, Abdomen, etc.)
         +
User types symptoms (e.g., "headache, fever, nausea")
         +
User sets severity slider (1–10)
         +
User selects duration and age/sex
         +
User clicks "Analyze with AI"

         │
         ▼

STEP 2: Validation
──────────────────
JavaScript checks if at least 1 symptom was entered
If empty → show error toast notification
If valid → proceed to AI call

         │
         ▼

STEP 3: Ollama Connection Check
────────────────────────────────
Browser fetches: GET http://localhost:11434/api/tags
- If Ollama not running → show setup instructions
- If no model installed → show "ollama pull llama3.2" instructions
- If OK → detect best available model (llama3.2, mistral, phi3...)

         │
         ▼

STEP 4: Prompt Engineering
───────────────────────────
JavaScript builds a structured medical prompt:

  "You are MediMind AI, an expert medical symptom analyzer.
   Patient symptoms: headache, fever, nausea
   Affected area: head
   Severity: 7/10
   Duration: 2-3 days
   Age: 35, Sex: male
   
   Respond ONLY with JSON containing:
   - urgency level (home/monitor/doctor/emergency)
   - possible conditions with probabilities
   - recommendations
   - summary"

         │
         ▼

STEP 5: Streaming AI Response
──────────────────────────────
Browser sends: POST http://localhost:11434/api/generate
- Uses streaming=true for real-time token output
- UI shows "Thinking... (150+ tokens)"
- AI model processes medical knowledge
- Returns structured JSON response

         │
         ▼

STEP 6: Parse & Display Results
────────────────────────────────
JavaScript extracts JSON from AI response
Renders results:
  ✅ Urgency card (🏠 Home Care / 👨‍⚕️ See Doctor / 🚨 Emergency)
  ✅ AI Summary paragraph
  ✅ Top 3-4 possible conditions with probability bars
  ✅ Specific actionable recommendations
  ✅ Specialist recommendation

         │
         ▼

STEP 7: Activity Logging
─────────────────────────
Event is added to the Dashboard activity feed
```

**Example AI Response (JSON):**
```json
{
  "urgency": "doctor",
  "conditions": [
    {
      "name": "Influenza (Flu)",
      "probability": 82,
      "description": "Viral respiratory illness with sudden onset",
      "specialist": "General Practitioner"
    },
    {
      "name": "COVID-19",
      "probability": 65,
      "description": "Coronavirus respiratory infection",
      "specialist": "General Practitioner / Infectious Disease"
    }
  ],
  "recommendations": [
    "Rest and stay well hydrated with fluids",
    "Take fever reducers (acetaminophen or ibuprofen)",
    "Monitor oxygen levels with a pulse oximeter",
    "See a doctor if symptoms worsen after 3 days"
  ],
  "summary": "Based on your symptoms of headache, fever, and nausea with a severity of 7/10 over 2-3 days, the most likely diagnosis is Influenza or COVID-19. Medical evaluation is recommended within 24-48 hours."
}
```

---

### Feature 2: 📊 Health Data Tracker

```
STEP 1: Log Vitals
───────────────────
User fills in any/all of:
  ❤️  Heart Rate (BPM)
  🩸  Blood Pressure (Systolic / Diastolic mmHg)
  💉  Blood Glucose (mg/dL)
  🌡️  Temperature (°F)
  🫁  SpO2 (%)
  ⚖️  Weight (lbs)

         │
         ▼

STEP 2: Save to localStorage
──────────────────────────────
Data saved as JSON array in browser:
{
  "ts": "2026-07-04T10:00:00Z",
  "hr": 74,
  "bpSys": 122,
  "bpDia": 80,
  "glucose": 98,
  "temp": 98.6,
  "spo2": 97,
  "weight": 168
}

         │
         ▼

STEP 3: Status Evaluation
──────────────────────────
Each vital is compared against medical normal ranges:
  Heart Rate:   Normal 60-100 BPM
  BP Systolic:  Normal <120 mmHg
  BP Diastolic: Normal <80 mmHg
  Glucose:      Normal 70-140 mg/dL
  Temperature:  Normal 97–99.5°F
  SpO2:         Normal 95-100%

Result: Color-coded status dot (🟢 Normal / 🟡 Warning / 🔴 Critical)

         │
         ▼

STEP 4: Health Score Calculation
──────────────────────────────────
Weighted average across all vitals (last 5 readings):
  - Heart Rate:    weight × 2
  - Blood Pressure: weight × 2
  - Glucose:       weight × 1.5
  - SpO2:          weight × 2
  - Temperature:   weight × 1

Score 0-100 displayed in animated ring:
  80-100: Excellent (green)
  60-79:  Good Condition (green)
  40-59:  Fair - Monitor (yellow)
  0-39:   Poor - See Doctor (red)

         │
         ▼

STEP 5: Chart Rendering (Chart.js)
───────────────────────────────────
4 line charts render with last 7 days of data:
  📈 Heart Rate trend
  📈 Blood Pressure (Systolic + Diastolic)
  📈 Blood Glucose trend
  📈 SpO2 trend

Charts use smooth curves, gradient fills, and interactive tooltips.

         │
         ▼

STEP 6: AI Insight Generation
───────────────────────────────
Rule-based AI analyzes the trends:

  Average HR > 100?  → "Elevated Heart Rate — reduce caffeine"
  Average BP > 140?  → "Hypertension detected — see doctor"
  Glucose > 140?     → "Elevated glucose — reduce carbs"
  SpO2 < 95?         → "Low oxygen saturation — seek care"
  HR trending up?    → "Heart rate increasing — monitor stress"

Insights displayed as color-coded cards:
  🟢 Good  |  🔵 Info  |  🟡 Warning  |  🔴 Critical
```

---

### Feature 3: 💬 AI Health Chatbot

```
STEP 1: User Types Question
────────────────────────────
User types: "How do I lower my blood pressure naturally?"
OR clicks a Quick Topic button

         │
         ▼

STEP 2: Intent Detection
─────────────────────────
JavaScript scans knowledge base for keyword matches:

  Input: "lower blood pressure naturally"
  
  Matches pattern: ["blood pressure", "hypertension", "high bp"]
  Score: 24 points (based on word overlap + length)
  
  Selects highest-scoring knowledge base entry

         │
         ▼

STEP 3: Simulate Typing Delay
───────────────────────────────
Bot shows animated "typing..." indicator for 0.8–2.0 seconds
Creates a realistic, human-like conversation experience

         │
         ▼

STEP 4: Display Rich Response
───────────────────────────────
Chatbot renders formatted HTML response with:
  - Bold headings
  - Bullet point lists
  - Important warnings
  - Medical thresholds and numbers

         │
         ▼

STEP 5: Quick Replies Update
─────────────────────────────
Suggested follow-up quick reply chips shown
User can continue the conversation thread

Knowledge Base covers 25+ topics:
  ✅ Diabetes, Blood Pressure, Heart Attack
  ✅ Headache, Fever, Cold & Flu, COVID-19
  ✅ Sleep, Stress & Anxiety, Depression
  ✅ Hydration, Diet, Exercise, Vitamins
  ✅ Normal Vital Signs, When to go to ER
  ✅ Weight Loss, Back Pain, Cholesterol
  ✅ Cancer awareness, Pregnancy
  ✅ Emergency guidance (911 prompts)
```

---

## 🤖 AI Integration (Ollama)

### What is Ollama?

Ollama is a tool that lets you run large language models (LLMs) **completely offline** on your own computer — no internet, no API key, no cost.

```
Without Ollama:                    With Ollama:
───────────────                    ────────────
Your App                           Your App
    │                                  │
    │  Internet required               │  Local only
    ▼                                  ▼
OpenAI Servers            localhost:11434 (Ollama)
(paid, external)              │
                          llama3.2 model
                          (runs on your GPU/CPU)
```

### How the Symptom Analyzer Calls Ollama

```javascript
// 1. Check if Ollama is running
GET http://localhost:11434/api/tags

// 2. Send symptom analysis request (streaming)
POST http://localhost:11434/api/generate
{
  "model": "llama3.2",
  "prompt": "[medical prompt with symptoms]",
  "stream": true,
  "options": {
    "temperature": 0.3,   // Lower = more focused, less random
    "top_p": 0.9
  }
}

// 3. Parse streaming response tokens in real-time
// 4. Extract JSON from completed response
// 5. Render results in UI
```

### Supported AI Models

| Model | Size | Speed | Quality | Command |
|---|---|---|---|---|
| **llama3.2** ⭐ | 2GB | Fast | Excellent | `ollama pull llama3.2` |
| mistral | 4GB | Medium | Very Good | `ollama pull mistral` |
| phi3 | 2.3GB | Fast | Good | `ollama pull phi3` |
| gemma2 | 5GB | Medium | Excellent | `ollama pull gemma2` |
| llama3.1 | 5GB | Slow | Best | `ollama pull llama3.1` |

---

## 📊 Data Flow Diagram

```
┌────────────────────────────────────────────────────────────┐
│                     MEDIMIND AI DATA FLOW                   │
└────────────────────────────────────────────────────────────┘

  USER INPUT                PROCESSING               OUTPUT
  ──────────               ────────────              ──────

  Symptoms          ──►  Build AI Prompt     ──►  Conditions List
  Body Part         ──►  Call Ollama API     ──►  Urgency Level
  Severity          ──►  Parse JSON          ──►  Recommendations
  Duration          ──►  Render Results      ──►  AI Summary

  Vitals Data       ──►  Validate Ranges     ──►  Status Colors
  (HR, BP, etc.)    ──►  Save to Storage     ──►  Status Cards
                    ──►  Calc Health Score   ──►  Score Ring (0-100)
                    ──►  Generate Insights   ──►  AI Insights Cards
                    ──►  Render Charts       ──►  Line Charts (7 days)

  Chat Message      ──►  Intent Detection    ──►  Rich HTML Response
  (text input)      ──►  Knowledge Match     ──►  Typing Animation
                    ──►  Format Response     ──►  Quick Replies
```

---

## 🚀 Setup & Installation Guide

### Prerequisites

- Windows 10/11 (or macOS/Linux)
- Python 3.x (for local server)
- Modern browser (Chrome, Firefox, Edge)
- 4GB+ RAM recommended for AI model

### Step 1: Start the Web Application

```powershell
# Navigate to the project folder
cd C:\Users\Hello\.gemini\antigravity\scratch\healthcare-ai-agent

# Start the local web server
python -m http.server 8080

# Open in browser:
# http://localhost:8080
```

### Step 2: Install Ollama (for AI Symptom Analysis)

```powershell
# Option A: Install via winget (Windows)
winget install Ollama.Ollama

# Option B: Download from website
# Visit: https://ollama.com → Download for Windows
```

### Step 3: Pull an AI Model

```powershell
# Recommended (small, fast, high quality)
ollama pull llama3.2

# Alternative options
ollama pull mistral    # Good for detailed medical responses
ollama pull phi3       # Very fast, smaller model
```

### Step 4: Verify Everything Works

```powershell
# Check Ollama is running
ollama list

# Test the API directly
curl http://localhost:11434/api/tags
```

### Step 5: Open the App

```
Open your browser and go to:
http://localhost:8080
```

---

## 📁 Project File Structure

```
healthcare-ai-agent/
│
├── 📄 index.html                  ← Main app (Single Page Application)
│   ├── Dashboard section
│   ├── Symptom Checker section
│   ├── Health Tracker section
│   └── AI Chatbot section
│
├── 📁 css/
│   └── 🎨 styles.css              ← Full design system
│       ├── CSS Variables (colors, spacing, fonts)
│       ├── Glassmorphism components
│       ├── Navigation styles
│       ├── Animated background orbs
│       ├── All 4 section layouts
│       ├── Chart cards
│       ├── Chat UI styles
│       └── Responsive breakpoints
│
└── 📁 js/
    ├── 🧠 symptom-engine.js       ← AI Symptom Analyzer
    │   ├── Ollama connection checker
    │   ├── Model auto-detection
    │   ├── Medical prompt builder
    │   ├── Streaming AI response handler
    │   ├── JSON parser & validator
    │   └── Results renderer
    │
    ├── 📊 health-tracker.js       ← Health Data Module
    │   ├── Vitals normal ranges database
    │   ├── Status evaluator (Normal/Warning/Critical)
    │   ├── localStorage read/write
    │   ├── Health Score algorithm
    │   ├── AI insights generator
    │   └── Dashboard updater
    │
    ├── 📈 charts.js               ← Data Visualization
    │   ├── Chart.js configuration
    │   ├── Gradient fill generator
    │   ├── Heart Rate chart
    │   ├── Blood Pressure chart (dual line)
    │   ├── Blood Glucose chart
    │   └── SpO2 chart
    │
    ├── 💬 chatbot-engine.js       ← AI Health Chatbot
    │   ├── 25+ topic knowledge base
    │   ├── Intent detection engine
    │   ├── Message renderer (HTML)
    │   ├── Typing animation
    │   └── Chat history manager
    │
    └── ⚙️ app.js                  ← Application Controller
        ├── Section navigation
        ├── Toast notification system
        ├── Activity feed manager
        ├── Keyboard shortcuts (Ctrl+1-4)
        └── Initialization orchestrator
```

---

## 🔒 Privacy & Security

| Feature | Details |
|---|---|
| **No cloud dependency** | Entire app runs 100% locally |
| **No data collection** | Zero telemetry or tracking |
| **No external API calls** | AI runs on your own machine |
| **Local storage only** | Health data stays in YOUR browser |
| **No account required** | No registration or login |
| **Offline capable** | Works without internet (after setup) |

> ✅ Your health data **never leaves your device**. This is a critical privacy advantage over cloud-based health apps.

---

## 🗺️ Future Roadmap

| Feature | Priority | Description |
|---|---|---|
| 🔗 Real Gemini/GPT API | High | Connect to cloud AI for better accuracy |
| 📱 Mobile App (PWA) | High | Install as phone app (Progressive Web App) |
| 📄 PDF Health Reports | Medium | Export health summary as PDF |
| 📷 Medical Image Analysis | Medium | Analyze skin conditions from photos |
| 🔔 Medication Reminders | Medium | Push notification for medication times |
| 👨‍👩‍👧 Multi-User Profiles | Low | Track health for family members |
| 🏥 Doctor Integration | Low | Share reports directly with physician |
| ⌚ Wearable Sync | Low | Import from Apple Health / Google Fit |

---

## ⚠️ Disclaimer

> **MediMind AI is an educational and informational tool only.**
>
> - ❌ It does **NOT** provide medical diagnosis
> - ❌ It is **NOT** a replacement for professional medical advice
> - ❌ It should **NOT** be used in medical emergencies without calling emergency services first
>
> **Always consult a qualified healthcare professional** for any health concerns, diagnosis, or treatment decisions.
>
> In case of emergency, call **911** (US) or your local emergency number immediately.

---

## 👨‍💻 Built With

- 💚 **HTML5 + CSS3 + JavaScript** — No framework dependencies
- 🤖 **Ollama** — Local AI inference engine
- 🦙 **Llama 3.2** — Meta's open-source language model
- 📊 **Chart.js** — Beautiful data visualization
- 🎨 **Glassmorphism Design** — Modern dark UI aesthetic

---

*MediMind AI — Empowering people with AI-driven health intelligence, locally and privately.*
