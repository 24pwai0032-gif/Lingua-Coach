# LinguaCoach API Specification

## Overview

**Base URL:** `https://api.linguacoach.ai/v1/`  
**Authentication:** OAuth 2.0 + JWT Bearer Tokens  
**Response Format:** JSON  
**Rate Limits:** 10,000 requests/hour per API key

---

## Authentication

### 1. OAuth 2.0 Authorization Code Flow

**Endpoint:** `POST /auth/authorize`

```json
{
  "client_id": "your_institution_id",
  "client_secret": "your_secret_key",
  "grant_type": "client_credentials",
  "scope": "read:students write:attempts read:analytics"
}
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "read:students write:attempts read:analytics"
}
```

### 2. Bearer Token Usage

All authenticated requests include:
```
Authorization: Bearer {access_token}
```

---

## API Endpoints

### User Management

#### Create Student
**POST** `/students`

**Request:**
```json
{
  "email": "student@school.edu",
  "first_name": "John",
  "last_name": "Doe",
  "class_id": "advanced-7",
  "language_level": "intermediate",
  "target_score": 7.0,
  "test_type": "IELTS"
}
```

**Response:** `201 Created`
```json
{
  "id": "std_abc123",
  "email": "student@school.edu",
  "created_at": "2024-12-20T10:30:00Z",
  "learner_memory": {
    "grammar_issues": [],
    "speaking_habits": [],
    "coach_notes": [],
    "last_updated": null
  }
}
```

#### Get Student Profile
**GET** `/students/{student_id}`

**Response:**
```json
{
  "id": "std_abc123",
  "email": "student@school.edu",
  "first_name": "John",
  "last_name": "Doe",
  "class_id": "advanced-7",
  "language_level": "intermediate",
  "target_score": 7.0,
  "test_type": "IELTS",
  "created_at": "2024-12-20T10:30:00Z",
  "last_active": "2024-12-22T14:15:00Z"
}
```

#### List Students (Paginated)
**GET** `/students?limit=50&offset=0&class_id=advanced-7`

**Response:**
```json
{
  "data": [
    { "id": "std_abc123", "email": "student@school.edu", ... },
    { "id": "std_def456", "email": "another@school.edu", ... }
  ],
  "meta": {
    "total": 125,
    "limit": 50,
    "offset": 0
  }
}
```

---

### Learner Memory

#### Get Learner Memory
**GET** `/students/{student_id}/memory`

**Response:**
```json
{
  "student_id": "std_abc123",
  "grammar_issues": [
    {
      "tag": "articles",
      "severity": 4,
      "examples": ["the students", "a university", "students attend university"],
      "created_at": "2024-12-20T11:00:00Z",
      "last_seen": "2024-12-22T14:00:00Z"
    },
    {
      "tag": "tense_shifting",
      "severity": 2,
      "examples": ["was attending and takes notes"],
      "created_at": "2024-12-21T09:30:00Z",
      "last_seen": "2024-12-22T14:00:00Z"
    }
  ],
  "speaking_habits": [
    {
      "tag": "fillers",
      "severity": 5,
      "examples": ["um", "uh", "like", "you know"],
      "created_at": "2024-12-20T12:15:00Z",
      "last_seen": "2024-12-22T10:30:00Z"
    }
  ],
  "coach_notes": [
    "Writing: main limiter = articles. Next: Article drills in verb phrases.",
    "Speaking: band ~6.0. Focus: filler elimination. Next: 45-second timed answers."
  ],
  "last_updated": "2024-12-22T14:00:00Z"
}
```

---

### Attempts (Submit & Retrieve)

#### Submit Writing Attempt
**POST** `/attempts/writing`

**Request:**
```json
{
  "student_id": "std_abc123",
  "prompt": "Some people believe university education should be free...",
  "text": "I agree that university education should be free for all students...",
  "duration_seconds": 1800,
  "metadata": {
    "test_type": "IELTS",
    "task": "Task 2",
    "word_count": 287
  }
}
```

**Response:** `201 Created`
```json
{
  "attempt_id": "att_xyz789",
  "student_id": "std_abc123",
  "type": "writing",
  "submitted_at": "2024-12-22T14:00:00Z",
  "result": {
    "band": 7.0,
    "rubric": {
      "task_achievement": 8,
      "coherence_cohesion": 7,
      "lexical_resource": 7,
      "grammatical_accuracy": 6
    },
    "metrics": {
      "word_count": 287,
      "sentence_count": 12,
      "average_sentence_length": 23.9,
      "unique_vocabulary_ratio": 0.82
    },
    "strengths": [
      "Clear thesis statement with developed support",
      "Effective use of linking phrases",
      "Relevant examples provided"
    ],
    "issues": [
      {
        "tag": "articles",
        "title": "Inconsistent article usage",
        "tip": "Articles: Use 'the' for specific nouns, 'a/an' for general singular",
        "severity": 3,
        "examples": ["X a education", "✓ education"]
      },
      {
        "tag": "passive_voice",
        "title": "Overuse of passive voice",
        "tip": "Vary voice; maintain active voice where appropriate",
        "severity": 2
      }
    ],
    "drills": [
      {
        "type": "Article placement",
        "question": "Fill in articles: '___ students attend ___ university'",
        "answer": "The students attend a university",
        "tip": "Specific use case: definite vs. indefinite."
      }
    ],
    "next_best_action": "Focus on article accuracy in verb phrases. Complete 5 article drills before next attempt."
  },
  "memory_updated": true
}
```

