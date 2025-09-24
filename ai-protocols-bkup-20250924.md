Last Modified: 2025-09-20

# 1. General AI Protocols

This file contains general operational protocols for the AI that apply across all roles.

## 1.1. AI Session Start-up
*   **Objective:** To ensure full context awareness at the beginning of every session.
*   **Action:** Execute the strict start-up protocol.
*   **Implementation / Instructions:**
    1.  **Acknowledge Protocol:** State that you will follow the AI Session Start-up Protocol.
    2.  **Read Workspace README:** Read the `README.md` file at the project root to learn the high-level environment structure.
    3.  **Read Global Documentation:** Read every file listed in the `# Documentation` section of the `global/README.md`. **Note:** Some documentation may be ignored by Git; you must disable gitignore handling in your tools if you cannot find a file.
    4.  **Load Project-Specific Context:** After the user specifies the project for the session (e.g., `moodle4.5`), you must check for a `README.md` file in that project's root directory. If it exists, read it immediately to load project-specific context and documentation links.
    5.  **Review Past Sessions:** Identify the current user, then list and read all of their session logs.
    6.  **Create Session Log:** Create a new session log file, asking the user for their name, the instance (LCL/VPS), and the session goal. Use the naming convention: `YYYYMMDD-S#-UserName-instance.md`.
    *   **Time Tracking:** The new log must include the time tracking block as defined in protocol `2.3`.
    7.  **Confirm Readiness:** Announce that you have completed the start-up protocol and are ready to proceed.
    8.  **Announce Reminders:** After confirming readiness, check the `## Next Steps & Reminders` section of the previous log. If it contains any `[Pending]` items, you must inform the user. (e.g., "I am ready to proceed. I also have pending reminders from our last session. Shall I list them?").

## 1.2. Session & Data Management
*   **Objective:** To correctly handle session logs and embedded data.
*   **Action:** Adhere to the established project data workflow.
*   **Implementation / Instructions:**
    1.  Store session logs in their respective user-specific folders.
    2.  Embed new issues and suggestions in the current session log under `### New Founded Issues` and `### Suggested New Protocols & Instructions`.
    3.  Do not modify the master `issues.md` file directly.

## 1.3. Global Issue Awareness
*   **Objective:** To prevent repeating past errors.
*   **Action:** Reference the master `issues.md` file before proposing solutions.
*   **Implementation / Instructions:**
    1.  Before implementing a solution, verify that the issue has not been previously documented in `issues.md` with a known faulty outcome.

## 1.4. Git Workflow
*   **Objective:** **CRITICAL: To prevent data loss and repository corruption by ensuring all Git operations are performed on the correct repository.**
*   **Action:** Use the designated wrapper scripts (e.g., `git-45.bat`, `git-global.bat`) for all Git commands. This is the most important protocol; failure to follow it can lead to irreversible errors.
*   **Implementation / Instructions:**
    1.  Always prefer a wrapper script if one exists for the target repository.
    2.  If a suitable wrapper script does not exist, you must first change the directory to the correct project sub-folder (e.g., `moodle4.5/`) before executing the `git` command.

## 1.5. Token & Context Management
*   **Objective:** To operate efficiently and prevent incomplete responses.
*   **Action:** Actively manage token usage and context scope.
*   **Implementation / Instructions:**
    1.  **Incremental Generation:** Deconstruct large tasks into a sequential plan and await user confirmation at each step.
    2.  **Context Scoping:** Utilize the project's `.aiexclude` file to ignore irrelevant directories and files.
    3.  **Efficient Log Analysis:** When debugging, read the last 30 lines of a log file first before reading the entire file.
    4.  **Manage Verbose Commands:** For commands that can produce large amounts of output (e.g., `ls -R`, `find`, `cat`), use flags to reduce verbosity (`-q`, `--brief`) or pipe the output to a pager like `head` to read only the first few lines.
    5.  **Formatted Message Handling:** For long or complex formatted messages, write the content to `ai-message.md` at the project root and notify the user to read that file. This prevents message truncation or formatting errors in the chat.
    6.  **User-Directed Message Formatting:** If the user's prompt ends with the `[aim]` command, you must treat it as a directive to write your response to the `ai-message.md` file.

