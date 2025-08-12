# AI Speaking Coach – Windsurf Sprint Plan

## Assumptions
- **Stack:** React/Next.js frontend, Node.js backend, Postgres DB, AWS/GCP cloud hosting.
- **AI Services:** Whisper (speech-to-text), OpenAI GPT-4o for NLP feedback, custom ML for pitch/pace metrics.
- **Windsurf Use:** Agents will handle code scaffolding, integration, testing, and deployment orchestration.
- **Sprint Length:** 2 weeks each.
- **Goal:** MVP by Sprint 6.

---

## Sprint 1 – Foundations & Setup
**Goal:** Establish the core technical environment, repo structure, and base authentication.

**Tasks**
1. Initialize Windsurf workspace & monorepo (frontend, backend, shared utils).
2. Set up CI/CD pipelines in Windsurf (auto deploy to staging).
3. Configure Postgres + Prisma ORM schema with `users`, `organizations`.
4. Implement basic authentication (email/password, Google OAuth).
5. Create profile management UI (name, photo, language, timezone).
6. Unit test scaffolding in Windsurf with Jest/Playwright.

**Acceptance Criteria**
- Users can sign up, log in, log out.
- Profile updates persist in DB.
- Deployment pipeline runs automatically on merge.
- Tests auto-run and pass in Windsurf.

**Dependencies**
- None.

---

## Sprint 2 – Speech Session Recording & Upload
**Goal:** Enable speech recording and uploading.

**Tasks**
1. Integrate browser-based audio/video recording (MediaRecorder API).
2. Implement file upload endpoint (S3/GCS storage).
3. Create `speech_sessions` schema & API routes.
4. UI for starting a session, recording, stopping, saving.
5. Upload validation (file type, size).
6. Windsurf tests for upload flow.

**Acceptance Criteria**
- Users can record or upload a speech.
- Files stored in cloud bucket.
- Metadata saved to DB.
- Basic playback available in UI.

**Dependencies**
- Sprint 1 authentication.

---

## Sprint 3 – AI Analysis (Speech-to-Text & Metrics)
**Goal:** Provide first version of AI analysis.

**Tasks**
1. Integrate Whisper API for speech-to-text.
2. Store transcripts in `speech_transcripts`.
3. Implement filler word detection via NLP.
4. Calculate WPM, pacing, and basic clarity score.
5. Display analysis results post-session.
6. Windsurf tests for API accuracy and UI rendering.

**Acceptance Criteria**
- After a session, users see:
  - Transcript
  - WPM
  - Filler word count
  - Clarity score
- All analysis saved in DB.

**Dependencies**
- Sprint 2 file upload.

---

## Sprint 4 – AI Audience Simulation (Basic)
**Goal:** Simulate audience scenarios.

**Tasks**
1. Define `scenarios` table and seed with basic data (pitch, keynote, classroom).
2. Create React-based audience avatars with simple animations.
3. Allow user to select scenario before starting a session.
4. Integrate basic AI-generated follow-up questions using GPT.
5. Capture and display user answers.

**Acceptance Criteria**
- Users can choose from 3+ audience scenarios.
- After speech, audience asks AI-generated questions.
- User responses recorded and stored.

**Dependencies**
- Sprint 3 AI analysis.

---

## Sprint 5 – Personalized Training Plans & Dashboard
**Goal:** Tailor learning plans to users and show progress.

**Tasks**
1. Create `training_plans` schema.
2. Build AI logic to generate personalized weekly plans based on analysis history.
3. Dashboard showing:
   - Historical metrics (graphs)
   - Plan status
   - Suggested exercises
4. Windsurf integration for automated chart rendering (Chart.js/Recharts).

**Acceptance Criteria**
- New users get an initial plan after first speech.
- Dashboard shows metric trends.
- Plan updates automatically each week.

**Dependencies**
- Sprint 3 analysis data.

---

## Sprint 6 – MVP Completion & QA
**Goal:** Polish, fix bugs, prepare for launch.

**Tasks**
1. Implement subscription & billing (Stripe).
2. Add multilingual speech analysis (min. 3 languages).
3. Accessibility features (color-coded pacing, captions).
4. Conduct cross-browser & mobile testing.
5. Prepare Windsurf deployment to production.

**Acceptance Criteria**
- MVP includes:
  - Onboarding
  - Recording & upload
  - AI analysis
  - Audience simulation
  - Training dashboard
  - Billing & accessibility
- All critical bugs resolved.

**Dependencies**
- All prior sprints.

---

## Post-MVP Backlog (Future Sprints)
- **Sprint 7+:** VR/AR audience simulation.
- Gamification (leaderboards, badges).
- Advanced emotional tone analysis.
- Integration with presentation tools (Google Slides, PowerPoint).
