# Take-Home Assignment: Mobile Application Developer

**Timeline:** 1–2 weeks  
**Submission:** Share a GitHub repository link with a working demo video

---

## Context

We are building **Scholera**, an AI-native Learning Management System (LMS) used by universities. The platform is currently web-only. We are hiring a mobile developer to build the native mobile companion for Scholera.

The mobile app is not a simplified version of the web — it is a first-class product that serves students in their day-to-day academic life: checking course updates on the go, viewing their progress, browsing course content, and staying on top of assignments.

Your task is to **build a mobile application prototype** that demonstrates your ability to ship a polished, functional, well-integrated mobile experience.

---

## How the Platform Works

Before diving into features, it helps to understand which side of the platform each piece of data comes from — because this shapes how the mobile app should present it.

| Data | Who creates it | Who sees it |
|------|---------------|-------------|
| Announcements | Professor | Students |
| Modules & content | Professor | Students |
| Grades | Professor (sets them) | Students (read-only) |
| Course Roadmap | Professor (builds the structure) | Both — professors edit, students track progress |
| Extracted Topics | System (AI-extracted from professor's uploaded lectures) | Both — students see topics surfaced on roadmap nodes |
| Profile | Student | Student |

The mobile app you are building is **student-facing only**. You will not be implementing any professor-side creation or editing flows. However, understanding where data originates will help you design the right interactions — for example, a student cannot edit a roadmap node, but they can mark their own progress on it.

---

## The Assignment

Build a mobile application for a student-facing LMS. You may use **any mobile framework** — React Native, Flutter, SwiftUI, Jetpack Compose, or Expo. Choose whatever you are most productive in.

The backend is provided as a REST API. Your app should authenticate users, fetch real data, and allow meaningful interactions — not just display hardcoded content.

---

## Backend API

We will provide you with a Supabase project URL and an anon key to use for the assignment. This gives you access to:

- **Auth** — Email/password sign-in via Supabase Auth
- **Courses** — A list of courses the authenticated student is enrolled in
- **Announcements** — Per-course announcements created by professors
- **Modules** — Course content organized in modules (lectures, videos, links, notes), created by professors
- **Grades** — Student's grades set by the professor
- **Roadmap** — Course topic roadmap built by the professor, with per-node progress status per student
- **Topics** — AI-extracted key topics from uploaded lecture documents, linked to roadmap nodes
- **Profile** — Student profile (name, avatar, bio, social links)

You will interact with these via the Supabase JS/Dart/Swift/Kotlin SDK, or directly via the PostgREST REST API. We will share the credentials when you confirm you are starting the assignment.

Supabase documentation: https://supabase.com/docs

---

## Required Features

### 1. Authentication
- Email and password sign-in
- Persist the session across app restarts (don't require re-login every time)
- Sign-out functionality
- Handle expired sessions gracefully

### 2. Home / Dashboard Screen
- Show a personalized greeting with the student's name
- Show a summary of what's new: unread announcements count, any recent grade updates
- This screen should load real data from the API, not be hardcoded

### 3. My Courses Screen
- List all courses the student is enrolled in
- Each course card should show: course name, professor name, section, and a visual enrollment status indicator
- Tapping a course opens the Course Detail screen

### 4. Course Detail Screen
A tabbed or sectioned view that includes:
- **Announcements tab** — List of course announcements posted by the professor. Tapping one opens the full text. *(Professor creates → Student reads)*
- **Modules tab** — Course content organized by module, uploaded by the professor. Each module shows its items (lecture files, videos, links, notes). Indicate item type visually (icon, label). *(Professor creates → Student browses)*
- **Grades tab** — Student's grades for this course, set by the professor. *(Professor sets → Student views, read-only)*

### 5. Course Roadmap Viewer
The course roadmap is built by the professor — they define the structure, the nodes, and the order. Students use the roadmap to track their own learning progress through the course.

- From the Course Detail screen, students can access the course roadmap
- Display the roadmap as a scrollable list or node graph — your choice, justify it
- Each node represents a topic or lecture item. Show the AI-extracted topics for that node (e.g. "Gradient Descent", "Backpropagation") alongside the node. *(Professor uploads lecture → System extracts topics → Student sees them)*
- Show the student's completion status per node (not started, in progress, complete)
- Students can mark their own progress on a node — this is the one interactive write action on a professor-defined structure

### 6. Profile Screen
- Display the student's avatar, name, email, and bio
- Allow editing the display name and bio (save changes back to the database)
- Show links (e.g. GitHub, LinkedIn) if set

### 7. Deep Linking
- The app should handle a direct link to a specific announcement (e.g. `scholera://courses/{courseId}/announcements/{announcementId}`)
- Opening this link from outside the app should navigate the user directly to that announcement (after login, if not already authenticated)

---

## Design Requirements

- The UI should feel **native and polished** — not a web page inside a WebView
- Use platform conventions (iOS or Android) where appropriate
- Consistent visual language throughout: spacing, typography, color usage
- Readable and well-organized — the information hierarchy should be obvious at a glance
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
- **Navigation:** Proper stack + tab navigation with deep linking support (see Feature #7)
- **No backend code required** — You are building a mobile client only
- The app should run on iOS Simulator, Android Emulator, or a physical device. Include setup instructions.

---

## What We Will Evaluate

| Dimension | What We Are Looking For |
|-----------|------------------------|
| **UI Quality** | Does it look like a real app? Is it polished and consistent? Would a student actually enjoy using it? |
| **API Integration** | Is real data loading correctly? Are edge cases handled (empty responses, failed requests, auth expiry)? |
| **Code Organization** | Is the code readable, modular, and maintainable? Is there a clear separation of concerns? |
| **Data Flow** | Is state managed sensibly? Does the app avoid unnecessary re-fetching? Does it cache where appropriate? |
| **Roadmap & Topics** | Are extracted topics displayed meaningfully on roadmap nodes? Is the student's progress interaction smooth and correctly persisted? |
| **Navigation** | Does deep linking work correctly? Is the navigation stack logical and recoverable (no dead ends)? |
| **Performance** | Does it feel fast? Are there obvious jank issues or loading delays that should not exist? |

---

## Stretch Goals (Optional)

These are not required but will strengthen your submission:

- **Push notifications** — When a new announcement is posted, show a local notification (simulated is fine)
- **Dark mode** — Full dark mode support following platform conventions
- **Accessibility** — Screen reader support, proper contrast ratios, touch target sizing
- **Biometric auth** — Face ID / fingerprint login for returning users
- **Animated transitions** — Smooth, purposeful screen transitions (not just fade)
- **Real-time announcements** — Use Supabase Realtime to push new announcements to the app without requiring a pull-to-refresh

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
   - Screenshots or GIFs of each major screen
   - Any known issues or limitations
   - "How I Used AI Assistance" section
4. Record a demo video (5–10 minutes) of the app running on a simulator or physical device, and include it in the repository or as a link in the README. The video should walk through:
   - Signing in
   - Viewing courses, announcements, and modules
   - The roadmap viewer — showing extracted topics on nodes and marking progress
   - Editing profile
   - Any stretch goals you completed

Share the repo link via email.

---

## Notes

- You may use any open-source libraries, frameworks, or third-party packages
- Do not add a backend — the Supabase client is your only data source
- You do not need to implement professor or admin views — students only
- We care more about UX quality and integration correctness than feature count
- If you run into API issues (missing data, unclear schema), document it and work around it — we value problem-solving over a perfectly polished demo
- If you decide to add a feature that is not listed here because you think it improves the student experience, go for it — but complete the required features first
