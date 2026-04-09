# Take-Home Assignment: Mobile Application Developer

**Timeline:** 1–2 weeks  
**Submission:** Share a GitHub repository link with a working demo video

---

## Context

We are building **Scholera**, an AI-native Learning Management System (LMS) used by universities. The platform is currently web-only. We are hiring a mobile developer to build the native mobile companion for Scholera.

The mobile app serves three distinct roles — admin, professor, and student — each with their own experience. Your task is to **build a mobile application prototype** that handles all three, demonstrating your ability to ship a polished, functional, role-aware mobile experience.

---

## How the Platform Works

Scholera has three user roles, each with a different relationship to the data:

| Role | Responsibilities |
|------|-----------------|
| **Admin** | Manages departments, professors, students, courses, and programs across the institution |
| **Professor** | Creates and manages their course sections — announcements, modules, grades, roadmaps |
| **Student** | Browses enrolled courses, views content posted by professors, tracks their own progress |

Some data is shared across roles but with different permissions:

| Data | Who creates it | Who sees it |
|------|---------------|-------------|
| Announcements | Professor | Students (read-only) |
| Modules & content | Professor | Students (read-only) |
| Grades | Professor (sets them) | Students (read-only) |
| Course Roadmap | Professor (builds structure) | Professor (edits), Student (tracks progress) |
| Extracted Topics | System — AI-extracted from professor's uploaded lectures | Both — surfaced on roadmap nodes |
| Departments / Programs | Admin | Admin |
| Profile | Each user (own profile) | That user |

---

## The Assignment

Build a mobile application for Scholera. You may use **any mobile framework** — React Native, Flutter, SwiftUI, Jetpack Compose, or Expo. Choose whatever you are most productive in.

The backend is provided as a REST API. Your app should authenticate users, detect their role, and render the appropriate experience — not just display hardcoded content.

---

## Backend API

We will provide you with a Supabase project URL and an anon key to use for the assignment. This gives you access to:

- **Auth** — Email/password sign-in via Supabase Auth; user role is stored in the profile
- **Profiles** — User profile with role field (`admin`, `professor`, `student`)
- **Courses / Sections** — Course sections with enrollment and assignment data
- **Announcements** — Per-course announcements created by professors
- **Modules** — Course content organized in modules (lectures, videos, links, notes)
- **Grades** — Per-student grades set by professors
- **Roadmap** — Course topic roadmap with per-node progress status per student
- **Topics** — AI-extracted key topics from uploaded lecture documents, linked to roadmap nodes
- **Departments / Programs** — Institution-level data managed by admins

You will interact with these via the Supabase JS/Dart/Swift/Kotlin SDK, or directly via the PostgREST REST API. We will share the credentials when you confirm you are starting the assignment.

Supabase documentation: https://supabase.com/docs

---

## Required Features

### 1. Authentication & Role-Based Routing
- Email and password sign-in
- After login, read the user's role from their profile (`admin`, `professor`, or `student`)
- Route each role to a completely separate home experience — the app should look and feel different depending on who is logged in
- Persist the session across app restarts
- Handle expired sessions gracefully
- Sign-out from any role

We will provide test credentials for all three roles so you can demo each one.

---

### Admin Experience

#### 2. Admin Dashboard
- Overview of institution stats: total students, professors, courses, departments
- All data fetched from the API — no hardcoding

#### 3. Department & Professor Management
- List all departments with their assigned professors
- Tapping a department shows its details and the professors within it
- Tapping a professor shows their profile and assigned courses

---

### Professor Experience

#### 4. My Courses
- List all course sections the professor teaches
- Tapping a course opens the Course Management screen

#### 5. Course Management Screen
A tabbed view for managing a single course section:
- **Announcements tab** — View existing announcements and create new ones (title + body)
- **Modules tab** — Browse module items uploaded to the course
- **Grades tab** — View the grade list for enrolled students; ability to set or update a grade for a student

#### 6. Course Roadmap Editor *(Professor side)*
The professor defines the roadmap structure — the nodes, the order, and the content linked to each node.

- View the course roadmap as a list or graph
- Each node shows its title and the AI-extracted topics associated with it *(System extracts topics from uploaded lectures → Professor sees them on the node)*
- Professor can update the completion status of a node (e.g. mark it as published or active)

---

### Student Experience

#### 7. My Courses
- List all courses the student is enrolled in
- Tapping a course opens the Course Detail screen

#### 8. Course Detail Screen
A tabbed view that includes:
- **Announcements tab** — Announcements posted by the professor. Tapping one opens the full text. *(Read-only)*
- **Modules tab** — Course content by module. Each item shows type visually (icon, label). *(Read-only)*
- **Grades tab** — The student's own grades for this course. *(Read-only)*

#### 9. Course Roadmap Viewer *(Student side)*
The roadmap structure is defined by the professor. Students use it to track their own learning progress.

