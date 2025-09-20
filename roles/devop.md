# Project Management Protocols

## Tasks Management
*   **Objective:** Standardize and streamline managing project tasks and activities.
*   **Action:** Tasks exuction must follow the following implementation and instructions guidlines.
*   **Implementation / Instructions:**
    *   Each session log must contains at least the following sections:
        *   `## Summary of Actions`
        *   `## Current Activity`
        *   `## Next Steps`
        *   `## Project Plan | Scope of Work`
    *   Before start executing any task, check the session log and make sure the task exists in the `## Project Plan | Scope of Work`. If not create a new task, and mark it as `[Next]`.
    *   Inform the user that you will start with the task, get the approval, and mark it as `[In Progress]`, and added it to `## Current Activity`.
    *   If the task requires further activities, create a sub-task or more as required and mark it as `[Next]`.
    *   If the task could not be completed due whatever reason, mark it as '[Failed]', remove it from the `## Current Activity`, and add it to `## Summary of Actions` with description on why failed and what the proposed solution.
    *   Once successfully completing a task, mark it as `[Done]` in the `## Project Plan | Scope of Work`, remove it from the `## Current Activity`, add it to the `## Summary of Actions`, mark the next task as `[Next]`, and repeat the workflow.
    *   If a task will require major changes, inform the user and check if he wants to create a checkpoint. If unsure whether a change is major, ask the user.
    *   If the user asks to end the session, if a task has `[In Progress]` status then mark it as `[Paused]` in both `## Current Activity` and `## Project Plan | Scope of Work`, and add it to `## Next Steps`.

# Development Protocols

## Checkpoints
*   **Objective:** Maintain backups and restoring checkpoints in case of major failure or to rollback unwanted changes.
*   **Action:** Create a checkpoint before committing a major change to prevent data loss.
*   **Implementation / Instructions:**
    *   If unsure whether a change is major, ask the user.
    *   Use the project's designated checkpoint script or backup method mentioned in the project documentation.

## Documentation Integrity
*   **Objective:** Maintain clean and up-to-date documentation is a key factor for project success.
*   **Action:** Update all relevant technical documentation after a change.
*   **Implementation / Instructions:**
    *   If a change impacts installation or configuration, update `installation.md`, `configuration.md`, or other relevant technical documents.

## Context Scoping via `.aiexclude`
*   **Objective:** Minimize input token usage and focus context on relevant application code.
*   **Action:** Create and maintain an `.aiexclude` file in the project root.
*   **Exclusion Targets:** Add high-volume, low-relevance directories such as `node_modules/`, `vendor/`, `build/`, `dist/`, `*.log`, and large media assets.
*   **Inclusion Rationale:** Dependency manifests (e.g., `package.json`, `composer.json`) are sufficient for dependency awareness.

## Debugging
*   **Objective:** Achieve a useful debugging process and avoid loops and distructive attempts.
*   **Action:** Follow a methodical, evidence-based approach to debugging.
*   **Implementation / Instructions:**
    *   Investigate the context fully before attempting a fix.
    *   Always check and analyze logs (`docker compose logs`, php logs, ngnix logs, etc.) as a part of the debugging process.
    *   Verify file state and permissions from within the relevant container, not just the host.
    *   Do NOT simply suggest fresh installs or tearing down environments without a good and clear reason with evidence that it will work.
    *   NEVER take a decision or try a solution or execute what can be considered a major or even a medium change without consulting with the user first. 

## Efficient Log File Analysis
*   **Objective:** Avoid consuming excessive input tokens on large log files.
*   **Action:** When debugging, do not read an entire log file. First, read the last 20-30 lines to check for recent errors.
*   **Implementation / Instructions:** Use OS-specific shell commands:
    *   **Linux/macOS:** `tail -n 30 <file_path>`
    *   **Windows:** `Get-Content <file_path> -Tail 30`
*   **Escalation:** If the initial tail is insufficient, progressively increase the line count or read specific sections as needed.

## Issue Documentation
*   **Objective:** Log issues for future context awareness and avoid repeatative mistakes.
*   **Action:** When a new issue is identified, it must be documented in the current session log using the standard format.
*   **Implementation / Instructions:**
    *   Review the issues.md file and check if this issue already exsits or a new one.
    *   Review the current session log.
    *   Create a `### New Founded Issues` section in the session log if it doesn't exist.
    *   Document the issue with the following structure:
    *   `#### Issue: [Concise Issue Title]`
    *   `*  **Date:** YYYY-MM-DD`
    *   `*  **Status:**` Resolved or Unresolved`
    *   `*  **Symptoms:**`
    *   `*  **Diagnosis & Troubleshooting:`

## UI Code Integration
*   **Objective:** A workflow to integrate externally generated UI code.
*   **Action:** Follow the instructions in the implementation below.
*   **Implementation / Instructions:**
    *   Take the provided HTML file as a visual reference.
    *   Restructure the code into the project's existing folder structure.
    *   Separate HTML, CSS, and JS into appropriate files.
    *   Break down the code into reusable components.
    *   Ensure the final code is clean, modular, and consistent with the application.