# AI Speaking Coach – Data Model

## 0. Conventions
- **PK** = Primary Key
- **FK** = Foreign Key
- **UK** = Unique Key
- Timestamps = `created_at`, `updated_at` in UTC
- Soft delete via `deleted_at` (nullable)
- All IDs are UUID (v4) unless stated
- PII tags indicate sensitivity (S1 low → S3 high)

---

## 1. Core Entities

### 1.1 Users
**users**
- id (PK)
- email `[PII:S2]` (UK)
- hashed_password
- first_name `[PII:S1]`
- last_name `[PII:S1]`
- profile_photo_url
- locale (e.g., `en-US`)
- timezone
- role (enum: `user`, `admin`, `coach`)
- feature_flags (jsonb)
- marketing_opt_in (bool)
- created_at, updated_at, deleted_at

---

### 1.2 Organizations (for team or enterprise plans)
**organizations**
- id (PK)
- name
- subscription_tier (enum: `free`, `pro`, `enterprise`)
- billing_id (FK → billing_accounts.id)
- created_at, updated_at, deleted_at

**organization_users**
- id (PK)
- organization_id (FK → organizations.id)
- user_id (FK → users.id)
- role (enum: `member`, `admin`, `owner`)
- created_at, updated_at, deleted_at

---

## 2. Speech Sessions & Analysis

### 2.1 Speech Sessions
**speech_sessions**
- id (PK)
- user_id (FK → users.id)
- title
- speech_type (enum: `keynote`, `pitch`, `interview`, `toast`, `practice`)
- language_code (ISO 639-1)
- duration_seconds
- audio_file_url
- video_file_url (optional)
- scenario_id (FK → scenarios.id)
- created_at, updated_at, deleted_at

---

### 2.2 AI Analysis Results
**speech_analysis**
- id (PK)
- speech_session_id (FK → speech_sessions.id)
- words_per_minute
- filler_word_count
- filler_words (jsonb)
- average_pitch (Hz)
- pitch_variability
- volume_average (dB)
- clarity_score (0–100)
- confidence_score (0–100)
- pronunciation_issues (jsonb)
- sentiment_profile (jsonb)
- created_at

---

### 2.3 Transcripts
**speech_transcripts**
- id (PK)
- speech_session_id (FK → speech_sessions.id)
- transcript_text (longtext)
- word_timestamps (jsonb)
- created_at

---

## 3. Training & Feedback

### 3.1 Training Plans
**training_plans**
- id (PK)
- user_id (FK → users.id)
- plan_name
- skill_focus (jsonb array: e.g., ["pacing", "clarity"])
- start_date
- end_date
- progress_percent
- created_at, updated_at

---

### 3.2 Feedback
**feedback_entries**
- id (PK)
- speech_session_id (FK → speech_sessions.id)
- feedback_type (enum: `automated`, `human`)
- feedback_text
- improvement_suggestions (jsonb)
- created_at

---

## 4. Audience Simulation

### 4.1 Scenarios
**scenarios**
- id (PK)
- name (e.g., "Investor Pitch", "Classroom Lecture")
- description
- difficulty_level (1–5)
- environment_assets (jsonb)
- created_at, updated_at

### 4.2 Q&A
**audience_questions**
- id (PK)
- scenario_id (FK → scenarios.id)
- question_text
- difficulty_level (1–5)
- generated_by_ai (bool)
- created_at

---

## 5. Billing & Subscription

### 5.1 Billing Accounts
**billing_accounts**
- id (PK)
- organization_id (FK → organizations.id, nullable for individual users)
- provider (enum: `stripe`, `paypal`)
- provider_customer_id
- created_at, updated_at

### 5.2 Subscriptions
**subscriptions**
- id (PK)
- billing_account_id (FK → billing_accounts.id)
- plan (enum: `free`, `pro`, `enterprise`)
- status (enum: `active`, `past_due`, `canceled`)
- current_period_start
- current_period_end
- created_at, updated_at

---

## 6. Analytics & Tracking

**user_activity_logs**
- id (PK)
- user_id (FK → users.id)
- activity_type (enum: `login`, `practice_started`, `analysis_viewed`, `plan_completed`)
- metadata (jsonb)
- created_at

**speech_metrics_history**
- id (PK)
- user_id (FK → users.id)
- metric_name (e.g., `wpm`, `clarity_score`)
- metric_value
- recorded_at

---
