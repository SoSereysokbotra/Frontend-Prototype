# JobFits User Flows Guide - Organized
**Document Version:** 3.0 (ER-Aligned, Development-Ready)  
**Based on:** JobFits ER Diagram v1.0 + SRS v2.0  
**Last Updated:** July 2026  
**Status:** MVP Scope - Complete Coverage  
**Database Entities Covered:** 24/24 ✅

---

## DOCUMENT OVERVIEW

This guide provides comprehensive documentation of all user journeys and UX flows for the JobFits platform, mapped to database entities and API requirements.

**Core User Journey:**  
Signup → Complete Profile → Upload Resume → View Recommendations → Apply → Track Progress → Interview Prep → Decision

---

## QUICK NAVIGATION

### Part 1: Foundation & Setup
- [Platform Overview](#platform-overview)
- [Navigation Structure](#navigation-structure)
- [User Roles & Personas](#user-roles--personas)

### Part 2: Core User Flows
- [Flow 0: Authentication](#flow-0-authentication)
- [Flow 1: Onboarding](#flow-1-onboarding-expanded)
- [Flow 2: Discovery](#flow-2-discovery-paths)
- [Flow 3: Job Journey](#flow-3-your-journey-expanded)
- [Flow 4: Profile & Resources](#flow-4-profile--resources-expanded)
- [Flow 5: Settings & Support](#flow-5-help--preferences-expanded)

### Part 3: Monetization & Growth
- [Flow 8: Subscriptions & Payment](#flow-8-subscriptions--payment)
- [Flow 9: Referral Program](#flow-9-referral-program)

### Part 4: Platform Integration
- [Flow 6: Chrome Extension](#flow-6-chrome-extension-integration)
- [Flow 7: Admin Panel](#flow-7-admin-panel)

### Part 5: Technical Reference
- [ER-to-Flow Mapping](#er-to-flow-mapping-matrix)
- [API Requirements](#backend-api-requirements)
- [Database Performance](#database-performance-notes)
- [Decision Points](#decision-points--alternative-paths)
- [Success Metrics](#metrics--success-indicators)

---

## PART 1: FOUNDATION & SETUP

---

### PLATFORM OVERVIEW

**JobFits** is a **job-seeker-centric platform** designed to help candidates find well-matched career opportunities through AI recommendations, application tracking, and career development resources. The platform monetizes through tiered subscriptions while maintaining a robust free tier.

**Architecture:**
- **24 database entities** mapped to 9 user flows
- **3 user tiers** (Free, Premium, Professional) with feature access control
- **API-first design** supporting web, mobile, and Chrome Extension

**Key Features:**
- AI-powered job matching & recommendations
- Resume optimization & parsing
- Application tracking with timeline
- Interview preparation resources
- Salary intelligence & comparison
- Career learning paths
- Referral program with rewards
- Multi-tier subscription system

---

### NAVIGATION STRUCTURE

#### Main Sidebar Layout (v3 with Monetization)

```
┌─ JOBFITS ──────────────────────────────────┐
│                                             │
│ 🏠 DASHBOARD                                │
│ ├─ Quick Stats (applications, interviews)  │
│ ├─ Next Steps Widget                       │
│ └─ Recent Activity                         │
│                                             │
│ DISCOVERY                                  │
│ ├─ 🔍 Search Jobs                          │
│ ├─ ⭐ AI Recommendations                    │
│ └─ 💾 Saved Jobs                           │
│                                             │
│ YOUR JOURNEY                                │
│ ├─ 📋 Applications (status tracking)       │
│ ├─ 📅 Interview Prep (tips, questions)    │
│ ├─ 💼 Offers & Decisions                   │
│ └─ 📊 My Stats (analytics)                 │
│                                             │
│ PROFILE & RESOURCES                        │
│ ├─ 👤 My Profile (personal info)          │
│ ├─ 💼 Skills & Experience (detailed)      │
│ ├─ 📄 Resumes (multi-version mgmt)        │
│ ├─ 📊 Career Insights (learning paths)    │
│ └─ 💡 Salary Intelligence (market data)   │
│                                             │
│ HELP & PREFERENCES                         │
│ ├─ 🔔 Notifications (settings + history)  │
│ ├─ ❓ Help Center (FAQs, knowledge base)  │
│ ├─ ⚙️ Settings (profile, privacy)         │
│ ├─ 🎁 Referrals (invite & rewards)        │
│ └─ ⭐ Subscription (tier & billing)       │
│                                             │
├─────────────────────────────────────────────│
│ 👤 [Avatar] John Doe                        │
│    john@example.com | Free Plan             │
│    [Logout]                                 │
└─────────────────────────────────────────────┘
```

#### Section Details

| Section | Purpose | Key Features |
|---------|---------|--------------|
| **Dashboard** | Central hub & quick overview | Stats, activity, next steps |
| **Discovery** | Job search & matching | Search, recommendations, saved jobs |
| **Your Journey** | Application management | Tracking, interviews, offers, analytics |
| **Profile & Resources** | Personal development | Profile, resumes, skills, learning, salary |
| **Help & Preferences** | Support & customization | Notifications, help, settings, referrals, subscription |

---

### USER ROLES & PERSONAS

#### Primary: Job Seekers (4 Personas)

##### **Persona 1: Sarah – Active Job Seeker (Daily User)**
- **Goal:** Find better opportunities matching her seniority (5 years experience)
- **Engagement:** Daily (10+ hours/week on job search)
- **Pain Points:** Overwhelmed by low-quality matches, uncertain about salary expectations
- **Navigation:** Dashboard → Search → Recommendations → Apply → Track
- **Tier:** Free or Premium (wants full resume optimization)
- **Database Focus:** users, profiles, resumes, skills, experience, jobs, recommendations, applications

##### **Persona 2: Marcus – Passive Job Seeker (Part-Time)**
- **Goal:** Discover high-quality opportunities without heavy setup
- **Engagement:** Infrequent (1–2x/month)
- **Pain Points:** Wants only relevant matches, no onboarding friction
- **Navigation:** Dashboard → Recommendations → Applications
- **Tier:** Free (minimal investment)
- **Database Focus:** users, profiles, recommendations, applications, saved_jobs

##### **Persona 3: Alex – Career Changer (Learning-Focused)**
- **Goal:** Transition to data science with skill gaps
- **Engagement:** Motivated (daily learning + job search)
- **Pain Points:** Uncertain about background relevance, needs structured learning
- **Navigation:** Profile → Resumes → Career Insights → Learning Paths → Search
- **Tier:** Professional (wants learning resources + job insights)
- **Database Focus:** users, profiles, resumes, skills, learning_paths, learning_progress, recommendations

##### **Persona 4: Nina – Recent Graduate (High Support)**
- **Goal:** Land first professional role
- **Engagement:** Daily (overwhelmed, learning-heavy)
- **Pain Points:** Resume uncertainty, interview anxiety, decision paralysis
- **Navigation:** Profile → Resumes → Interview Prep → Help → Recommendations
- **Tier:** Premium (interview prep + resume optimization)
- **Database Focus:** users, profiles, resumes, education, interview_tips, interview_questions, applications

#### Secondary: Admin Users
- Manage system health, content quality, user issues
- Separate admin dashboard with role-based permissions
- [See Flow 7: Admin Panel](#flow-7-admin-panel)

#### Tertiary: Employers (Future Phase)
- Post jobs, review applications, manage candidate communication
- Not included in MVP flows; handled by separate admin panel

---

## PART 2: CORE USER FLOWS

---

## FLOW 0: AUTHENTICATION

**Overview:** Complete authentication lifecycle including signup, email verification, password reset, and session management.

**ER Tables:** users, refresh_tokens  
**Database Constraints:**
- Email must be unique (UNIQUE constraint)
- Password requires hash (bcrypt or similar)
- Email verification token must expire within 24h
- Refresh token must expire after 30 days

### 0.1: New User Signup Path

**SRS Reference:** FR-AUTH-001

#### Option 1: OAuth Signup (Google or LinkedIn)
```
User clicks [Sign up with Google]
    ↓
Redirect to OAuth provider consent screen
    ↓
Provider returns: email, firstName, lastName, profilePhotoUrl
    ↓
Backend creates users record:
├─ INSERT users (email, firstName, lastName, profilePhotoUrl, isEmailVerified=true, role='USER', createdAt=NOW)
├─ Generate JWT session token (30-day expiry)
└─ CREATE refresh_tokens (userId, token, expiresAt=NOW+30days)
    ↓
Skip email verification (OAuth provides trust)
    ↓
Auto-redirect to Flow 1 (Onboarding - Step 1: Profile Setup)
```

#### Option 2: Email/Password Signup
```
User fills signup form:
├─ Email [required, validate format]
├─ Password [required, 8+ chars, 1 uppercase, 1 number, 1 special char]
├─ Confirm Password [must match]
└─ I agree to Terms [checkbox required]
    ↓
Client-side validation:
├─ Email format (RFC 5322 basic check)
├─ Password strength indicator (real-time)
└─ Terms checkbox checked
    ↓
On [Create Account]:
├─ Backend validation:
│  ├─ Email uniqueness check
│  ├─ Password hash with bcrypt (10 rounds minimum)
│  └─ Reject if email already exists
├─ INSERT users (email, passwordHash, role='USER', createdAt=NOW, isEmailVerified=false)
├─ Generate email verification token (24-hour expiry)
└─ Send verification email
    ↓
Email Verification Page:
├─ Display: "Verify Your Email"
├─ Message: "We've sent a verification link to john@example.com"
├─ Timer: "Link expires in 24 hours"
├─ Actions: [Open Email App] [Resend Verification] [Change Email]
    ↓
User clicks verification link:
├─ Backend validates token
├─ If valid: UPDATE users SET isEmailVerified=true, emailVerificationCode=NULL
├─ Display: "✓ Email verified!"
└─ Auto-redirect to Flow 1 (Onboarding)
```

### 0.2: Existing User Login Path

```
User navigates to login page
    ↓
Display: Login Form
├─ Email [required]
├─ Password [required]
├─ [Sign in with Google] [Sign in with LinkedIn]
├─ Link: "Forgot password?"
└─ Link: "No account? Sign up"
    ↓
Option 1: Email/Password Login
├─ User enters credentials
├─ Backend:
│  ├─ SELECT * FROM users WHERE email=?
│  ├─ If not found: Show "Email or password incorrect"
│  ├─ If found: Compare password hash (bcrypt.compare)
│  ├─ If mismatch: Increment failed attempts
│  │  └─ Lock account if 5+ failed attempts in 15 min
│  ├─ If match: Generate JWT session token + refresh token
│  │  ├─ INSERT refresh_tokens (userId, token, expiresAt=NOW+30days)
│  │  └─ UPDATE users SET lastLoginAt=NOW
│  └─ Return { sessionToken, refreshToken, userId }
├─ Store tokens (secure httpOnly cookies)
└─ Redirect to Dashboard
    ↓
Option 2: OAuth Login
├─ Similar to signup but check if user exists
├─ If not found: Treat as signup (auto-create user)
└─ If found: Update lastLoginAt, return tokens
    ↓
Account Locked Edge Case:
├─ If 5+ failed attempts in 15 minutes:
│  ├─ UPDATE users SET locked=true
│  ├─ Show: "Account temporarily locked. Check email for unlock link."
│  ├─ Send unlock email with 15-minute token
│  └─ User must click email link to unlock or wait 15 min
```

**API Endpoints:**
```
POST /api/auth/signup
├─ Body: { email, password, agreeToTerms }
└─ Response: { sessionToken, refreshToken, userId }

POST /api/auth/signup/oauth
├─ Body: { provider, oauthCode }
└─ Response: { sessionToken, refreshToken, userId }

POST /api/auth/login
├─ Body: { email, password }
└─ Response: { sessionToken, refreshToken, userId, user { id, email, firstName, role } }

POST /api/auth/verify-email
├─ Body: { code }
└─ Response: { success, message }

POST /api/auth/resend-verification
├─ Body: { email }
└─ Response: { success, message }

GET /api/auth/me
├─ Headers: Authorization: Bearer {sessionToken}
└─ Response: { user { id, email, firstName, lastName, role, createdAt } }

POST /api/auth/refresh
├─ Body: { refreshToken }
└─ Response: { sessionToken, refreshToken }

POST /api/auth/logout
├─ Headers: Authorization: Bearer {sessionToken}
├─ Body: { refreshToken }
└─ Response: { success }
```

### 0.3: Password Reset Flow

```
User clicks "Forgot password?" on login page
    ↓
Display: Password Reset Form
├─ Email [required]
├─ CTA: [Send Reset Link]
└─ Link: "Back to login"
    ↓
User enters email:
├─ Backend:
│  ├─ SELECT * FROM users WHERE email=?
│  ├─ If found: Generate reset token (1-hour expiry)
│  │  └─ UPDATE users SET passwordResetToken=?, passwordResetTokenExpiry=NOW+1h
│  │  └─ Send email with reset link
│  └─ If not found: Still show "Check your email" (security)
├─ Display: "Check your email for reset instructions"
└─ Link: "Didn't receive? Resend" (rate limited)
    ↓
User clicks reset link in email:
├─ Display: New Password Form
│  ├─ New Password [required, 8+ chars]
│  ├─ Confirm Password [must match]
│  └─ [Reset Password]
├─ Backend:
│  ├─ SELECT * FROM users WHERE passwordResetToken=? AND passwordResetTokenExpiry > NOW
│  ├─ If found:
│  │  ├─ Hash new password
│  │  └─ UPDATE users SET passwordHash=?, passwordResetToken=NULL
│  └─ If expired: Show "Link expired, request new one"
├─ Display: "✓ Password reset successful!"
└─ Redirect to login after 2s
```

**API Endpoints:**
```
POST /api/auth/password-reset-request
├─ Body: { email }
└─ Response: { success, message }

POST /api/auth/password-reset
├─ Body: { token, newPassword }
└─ Response: { success, message }
```

---

## FLOW 1: ONBOARDING (EXPANDED)

**Overview:** Complete multi-step onboarding that captures all profile data, creates initial resume, and surfaces first recommendations.

**ER Tables:** users, profiles, resumes, skills, experience, education, certifications, projects, recommendations, user_analytics, user_settings, notification_preferences  
**Duration Target:** 5-10 minutes (depending on user depth)  
**Success Metric:** Profile >80% complete, first recommendation shown

### 1.1: Phase 1 - Profile Setup (Step 1 of 5)

```
Auto-redirect from email verification
    ↓
Display: "Let's set up your profile" (Progress: 1/5)
    ↓
Form Fields:
├─ First Name [prefilled from OAuth or empty]
├─ Last Name [prefilled from OAuth or empty]
├─ Phone Number [optional, E.164 format]
├─ Date of Birth [optional]
├─ Location [required, autocomplete Google Places]
├─ Bio [optional textarea, max 500 chars]
├─ Profile Photo [optional, upload or avatar]
└─ Profile Visibility [radio: Public | Private]
    ↓
Backend on [Next]:
├─ INSERT profiles (userId, location, bio, dateOfBirth, profileVisibility)
├─ If photo uploaded:
│  ├─ Save to media table
│  └─ INSERT media (fileId, path, mimeType, width, height, mediaType='IMAGE', ownerType='USER', ownerId=userId)
└─ UPDATE users SET firstName, lastName, profilePhotoUrl
```

**API:**
```
POST /api/profiles
├─ Body: { firstName, lastName, phone, dateOfBirth, location, bio, profileVisibility, photoUrl }
├─ Files: photo (image)
└─ Response: { profileId, photoUrl }

POST /api/media/upload
├─ Files: file (image, pdf, video)
└─ Response: { mediaId, path, mimeType, url }
```

### 1.2: Phase 2 - Work Preferences (Step 2 of 5)

```
Display: "What are you looking for?" (Progress: 2/5)
    ↓
Form Fields:
├─ Remote Preference [dropdown: Remote Only | Hybrid | On-Site | Flexible]
├─ Expected Salary Min [number input, currency selector]
├─ Expected Salary Max [number input]
├─ Preferred Job Roles [multi-select]
└─ Willing to Relocate? [Yes | No]
    ↓
Backend on [Next]:
└─ UPDATE profiles (remotePreference, expectedSalaryMin, expectedSalaryMax, preferredRoles)
```

**API:**
```
PATCH /api/profiles/{id}
├─ Body: { remotePreference, expectedSalaryMin, expectedSalaryMax, preferredRoles }
└─ Response: { success }
```

### 1.3: Phase 3 - Resume Upload & Parsing (Step 3 of 5)

```
Display: "Upload your resume" (Progress: 3/5)
├─ Drag-and-drop zone or [Browse Files]
├─ Accepted: PDF, DOCX, DOC
├─ Max size: 5MB
└─ Show: "We'll auto-fill your skills and experience"
    ↓
User uploads resume:
├─ Frontend:
│  ├─ Validate file type and size
│  ├─ Show uploading progress bar
│  └─ Submit multipart/form-data
├─ Backend:
│  ├─ INSERT resumes (userId, fileName, fileUrl, parsingStatus='pending', version=1)
│  ├─ Save file to cloud storage (S3, GCS, etc.)
│  ├─ Queue async job: Parse resume
│  │  └─ Extract: skills[], experience[], education[], certifications[]
│  ├─ UPDATE resumes SET parsedData=JSON, parsingStatus='success' (or 'failed')
│  └─ Auto-create skill records:
│     ├─ INSERT skills (userId, name, proficiency='intermediate', yearsOfExperience)
│     └─ Calculate ATS score & resume score
    ↓
Resume Parsing Success:
├─ Display: "Resume parsed successfully!"
├─ Show parsed data for user review:
│  ├─ Skills found (allow add/remove)
│  ├─ Experience entries (allow add/remove)
│  └─ Education entries (allow add/remove)
└─ [Confirm] → Navigate to Phase 4
    ↓
Resume Parsing Failed:
├─ Display error: "We couldn't parse your resume. Please upload again or fill manually."
├─ [Re-upload] or [Manual Entry]
└─ If Manual Entry: Jump to Phase 4 with empty forms
```

**API:**
```
POST /api/resumes/upload
├─ Files: resume (pdf, docx)
└─ Response: { resumeId, fileName, parsingStatus, parsedData { skills, experience, education } }

GET /api/resumes/{id}
├─ Response: { id, fileName, fileUrl, parsedData, atsScore, resumeScore, parsingStatus }

PATCH /api/resumes/{id}/parsed-data
├─ Body: { skills[], experience[], education[], certifications[] }
└─ Response: { success }
```

### 1.4: Phase 4 - Skills, Experience & Education (Step 4 of 5)

```
Display: "Review & complete your background" (Progress: 4/5)
    ↓
Three tabs or accordion sections:
    ↓
TAB 1: SKILLS
├─ List of auto-parsed skills (or empty if manual)
├─ For each skill:
│  ├─ Skill name
│  ├─ Proficiency [dropdown: Beginner | Intermediate | Advanced | Expert]
│  ├─ Years of Experience [number input]
│  └─ [Remove]
├─ [Add Skill] button with autocomplete from jobs table
│  ↓
TAB 2: EXPERIENCE
├─ List of auto-parsed experience or empty
├─ For each entry:
│  ├─ Company name [required]
│  ├─ Position title [required]
│  ├─ Start date [required]
│  ├─ End date [optional - leave blank if current]
│  ├─ Responsibilities [textarea]
│  ├─ Technologies used [multi-tag input]
│  ├─ Key achievements [textarea]
│  └─ [Remove]
├─ [Add Experience] button
└─ Mark one as "Current" if applicable
    ↓
TAB 3: EDUCATION
├─ For each entry:
│  ├─ Institution name [required]
│  ├─ Degree level [dropdown: HS | Bachelor | Master | PhD | Bootcamp | Certificate]
│  ├─ Field of study [required]
│  ├─ Graduation year [required]
│  ├─ GPA [optional]
│  ├─ Honors [optional]
│  └─ [Remove]
└─ [Add Education] button
    ↓
Backend on [Next]:
├─ INSERT/UPDATE skills table
├─ INSERT/UPDATE experiences table
├─ INSERT/UPDATE education table
├─ Trigger resume re-scoring
└─ Update user_analytics counters
```

**API:**
```
POST /api/skills
├─ Body: { name, proficiency, yearsOfExperience }
└─ Response: { skillId }

POST /api/experience
├─ Body: { company, position, startDate, endDate, responsibilities, technologiesUsed[] }
└─ Response: { experienceId }

POST /api/education
├─ Body: { institution, degreeLevel, fieldOfStudy, graduationYear, gpa, honors }
└─ Response: { educationId }

DELETE /api/skills/{id}
DELETE /api/experience/{id}
DELETE /api/education/{id}
```

### 1.5: Phase 5 - Notification & App Preferences (Step 5 of 5)

```
Display: "Stay updated with JobFits" (Progress: 5/5)
    ↓
NOTIFICATION SETTINGS:
├─ Frequency [radio: Real-time | Daily Digest | Weekly]
├─ Notification Types [checkboxes]:
│  ├─ Resume parsed
│  ├─ New recommendations
│  ├─ Interview reminders
│  ├─ Application deadlines
│  ├─ Status updates
│  └─ Weekly digest
├─ Channels [checkboxes: In-app | Email | Push notification]
├─ Quiet Hours [toggle + time range: 9 PM - 8 AM]
└─ Note: "We'll never send spam. Unsubscribe anytime."
    ↓
APP PREFERENCES:
├─ Theme [radio: Light | Dark | System]
├─ Language [dropdown: English | Spanish | French | German | Chinese]
├─ Timezone [autocomplete]
└─ Two-Factor Auth [toggle to enable - optional]
    ↓
Backend on [Finish Setup]:
├─ INSERT notification_preferences (userId, frequency, enabledTypes, channels, quietHours)
├─ INSERT user_settings (userId, theme, language, timezone, twoFactorEnabled)
├─ INSERT user_analytics (userId, totalApplications=0, totalInterviews=0, ...)
├─ Generate initial recommendations (async job)
└─ UPDATE users SET onboardingComplete=true
    ↓
Success Screen:
├─ Display: "✓ Profile Complete!"
├─ Message: "We're finding your perfect matches..."
└─ Auto-redirect after 3s or [View Dashboard]
```

**API:**
```
POST /api/notification-preferences
├─ Body: { frequency, enabledTypes[], channels[], quietHoursStart, quietHoursEnd }
└─ Response: { preferenceId }

POST /api/settings
├─ Body: { theme, language, timezone, twoFactorEnabled }
└─ Response: { success }

POST /api/users/{id}/complete-onboarding
├─ Response: { success, redirectUrl }
└─ Triggers: async recommendation generation job
```

---

## FLOW 2: DISCOVERY PATHS

**Overview:** Job search, AI recommendations, and saved job management.

**ER Tables:** jobs, companies, recommendations, saved_jobs, salary_data

### 2A: AI Recommendations

```
User lands on Discovery → Recommendations tab
    ↓
Display: "Personalized Opportunities"
├─ Loading: "Finding your best matches..." (while algorithm runs)
├─ Each recommendation card shows:
│  ├─ Company logo + name
│  ├─ Job title
│  ├─ Location + remote type
│  ├─ Salary range (if available)
│  ├─ Match score (0-100%) with breakdown:
│  │  ├─ Skills match: XX%
│  │  ├─ Experience match: XX%
│  │  ├─ Location match: XX%
│  │  ├─ Salary match: XX%
│  │  └─ Culture match: XX%
│  ├─ Why this match: "Your Python skills align with this role"
│  ├─ Actions: [View Details] [Save] [Skip] [Not Interested]
│  └─ Swipe gesture on mobile: Left (skip), Right (save)
```

#### Recommendation Algorithm (Backend - Async Job)

```
Trigger: Every 24h OR manual request
    ↓
Process:
├─ FOR each job in jobs table WHERE NOT previously_recommended:
│  ├─ Calculate skills_match: overlap(user_skills, job_requirements) / job_requirements.count
│  ├─ Calculate experience_match: years_user_exp vs job_years_required
│  ├─ Calculate location_match: same_location OR remotePreference matches jobRemoteType
│  ├─ Calculate salary_match: user_salary within job_salary_range
│  ├─ Calculate culture_match: company_glassdoorRating vs user_preference
│  ├─ Weighted average: matchScore = (skills*0.4 + exp*0.2 + location*0.15 + salary*0.15 + culture*0.1)
│  ├─ Generate explanation text
│  ├─ INSERT recommendations (userId, jobId, matchScore, skillsMatch, experienceMatch, etc.)
│  └─ CREATE notification (if matchScore > 70)
└─ ORDER BY matchScore DESC LIMIT 100
```

#### User Actions on Recommendation

```
[View Details]: Navigate to job detail page
[Save]: 
├─ INSERT saved_jobs (userId, jobId, folderName='All', notes='', createdAt=NOW)
├─ Show toast: "✓ Saved"
└─ UPDATE recommendations SET userAction='saved'

[Skip]:
├─ UPDATE recommendations SET userAction='skipped'
└─ Remove from feed

[Not Interested]:
├─ UPDATE recommendations SET userAction='not_interested'
├─ Remove from feed for 30 days
└─ Exclude similar jobs (same company, similar role)
```

#### Display Features

```
User can filter:
├─ Match score range: [slider 0-100]
├─ Job location: [autocomplete]
├─ Remote type: [multi-select]
├─ Salary range: [slider]
└─ Company: [search]
```

**API:**
```
GET /api/recommendations
├─ Query params: ?skip=0&limit=20&minScore=0&location=&remoteType=&salaryMin=&salaryMax=
└─ Response: [{ id, job, matchScore, skillsMatch, experienceMatch, explanation }]

PATCH /api/recommendations/{id}/action
├─ Body: { userAction: 'applied' | 'saved' | 'skipped' | 'not_interested' }
└─ Response: { success }

POST /api/recommendations/generate
├─ Response: { jobsProcessed, recommendationsCreated }
└─ Triggers: Async job to calculate all recommendations
```

### 2B: Job Search

```
User clicks Search Jobs (Discovery → Search)
    ↓
Display: Job Search Interface
├─ Search bar [placeholder: "Job title, company, skills..."]
├─ Filter panel:
│  ├─ Location [autocomplete, multi-select]
│  ├─ Remote type [checkbox: Remote | Hybrid | On-Site]
│  ├─ Salary range [slider: min-max]
│  ├─ Experience level [dropdown: All | Entry | Mid | Senior]
│  ├─ Date posted [radio: Last 24h | Last week | Last month | Any time]
│  ├─ Company [autocomplete, multi-select]
│  └─ [Clear all filters]
    ↓
Results Display:
├─ Grid/List toggle (save preference)
├─ Sort by: [dropdown: Relevance | Match Score | Salary | Recency]
├─ Showing: "45 jobs match your search"
├─ Each job card:
│  ├─ Company logo + name
│  ├─ Job title
│  ├─ Location + remote type
│  ├─ Salary (if available)
│  ├─ Snippet of description
│  ├─ Quick stats: applicants count, posted date
│  └─ [View Details]
    ↓
Backend Search:
├─ Query jobs table with Elasticsearch or SQL LIKE:
│  ├─ WHERE (title LIKE ? OR description LIKE ? OR company.name LIKE ?)
│  ├─ AND location IN (?)
│  ├─ AND remoteType IN (?)
│  ├─ AND salary_min >= ? AND salary_max <= ?
│  ├─ AND postedAt > ?
│  └─ ORDER BY {sortBy} LIMIT 50
├─ Calculate match score for each job
└─ Paginate results (20 per page)
```

#### Job Detail Page

```
Layout:
├─ Header:
│  ├─ Company logo, job title, location, remote type, posted date
│  ├─ Key info card:
│  │  ├─ Salary: "$120k - $150k"
│  │  ├─ Job type: "Full-time"
│  │  ├─ Experience level: "Senior"
│  │  └─ Applicants: "142 have applied"
│  └─ Match score (if recommendation):
│     ├─ Overall: 85%
│     ├─ Breakdown: Skills 90%, Experience 85%, Location 70%, Salary 80%
│     └─ "Your Python and AWS skills match well"
    ↓
├─ Company Info:
│  ├─ Name, logo, glassdoor rating
│  ├─ Industry, size, founded year
│  ├─ "See salary data for this company"
│  └─ [View Company Profile]
    ↓
├─ Job Description:
│  ├─ Full description (markdown or HTML)
│  ├─ Requirements: bulleted list
│  ├─ Responsibilities: bulleted list
│  ├─ Benefits: bulleted list
│  └─ Nice-to-haves
    ↓
└─ Sidebar:
   ├─ [Apply Now] (primary CTA)
   ├─ [Save Job] (secondary)
   ├─ [Share] (social share)
   └─ [Report Job] (spam/scam reporting)
```

**API:**
```
GET /api/jobs/search
├─ Query params: ?q=&location=&remoteType=&salaryMin=&salaryMax=&postedAfter=&companies=&sort=relevance&skip=0&limit=50
└─ Response: { total, jobs: [{ id, title, company, location, remoteType, salary, description, applicantCount, postedAt, matchScore }] }

GET /api/jobs/{id}
├─ Response: { id, title, description, requirements[], responsibilities[], benefits[], salary, location, remoteType, company, applicantCount, postedAt, externalUrl }

GET /api/jobs/{id}/salary-data
├─ Response: { role, location, salaryMin, salaryMax, bonus, equity, totalComp, dataPoints }

GET /api/companies/{id}
├─ Response: { id, name, logo, website, industry, employeeCount, glassdoorRating, reviewCount, founded, fundingStage, description }
```

### 2C: Saved Jobs Management

```
User clicks Saved Jobs (Discovery → Saved)
    ↓
Display: Saved Jobs Library
├─ Sidebar: Folders
│  ├─ All (count: 24)
│  ├─ Interested (count: 8)
│  ├─ For Later (count: 12)
│  ├─ [+ New Folder]
│  └─ Custom folders (user-created)
    ↓
├─ Main area: Job list
│  ├─ Selected folder: "Interested"
│  ├─ Showing: 8 saved jobs
│  ├─ For each job:
│  │  ├─ Company logo + job title
│  │  ├─ Location, salary, remote type
│  │  ├─ Date saved: "Saved 3 days ago"
│  │  ├─ User notes (if any)
│  │  └─ Actions: [View] [Edit Notes] [Move to Folder] [Unsave]
│  ├─ Bulk actions: [Delete All] [Move All] [Export]
│  └─ Sort by: [Date saved | Salary | Title | Relevance]
```

#### Folder Management

```
[+ New Folder]:
├─ Show modal
├─ Folder name [required]
└─ [Create]

Drag-and-drop: Move job card to folder
├─ Backend: UPDATE saved_jobs SET folderName=? WHERE id=?

Right-click job card: [Move to Folder] dropdown
├─ Show list of folders + [New Folder] option

[Delete Folder]:
├─ Confirm: "Delete 'Interested' folder?"
└─ Backend: UPDATE saved_jobs SET folderName='All' WHERE folderName='Interested'

Edit Job Notes:
├─ Click job card → Modal
├─ Notes textarea (max 1000 characters)
└─ [Save Notes]

Unsave Job:
├─ [Unsave] button → Confirm
└─ Backend: DELETE FROM saved_jobs WHERE id=?
```

**API:**
```
GET /api/saved-jobs
├─ Query params: ?folder=&sort=dateAdded
└─ Response: { folders: ['All', 'Interested', ...], jobs: [{ id, job, folderName, notes, savedAt }] }

POST /api/saved-jobs
├─ Body: { jobId, folderName }
└─ Response: { savedJobId }

PATCH /api/saved-jobs/{id}
├─ Body: { folderName, notes }
└─ Response: { success }

DELETE /api/saved-jobs/{id}
└─ Response: { success }

POST /api/saved-jobs/folders
├─ Body: { folderName }
└─ Response: { folderId }
```

---

## FLOW 3: YOUR JOURNEY (EXPANDED)

**Overview:** Application tracking, interview preparation, offer management, and personal analytics.

**ER Tables:** applications, application_timeline, interview_tips, interview_questions, salary_data, user_analytics, contact_persons

### 3A: Application Tracking

```
User clicks Applications (Your Journey → Applications)
    ↓
Display: Application Dashboard
├─ Status filters (pills/buttons):
│  ├─ All (15)
│  ├─ Applied (4)
│  ├─ Interview (3)
│  ├─ Offer (2)
│  ├─ Accepted (1)
│  ├─ Rejected (5)
│  └─ Saved (3)
    ↓
├─ View toggle: [Kanban Board] [List] [Timeline]
├─ Search/filter bar: Search by company or job title
```

#### Kanban View (Drag-and-Drop)

```
┌─────────────┬──────────────┬───────────┬──────────┬───────────┐
│   Applied   │  Interview   │   Offer   │ Accepted │ Rejected  │
│    (4)      │  Scheduled   │ Received  │   (1)    │   (5)     │
│             │    (3)       │   (2)     │          │           │
├─────────────┼──────────────┼───────────┼──────────┼───────────┤
│ [Card 1]    │ [Card 2]     │ [Card 3]  │ [Card 4] │ [Card 5]  │
│ [Card 6]    │ [Card 7]     │ [Card 8]  │          │ [Card 9]  │
│ [Card 10]   │              │           │          │ [Card 11] │
│ [Card 12]   │              │           │          │           │
└─────────────┴──────────────┴───────────┴──────────┴───────────┘
```

#### Each Card Shows:
```
├─ Company logo + job title
├─ Location, role level
├─ Status badge: "Applied · 5 days ago"
├─ Next action: "Interview on July 15"
├─ Salary (if offered): "$130k"
└─ Actions: [View] [Update Status] [Add Note] [Archive]
```

#### List View Alternative

```
Table Columns:
├─ Company
├─ Job Title
├─ Status
├─ Date Applied
├─ Next Action
├─ Salary Offer
└─ Actions [View Details]

Sortable by any column
Filterable by status, date range, salary
```

#### Application Detail Page

```
Header:
├─ Company logo, name, job title
├─ Job description link
└─ Location, remote type
    ↓
Timeline section (vertical):
├─ Event 1: "Applied · July 10, 2024"
├─ Event 2: "First Contact from HR · July 12, 2024"
│  ├─ Contact info: Jane Doe, jane@company.com
│  └─ "Send follow-up email" button
├─ Event 3: "Interview Scheduled · July 15, 2024, 2:00 PM"
│  ├─ "Add to calendar" button
│  └─ "Prepare for interview" button
├─ Event 4: "Interview Completed · July 15, 2024"
│  └─ [Add notes]
├─ Event 5: "Offer Received · July 18, 2024"
│  ├─ "Salary: $130k"
│  ├─ "Signing bonus: $15k"
│  ├─ "Stock options: Yes"
│  └─ [View full offer]
└─ Event 6: "Offer Accepted · July 19, 2024"
    ↓
Notes section:
├─ "Great team, flexible hours. Very interested!"
├─ [Edit] button
└─ Editable textarea
    ↓
Contact person card (if added):
├─ Name, title, email, phone
├─ Last contacted: "3 days ago"
├─ [Email] [Call] [Add to Contacts]
└─ [Edit]
    ↓
Actions sidebar:
├─ [Update Status] dropdown
│  ├─ Applied → Interview Scheduled
│  ├─ Interview Scheduled → Offer Received
│  ├─ Offer Received → Accepted | Rejected
│  └─ Any → Withdrawn
├─ [Archive] / [Unarchive]
├─ [Share application]
└─ [Delete]
```

#### Apply to Job Flow (from job detail)

```
User clicks [Apply Now] on job detail page
    ↓
If not logged in: Redirect to login
    ↓
If logged in: Show application modal
├─ Auto-fill: First Name, Last Name, Email, Phone (from profile)
├─ Resume dropdown: Select which resume to submit
│  ├─ "Primary Resume" (marked as default)
│  ├─ "Data Science Resume"
│  └─ [Upload new resume]
    ↓
├─ Optional fields:
│  ├─ Cover letter [textarea]
│  ├─ Links [URL inputs]: Portfolio, GitHub, LinkedIn
│  └─ Additional info [textarea]
    ↓
├─ [Apply] button
    ↓
Backend:
├─ INSERT applications (userId, jobId, status='applied', appliedAt=NOW)
├─ INSERT application_timeline (applicationId, eventType='applied', eventDate=NOW)
├─ UPDATE jobs SET applicantCount+=1
├─ UPDATE recommendations SET userAction='applied' (if recommendation exists)
├─ CREATE notification: "Application submitted to Company"
└─ Optional: Send application to external job board via API
    ↓
Success message: "✓ Application submitted!"
├─ Options: [View Application] [Continue Browsing] [Share]
```

#### Update Application Status

```
User clicks [Update Status] → Show dropdown
├─ Select new status:
│  ├─ Applied (current)
│  ├─ First Contact
│  ├─ Interview Scheduled
│  ├─ Interview Completed
│  ├─ Offer Received
│  ├─ Offer Accepted
│  ├─ Offer Rejected
│  └─ Rejected
    ↓
Backend:
├─ UPDATE applications SET status=?
├─ INSERT application_timeline (applicationId, eventType=?, eventDate=NOW, notes=?)
└─ CREATE notification if milestone event
```

**API:**
```
POST /api/applications
├─ Body: { jobId, resumeId, coverLetter, notes }
└─ Response: { applicationId, jobId, status, appliedAt }

GET /api/applications
├─ Query params: ?status=&sort=dateApplied&skip=0&limit=50
└─ Response: { total, applications: [{ id, job, company, status, appliedAt, salary, nextAction }] }

GET /api/applications/{id}
├─ Response: { id, job, company, status, appliedAt, resume, notes, timeline: [], contactPerson }

PATCH /api/applications/{id}
├─ Body: { status, notes }
└─ Response: { success }

POST /api/applications/{id}/timeline
├─ Body: { eventType, eventDate, notes }
└─ Response: { timelineId }

DELETE /api/applications/{id}
└─ Response: { success }
```

### 3B: Interview Preparation

```
User clicks Interview Prep (Your Journey → Interview Prep)
    ↓
Display: Interview Prep Hub
├─ Upcoming interviews widget:
│  ├─ "Your next interview: 3 days away"
│  ├─ Company: Acme Corp
│  ├─ Date: July 15, 2024, 2:00 PM
│  ├─ Type: "Phone screen - 30 minutes"
│  └─ [Start prep]
    ↓
Three main tabs:
├─ TAB 1: QUESTION BANK
├─ TAB 2: TIPS & GUIDES
└─ TAB 3: MY INTERVIEW NOTES
```

#### Tab 1: Question Bank

```
Filter by:
├─ Category dropdown:
│  ├─ Behavioral (e.g., "Tell me about a time...")
│  ├─ Technical (code challenges, system design)
│  ├─ Salary (negotiation questions)
│  ├─ Company-specific (culture fit)
│  └─ All
├─ Difficulty: [Beginner | Intermediate | Advanced]
├─ Company [autocomplete]
└─ Role [autocomplete]
    ↓
Display: List of interview_questions
├─ For each question:
│  ├─ Question text
│  ├─ Category badge: "Behavioral"
│  ├─ Difficulty badge: "Intermediate"
│  ├─ [View Answer Guidance]
│  ├─ [Mark as Practiced]
│  └─ [Bookmark]
    ↓
Question detail (on click):
├─ Full question text
├─ Suggested answer guidance
├─ Related questions
├─ [Record my answer] → Launch audio/video recorder
│  ├─ User records their answer
│  ├─ Playback to self-review
│  └─ Optional: Save to profile for feedback
├─ [Mark as Practiced]
└─ [Bookmark for later]
```

#### Tab 2: Tips & Guides

```
Curated content from interview_tips table
    ↓
Filter by:
├─ Category: [All | Before the Interview | During | After | Negotiation]
├─ Difficulty: [Beginner | Intermediate | Advanced]
└─ Duration: [< 5 min | 5-15 min | > 15 min]
    ↓
Each tip shows:
├─ Title (e.g., "How to Answer Behavioral Questions Using STAR Method")
├─ Category badge
├─ Difficulty badge
├─ Reading time estimate
└─ [Read] button
    ↓
Tip detail page:
├─ Full content (markdown or HTML)
├─ Video (if available)
├─ Checklist (if applicable)
├─ [Mark as Helpful]
└─ Related resources
```

#### Tab 3: My Interview Notes

```
For each upcoming/past interview:
├─ Company, job title, date
├─ Interview type: Phone | Video | In-person | Panel
├─ Duration: 30 min
├─ Interviewer(s): Name, title
    ↓
├─ Notes section [textarea]:
│  ├─ Company background
│  ├─ Key topics to cover
│  ├─ Questions for interviewer
│  └─ Post-interview: Observations, feedback
    ↓
├─ [Set Reminder] - 1 hour before
├─ [Add to Calendar] - iCal, Google Cal, Outlook
└─ [Edit] / [Delete]
    ↓
New interview:
├─ [+ Add Interview] button
├─ Form:
│  ├─ Application dropdown (or manual entry)
│  ├─ Date & time
│  ├─ Interview type
│  ├─ Interviewer info
│  └─ Initial notes
    ↓
Backend:
├─ Store in applications table (interviewDate field)
└─ Create calendar event via external API if needed
```

#### Interview Preparation Workflow (Suggested)

```
When interview scheduled:
├─ Show toast: "You have an interview! Start preparing."
├─ Show suggested flow:
│  1. Research company (use salary_data, company info)
│  2. Review relevant interview questions
│  3. Read interview tips
│  4. Record answers to 3-5 practice questions
│  5. Write down notes & questions for interviewer
│  6. Set calendar reminder
│  └─ Track completion: Show progress bar
└─ [View Checklist]
```

**API:**
```
GET /api/interview-questions
├─ Query params: ?category=&difficulty=&company=&role=
└─ Response: [{ id, question, answerGuidance, category, difficulty }]

GET /api/interview-questions/{id}
└─ Response: { id, question, answerGuidance, category, difficulty, relatedQuestions: [] }

PATCH /api/interview-questions/{id}/practice
├─ Body: { practiced: true }
└─ Response: { success }

GET /api/interview-tips
├─ Query params: ?category=&difficulty=
└─ Response: [{ id, title, content, category, difficulty, duration }]

GET /api/interview-tips/{id}
└─ Response: { id, title, content, category, difficulty, relatedTips: [] }

GET /api/applications/{id}/interview-prep
└─ Response: { applicationId, upcomingInterviews: [], suggestedQuestions: [], suggestedTips: [] }
```

### 3C: Offers & Salary Intelligence

```
User clicks Offers & Decisions (Your Journey → Offers)
    ↓
Display: Offers Dashboard
├─ Offer cards (for applications with status='offer_received'):
│  ├─ Company logo, job title
│  ├─ Status: "Offer Received · July 18"
│  ├─ Key offer details:
│  │  ├─ Base salary: $130,000
│  │  ├─ Signing bonus: $15,000
│  │  ├─ Stock/equity: "TBD" or "50,000 shares, 4-year vest"
│  │  ├─ Bonus: "15% annual"
│  │  └─ Total comp: $149,500
│  ├─ Compared to market:
│  │  ├─ "Market avg for this role: $120k-$145k"
│  │  ├─ "This offer is at market ✓"
│  │  └─ "See comparable offers"
│  ├─ Deadline: "Decide by July 25"
│  ├─ Actions: [View Full Offer] [Accept] [Negotiate] [Reject]
│  └─ Notes: User's written notes
```

#### Accept Offer

```
[Accept Offer]:
├─ Confirm modal: "Accept offer from Acme Corp?"
    ↓
Backend:
├─ UPDATE applications SET status='accepted'
├─ INSERT application_timeline (eventType='offer_accepted', eventDate=NOW)
├─ Set other open applications to "Withdrawn" or "Rejected"
│  └─ Notify those companies: "Candidate withdrew application"
├─ CREATE notification: "Congratulations! You've accepted an offer!"
└─ Update user_analytics: totalOffers += 1
    ↓
Display: "✓ Offer accepted! Welcome to Acme Corp 🎉"
└─ Option: [Archive other applications]
```

#### Negotiate Offer

```
[Negotiate]:
├─ Modal: "Salary Negotiation Assistant"
    ↓
Show: Market data for this role
├─ Median salary in location
├─ Median bonus & equity
└─ 25th/75th percentile ranges
    ↓
Form to draft counter-offer:
├─ Desired base salary
├─ Desired bonus
├─ Desired stock options
├─ Proposed changes to benefits, time off, etc.
└─ Cover letter/message to hiring manager
    ↓
├─ [Generate Message] AI helper: Suggest negotiation script
└─ [Send Counter-Offer] → Save as draft + notify contact person
```

#### Reject Offer

```
[Reject]:
├─ Confirm & optional feedback
└─ Backend: UPDATE applications SET status='rejected'
```

#### Salary Comparison Tool

```
User can view salary_data for any job/company/location
    ↓
Show:
├─ Role: "Senior Software Engineer"
├─ Location: "San Francisco, CA"
├─ Data points: "Based on 234 salary submissions"
├─ Salary range: $145k - $220k (median: $175k)
├─ Bonus: 10-25% (median: 15%)
├─ Stock: Common equity ranges
├─ Total comp: $160k - $250k
├─ Related roles in this location (dropdown)
├─ Salary comparison chart (box plot)
└─ "Learn more about negotiation" link
```

**API:**
```
GET /api/applications?status=offer
└─ Response: [{ id, job, company, status, offerSalary, offerDetails, deadline }]

GET /api/salary-data
├─ Query params: ?role=&location=
└─ Response: { role, location, salaryMin, salaryMax, bonus, equity, totalComp, dataPoints, updated }

GET /api/salary-data/comparison
├─ Query params: ?role=&location1=&location2=
└─ Response: { role, locations: [{ location, salaryMin, salaryMax, ... }] }

PATCH /api/applications/{id}
├─ Body: { status: 'accepted' | 'rejected' | 'negotiating', notes: '' }
└─ Response: { success }
```

### 3D: My Stats (User Analytics)

```
User clicks My Stats (Your Journey → My Stats)
    ↓
Display: Personal Analytics Dashboard
    ↓
Key metrics cards:
├─ Total Applications: "15"
├─ Interviews Scheduled: "3"
├─ Offers Received: "2"
├─ Interview Rate: "20%" (interviews / applications)
└─ Offer Rate: "67%" (offers / interviews)
    ↓
Charts:
├─ Applications timeline:
│  ├─ Line chart: Applications per month (last 3 months)
│  └─ Show trend: "2 more applications this month"
├─ Application status breakdown:
│  ├─ Pie chart: Percentages for each status
│  └─ Applied: 40%, Interview: 20%, Offer: 13%, Rejected: 27%
├─ Application source:
│  ├─ Bar chart: From Recommendations vs Search vs Saved vs Extension
│  └─ "Most apps from Recommendations (67%)"
└─ Salary range over time:
   ├─ Line chart: Salary offers trend
   └─ Show average, min, max
    ↓
Top metrics:
├─ Most applied for companies: "Acme Corp (3), TechCo (2), ..."
├─ Most applied for roles: "Senior Engineer (5), Tech Lead (3), ..."
├─ Last application date: "3 days ago"
└─ Last interview date: "5 days ago"
    ↓
Insights:
├─ "Your interview rate is 20% vs 15% platform average"
├─ "Companies in San Francisco pay 15% more on average"
├─ "You're strongest in Python (88% match avg)"
└─ "Improve your skills in Go (+12% match potential)"
    ↓
Export:
├─ [Export as PDF]
└─ [Export as CSV]
```

**API:**
```
GET /api/user-analytics
└─ Response: { totalApplications, totalInterviews, totalOffers, interviewRate, offerRate, lastApplicationDate, lastInterviewDate }

GET /api/user-analytics/timeline
└─ Response: [{ month, applications, interviews, offers }]

GET /api/user-analytics/source
└─ Response: { recommendations: X, search: Y, saved: Z, extension: W }

GET /api/user-analytics/top-companies
└─ Response: [{ company, count }]

GET /api/user-analytics/top-roles
└─ Response: [{ role, count }]

GET /api/user-analytics/export
├─ Query params: ?format=pdf|csv
└─ Response: File download
```

---

## FLOW 4: PROFILE & RESOURCES (EXPANDED)

**Overview:** Comprehensive profile management including basic info, skills, work history, resumes, learning paths, and salary insights.

**ER Tables:** profiles, resumes, skills, experience, education, certifications, projects, learning_paths, learning_progress, user_analytics

### 4A: My Profile

```
User clicks My Profile (Profile & Resources → My Profile)
    ↓
Display: Profile Overview Page
├─ Profile header:
│  ├─ Profile photo [click to change]
│  ├─ Name (editable inline)
│  ├─ Current location (editable)
│  ├─ "Edit Profile" button
│  └─ Profile visibility: "Public" [toggle to Private]
    ↓
├─ About section:
│  ├─ Bio (textarea) - "Passionate about cloud infrastructure..."
│  └─ [Edit]
    ↓
├─ Contact info:
│  ├─ Email: john@example.com [verified ✓]
│  ├─ Phone: +1-555-0100 [verify]
│  ├─ LinkedIn: linkedin.com/in/johndoe
│  ├─ GitHub: github.com/johndoe
│  ├─ Portfolio: johndoe.dev
│  └─ [Edit] for each field
    ↓
├─ Work preferences:
│  ├─ Remote preference: "Hybrid"
│  ├─ Salary expectation: "$120k - $150k"
│  ├─ Preferred roles: ["Senior Engineer", "Tech Lead"]
│  └─ [Edit]
    ↓
└─ Profile completion score:
   ├─ Progress bar: 85% complete
   ├─ Suggestions: "Add a portfolio link", "Upload a profile photo"
   └─ Each suggestion with [Fix] button
    ↓
Edit Profile Modal:
├─ Form with all editable fields
├─ Real-time validation
├─ [Save Changes] button
    ↓
Backend:
├─ UPDATE profiles SET firstName, lastName, phone, dateOfBirth, location, bio, profileVisibility, linkedinUrl, githubUrl, portfolioUrl, remotePreference, expectedSalaryMin, expectedSalaryMax, preferredRoles
└─ Trigger: Update user_analytics.profileCompleteness
    ↓
Toast: "✓ Profile updated"
```

**API:**
```
GET /api/profiles/{userId}
└─ Response: { id, userId, firstName, lastName, phone, dateOfBirth, location, bio, photo, visibility, contactLinks, workPreferences, completeness: 85 }

PATCH /api/profiles/{userId}
├─ Body: { firstName, lastName, phone, dateOfBirth, location, bio, profileVisibility, linkedinUrl, githubUrl, portfolioUrl, remotePreference, expectedSalaryMin, expectedSalaryMax, preferredRoles }
└─ Response: { success, completeness }
```

### 4B: Resume Management

```
User clicks Resumes (Profile & Resources → Resumes)
    ↓
Display: Resume Library
├─ Active resume badge:
│  ├─ "Primary Resume" (marked as default)
│  ├─ Used in all applications unless user selects different
│  ├─ Last updated: "3 weeks ago"
│  └─ [Set as Default] (if not primary)
    ↓
├─ Resume list:
│  ├─ Resume 1: "Primary Resume"
│  │  ├─ Uploaded: "July 1, 2024"
│  │  ├─ ATS Score: 92/100 [+ detailed breakdown]
│  │  ├─ Resume Score: 88/100 [+ detailed breakdown]
│  │  ├─ Parsing status: "✓ Success"
│  │  ├─ Skills extracted: Python, AWS, Kubernetes (show top 3 + count)
│  │  └─ Actions: [Download] [Preview] [Edit] [Duplicate] [Delete] [Set as Default]
│  └─ [Upload New Resume]
```

#### Upload New Resume

```
Drag-and-drop or [Browse]
├─ Auto-fill: Name (optional), set as default (toggle)
    ↓
Backend:
├─ INSERT resumes (userId, fileName, fileUrl, parsingStatus='pending', version=latest+1)
├─ Queue async parsing job
├─ Calculate ATS score & resume score
└─ Extract skills, experience, education
    ↓
Show progress: "Parsing your resume... (30%)"
    ↓
On completion:
├─ Display parsing results with edit buttons
├─ Show ATS and Resume scores with tips
└─ [Confirm] to finalize
```

#### Resume Preview/Download

```
[Preview] → Render PDF in modal
[Download] → Download original file
[Edit] → Allow editing of parsed data:
├─ Show parsed content
├─ Inline edit buttons
├─ Add/remove sections
└─ Re-calculate scores on save
```

#### ATS Score Breakdown

```
Modal showing:
├─ Overall ATS Score: 92/100
├─ Formatting: 95/100 (Good ATS compatibility)
├─ Keywords: 90/100 (Matches 18/20 job keywords)
├─ Parsing: 92/100 (Successfully parsed 95%)
├─ Length: 88/100 (Ideal length 1-2 pages, yours is 1.5)
├─ Contact info: 100/100 (All required fields)
└─ Tips: "Add more specific job descriptions"
```

#### Resume Score Breakdown

```
Modal showing:
├─ Overall Resume Score: 88/100
├─ Content quality: 85/100
├─ Completeness: 90/100
├─ Grammar/Spelling: 92/100
├─ Keywords: 85/100
└─ Recommendations: "Add quantifiable achievements", "Use stronger verbs"
```

#### Resume Optimization Tool

```
[Optimize Resume] button (Premium feature)
    ↓
AI suggestions:
├─ "Weak bullets: 'Responsible for code reviews' → 'Led code review process...'"
├─ "Missing keywords: Add 'Kubernetes', 'Docker'"
├─ "Grammar: Fix 'was built' → 'built'"
└─ [Apply Suggestions] or [Manual Edit]
    ↓
Generate cover letter:
├─ [Generate Cover Letter] → AI generates based on resume + job description
├─ User can edit, customize
└─ Use in applications
```

**API:**
```
GET /api/resumes
└─ Response: { default: { id, name, atsScore, resumeScore, ... }, all: [{ id, name, uploadedAt, atsScore, resumeScore, ... }] }

POST /api/resumes/upload
├─ Body: { name, setAsDefault }
├─ Files: resume (pdf, docx)
└─ Response: { id, parsingStatus, parsedData: { skills, experience, education } }

GET /api/resumes/{id}
└─ Response: { id, fileName, fileUrl, parsedData, atsScore, resumeScore, parsingStatus }

GET /api/resumes/{id}/ats-breakdown
└─ Response: { overall, formatting, keywords, parsing, length, contactInfo, recommendations }

GET /api/resumes/{id}/resume-score-breakdown
└─ Response: { overall, contentQuality, completeness, grammarSpelling, keywords, recommendations }

PATCH /api/resumes/{id}
├─ Body: { name, setAsDefault, parsedData }
└─ Response: { success }

DELETE /api/resumes/{id}
└─ Response: { success }

POST /api/resumes/{id}/optimize
└─ Response: { suggestions: [{ type, original, improved, reason }] }
```

### 4C: Skills & Experience

```
User clicks Skills & Experience (Profile & Resources → Skills & Experience)
    ↓
Display: Skills & Background Hub
├─ Three tabs:
│  ├─ TAB 1: SKILLS
│  ├─ TAB 2: EXPERIENCE
│  └─ TAB 3: EDUCATION & CERTIFICATIONS
```

#### TAB 1: SKILLS

```
Skill list with cards:
├─ Skill name: "Python"
├─ Proficiency level: "Expert"
├─ Years of experience: "8 years"
├─ Endorsements: "23 people"
├─ Match score: "Your Python matches 73 jobs"
└─ Actions: [Edit] [Delete]
    ↓
Sorting: [By Name] [By Years] [By Proficiency]
    ↓
[Add Skill] button:
├─ Skill name [autocomplete from jobs.requirements]
├─ Proficiency [dropdown: Beginner | Intermediate | Advanced | Expert]
├─ Years of experience [number input]
└─ [Add]
    ↓
Backend:
├─ INSERT skills (userId, name, proficiency, yearsOfExperience)
└─ Re-calculate resume score
```

#### TAB 2: EXPERIENCE

```
Timeline view or card view:
├─ [Card View] [Timeline View] toggle
    ↓
Card view:
├─ For each experience:
│  ├─ Company name
│  ├─ Job title
│  ├─ Duration: "Jan 2022 - Present (2.5 years)"
│  ├─ Bullet points: Show 3 top responsibilities
│  ├─ Technologies: "Python, AWS, PostgreSQL"
│  └─ Actions: [Edit] [Delete]
└─ [Add Experience] button
    ↓
Add/Edit Experience form:
├─ Company name [required]
├─ Job title [required]
├─ Start date [month/year picker]
├─ End date [month/year picker or "Present"]
├─ Responsibilities [textarea, bullet format]
├─ Key achievements [textarea]
├─ Technologies used [multi-tag input with autocomplete]
├─ [+ Add another role]
└─ [Save]
    ↓
Backend:
├─ INSERT experiences (userId, company, position, startDate, endDate, responsibilities, technologiesUsed, achievements)
└─ Re-calculate resume & analytics
```

#### TAB 3: EDUCATION & CERTIFICATIONS

```
Education subsection:
├─ For each education entry:
│  ├─ Institution name
│  ├─ Degree: "Master of Science"
│  ├─ Field: "Computer Science"
│  ├─ Graduation: "May 2020"
│  ├─ GPA: "3.8"
│  ├─ Honors: "Cum Laude"
│  └─ Actions: [Edit] [Delete]
├─ [Add Education] button
└─ Form: Institution, Degree level, Field, Year, GPA, Honors
    ↓
Certifications subsection:
├─ For each certification:
│  ├─ Name: "AWS Certified Solutions Architect"
│  ├─ Organization: "Amazon Web Services"
│  ├─ Issued date: "July 2023"
│  ├─ Expiration: "July 2025"
│  ├─ Credential ID: "AWSSAA-123456"
│  ├─ Credential URL: [clickable link]
│  └─ Actions: [Edit] [Delete]
├─ [Add Certification] button
└─ Form: Name, Organization, Issued date, Expiration date, Credential ID, Credential URL
    ↓
Backend:
├─ INSERT education (...) or UPDATE
├─ INSERT certifications (...)
└─ Re-calculate resume score
```

**API:**
```
GET /api/skills
└─ Response: [{ id, name, proficiency, yearsOfExperience, endorsements, jobMatches }]

POST /api/skills
├─ Body: { name, proficiency, yearsOfExperience }
└─ Response: { skillId }

PATCH /api/skills/{id}
├─ Body: { proficiency, yearsOfExperience }
└─ Response: { success }

DELETE /api/skills/{id}

GET /api/experience
└─ Response: [{ id, company, position, startDate, endDate, responsibilities, achievements, technologiesUsed }]

POST /api/experience
├─ Body: { company, position, startDate, endDate, responsibilities, achievements, technologiesUsed[] }
└─ Response: { experienceId }

PATCH /api/experience/{id}
├─ Body: { ... }
└─ Response: { success }

DELETE /api/experience/{id}

GET /api/education
└─ Response: [{ id, institution, degreeLevel, fieldOfStudy, graduationYear, gpa, honors }]

POST /api/education
├─ Body: { institution, degreeLevel, fieldOfStudy, graduationYear, gpa, honors }
└─ Response: { educationId }

GET /api/certifications
└─ Response: [{ id, name, organization, issuedDate, expirationDate, credentialId, credentialUrl }]

POST /api/certifications
├─ Body: { name, organization, issuedDate, expirationDate, credentialId, credentialUrl }
└─ Response: { certificationId }
```

### 4D: Career Insights & Learning Paths

```
User clicks Career Insights (Profile & Resources → Career Insights)
    ↓
Display: Learning & Career Development Hub
├─ Personalized insights:
│  ├─ "Your target role: Senior Software Engineer"
│  ├─ "Current skill gap: Missing Docker (45% of target jobs)"
│  ├─ "Recommended: Add 2-3 years in leadership roles"
│  └─ Actionable: "Learn Docker - 4 weeks"
    ↓
Two main sections:
├─ LEARNING PATHS
└─ YOUR PROGRESS
```

#### Learning Paths

```
Display: Grid or list of learning_paths
    ↓
For each path:
├─ Skill name: "Docker & Containerization"
├─ Difficulty: "Intermediate"
├─ Estimated time: "4 weeks"
├─ Jobs requiring skill: "245 jobs match"
├─ Description: "Learn containerization with Docker..."
├─ Progress (if in progress): "1/5 courses completed"
├─ Resources preview:
│  ├─ Courses (e.g., Udemy, Coursera)
│  ├─ Tutorials (e.g., FreeCodeCamp)
│  ├─ Books
│  └─ Practice projects
└─ [Start Learning] or [Continue] button
    ↓
Filter paths by:
├─ Difficulty: [All | Beginner | Intermediate | Advanced]
├─ Time commitment: [< 2 weeks | 2-4 weeks | > 4 weeks]
├─ Job demand: [Sort by number of jobs]
└─ Your gaps: [Highlight missing skills]
```

#### Start Learning Path

```
Click [Start Learning] → Enroll modal
    ↓
Show:
├─ Full path description
├─ Complete resource list:
│  ├─ Courses with links
│  ├─ Free tutorials
│  ├─ Recommended books
│  └─ Hands-on projects
├─ Suggested schedule: "2 hours/week, 4 weeks total"
└─ [Enroll] button
    ↓
Backend:
├─ INSERT learning_progress (userId, pathId, status='in_progress', coursesCompleted=0, certificatesEarned=0)
└─ Track progress
```

#### Your Progress

```
Completed paths:
├─ "Python Mastery" - Completed 3 weeks ago
├─ Resources: 5/5 courses, 3/3 projects completed
├─ Certificates earned: 2
└─ [View Certificate]
    ↓
In-progress paths:
├─ "Docker & Containerization"
├─ Progress: 1/5 courses (20%)
├─ Estimated completion: "Aug 15"
├─ Resources:
│  ├─ ✓ Udemy: Docker Mastery (completed)
│  ├─ ○ Coursera: Kubernetes Basics (in progress)
│  ├─ ○ Project: Deploy app with Docker
│  └─ ○ FreeCodeCamp: Docker Tutorial
└─ [Continue Learning]
    ↓
Recommended paths (based on gaps):
├─ Sorted by job demand
├─ "Kubernetes" - 312 jobs
├─ "Terraform" - 287 jobs
└─ [Start Learning]
```

**API:**
```
GET /api/learning-paths
├─ Query params: ?difficulty=&timeCommitment=&sortBy=jobDemand
└─ Response: [{ id, skillName, description, estimatedWeeks, jobsRequiring, resources: [], practiceProjects: [] }]

GET /api/learning-paths/{id}
└─ Response: { id, skillName, description, estimatedWeeks, jobsRequiring, resources: [], practiceProjects: [], relatedPaths: [] }

POST /api/learning-progress
├─ Body: { pathId }
└─ Response: { progressId, pathId, status: 'in_progress' }

GET /api/learning-progress
└─ Response: { completed: [], inProgress: [], recommended: [] }

PATCH /api/learning-progress/{id}
├─ Body: { status, coursesCompleted, certificatesEarned }
└─ Response: { success }
```

### 4E: Salary Intelligence

```
User clicks Salary Intelligence (Profile & Resources → Salary Intelligence)
    ↓
Display: Salary Transparency Hub
├─ Your salary expectation card:
│  ├─ Range: "$120k - $150k"
│  ├─ Market comparison: "Your expectation is at market median ✓"
│  ├─ [Edit expectation]
│  └─ "Based on X salary submissions"
    ↓
Salary lookup tool:
├─ Role [autocomplete from jobs.title]
├─ Location [autocomplete from jobs.location]
├─ Experience level [radio: Entry | Mid | Senior]
└─ [Search]
    ↓
Results display:
├─ Role: "Senior Software Engineer"
├─ Location: "San Francisco, CA"
├─ Experience: "Senior"
├─ Salary box plot:
│  ├─ Min: $140k
│  ├─ 25th percentile: $160k
│  ├─ Median: $175k
│  ├─ 75th percentile: $210k
│  ├─ Max: $240k
│  └─ Data points: "Based on 234 submissions"
    ↓
├─ Bonus breakdown:
│  ├─ Median bonus: 15% ($26k)
│  ├─ Range: 10-25%
│  └─ "X% of offers include bonus"
    ↓
├─ Equity breakdown:
│  ├─ "X% of offers include equity"
│  ├─ Median shares: "50,000 options"
│  ├─ Vesting: "4 years, 1 year cliff"
│  └─ Worth estimate: "Varies by company"
    ↓
├─ Total compensation:
│  ├─ Median total comp: $201k
│  ├─ Range: $150k - $280k
│  └─ Breakdown pie chart
    ↓
└─ Insights:
   ├─ "Senior engineers in SF earn 45% more than US avg"
   ├─ "This role typically includes 15-25% bonus"
   └─ "Stock options are standard in this market"
    ↓
Compare locations:
├─ Show same role in multiple locations
├─ San Francisco: $175k median
├─ New York: $165k median
├─ Austin: $135k median
├─ Remote: $155k median
└─ Cost of living adjustment calculator
    ↓
Industry comparison:
├─ Show role salary across industries
├─ Tech: $175k
├─ Finance: $195k
├─ Healthcare: $145k
└─ Insights on which industries pay more
```

**API:**
```
GET /api/salary-data
├─ Query params: ?role=&location=&experienceLevel=
└─ Response: { role, location, experienceLevel, salaryMin, salaryMax, salaryMedian, bonus, bonusRange, equity, totalComp, dataPoints }

GET /api/salary-data/comparison
├─ Query params: ?role=&locations=SF,NY,Austin
└─ Response: { role, locations: [{ location, salaryMin, salaryMax, median, ... }] }

GET /api/salary-data/industry-comparison
├─ Query params: ?role=&industries=tech,finance,healthcare
└─ Response: { role, industries: [{ industry, median, ... }] }
```

---

## FLOW 5: HELP & PREFERENCES (EXPANDED)

**Overview:** Notifications, preferences, help center, settings, and referrals.

**ER Tables:** notifications, notification_preferences, faqs, knowledge_base, help_center, user_settings

### 5A: Notifications

```
User clicks Notifications (Help & Preferences → Notifications)
    ↓
Display: Notification Center
├─ Notification types (tabs):
│  ├─ All (unread count badge)
│  ├─ Resume Parsed
│  ├─ Recommendations
│  ├─ Interview Reminders
│  ├─ Application Updates
│  ├─ Offers
│  └─ Weekly Digest
    ↓
For each notification:
├─ Type icon
├─ Message: "Your resume has been parsed successfully"
├─ Timestamp: "2 hours ago"
├─ Related entity link: [View Resume] or [View Recommendation]
├─ [Read] / [Unread] toggle
├─ [Delete]
└─ Interactive: Click to view details
    ↓
Features:
├─ [Settings] link → Flow 5C
├─ [Clear All] notifications
├─ Timeline view showing notifications over time
```

#### Notification Types

```
├─ resume_parsed: "Your resume 'Primary Resume' has been parsed"
├─ new_recommendation: "New opportunity: Senior Engineer at TechCo (85% match)"
├─ interview_reminder: "Interview with Acme Corp in 1 hour"
├─ application_deadline: "Application deadline in 2 days: TechCo"
├─ status_update: "Your application status changed to Interview Scheduled"
├─ weekly_digest: "Your weekly JobFits summary: 5 new recommendations..."
└─ new_feature: "Try new resume optimization feature"
```

#### Backend

```
Notifications stored in notifications table:
├─ userId (FK)
├─ notificationType (enum)
├─ title, message
├─ metadata (JSON with jobId, applicationId, etc.)
├─ isRead (boolean)
├─ readAt (timestamp)
├─ channel (in_app, email, push)
└─ createdAt (timestamp)
    ↓
Async jobs trigger notifications:
├─ Resume parsing completes → INSERT notification (type: resume_parsed)
├─ Recommendation generated → INSERT notification (type: new_recommendation)
├─ Application status changes → INSERT notification (type: status_update)
├─ Nightly job generates digest → INSERT notification (type: weekly_digest)
└─ 1-hour before interview → INSERT notification (type: interview_reminder)
    ↓
User marks as read:
├─ PATCH notifications/{id} SET isRead=true, readAt=NOW
└─ Decrement unread badge count
```

**API:**
```
GET /api/notifications
├─ Query params: ?type=&unreadOnly=false&skip=0&limit=50
└─ Response: { unreadCount, notifications: [{ id, type, title, message, metadata, isRead, createdAt }] }

PATCH /api/notifications/{id}
├─ Body: { isRead }
└─ Response: { success }

DELETE /api/notifications/{id}
└─ Response: { success }

POST /api/notifications/clear-all
└─ Response: { success }
```

### 5B: Help Center

```
User clicks Help & Feedback (Help & Preferences → Help)
    ↓
Display: Help Center Hub
├─ Search bar: "Search help articles"
    ↓
Popular topics:
├─ "How to optimize your resume"
├─ "Understanding ATS scores"
├─ "Interview preparation tips"
├─ "Salary negotiation guide"
└─ [More topics]
    ↓
FAQ categories:
├─ Getting Started
│  ├─ "How do I create an account?"
│  ├─ "How do I upload my resume?"
│  ├─ "How do I find jobs?"
│  └─ [View all]
├─ Resumes & Applications
│  ├─ "What is an ATS score?"
│  ├─ "Why did my resume fail to parse?"
│  └─ [View all]
├─ Interview Prep
│  ├─ "How do I prepare for interviews?"
│  ├─ "What is the STAR method?"
│  └─ [View all]
├─ Recommendations
│  ├─ "How does JobFits match jobs?"
│  ├─ "Why is this job recommended?"
│  └─ [View all]
├─ Subscriptions & Billing
│  ├─ "What features are in Premium?"
│  ├─ "Can I upgrade/downgrade anytime?"
│  └─ [View all]
└─ Troubleshooting
   ├─ "I can't log in"
   ├─ "My resume won't upload"
   └─ [View all]
    ↓
Contact support:
├─ [Contact Us] button
├─ Email: support@jobfits.com
├─ Chat support (if available)
└─ Submit a ticket
    ↓
Knowledge base articles:
├─ Article: "How to Write Better Resume Bullets"
├─ Published: "June 15, 2024"
├─ Topic: "Resumes"
├─ Read time: "5 min"
└─ [Read Article]
    ↓
Article detail page:
├─ Title, author, published date
├─ Full content (markdown rendered as HTML)
├─ Table of contents (if long)
├─ Related articles
├─ [Was this helpful?] - thumbs up/down
├─ Comments section (optional)
└─ [Share] button
```

**API:**
```
GET /api/faqs
├─ Query params: ?category=&sort=helpful&search=
└─ Response: [{ id, question, answer, category, helpfulCount }]

GET /api/knowledge-base
├─ Query params: ?category=&published=true&search=
└─ Response: [{ id, title, slug, excerpt, publishedAt }]

GET /api/knowledge-base/{slug}
└─ Response: { id, title, slug, content, publishedAt, relatedArticles: [] }

PATCH /api/faqs/{id}/helpful
├─ Body: { helpful: true }
└─ Response: { success, helpfulCount }

POST /api/support/contact
├─ Body: { name, email, subject, message }
└─ Response: { ticketId }
```

### 5C: Settings & Preferences

```
User clicks Settings (Help & Preferences → Settings)
    ↓
Display: Settings Hub with 5 tabs
├─ Account Settings
├─ Notification Preferences
├─ Privacy & Security
├─ Display & Appearance
└─ Billing & Subscription
```

#### TAB 1: Account Settings

```
Basic info:
├─ Email: john@example.com [Change Email] [Verify]
├─ Phone: +1-555-0100 [Change] [Verify]
├─ Password: [Change Password]
└─ Delete account: [Delete My Account]
    ↓
Connected accounts:
├─ Google: [Connected] [Disconnect]
├─ LinkedIn: [Connected] [Disconnect]
└─ Other OAuth providers
    ↓
Backend:
├─ Email change: Send verification to new email
├─ Password change: Validate current password
└─ Account deletion: Soft delete (mark deletedAt)
```

#### TAB 2: Notification Preferences

```
Notification frequency:
├─ ◉ Real-time
├─ ◉ Daily digest (one email per day at 9 AM)
├─ ◉ Weekly digest
└─ ◉ Off
    ↓
Notification types (multi-select):
├─ ☑ Resume parsed
├─ ☑ New recommendations
├─ ☑ Interview reminders
├─ ☑ Application deadlines
├─ ☑ Status updates
├─ ☑ Weekly digest
├─ ☑ New features
└─ ☑ Promotional
    ↓
Notification channels (multi-select):
├─ ☑ In-app (bell icon)
├─ ☑ Email
├─ ☑ Push notification (mobile)
└─ Note: "Check specific types above"
    ↓
Quiet hours:
├─ Enable: ☑ Don't send notifications between:
├─ Start time: [Time picker: 9:00 PM]
├─ End time: [Time picker: 8:00 AM]
└─ Timezone: [Auto-detect or select]
    ↓
[Save Preferences]
    ↓
Backend:
├─ INSERT/UPDATE notification_preferences:
│  ├─ userId, frequency, enabledTypes[], channels[], quietHoursStart, quietHoursEnd
│  └─ updatedAt
├─ Respect settings when sending notifications:
│  ├─ Check if channel enabled + outside quiet hours
│  └─ Batch notifications during quiet hours
```

#### TAB 3: Privacy & Security

```
Email visibility:
├─ ◉ Public (visible on profile when public)
├─ ◉ Private (only to people you've contacted)
└─ ◉ Hidden (never show)
    ↓
Two-factor authentication:
├─ Status: ◉ Off / ◉ Enabled
├─ [Enable 2FA] button:
│  ├─ Show QR code for authenticator app
│  ├─ User scans with authenticator
│  ├─ Require 6-digit code to confirm
│  ├─ Show backup codes (save these)
│  └─ 2FA now enabled
└─ [Disable 2FA] (requires password)
    ↓
Active sessions:
├─ Show list of active sessions/devices:
│  ├─ Browser: Chrome on Windows
│  ├─ IP: 192.168.1.1
│  ├─ Last active: 2 hours ago
│  └─ [Logout] button
└─ [Logout all devices] button
    ↓
Blocked users:
├─ Show list of blocked users
└─ [Block] / [Unblock] buttons
    ↓
Data export:
├─ [Download my data] button
├─ Exports: Profile, resume, applications, activity
└─ Format: JSON or CSV
    ↓
Backend:
├─ user_settings.twoFactorEnabled toggle
├─ Store 2FA secret in encrypted field
├─ Log all login attempts
└─ Soft delete vs hard delete policy
```

#### TAB 4: Display & Appearance

```
Theme:
├─ ◉ Light mode
├─ ◉ Dark mode
└─ ◉ System (follow OS preference)
    ↓
Language:
├─ Dropdown: [English | Spanish | French | German | Chinese Simplified | Chinese Traditional]
└─ Auto-saves on select
    ↓
Timezone:
├─ Autocomplete dropdown
└─ Used for interview reminders, deadlines
    ↓
Resume view preference:
├─ ◉ Grid view
└─ ◉ List view
    ↓
Backend:
├─ user_settings table: theme, language, timezone
└─ Frontend applies these on app load
```

#### TAB 5: Billing & Subscription

```
Current subscription:
├─ Plan: "Free" or "Premium" or "Professional"
├─ Status: "Active"
├─ Billing cycle: "Monthly" or "Annual"
├─ Next billing date: "August 15, 2024"
├─ Amount: "$9.99/month"
└─ [Manage Subscription] or [Upgrade]
    ↓
Upgrade/downgrade:
├─ Show pricing table with tiers
├─ Free: $0/month
├─ Premium: $9.99/month
├─ Professional: $29.99/month
├─ [Upgrade to Premium] / [Downgrade to Free]
└─ Confirm modal with proration info
    ↓
Billing history:
├─ List of past invoices:
│  ├─ Date: "July 15, 2024"
│  ├─ Amount: "$9.99"
│  ├─ Status: "Paid"
│  └─ [Download PDF]
└─ [Download All Invoices]
    ↓
Payment method:
├─ Saved card: "Visa ending in 4242"
├─ Expires: "12/26"
├─ [Update Payment Method]
├─ [Add Another Card]
└─ [Remove Card]
    ↓
Billing email:
├─ Email for invoices: john@example.com
├─ [Change] button
└─ [Send test invoice]
    ↓
Cancellation:
├─ [Cancel Subscription] button
├─ Show survey: "Why are you canceling?"
├─ Confirm: "Subscription ends at end of billing cycle"
└─ [Confirm Cancellation]
    ↓
Backend (subscriptions & payments):
├─ subscriptions: userId, tier, stripeCustomerId, stripeSubscriptionId, billingCycleStart, billingCycleEnd, status
├─ payments: subscriptionId, stripePaymentIntentId, amount, status, processedAt
└─ Integrate with Stripe API
```

**API:**
```
GET /api/user-settings
└─ Response: { userId, theme, language, timezone, twoFactorEnabled }

PATCH /api/user-settings
├─ Body: { theme, language, timezone }
└─ Response: { success }

GET /api/notification-preferences
└─ Response: { userId, frequency, enabledTypes[], channels[], quietHoursStart, quietHoursEnd }

PATCH /api/notification-preferences
├─ Body: { frequency, enabledTypes[], channels[], quietHoursStart, quietHoursEnd }
└─ Response: { success }

POST /api/auth/enable-2fa
└─ Response: { secret, qrCode, backupCodes }

POST /api/auth/confirm-2fa
├─ Body: { code }
└─ Response: { success }

POST /api/auth/disable-2fa
├─ Body: { password }
└─ Response: { success }

GET /api/auth/sessions
└─ Response: [{ id, browser, os, ipAddress, lastActive, isCurrent }]

POST /api/auth/logout-all
└─ Response: { success }

POST /api/data/export
├─ Query params: ?format=json|csv
└─ Response: File download

POST /api/account/delete
├─ Body: { password, reason }
└─ Response: { success }
```

---

## PART 3: MONETIZATION & GROWTH

---

## FLOW 8: SUBSCRIPTIONS & PAYMENT

**Overview:** Monetization flow for premium features.

**ER Tables:** subscriptions, payments

### Phase 1: Pricing Page

```
User discovers subscription (sidebar link or "Upgrade for more features")
    ↓
Display: Pricing & Plans Page
├─ Headline: "Choose Your Plan"
    ↓
Three pricing cards (grid):
    ↓
Plan 1: FREE
├─ Price: $0/month
├─ Billing: Always free
├─ Features:
│  ├─ ✓ Job search & recommendations
│  ├─ ✓ 1 resume upload
│  ├─ ✓ Application tracking
│  ├─ ✗ Resume optimization
│  ├─ ✗ Priority support
│  └─ ✗ Interview prep (limited)
└─ Current plan badge (if on free)
    ↓
Plan 2: PREMIUM (Popular / Recommended badge)
├─ Price: $9.99/month or $79.99/year (save 33%)
├─ Billed: Monthly or Annual toggle
├─ Features:
│  ├─ ✓ Everything in Free
│  ├─ ✓ Unlimited resume uploads
│  ├─ ✓ Resume optimization (AI-powered)
│  ├─ ✓ Interview prep & practice
│  ├─ ✓ Salary negotiation guide
│  └─ ✗ Professional job board
└─ [Upgrade to Premium] CTA (highlighted)
    ↓
Plan 3: PROFESSIONAL
├─ Price: $29.99/month or $239.99/year (save 33%)
├─ Billed: Monthly or Annual toggle
├─ Features:
│  ├─ ✓ Everything in Premium
│  ├─ ✓ Interview coaching (video call)
│  ├─ ✓ Salary negotiation consulting
│  ├─ ✓ Priority support (1-hour response)
│  └─ ✓ Professional job board access
└─ [Upgrade to Professional] CTA
    ↓
Comparison table (below cards):
├─ Toggle: [Annual] [Monthly] billing
├─ Columns: Feature | Free | Premium | Professional
└─ Show pricing update when toggled
    ↓
FAQ section:
├─ "Can I change plans anytime?"
├─ "Will I get a refund if I downgrade?"
├─ "Is payment secure?"
└─ [View all FAQs]
    ↓
Trust signals:
├─ "30-day money-back guarantee"
├─ "Secure payment with Stripe"
└─ "Cancel anytime, no hidden fees"
```

### Phase 2: Checkout Flow

```
User clicks [Upgrade to Premium] or [Upgrade to Professional]
    ↓
Display: Checkout Page
    ↓
Order summary (left side or top):
├─ Plan: "Premium Annual"
├─ Price: "$79.99/year"
├─ Discount: "-$20.01 (save 20%)"
├─ Tax: "$6.40"
└─ Total: "$86.39"
    ↓
Billing information:
├─ Email: john@example.com [auto-fill, editable]
├─ Billing address:
│  ├─ Country: [Dropdown]
│  ├─ Zip/Postal code: [For tax]
│  └─ Address (optional)
└─ [Auto-fill from profile]
    ↓
Payment method:
├─ Card details:
│  ├─ Cardholder name
│  ├─ Card number (Stripe iframe)
│  ├─ Expiry date (Stripe iframe)
│  ├─ CVC (Stripe iframe)
│  └─ Save this card [checkbox]
├─ OR select saved card
└─ Billing name & zip (verification)
    ↓
Billing interval options:
├─ ◉ Monthly: $9.99/month, auto-renew
├─ ◉ Annual: $79.99/year, auto-renew
└─ "Save 33% with annual billing"
    ↓
[Complete Purchase] button
    ↓
Security badges:
├─ "Stripe secure payment"
├─ "30-day money-back guarantee"
└─ "Cancel anytime"
    ↓
Backend on [Complete Purchase]:
├─ Call Stripe API:
│  ├─ Create/get Stripe customer
│  ├─ Create payment intent
│  ├─ Confirm payment (3DS if needed)
│  └─ Create Stripe subscription
├─ On success:
│  ├─ INSERT subscriptions (userId, tier='premium', stripeCustomerId, stripeSubscriptionId, status='active')
│  ├─ UPDATE users SET role='PREMIUM'
│  ├─ INSERT payments (subscriptionId, stripePaymentIntentId, amount, status='succeeded')
│  ├─ CREATE notification: "Welcome to Premium!"
│  └─ Redirect to success page
└─ On failure:
   ├─ Show error: "Payment declined. Try another card."
   └─ Stay on checkout page
```

### Phase 3: Success Page

```
Headline: "🎉 Welcome to Premium!"
    ↓
Confirmation details:
├─ Plan: "Premium - Annual"
├─ Amount charged: "$86.39"
├─ Billing cycle: "August 15, 2024 - August 15, 2025"
├─ Confirmation email: "john@example.com"
└─ Subscription ID: "sub_ABC123..."
    ↓
Next steps:
├─ "Your resume will now be optimized"
├─ "Access interview prep materials"
├─ "View your subscription settings"
└─ [Go to Dashboard] or [View Settings]
```

### Phase 4: Subscription Management (in Settings)

```
Additional flows:
    ↓
Upgrade from Free → Premium/Professional:
├─ Same checkout flow
└─ Immediate feature activation
    ↓
Upgrade from Premium → Professional:
├─ Show prorated amount
├─ Confirm modal: "Upgrade now? Charged $X.XX"
└─ [Confirm]
    ↓
Downgrade from Premium → Free:
├─ Warning: "You'll lose access to:"
│  ├─ Resume optimization
│  ├─ Interview prep materials
│  └─ Priority support
├─ "Takes effect at end of billing cycle: August 15, 2024"
├─ Survey: "Why are you downgrading?"
└─ [Confirm Downgrade]
    ↓
Cancel subscription:
├─ Show churn survey
├─ Offer to downgrade instead
├─ "Takes effect at end of billing cycle"
└─ [Confirm Cancellation]
    ↓
Reactivate cancelled subscription:
├─ If user cancelled, show: "Reactivate your Premium?"
├─ [Reactivate] button
└─ Charge immediately at prorated rate
```

**API:**
```
GET /api/subscriptions/plans
└─ Response: [{ id, name, price, interval, features: [], highlighted }]

POST /api/subscriptions/checkout
├─ Body: { planId, interval: 'monthly'|'annual' }
└─ Response: { clientSecret, totalAmount, tax }

POST /api/subscriptions/create
├─ Body: { planId, interval, stripeToken }
└─ Response: { subscriptionId, status }

GET /api/subscriptions/current
└─ Response: { id, userId, tier, interval, billingCycleStart, billingCycleEnd, status, amount }

PATCH /api/subscriptions/{id}/upgrade
├─ Body: { newPlanId }
└─ Response: { success, proratedAmount }

PATCH /api/subscriptions/{id}/downgrade
├─ Body: { newPlanId }
└─ Response: { success, effectiveDate }

POST /api/subscriptions/{id}/cancel
├─ Body: { reason }
└─ Response: { success, effectiveDate }

POST /api/subscriptions/{id}/reactivate
└─ Response: { success }

GET /api/payments
├─ Query params: ?subscriptionId=
└─ Response: [{ id, date, amount, status, invoice_url }]
```

---

## FLOW 9: REFERRAL PROGRAM

**Overview:** User invites friends, earns rewards.

**ER Tables:** referrals

### Phase 1: Referral Hub

```
User clicks Referrals (Help & Preferences → Referrals)
    ↓
Display: Referral Program Hub
├─ Banner: "Earn $50 for each friend you refer"
    ↓
Your stats:
├─ Referrals sent: 3
├─ Referrals converted: 2
├─ Referrals rewarded: 1
├─ Total earnings: $50
├─ Pending rewards: 1
└─ Next reward: Complete when "Bob" upgrades
    ↓
Your referral link:
├─ Display: "https://jobfits.com/?ref=abc123xyz"
├─ [Copy Link] button
├─ [Copy Referral Code]: "ABC123XYZ"
├─ Share buttons: [LinkedIn] [Twitter] [Email]
└─ Track clicks: "Opened 12 times (3 signups)"
    ↓
Referral history:
├─ For each referral:
│  ├─ Name: "Alice Johnson"
│  ├─ Referral date: "July 10, 2024"
│  ├─ Status: "Converted ✓"
│  ├─ Reward status: "Pending - Waiting for upgrade" | "Completed - $50" | "Not yet"
│  └─ [View profile]
└─ Sort/filter: [All | Pending | Converted | Completed]
    ↓
Rewards section:
├─ "How referrals work:"
├─ 1. Share your link
├─ 2. Friend signs up with your link
├─ 3. Friend verifies email (referral = "converted")
├─ 4. Friend upgrades to Premium (you earn $50!)
└─ "You keep your reward even if friend cancels"
    ↓
FAQ & Backend:
├─ referrals table:
│  ├─ referrerId (FK users)
│  ├─ referredUserId (FK users, nullable)
│  ├─ referralCode (unique, short)
│  ├─ referralLink (full URL with ?ref=code)
│  ├─ status (pending | converted | rewarded)
│  ├─ referredAt, convertedAt (timestamps)
│  ├─ bonusAmount (e.g., 50 for $50)
│  └─ createdAt
├─ When friend signs up with ref code:
│  ├─ INSERT referrals (referrerId, status='pending', referredAt=NOW)
├─ When friend verifies email:
│  └─ UPDATE status='converted'
└─ When friend upgrades:
   └─ UPDATE status='rewarded', INSERT payment/credit
```

### Phase 2: Signup with Referral Code

```
Friend clicks referral link: https://jobfits.com/?ref=abc123xyz
    ↓
Backend detects ref parameter:
├─ Extract referral code from URL
├─ Validate referral code
├─ Check: Referrer exists, not self-referral
└─ Store in session: sessionStorage['referralCode'] = 'abc123xyz'
    ↓
Display: Signup page (with optional referral banner)
├─ Optional banner:
│  ├─ "You were referred by [Referrer Name]"
│  └─ "You'll both earn $50 if you complete signup"
└─ Proceed with normal signup (Flow 0-1A)
    ↓
At end of signup:
├─ Associate referral:
│  └─ UPDATE referrals SET referredUserId={newUserId} WHERE referralCode=?
└─ referrals.status still 'pending' until email verified
    ↓
On email verification:
├─ UPDATE referrals SET status='converted', convertedAt=NOW
├─ CREATE notification for referrer: "Your referral [Name] confirmed!"
└─ Status remains 'converted' until referred user upgrades
```

### Phase 3: Reward Trigger

```
When referred user upgrades to Premium/Professional:
├─ Backend detects upgrade:
│  ├─ SELECT referrals WHERE referredUserId={upgradingUserId}
│  ├─ UPDATE referrals SET status='rewarded', bonusAmount=50
│  ├─ Create reward for referrer:
│  │  └─ Either: $50 credit OR direct bank transfer OR coupon code
│  └─ CREATE notifications:
│     ├─ For referred user: "Your referrer earned $50"
│     └─ For referrer: "Congratulations! You earned $50"
    ↓
Reward display in referral hub:
├─ "$50 reward (earned!)" next to referral
├─ Total earnings updated
└─ If multiple: Show pending + completed
```

**API:**
```
GET /api/referrals/my-program
└─ Response: { referrerId, referralCode, referralLink, totalSent, totalConverted, totalRewarded, totalEarnings, pendingEarnings, referrals: [] }

GET /api/referrals
├─ Query params: ?status=&sort=dateReferred
└─ Response: [{ referralId, referredUser { name }, status, referredAt, convertedAt, bonusAmount }]

POST /api/referrals/share
├─ Body: { channel: 'linkedin'|'twitter'|'email'|'copy' }
└─ Response: { shareUrl, code }

GET /api/referrals/validate
├─ Query params: ?code=
└─ Response: { valid, referrerName }
```

---

## PART 4: PLATFORM INTEGRATION

---

## FLOW 6: CHROME EXTENSION INTEGRATION

```
User installs JobFits Chrome Extension
    ↓
Extension detects when user visits job boards:
├─ LinkedIn Jobs
├─ Indeed
├─ Glassdoor
└─ etc.
    ↓
On job page:
├─ Extension injects UI button: "Analyze with JobFits" (top-right corner)
├─ Extracts job details from page (title, company, description, salary if visible)
└─ Shows match score badge
    ↓
User clicks "Analyze with JobFits":
├─ If not logged in: Show login popup within extension
├─ If logged in:
│  ├─ Extension sends job details to backend API
│  ├─ Backend matches against user profile
│  ├─ Calculate match score
│  └─ Returns: { matchScore, skills_match, explanation, actions }
    ↓
Display in modal/popup:
├─ Match score with breakdown
├─ Your skills match: "Your Python (Expert) matches"
├─ Experience match: "Your 8 years matches"
├─ Salary range comparison
├─ "X people at this company"
└─ [Save to JobFits] [Apply] [Share]
    ↓
[Save to JobFits]:
├─ If job doesn't exist in JobFits DB:
│  ├─ INSERT jobs (externalId, externalSource='linkedin', title, company, location, ...)
│  └─ Generate external URL
├─ INSERT saved_jobs (userId, jobId)
├─ Show: "✓ Saved! View in JobFits app"
└─ Backend stores externalSource
    ↓
[Apply]:
├─ Redirect to JobFits app
├─ Auto-fill job details
└─ Show application form
    ↓
Backend API for Extension:
├─ POST /api/extension/analyze-job
│  ├─ Headers: Authorization: Bearer {extensionToken}
│  ├─ Body: { jobData: { title, company, location, description, salary, externalUrl, source } }
│  └─ Response: { matchScore, breakdown, recommendation }
├─ POST /api/extension/save-job
│  ├─ Body: { jobId or jobData }
│  └─ Response: { success, jobId }
└─ POST /api/extension/login
   ├─ Body: { email, password }
   └─ Response: { token, user }
```

---

## FLOW 7: ADMIN PANEL

```
Admin user logs in with role='ADMIN'
    ↓
Display: Admin Dashboard
```

### Sidebar: Admin Sections

```
├─ Dashboard (overview metrics)
├─ System Health
├─ Content Moderation
├─ User Management
├─ Analytics & Monitoring
└─ Settings
```

### DASHBOARD:

```
Key metrics (cards):
├─ Platform uptime: "99.8% (this month)"
├─ Active users: "2,456 logged in now"
├─ API latency: "145ms avg"
├─ New signups (today): "45"
├─ New applications (today): "1,234"
└─ Failed transactions: "2" (alert if > 0)
    ↓
Alerts section:
├─ 🔴 Critical: "Resume parsing failing (2 errors in last hour)"
├─ 🟡 Warning: "API response time high (200ms avg)"
└─ ✅ Info: "Daily backup completed"
    ↓
Quick actions:
├─ [Run recommendation batch job]
├─ [Clear cache]
├─ [Send test email]
└─ [View system logs]
```

### SYSTEM HEALTH:

```
Jobs ingested:
├─ "2,345 jobs (last 24h)"
├─ Chart: Jobs per day (trend)
└─ [View ingestion logs]
    ↓
Resume parsing:
├─ Success rate: "97.4%"
├─ Processed: "234 resumes (last 24h)"
├─ Failed: "6 resumes"
└─ [View failures]
    ↓
Recommendation generation:
├─ Last run: "Today 11:15 PM"
├─ Candidates processed: "10,234"
├─ Time taken: "42 seconds"
├─ [Run now] / [Schedule]
└─ [View logs]
    ↓
Email delivery:
├─ Sent (24h): "1,234"
├─ Delivered: "1,227 (99.4%)"
├─ Bounced: "5"
├─ Complained: "2"
└─ [View report]
    ↓
API performance:
├─ Average latency: "145ms"
├─ 95th percentile: "320ms"
├─ Errors: "0.2%"
└─ [View detailed metrics]
```

### CONTENT MODERATION:

```
Flagged jobs (spam, duplicate, low quality):
├─ List of flagged jobs
├─ For each:
│  ├─ Job title, company
│  ├─ Flag reason: "Likely duplicate", "Spam", "Missing info"
│  ├─ Confidence score: "92% likely duplicate"
│  └─ Actions: [Approve] [Reject] [Merge with original]
└─ Bulk: [Approve all] [Reject all]
    ↓
Flagged resumes (parsing errors):
├─ Resumes with parsingStatus='failed'
├─ For each:
│  ├─ User info, resume name
│  ├─ Error: "Failed to parse PDF"
│  ├─ Preview: Show parse attempt
│  └─ Actions: [Retry parsing] [Approve] [Request re-upload]
└─ Bulk: [Retry all]
    ↓
Manually flagged content:
├─ Users can report jobs as spam
├─ Show reported jobs + reason
└─ Actions: [Review] [Take down]
```

### USER MANAGEMENT:

```
Search/filter users:
├─ Search by name, email
├─ Filter by: signup date, last login, subscription, status
└─ Sort by: signup date, last active
    ↓
User list (table):
├─ Name, email, signup date, last login, tier, status
└─ For each: [View] [Reset password] [Suspend] [Delete]
    ↓
User detail page:
├─ Profile info, signup date, last login
├─ Activity: Applications, interviews, offers
├─ Subscription: tier, billing status
├─ Account actions:
│  ├─ [Send message] (email user)
│  ├─ [Reset password] (send reset link)
│  ├─ [Suspend account] (with reason)
│  ├─ [Unsuspend]
│  ├─ [Delete account] (soft delete)
│  └─ [Refund payment] (if applicable)
    ↓
Bulk actions:
├─ [Send verification email] (unverified users)
├─ [Unlock accounts] (locked after 5 failed attempts)
└─ [Export users] (CSV)
```

### ANALYTICS & MONITORING:

```
Application metrics:
├─ New applications (chart): Today, week, month
├─ Top companies applied to
├─ Top roles applied to
├─ Application status breakdown (pie)
├─ Source breakdown: Recommendations, search, saved, extension
└─ Conversion funnel: Search → Apply → Interview → Offer
    ↓
Recommendation quality:
├─ Average match score
├─ Click-through rate
├─ Dismissal rate
├─ "User rating of recommendations"
└─ [Fine-tune algorithm]
    ↓
Candidate metrics:
├─ Total candidates: "12,456"
├─ New (today): "45"
├─ Active (last 7 days): "3,456"
├─ Retention rate: "45% return within 30 days"
├─ Top locations, industries, roles
└─ Engagement: DAU, MAU
    ↓
Revenue metrics:
├─ MRR: "Monthly recurring revenue"
├─ Active subscriptions: "456" (Premium) + "123" (Professional)
├─ Churn rate: "5% (monthly)"
├─ Upgrade rate: "12% of free users → paid"
└─ LTV: "Lifetime value"
```

### SETTINGS:

```
Email settings:
├─ SMTP server, from address
├─ Email templates (preview + edit)
├─ [Send test email]
└─ [View email logs]
    ↓
Integration settings:
├─ Stripe API keys (payments)
├─ Google OAuth credentials
├─ LinkedIn OAuth credentials
├─ Email service (SendGrid, AWS SES, etc.)
├─ Job board APIs (Indeed, LinkedIn, Glassdoor)
└─ Resume parser service
    ↓
Feature flags:
├─ Toggle features on/off for testing
├─ Example: "Enable new recommendation algorithm"
├─ Gradual rollout: "Roll out to 10% of users"
└─ [View logs of flag changes]
    ↓
Admin users:
├─ List of admin accounts
├─ [Add admin]
├─ [Remove admin]
├─ Audit log: "Who changed what, when"
└─ Permissions: View, edit, delete by section
```

---

## PART 5: TECHNICAL REFERENCE

---

## ER-TO-FLOW MAPPING MATRIX

| ER Table | Purpose | Used In Flows | Notes |
|----------|---------|---------------|-------|
| **users** | Authentication & identity | Flow 0, all flows | roles: USER, PREMIUM, PROFESSIONAL, ADMIN |
| **refresh_tokens** | Session management | Flow 0 | 30-day expiry, JWT-based |
| **profiles** | Profile info & preferences | Flow 1, 4A | Bio, location, remotePreference, expectedSalary |
| **resumes** | Resume documents & scoring | Flow 1, 4B | Multiple versions, ATS score, parsing status |
| **skills** | User skills with proficiency | Flow 1, 4C, 2A | Linked to jobs via matching algorithm |
| **experience** | Work history | Flow 1, 4C | Company, position, dates, responsibilities |
| **education** | Educational background | Flow 1, 4C | Degree, institution, GPA, honors |
| **certifications** | Professional certifications | Flow 1, 4C | Name, organization, dates |
| **projects** | Personal projects portfolio | Flow 4C | Name, description, technologies, URLs |
| **media** | File uploads | Flow 1, 4B | Profile photos, resumes, cover letters |
| **companies** | Company information | Flow 2B, 3C | Logo, industry, rating, funding stage |
| **jobs** | Job postings | Flow 2A, 2B, 3, 6 | External source, salary, remote type |
| **recommendations** | AI-generated job matches | Flow 1, 2A | Match scores, explanation, user action |
| **saved_jobs** | Job bookmarks with folders | Flow 2C | User-organized folders, notes, deadlines |
| **applications** | Job applications & timeline | Flow 3A | Status tracking, offer salary, interview dates |
| **application_timeline** | Application event history | Flow 3A | Tracks applied, interview, offer, rejected |
| **contact_persons** | Hiring manager/recruiter | Flow 3A | Email, phone, company, last contact |
| **interview_tips** | Interview preparation content | Flow 3B | Categorized tips, difficulty, category |
| **interview_questions** | Practice interview questions | Flow 3B | Answer guidance, difficulty, category |
| **salary_data** | Market salary intelligence | Flow 3C, 4E | Role, location, bonus, equity, data points |
| **learning_paths** | Skill development courses | Flow 4D | Resources, estimated time, job demand |
| **learning_progress** | Track skill development | Flow 4D | Status (not started, in progress, completed) |
| **referrals** | Referral program | Flow 9 | Referrer, referred user, bonus, status |
| **notifications** | User notifications | Flow 5A | Type, message, metadata, read status |
| **notification_preferences** | User notification settings | Flow 1, 5C | Frequency, types, channels, quiet hours |
| **user_analytics** | Personal performance metrics | Flow 3D | Application count, interview rate, offer rate |
| **user_settings** | User preferences | Flow 1, 5C | Theme, language, timezone, 2FA |
| **subscriptions** | Paid tier management | Flow 8 | Tier, billing cycle, Stripe subscription ID |
| **payments** | Payment transactions | Flow 8 | Amount, status, Stripe payment intent ID |

---

## BACKEND API REQUIREMENTS

### Authentication (Flow 0)
```
POST /api/auth/signup
POST /api/auth/signup/oauth
POST /api/auth/login
POST /api/auth/login/oauth
POST /api/auth/logout
POST /api/auth/refresh
POST /api/auth/verify-email
POST /api/auth/resend-verification
POST /api/auth/password-reset-request
POST /api/auth/password-reset
GET /api/auth/me
POST /api/auth/enable-2fa
POST /api/auth/confirm-2fa
```

### Profile & Account (Flows 1, 4A)
```
GET/PATCH /api/profiles/{userId}
GET/PATCH /api/user-settings
GET/PATCH /api/notification-preferences
```

### Resume Management (Flows 1, 4B)
```
GET /api/resumes
POST /api/resumes/upload
GET/PATCH/DELETE /api/resumes/{id}
GET /api/resumes/{id}/ats-breakdown
GET /api/resumes/{id}/resume-score-breakdown
POST /api/resumes/{id}/optimize
POST /api/media/upload
```

### Skills, Experience, Education (Flows 1, 4C)
```
GET/POST /api/skills
PATCH/DELETE /api/skills/{id}
GET/POST /api/experience
PATCH/DELETE /api/experience/{id}
GET/POST /api/education
GET/POST /api/certifications
```

### Job Search & Recommendations (Flow 2)
```
GET /api/jobs/search
GET /api/jobs/{id}
GET /api/recommendations
PATCH /api/recommendations/{id}/action
POST /api/recommendations/generate
GET /api/jobs/{id}/salary-data
GET /api/companies/{id}
```

### Applications (Flow 3A)
```
GET/POST /api/applications
GET /api/applications/{id}
PATCH /api/applications/{id}
POST /api/applications/{id}/timeline
DELETE /api/applications/{id}
```

### Saved Jobs (Flow 2C)
```
GET /api/saved-jobs
POST /api/saved-jobs
PATCH /api/saved-jobs/{id}
DELETE /api/saved-jobs/{id}
POST /api/saved-jobs/folders
```

### Interview Prep & Salary (Flows 3B, 3C, 4E)
```
GET /api/interview-questions
GET /api/interview-tips
GET /api/salary-data
GET /api/salary-data/comparison
GET /api/salary-data/industry-comparison
```

### Learning & Analytics (Flows 3D, 4D)
```
GET /api/learning-paths
POST /api/learning-progress
PATCH /api/learning-progress/{id}
GET /api/user-analytics
GET /api/user-analytics/timeline
GET /api/user-analytics/source
GET /api/user-analytics/top-companies
GET /api/user-analytics/top-roles
GET /api/user-analytics/export
```

### Notifications & Help (Flows 5A, 5B)
```
GET /api/notifications
PATCH /api/notifications/{id}
DELETE /api/notifications/{id}
POST /api/notifications/clear-all
GET /api/faqs
GET /api/knowledge-base
GET /api/knowledge-base/{slug}
PATCH /api/faqs/{id}/helpful
POST /api/support/contact
```

### Subscriptions & Payments (Flow 8)
```
GET /api/subscriptions/plans
POST /api/subscriptions/checkout
POST /api/subscriptions/create
GET/PATCH /api/subscriptions/{id}
GET /api/payments
```

### Referrals (Flow 9)
```
GET /api/referrals/my-program
GET /api/referrals
POST /api/referrals/share
GET /api/referrals/validate
```

### Chrome Extension (Flow 6)
```
POST /api/extension/analyze-job
POST /api/extension/save-job
POST /api/extension/login
```

### Admin (Flow 7)
```
GET /api/admin/dashboard
GET /api/admin/users
PATCH /api/admin/users/{id}
GET /api/admin/jobs (flagged)
GET /api/admin/analytics
```

---

## DATABASE PERFORMANCE NOTES

### Indexes Required:
```
users.email (UNIQUE)
applications.userId, jobId, status (composite)
applications.status (for filtering)
recommendations.userId, matchScore (for sorting & pagination)
saved_jobs.userId, jobId (UNIQUE)
jobs.parsingStatus='pending' (queue)
skills.userId (for profile completion)
learning_progress.userId, status (for tracking)
notifications.userId, isRead (for unread count)
subscriptions.userId, status (for active subscriptions)
referrals.referralCode (UNIQUE)
```

### Read-Heavy, Batch Processing:
```
Jobs table: Ingested nightly, no user writes
Recommendations: Generated by async job, read-heavy
Analytics: Aggregated nightly, read-heavy dashboard
Salary data: Updated monthly, read-heavy comparisons
```

---

## DECISION POINTS & ALTERNATIVE PATHS

```
Resume Parsing Failure:
├─ User can re-upload OR manually enter data (Flow 1)

Application Status Changes:
├─ Trigger notifications & update timeline (Flow 3A)

Recommendation Dismissed:
├─ Flag in recommendations table, exclude similar jobs (Flow 2A)

Payment Declined:
├─ Show error, allow retry with different card (Flow 8)

Interview Scheduled:
├─ Auto-generate reminder notification, suggest prep content (Flow 3B)

Offer Received:
├─ Trigger salary comparison, negotiation assistant (Flow 3C)

Account Locked:
├─ Send unlock email with 15-minute token (Flow 0)
```

---

## METRICS & SUCCESS INDICATORS

```
Signup → First Recommendation: <10 min (Phase A) or <24h (Phase B)
Profile Completion: >80% within 1 week
Resume Upload: >85% of users
Application Rate: >20% of viewed jobs
Return Visit (7-day): >60%
Interview Rate: >15% (interviews / applications)
Offer Rate: >40% (offers / interviews)
NPS: >50
Match Quality: >80% rated good by users
Platform Uptime: >99.5%
```

---

**Document Version:** 3.0  
**Last Updated:** July 2026  
**Status:** MVP Complete - Ready for Development  
**Total Flows:** 9 (+ Admin Panel)  
**Database Entities:** 24/24 ✅  

This organized document provides clear navigation and structure for all user flows, making it easy to reference specific sections during development and implementation.