#### Submit Speaking Attempt
**POST** `/attempts/speaking`

**Request:**
```json
{
  "student_id": "std_abc123",
  "prompt": "Describe a time when you had to work as part of a team",
  "transcript": "I remember a project where I worked with two colleagues...",
  "duration_seconds": 120,
  "audio_url": "https://storage.azure.com/audio/att_xyz_audio.wav"
}
```

**Response:** `201 Created`
```json
{
  "attempt_id": "att_spk456",
  "student_id": "std_abc123",
  "type": "speaking",
  "submitted_at": "2024-12-22T14:05:00Z",
  "result": {
    "band": 6.5,
    "rubric": {
      "fluency_coherence": 6,
      "lexical_resource": 7,
      "grammatical_accuracy": 6,
      "pronunciation": 6
    },
    "metrics": {
      "word_count": 89,
      "duration_seconds": 120,
      "words_per_minute": 44.5,
      "filler_words": ["um", "like", "you know"],
      "filler_count": 8,
      "fillers_per_minute": 4.0
    },
    "strengths": [
      "Vocabulary appropriate for task",
      "Clear narrative structure"
    ],
    "issues": [
      {
        "tag": "fillers",
        "title": "Too many filler words",
        "tip": "Replace 'um' with a 1-2 second pause. Silence is confidence.",
        "severity": 4,
        "examples": ["um", "like", "you know"]
      },
      {
        "tag": "pace",
        "title": "Speaking pace is slow",
        "tip": "Target 120-150 WPM. Practice with a timer.",
        "severity": 2
      }
    ],
    "next_best_action": "Do 45-second re-record: eliminate 3+ filler words."
  },
  "memory_updated": true
}
```

#### Get Attempt History
**GET** `/students/{student_id}/attempts?limit=10&type=writing`

**Response:**
```json
{
  "data": [
    {
      "attempt_id": "att_xyz789",
      "type": "writing",
      "submitted_at": "2024-12-22T14:00:00Z",
      "band": 7.0,
      "next_best_action": "Focus on article accuracy..."
    }
  ],
  "meta": {
    "total": 42,
    "limit": 10,
    "offset": 0
  }
}
```

---

### Analytics

#### Get Student Performance Analytics
**GET** `/students/{student_id}/analytics?period=30d`

**Response:**
```json
{
  "student_id": "std_abc123",
  "period": "30d",
  "summary": {
    "total_attempts": 12,
    "writing_attempts": 6,
    "speaking_attempts": 6,
    "avg_writing_band": 6.7,
    "avg_speaking_band": 6.3,
    "improvement_trend": "positive"
  },
  "writing": {
    "attempts": 6,
    "avg_band": 6.7,
    "max_band": 7.5,
    "min_band": 6.0,
    "trend": [
      { "date": "2024-12-15", "band": 6.0 },
      { "date": "2024-12-17", "band": 6.5 },
      { "date": "2024-12-20", "band": 7.0 },
      { "date": "2024-12-22", "band": 7.5 }
    ]
  },
  "speaking": {
    "attempts": 6,
    "avg_band": 6.3,
    "max_band": 6.5,
    "min_band": 6.0,
    "trend": [...]
  },
  "top_issues": [
    { "tag": "articles", "severity": 4, "frequency": 4 },
    { "tag": "fillers", "severity": 5, "frequency": 6 }
  ]
}
```

#### Get Class Analytics
**GET** `/classes/{class_id}/analytics?period=30d`

**Response:**
```json
{
  "class_id": "advanced-7",
  "period": "30d",
  "students_count": 25,
  "average_writing_band": 6.8,
  "average_speaking_band": 6.4,
  "improvement_rate": 0.12,
  "top_issues_class": [
    { "tag": "articles", "frequency_pct": 68 },
    { "tag": "fillers", "frequency_pct": 56 },
    { "tag": "structure", "frequency_pct": 44 }
  ],
  "student_performance": [
    {
      "student_id": "std_abc123",
      "name": "John Doe",
      "avg_band": 6.7,
      "attempts": 12,
      "improvement": "+0.5"
    }
  ]
}
```

---

### Reports

#### Generate PDF Report
**POST** `/reports/generate`

**Request:**
```json
{
  "student_id": "std_abc123",
  "report_type": "progress",
  "format": "pdf",
  "include_sections": ["summary", "trend", "issues", "recommendations"]
}
```