- View the course roadmap as a scrollable list or node graph — your choice, justify it
- Each node shows its title and the AI-extracted topics (e.g. "Gradient Descent", "Backpropagation") *(Professor uploads lecture → System extracts topics → Student sees them)*
- Show the student's own completion status per node (not started, in progress, complete)
- Student can mark their own progress on a node

> **Note:** The topic data is already pre-computed and stored in the database — you are not expected to run any AI or extraction logic. Your job is to fetch it and display it clearly. We are evaluating API integration and UI quality, not AI knowledge.

---

### Shared

#### 10. Profile Screen
- Any role can view and edit their own profile (display name, bio, avatar)
- Save changes back to the database

#### 11. Deep Linking
- The app should handle a direct link to a specific announcement (e.g. `scholera://courses/{courseId}/announcements/{announcementId}`)
- Opening this link should navigate the user directly to that announcement after login

---

## Design Requirements

- The UI should feel **native and polished** — not a web page inside a WebView
- Each role's experience should feel distinct and purposeful — not just the same screens with different data
- Use platform conventions (iOS or Android) where appropriate
- Consistent visual language throughout: spacing, typography, color usage
- Empty states should be handled (e.g. "No announcements yet" instead of a blank screen)
- Loading states should be smooth (skeletons, spinners, or shimmer — not abrupt flashes)
- Errors should surface to the user in a friendly way (not crash, not silently fail)

You do not need to match our web design exactly. Use your design judgment.

---

## Technical Requirements

- **Framework:** Any — React Native, Expo, Flutter, SwiftUI, Jetpack Compose
- **Language:** TypeScript, Dart, Swift, or Kotlin
- **Database integration:** Must use the provided Supabase backend (no fake/mock data in the final submission)
- **State management:** Your choice — use what you're comfortable with
- **Navigation:** Role-based routing from login, plus proper stack + tab navigation with deep linking support
- **No backend code required** — You are building a mobile client only
- The app should run on iOS Simulator, Android Emulator, or a physical device. Include setup instructions.

---

## What We Will Evaluate

| Dimension | What We Are Looking For |
|-----------|------------------------|
| **Role-Based Routing** | Does login correctly detect the role and route to the right experience? Is the separation clean? |
| **UI Quality** | Does each role's experience feel polished and appropriate? Would the actual users enjoy using it? |
| **API Integration** | Is real data loading correctly across all roles? Are edge cases handled (empty states, errors, auth expiry)? |
| **Code Organization** | Is the code readable and maintainable? Is role-based logic cleanly separated from UI logic? |
| **Roadmap & Topics** | Are extracted topics displayed meaningfully on nodes? Is student progress correctly persisted? |
| **Navigation** | Does deep linking work? Is the stack logical and recoverable across all roles? |
| **Performance** | Does it feel fast? Are there obvious jank or loading issues? |

---

## Stretch Goals (Optional)

These are not required but will strengthen your submission:

- **Push notifications** — When a new announcement is posted, show a local notification (simulated is fine)
- **Dark mode** — Full dark mode support following platform conventions
- **Accessibility** — Screen reader support, proper contrast ratios, touch target sizing
- **Biometric auth** — Face ID / fingerprint login for returning users
- **Animated transitions** — Smooth, purposeful screen transitions (not just fade)
- **Real-time announcements** — Use Supabase Realtime to push new announcements without requiring a pull-to-refresh

---

## AI Coding Assistant Usage

We actively encourage using AI coding tools — Claude Code, Cursor, GitHub Copilot, or any other assistant. We use them ourselves.

Include a short section in your README titled **"How I Used AI Assistance"** covering:

- Which tools you used (e.g. Claude Code, Cursor, Copilot)
- Where AI help was most valuable (e.g. setting up Supabase auth, designing a component, debugging a layout issue)
- Where you had to override or correct the AI's suggestions
- One specific example of a prompt you gave and how it influenced your implementation

We are interested in your judgment and self-awareness about when to trust AI output and when not to — not in whether or how much you used it.

---

## Submission

1. Create a new public GitHub repository from scratch
2. Push your completed work with a clear commit history (we will look at your commits)
3. Include a `README.md` with:
   - Setup and run instructions (should take under 5 minutes)
   - Framework and key libraries used, with a brief "why" for each major choice
   - Screenshots or GIFs of key screens across all three roles
   - Any known issues or limitations
   - "How I Used AI Assistance" section
4. Record a demo video (5–10 minutes) of the app running on a simulator or physical device, and include it in the repository or as a link in the README. The video should walk through:
   - Signing in as each of the three roles (admin, professor, student)
   - The admin department/professor view
   - The professor course management and roadmap editor
   - The student course view, roadmap with extracted topics, and marking progress
   - Any stretch goals you completed

Share the repo link via email.

---

## Notes

- You may use any open-source libraries, frameworks, or third-party packages
- Do not add a backend — the Supabase client is your only data source
- We will provide test credentials for all three roles
- We care more about UX quality and integration correctness than feature count
- If you run into API issues (missing data, unclear schema), document it and work around it — we value problem-solving over a perfectly polished demo
- If you decide to add a feature not listed here because it improves the experience, go for it — but complete the required features first
