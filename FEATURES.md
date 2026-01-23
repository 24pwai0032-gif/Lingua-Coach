# LinguaCoach - Complete Feature Guide


---

## 📚 Features Overview

### 1. **Micro-Mock Flow** (Tab: Micro-Mock)
- Quick 2-3 minute practice session
- Select test type: IELTS or Duolingo
- Set your goal score
- Quick links to Writing, Speaking, and dashboard

### 2. **Writing Practice** (Tab: Writing)
- **Prompt:** Context-specific writing task
- **Analysis includes:**
  - Band score estimation (0-9)
  - Rubric breakdown (Task Response, Coherence, Vocabulary, Grammar)
  - Identified strengths
  - Top issues limiting your score
  - Sentence-by-sentence highlighting with feedback
  - Suggested rewrite example
  - 6 personalized drills to improve weaknesses
  - "Next Best Action" (10-minute improvement goal)
- **Learner Memory:** Issues are saved and tracked over time

### 3. **Speaking Practice** (Tab: Speaking)
- **Prompt:** Speaking task (45-60 seconds)
- **Live Transcript Support:**
  - Use browser Speech Recognition (works best in Chrome)
  - Or manually paste your transcript
- **Analysis includes:**
  - Band score estimation (0-9)
  - Rubric breakdown (Fluency, Coherence, Vocabulary, Pronunciation)
  - Metrics (WPM, filler words/min, word count, duration)
  - Identified issues (pace, structure, fillers)
  - 5 personalized speaking drills
  - "Next Best Action" recommendation
- **Learner Memory:** Speaking habits tracked with severity levels

### 4. **Delivery Practice** (Tab: Delivery) 🎥 **NEW**
- **Webcam Mode:** Practice on video
- **Real-time Analysis:**
  - Eye contact score (0-100) - face detection if supported
  - Movement control score (based on motion detection)
  - Coach notes on your delivery
- **Feedback includes:**
  - Eye contact consistency assessment
  - Body movement and stability evaluation
  - Actionable tips for improvement
- **Memory Integration:** Delivery insights saved to your Learner Memory

### 5. **Learning Center** (Tab: Learning) 📚 **NEW**
- **AI-Generated Study Plan:** Based on your writing and speaking analysis
- **Personalized Lessons:**
  - Each lesson targets your specific weakness
  - Severity level (1-5) indicates priority
  - Custom tips (3 actionable study strategies)
  - Your real examples from attempts
- **Interactive Learning:**
  - View lesson details
  - Mark lessons as complete
  - Track progress
- **Spaced Repetition:** Lessons prioritized by issues severity

### 6. **Dashboard** (Tab: Dashboard)
- **Progress Snapshot:**
  - Total attempts
  - Average writing band (last 5)
  - Average speaking band (last 5)
- **Learner Memory Display:**
  - Top grammar limiters (writing issues)
  - Top speaking habits (speaking issues)
  - Each with severity levels and real examples
- **Coach Notes:**
  - Historical notes from your coaching sessions
  - Shows progression and key feedback points
- **Action Items:**
  - Reset demo (start fresh)

---

## 🧠 Learner Memory System

### How It Works
1. **Automatic Capture:** Every time you complete Writing, Speaking, or Delivery practice, issues are detected
2. **Severity Tracking:** Issues are bumped (increased severity 1-5) if they appear repeatedly
3. **Example Collection:** Actual examples from your work are stored (up to 3 per issue)
4. **Timeline:** Last update timestamp shows when memory was last modified
5. **Coach Notes:** Auto-generated notes capture the coaching essence

### Data Persisted
- ✅ Grammar issues from writing
- ✅ Speaking habits from speaking practice
- ✅ Delivery signals from video
- ✅ Coach notes (max 12 most recent)
- ✅ Attempt history (all sessions)

---

## 🎓 Personalized Learning Flow

```
Do Writing → Issues Detected → Memory Updated
    ↓
Do Speaking → More Issues → Memory Enhanced
    ↓
Do Delivery → Movement Insights → Integrated
    ↓
Generate Lessons → Based on Top Issues
    ↓
Study Tips → Custom to Your Weaknesses
    ↓
Mark Complete → Progress Tracked
```

---

## 💾 Data Storage

- **Location:** Browser's LocalStorage
- **Key:** `linguacoach_store`
- **Auto-saved:** After every action
- **Persistent:** Survives browser closes
- **No account needed:** Everything local

---

## 🚀 Test It Out

1. **Micro-Mock:** Click "Start Writing" → Write 100+ words → Generate Feedback
2. **Writing:** Fill in response → See rubric, issues, drills → Memory updates
3. **Speaking:** Paste/record transcript → See fluency metrics → Memory updates
4. **Delivery:** Click "Start Camera" → Practice → Get eye contact score → Memory updates
5. **Learning:** Click "Generate Study Plan" → See personalized lessons → Study and complete
6. **Dashboard:** See all memory tracked → Review progress → Read coach notes

---

## 📊 Analysis Details

### Writing Analysis Checks
- Word count (target 180-250)
- Paragraph structure
- Linking words/coherence markers
- Article usage
- Tense consistency
- Vocabulary repetition
- Overall task response

### Speaking Analysis Checks
- Words per minute (target 90-165)
- Filler words frequency
- Answer structure markers
- Vocabulary precision
- Pronunciation feedback

### Delivery Analysis Checks
- Eye contact consistency (face detection)
- Body movement/fidgeting (motion tracking)
- Posture stability
- Confidence signals

---

## ✨ Next Steps for Enhancement

Future versions could add:
- ✅ Azure OpenAI integration for better rewrites
- ✅ Azure Speech API for pronunciation scoring
- ✅ Database backend for progress analytics
- ✅ Export reports (PDF)
- ✅ Multi-language support
- ✅ Mobile app version
- ✅ Timed practice modes
- ✅ Social comparison (anonymized benchmarks)

---

## 🛠️ Technical Stack

- **Frontend:** React 19 + CSS
- **Build:** Vite
- **Storage:** Browser LocalStorage
- **APIs Used:** 
  - WebRTC (optional camera)
  - Web Speech API (optional speech recognition)
  - Face Detection API (if browser supports)
  
**No external dependencies** - fully self-contained MVP!

---

**Happy studying! 🎉**
