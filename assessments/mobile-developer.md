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
| Course Roadmap | Professor (builds structure, sets coverage status) | Professor (manages), Student (tracks own progress) |
| Extracted Topics | System — AI-extracted from professor's uploaded lectures | Both — surfaced on roadmap items |
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
- **Roadmap** — Course topic roadmap with per-node status (professor coverage + student progress)
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
- **Modules tab** — See below

#### 6. Module Management *(The foundation of the professor experience)*

Think of modules as folders, and module items as the files inside them. A professor organizes their course into modules (e.g. "Week 1 — Introduction", "Week 2 — Neural Networks"), and within each module they upload content — lecture slides, videos, links, notes, etc.

This is the most important feature on the professor side, because everything else in the course flows from it:
- The **roadmap** is built automatically from whatever the professor puts in their modules
- The **AI-extracted topics** come from the lecture files the professor uploads here
- Students browse this content to learn

Build a screen where the professor can:
- See all their modules for a course, in order
- See the items inside each module (with the item type shown clearly — lecture, video, link, note, etc.)
- Create a new module (just a title is enough)
- Add a new item to a module — at minimum, support adding a link (URL + title) and a note (plain text). File upload is a bonus.

Keep it simple and functional. We are not looking for a full content editor — we want to see how you think about hierarchy and CRUD on a nested data structure.

#### 7. Course Roadmap *(Professor side)*

Once a professor has uploaded content into their modules, the roadmap is generated automatically from that content. The professor does not draw it manually — the structure comes straight from their modules and items.

What the professor sees on the roadmap:
- Each module appears as a group, and each item within it appears underneath
- Next to each item, the system has automatically extracted the key topics from that lecture (e.g. a slide deck on neural networks might show topics like "Gradient Descent", "Backpropagation", "Activation Functions")
- The professor can mark each item as not started, in progress, or complete — this is how they signal to students what has been covered in class so far

Build a screen that shows this view clearly. The layout is your choice — a list, a card stack, a simple tree — whatever feels most natural on mobile. The goal is that a professor can glance at their roadmap and immediately see what they've covered and what topics each lecture contains.

---

### Student Experience

#### 7. My Courses
- List all courses the student is enrolled in
- Tapping a course opens the Course Detail screen

#### 8. Course Detail Screen
A tabbed view that includes:
- **Announcements tab** — Announcements posted by the professor. Tapping one opens the full text. *(Read-only)*
- **Modules tab** — Course content by module. Each item shows type visually (icon, label). *(Read-only)*

#### 9. Course Roadmap *(Student side)*

The student's roadmap is the same structure the professor built — but the student's job is to use it to track their own learning progress through the course.

What the student sees:
- The same modules and items the professor organized, in the same order
- Next to each item, the topics that were automatically extracted from that lecture (e.g. "Gradient Descent", "Backpropagation") — so the student knows at a glance what that lecture is about before opening it
- Whether the professor has marked that item as covered in class yet (not started / in progress / complete)
- The student's own personal progress on each item — they can mark things as done for themselves, independently of what the professor has set

The key distinction to get right: the professor's status and the student's own progress are two separate things. A professor marking a lecture as "complete" means they've taught it. A student marking it as "complete" means they've studied it.

Build this screen in whatever layout works best on mobile. It should be easy to scan, easy to update, and clearly show both the topic content and the student's own progress state.

> **Note:** The topic data is already extracted and stored in the database — no AI work needed from you. Just fetch it and show it clearly. We are looking at how well you display structured information and handle interactive state, not at AI knowledge.

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
| **Module Hierarchy** | Is the module → item hierarchy presented clearly? Does the professor's CRUD work correctly? |
| **Roadmap & Topics** | Are extracted topics shown clearly on each item? Is the professor/student progress distinction correct? |
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
   - The professor module management (creating a module, adding an item)
   - The professor roadmap — showing coverage status and extracted topics per item
   - The student course view and roadmap — showing topics and marking their own progress
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