## 1.6. Role-Based Protocols
*   **Objective:** To streamline the workflow by operating under conceptual roles.
*   **Action:** Internally manage tasks according to the VPM, DevOp, and SME roles.
*   **Implementation / Instructions:**
    1.  **VPM (Virtual Project Manager):** High-level planning, task breakdown, and documentation.
    2.  **DevOp (Developer & Operations):** Technical execution of tasks, coding, and infrastructure management.
    3.  **SME (Subject Matter Expert):** Research, analysis, and providing well-informed advice with pros and cons.

# 2. Project Management Protocols

## 2.1. Tasks Management
*   **Objective:** To standardize and streamline the management of all project tasks.
*   **Action:** Execute the task management lifecycle.
*   **Implementation / Instructions:**
    1.  **Session Log Structure:** The session log must contain:
        *   `## Summary of Actions`: Log of completed tasks.
        *   `## Project Plan | Scope of Work`: Tracks core, planned tasks with status markers.
        *   `## Current Activity`: Tracks ad-hoc, unplanned tasks or brainstorming.
        *   `## Next Steps & Reminders`: Contains reminders and instructions for the next session.
        *   `## New Founded Issues`: Documents new issues found during the session.
        *   `## Suggested New Protocols & Instructions`: Documents suggestions for workflow improvements.
    2.  **Project Plan & Reminders Persistence:** When creating a new session log, you must copy the entire `## Project Plan | Scope of Work` and `## Next Steps & Reminders` sections from the most recent previous session log, unless the user specifies otherwise.
    3.  **Reminder Handling:** When a user asks for a reminder, classify it. If it is a core project task, add it to the `Project Plan`. If it is an ad-hoc reminder, add it to `## Next Steps & Reminders` with a `[Pending]` status.
    4.  **Task Initiation:** A task must exist in the `## Project Plan | Scope of Work` with a `[Next]` status before execution.
    5.  **Execution Start:** To begin a `[Next]` task, you must follow this exact sequence:
    a.  **Get Approval:** Obtain user approval to start the task.
    b.  **Update Log (State):** Announce your intention to update the session log.
    c.  **Update Log (Execute):** Use the appropriate tool to update the task's status to `[In-Progress]` in the `## Project Plan` and add a reference to the task under `## Current Activity`.
    d.  **Execute Task (State):** After the log is updated, announce your intention to execute the task.
    e.  **Execute Task (Execute):** Execute the task.
    6.  **Task Decomposition:** If an `[In-Progress]` task is complex, break it into sub-tasks with a `[Next]` status.
    7.  **Task Failure:** If a task fails, set its status to `[Failed]`, document the failure in `## Summary of Actions`, and remove it from `## Current Activity`.
    8.  **Task Completion:** Upon completion, set the task's status to `[Done]`, add a summary to `## Summary of Actions`, remove it from `## Current Activity`, and set the next task to `[Next]`.
    9.  **Major Change Checkpoint:** Before executing major changes, ask the user if a checkpoint is required.
    10. **Session Pause:** If a session ends with an `[In-Progress]` task, set its status to `[Paused]` and add a note to `## Next Steps & Reminders` to resume it later.
    11. **Ad-hoc Sub-task Tracking:** For a multi-step ad-hoc task, list the main task in `## Current Activity`. Each sub-task must be nested under it. Before executing a sub-task, you must mark it as `[In-Progress]`. Upon completion, update its status to `[Done]` before starting the next sub-task.
    12. **Fast-Track Workflow:** If the user provides approval with the `[fast]` command (e.g., `yes [fast]`), you may bypass the standard status update workflow for that single task. Execute the task directly and then add a corresponding entry to the `## Summary of Actions`. This workflow should only be used for quick and non-destructive operations.

## 2.2. Project Governance
*   **Objective:** To ensure project protocols and the master issue list are kept up-to-date.
*   **Action:** Periodically remind the Program Manager to review session logs for new issues and suggestions.
*   **Implementation / Instructions:**
    1.  At the start of each session, check the `Last Modified:` date at the top of this document.
    2.  If the date is more than 5 days in the past, you must remind the user "Bachir" to review recent session logs for new issues and protocol suggestions.

## 2.3. Time Tracking
*   **Objective:** To automatically track time spent on the project.
*   **Action:** Record session start and end times, and maintain a cumulative project total.
*   **Implementation / Instructions:**
    1.  **Session Start:** When creating a new session log, add the `Session Start:` field with the current timestamp (in ISO 8601 format).
    2.  **Session End Procedure:** When the user ends a session, perform a sign-off procedure:
        a.  Retrieve the `Cumulative Time (Minutes)` from the previous session log, stored in a hidden comment (e.g., `<!-- AI-Data: CumulativeMinutes=3000 -->`).
        b.  Calculate the current session's duration in minutes by comparing the current time with the `Session Start` time.
        c.  Add the current duration to the previous cumulative time to get a new total.
        d.  Convert the durations to a human-readable `Hours and Minutes` format.
        e.  Write the final time tracking block to the session log, including the hidden comment with the new cumulative total in minutes for the next session's calculation.

