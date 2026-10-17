# TaskBridge
**Turn Unclear Requests into Clear, Actionable Plans**

A web application concept for organizing goals into manageable plans and tasks, tracking progress, and keeping user data secure.

## 🎯 Problem

### The Context
Freelancers, recent graduates, and early-career professionals often have goals they want to achieve but need a structured way to turn those goals into practical steps.

### The Problem
Users may struggle to break larger goals into manageable tasks, set priorities, track task statuses, and understand how much work remains.

### Why It Matters
Without a clear structure, users may lose track of priorities, overlook tasks, or find it difficult to measure progress. TaskBridge is designed to connect goal setting with organized, trackable execution.

## 💡 Solution

TaskBridge is a responsive web application designed to help individual users organize their goals into plans, manage related tasks, and monitor their progress through a structured interface.

### What does the system do?

- Allows users to create, view, edit, and delete plans.
- Organizes tasks under their related plans.
- Supports task priorities: `High`, `Medium`, and `Low`.
- Tracks task statuses: `To Do`, `In Progress`, and `Done`.
- Calculates plan completion based on completed tasks.
- Stores plans and tasks for later access.
- Uses authentication and database access policies to protect users' private data.

The initial scope focuses on individual productivity rather than team collaboration or automatic task execution.

## 👤 Users

**Primary Users:**
- **Freelancers:** People managing independent projects and professional goals.
- **Recent Graduates:** Graduates organizing career development and job-search activities.
- **Early-Career Professionals:** Professionals managing tasks and personal development goals.
- **Individual Users:** Anyone who needs a structured way to organize plans and tasks.

**User Need:**

Users need a clear and manageable workflow for turning goals into plans, dividing plans into tasks, setting priorities, updating task statuses, and tracking progress in one place.

## 🧠 How I Designed the Solution

I approached TaskBridge as a complete application system rather than just a user interface.

The main design decisions were:

- **Start with a focused MVP:** Prioritize plan and task management before adding advanced features.
- **Define the data relationships:** Structure the system around users, plans, and tasks, with each task linked to its parent plan.
- **Keep progress measurable:** Calculate completion from task statuses instead of relying on manually entered percentages.
- **Plan for data persistence:** Use a database so users can retrieve their plans beyond a single page session.
- **Consider security from the beginning:** Use authentication and Row Level Security (RLS) to restrict access to each user's data.
- **Separate current scope from future ideas:** Keep AI-generated plans and external automation outside the initial version.

The implementation details and actual database configuration should be verified against the deployed application before being presented as completed.

## ⚙️ How It Works

**Input**

The user creates a plan with information such as its title, goal, description, deadline, and next step, then adds related tasks.

↓

**Step 1 — Organize**

The application stores the plan and associates its tasks with the correct plan.

↓

**AI / Logic**

The initial version relies on application logic rather than an AI planning feature. The system manages priorities and task statuses and calculates progress from completed tasks.

↓

**Step 2 — Update and Track**

The user updates task statuses as work progresses. The application recalculates the plan's completion percentage.

↓

**Output**

An organized plan containing its tasks, priorities, current statuses, and calculated progress.

**Progress calculation:**

`Progress = (Completed Tasks ÷ Total Tasks) × 100`

If a plan contains no tasks, its progress is `0%`.

## 🤖 Role of AI

### What does AI do in this solution?

- **AI-Assisted Development:** Lovable is used as an AI App Builder to support the application-building process.
- **Development Assistance:** Natural-language instructions can help translate requirements into application interfaces and functionality.
- **Implementation Support:** AI-assisted development can accelerate prototyping and iteration, while the resulting functionality still requires verification.

**Important distinction:** AI-assisted development does not mean that TaskBridge itself generates plans using AI. AI-powered plan generation is outside the initial application scope.

### What AI Should NOT Do

- The application should not be described as having an in-app AI planning assistant unless that feature is actually implemented and verified.
- AI-generated code or configuration should not be assumed correct without testing.
- Security should not be assumed merely because AI-generated code includes authentication or RLS policies.
- Planned functionality should not be presented as a successfully implemented feature without evidence.

## 🔄 Automation

The initial version focuses on automating routine calculations and handling data operations within the application.

- **Progress Calculation:** Calculate completion from the number of completed tasks.
- **Data Operations:** Support creating, reading, updating, and deleting plans and tasks through the application.
- **Data Persistence:** Store information in the database so it can be retrieved later.
- **Access Control:** Apply database policies to restrict data access according to user ownership, when correctly configured.

The initial scope does not include external automation workflows, automatic task execution, notifications, or AI-generated plans.

## 🗂️ Data

### Input

The system is designed to receive:

**User and Authentication Data**
- Account information managed through Supabase Auth.

**Plan Data**
- Plan title
- Description
- Goal
- Deadline
- Deliverables
- Next step

**Task Data**
- Task title
- Description
- Priority
- Status
- Display order

### Output

The system is designed to produce:

- A list of the user's plans.
- Plan details and their associated tasks.
- Task priorities and current statuses.
- A calculated completion percentage for each plan.
- Persisted data that can be retrieved in subsequent sessions.

The actual stored fields and outputs should be checked against the deployed database and application.

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Lovable | AI-assisted application building |
| Supabase Auth | User authentication and session management |
| Supabase PostgreSQL | Relational storage for plans and tasks |
| Supabase RLS | Database-level access control |
| GitHub | Source code hosting and project documentation |

## 🔑 Key Design Decisions

### Decision 1 — Build an Individual-User MVP

**Decision:**