**Response:** `202 Accepted`
```json
{
  "report_id": "rpt_abc123",
  "status": "processing",
  "estimated_completion": "2024-12-22T14:15:00Z",
  "download_url": "https://reports.linguacoach.ai/rpt_abc123.pdf"
}
```

#### Download Report
**GET** `/reports/{report_id}`

Returns PDF binary with header:
```
Content-Type: application/pdf
Content-Disposition: attachment; filename="linguacoach-progress.pdf"
```

---

### Lessons (AI-Generated Learning Plans)

#### Get Recommended Lessons
**GET** `/students/{student_id}/lessons/recommended`

**Response:**
```json
{
  "student_id": "std_abc123",
  "lessons": [
    {
      "lesson_id": "les_art001",
      "title": "Master Articles in Verb Phrases",
      "description": "Fix your #1 limiter: article usage in complex sentences",
      "duration_minutes": 15,
      "difficulty": "intermediate",
      "based_on_issue": {
        "tag": "articles",
        "severity": 4,
        "reason": "Appeared in 4/6 writing attempts"
      },
      "content": {
        "explanation": "Articles: Use 'the' for specific...",
        "examples": [...],
        "drills": [...]
      },
      "success_criteria": "Score 8/10 on practice questions",
      "created_at": "2024-12-22T10:00:00Z"
    }
  ]
}
```

#### Mark Lesson Complete
**POST** `/students/{student_id}/lessons/{lesson_id}/complete`

**Request:**
```json
{
  "score": 9,
  "time_spent_minutes": 18,
  "drills_completed": 5
}
```

**Response:** `200 OK`
```json
{
  "lesson_id": "les_art001",
  "status": "completed",
  "completed_at": "2024-12-22T14:30:00Z",
  "next_recommended_lesson": {
    "lesson_id": "les_tnx002",
    "title": "Tense Consistency in Narratives",
    "based_on_issue": { "tag": "tense_shifting", "severity": 2 }
  }
}
```

---

## Error Responses

### Standard Error Format
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Missing required field: student_id",
    "details": {
      "field": "student_id",
      "provided": null,
      "expected": "string (UUID)"
    },
    "timestamp": "2024-12-22T14:00:00Z",
    "request_id": "req_abc123"
  }
}
```

### Common Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 202 | Accepted (async) |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Rate Limited |
| 500 | Server Error |

---

## Webhooks

### Attempt Submitted Event

**Event:** `attempt.completed`

```json
{
  "event_id": "evt_abc123",
  "event_type": "attempt.completed",
  "timestamp": "2024-12-22T14:00:00Z",
  "data": {
    "attempt_id": "att_xyz789",
    "student_id": "std_abc123",
    "type": "writing",
    "band": 7.0,
    "next_best_action": "Focus on article accuracy..."
  }
}
```

### Configure Webhook Endpoint

**POST** `/webhooks/configure`

```json
{
  "url": "https://your-school.edu/api/linguacoach-webhook",
  "events": ["attempt.completed", "lesson.completed", "memory.updated"],
  "secret": "whsec_your_secret_key"
}
```

---

## SDKs

Official SDKs available for:
- **Python:** `pip install linguacoach-sdk`
- **JavaScript/Node:** `npm install linguacoach-sdk`
- **Go:** `go get github.com/linguacoach/sdk-go`
- **C#/.NET:** `dotnet add package LinguaCoach.SDK`

**Example (Python):**
```python
from linguacoach import Client

client = Client(
    api_key="sk_live_...",
    api_secret="ss_live_..."
)

# Create student
student = client.students.create(
    email="john@school.edu",
    first_name="John",
    test_type="IELTS"
)

# Submit attempt
result = client.attempts.submit_writing(
    student_id=student.id,
    text="I agree that...",
    prompt="Some people believe..."
)

# Get analytics
analytics = client.analytics.get_student(student.id, period="30d")
```

---

## Rate Limiting

**Headers:**
```
X-RateLimit-Limit: 10000
X-RateLimit-Remaining: 9999
X-RateLimit-Reset: 1703261400
```

**Exceeded Limit Response:**
```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Rate limit exceeded. Reset in 3600 seconds.",
    "retry_after": 3600
  }
}
```

---

## Changelog

### v1.0 (Current)
- Initial release
- Student management, attempts, learner memory
- Analytics, reporting, lesson generation

### v1.1 (Q1 2025)
- Webhook events
- Batch operations
- Advanced filtering

### v2.0 (Q2 2025)
- Real-time WebSocket support
- Graphql endpoint
- Collaboration features (teacher feedback)

---

## Support

**Documentation:** https://docs.linguacoach.ai  
**Status Page:** https://status.linguacoach.ai  
**Support Email:** api-support@linguacoach.ai  
**Slack Community:** https://community.linguacoach.ai  

---

*Last Updated: December 22, 2024*