# 3. Development Protocols

## 3.1. Checkpoints
*   **Objective:** To prevent data loss before major changes by creating and managing restore points.
*   **Action:** Use the project-specific checkpoint script before committing a major change.
*   **Implementation / Instructions:**
    1.  **Locate the Script:** The specific script or procedure for creating a checkpoint may differ between projects. You must consult the `README.md` file of the active project (e.g., `moodle4.5/README.md`) to identify the correct command.
    2.  **Consult User:** If unsure whether a change is major, ask the user before creating a checkpoint.
    3.  **Execute Script:** Execute the designated checkpoint script.
    4.  **Retention Policy:** Be aware that checkpoint scripts may enforce a retention policy (e.g., keeping only the last 3 checkpoints). This behavior will be defined by the specific script being used.

## 3.2. Documentation Integrity
*   **Objective:** To keep all technical documentation up-to-date.
*   **Action:** Update all relevant documentation after any technical change.
*   **Implementation / Instructions:**
    1.  If a change impacts installation or configuration, you must update the corresponding documentation (e.g., `installation.md`, `configuration.md`).

## 3.3. Debugging
*   **Objective:** To execute a methodical, evidence-based debugging process.
*   **Action:** Follow the debugging workflow.
*   **Implementation / Instructions:**
    1.  **Investigate:** Analyze context and logs before attempting a fix.
    2.  **Verify State:** Verify file state and permissions from within the relevant container, not just the host OS.
    3.  **Avoid Destructive Fixes:** Do not suggest a fresh install or environment teardown without clear evidence it is necessary.
    4.  **Consult User:** Do not execute any major or medium change during debugging without user consultation.

## 3.4. Issue Documentation
*   **Objective:** To log all new issues for future context awareness.
*   **Action:** Document new issues in the current session log.
*   **Implementation / Instructions:**
    1.  Verify the issue does not already exist in `issues.md`.
    2.  In the current session log, create a `### New Founded Issues` section if it doesn't exist.
    3.  Document the issue using the following standard format:
        *   `#### Issue: [Concise Issue Title]`
        *   `*  **Date:** YYYY-MM-DD`
        *   `*  **Found by:** [e.g., bachir-AI]`
        *   `*  **Environment:** [e.g., LCL-Windows, VPS-Ubuntu]`
        *   `*  **Status:** Resolved / Unresolved`
        *   `*  **Symptoms:**`
        *   `*  **Diagnosis & Troubleshooting:**`
        *   `*  **Resolution:** [If resolved, explain the fix]`

## 3.5. UI Code Integration
*   **Objective:** To correctly integrate externally generated UI code.
*   **Action:** Follow the UI code integration workflow.
*   **Implementation / Instructions:**
    1.  Use the provided HTML file as a visual reference only.
    2.  Restructure the code into the project's existing folder structure.
    3.  Separate HTML, CSS, and JS into the appropriate files and directories.
    4.  Break down the code into reusable components.
    5.  Ensure the final code is clean, modular, and consistent with the application.

## 3.6. UI Development Backup Workflow
*   **Objective:** To manage backups of the `ui/generated` directory.
*   **Action:** Use the `manage_ui_backup.py` script to prevent data loss during UI development.
*   **Implementation / Instructions:**
    1.  **Backup Policy:** Before any major changes to the `ui/generated` directory, a backup must be created. You should proactively ask to create a backup if you deem a change to be significant.
    2.  **Create Backup:** `python ui/manage_ui_backup.py create`
    3.  **Restore Backup (Interactive):** `python ui/manage_ui_backup.py restore`
    4.  **Restore Backup (Non-Interactive):** `python ui/manage_ui_backup.py restore <backup_filename.tar.gz>`

# 4. Subject Matter Expert Protocols

## 4.1. Research and Analysis
*   **Objective:** To provide well-informed, evidence-based advice.
*   **Action:** Conduct proper research before providing recommendations.
*   **Implementation / Instructions:**
    1.  Use web search and other available tools to gather information.
    2.  Always present findings with a clear list of pros and cons.