Focus the first version on personal plan and task management rather than multi-user collaboration.

**Why:**

This keeps the initial scope manageable and allows the core workflow to be developed and evaluated before introducing team permissions and shared ownership.

### Decision 2 — Use a Relational Data Structure

**Decision:**

Organize the data around users, plans, and tasks, with each task associated with a specific plan.

**Why:**

This structure supports clear relationships, consistent task organization, and more reliable data retrieval.

### Decision 3 — Calculate Progress from Task Statuses

**Decision:**

Derive plan progress from completed tasks instead of asking users to update a percentage manually.

**Why:**

It reduces duplicate input and keeps the progress indicator connected to the actual task statuses.

### Decision 4 — Include Data Ownership in the Design

**Decision:**

Use authenticated user identities and RLS policies to restrict access to private plans and tasks.

**Why:**

Personal productivity data should remain accessible only to its authorized owner. The effectiveness of the policies must be verified through security testing.

### Decision 5 — Keep AI Planning Outside the Initial Scope

**Decision:**

Use AI to assist application development without making AI-generated plans a required feature of the first version.

**Why:**

This keeps the core product focused and makes it easier to test the fundamental plan-management workflow before introducing additional AI behavior.

### Decision 6 — Separate Requirements from Verified Implementation

**Decision:**

Document the intended data structure, features, and security behavior while checking them against the actual application.

**Why:**

A clear distinction between design and implementation avoids unsupported claims and makes testing results more trustworthy.

## 🧪 Testing

The following tests define how the core workflow should be evaluated. Actual results must be recorded after running the tests against the deployed application.

### Test 1 — Plan and Task Management

**Input:**

Create a plan, add two tasks, and set their priorities and statuses.

**Expected Result:**

The plan should display its associated tasks, and each task should retain its selected priority and status.

**Actual Result:**

Pending verification against the deployed application.

**Status:** Pending verification

### Test 2 — Progress Calculation

**Input:**

Create a plan with four tasks and mark two as `Done`.

**Expected Result:**

The plan should display `50%` progress. If the plan has no tasks, its progress should be `0%`.

**Actual Result:**

Pending verification against the deployed application.

**Status:** Pending verification

### Additional Tests to Perform

- Verify that plan and task changes persist after refreshing the page.
- Verify that users can access only their own private data.
- Verify that a task cannot be associated with a plan owned by another user.
- Verify that deleting a plan handles its associated tasks as intended.
- Verify authentication, logout, and access restrictions for private pages.

No test should be marked as passed until its actual result has been observed.

## 📸 Evidence

### Final Result

**Live application:** [Open TaskBridge](https://pixel-perfect-vision-5072.lovable.app)

Capture screenshots of the deployed application after verifying the relevant features. Recommended evidence:

- Landing page or dashboard.
- Plan details with tasks and progress.
- Task creation or editing interface.

### Workflow

Document the workflow from plan creation to task management and progress tracking.

Suggested evidence:

- Application workflow diagram.
- Relevant Lovable build steps or implementation screenshots.
- Supabase tables, relationships, and RLS policies, with sensitive information hidden.

Evidence should represent the actual implementation, not only the proposed design.

## ⚠️ Challenges & Failures

### What didn't work as expected?

The main implementation risks to investigate are:

- Whether the database structure matches the intended relationships between users, plans, and tasks.
- Whether the application saves and retrieves data correctly across sessions.
- Whether progress calculations handle empty plans and status changes correctly.
- Whether RLS policies prevent unauthorized access in practice.
- Whether all documented features are implemented in the deployed version.

These are verification points, not claims that a specific failure has already occurred.

### How did I solve it?

The appropriate approach is to test each core feature independently, compare actual behavior with the requirements, inspect the relevant database configuration, and correct any discrepancies before confirming completion.

Any specific issue and its resolution should be added here once it has been observed during implementation or testing.

## 📚 What I Learned

### Technical Lesson

A working application requires more than a well-designed interface. Data relationships, persistence, CRUD operations, authentication, access control, and business logic must work together.

### Problem-Solving Lesson

Breaking a broad goal into smaller, testable requirements makes it easier to prioritize development, identify gaps, and verify individual behaviors.

### AI Lesson

AI App Builders can accelerate application development, but they do not remove the need to understand the generated system, review its implementation, and test its behavior. AI-assisted development and AI-powered application features are also two different things.

## 🔮 Future Improvements

If I continued developing this project, I would:

- **AI Plan Generation:** Generate an initial plan from a user's natural-language goal.
- **AI Output Evaluation:** Evaluate proposed plans for clarity, completeness, and feasibility.
- **Automation Integration:** Connect external services to support selected workflows.
- **Progressive Web App (PWA):** Improve the experience across supported devices.
- **Notifications:** Add optional deadline reminders and task notifications.
- **Collaboration:** Explore shared plans and role-based permissions for teams.

These are future development ideas and are not part of the initial scope.

## 🔗 Project Links

- **Live Demo:** [Open TaskBridge](https://pixel-perfect-vision-5072.lovable.app)
- **Repository:** [View TaskBridge on GitHub](https://github.com/raghadesamaljomasy-code/TaskBridge-website.git)

## 👩‍💻 Author

**Raghad Esam Aljomasy**

- [GitHub](https://github.com/raghadesamaljomasy-code)
- LinkedIn: www.linkedin.com/in/raghad-aljomasy-5b5a06404.

---

**TaskBridge — Turn Unclear Requests into Clear, Actionable Plans.**

*Build the System, Not the Screen.*
