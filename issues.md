# Known Issues

This document tracks known issues, their diagnosis, and resolutions encountered during the project's development.

---

#### Issue: `run_shell_command` Directory Path Error
*   **Date:** 2025-09-19
*   **Found by:** bachir-AI
*   **Environment:** LCL-Windows
*   **Status:** Resolved
*   **Symptoms:**
    *   When using the `run_shell_command` tool, attempting to specify a relative path for the `directory` parameter (e.g., `directory: 'moodle4.5'`) results in the error: `Directory is not a registered workspace directory.`
*   **Diagnosis & Troubleshooting:**
    *   The initial attempt to run `git init` in the `moodle4.5` sub-directory failed with the "not a registered workspace directory" error.
    *   The error indicates that the tool does not accept arbitrary relative paths for the `directory` parameter.
*   **Resolution:**
    *   The workaround is to execute the command from the root of the project (the default directory for the tool) and use `cd` within the command string to change to the desired directory.
    *   **Example:** `cmd.exe /c "cd moodle4.5 && git init"`

---

#### Issue: 404 Error Accessing Custom PHP Tool in Subdirectory
*   **Date:** 2025-09-14
*   **Found by:** bachir-AI
*   **Environment:** LCL-Windows
*   **Status:** Resolved
*   **Symptoms:**
    *   A custom tool created at `/dev_tools/tasks.php` was inaccessible via the browser (`http://localhost:9000/dev_tools/tasks.php`), consistently returning a 404 "Not Found" error.
*   **Diagnosis & Troubleshooting:**
    *   **Initial Hypothesis (Incorrect):** The Nginx configuration (`nginx/default.conf`) was suspected to be too restrictive. Several attempts were made to add new `location` blocks and modify existing PHP handlers to specifically allow access to the `/dev_tools/` directory.
    *   **Action:** The Nginx configuration was modified multiple times and the containers were restarted. This did not resolve the issue.
    *   **Debugging Step:** An `ls -l` command was executed from within the `moodle-nginx` container to inspect the contents of the web root: `docker compose exec moodle-nginx ls -l /var/www/html/dev_tools`.
    *   **Root Cause:** The command failed with "No such file or directory". This proved the issue was not with Nginx configuration, but that the files created on the host machine were not visible inside the container. The `docker-compose.yml` file was using a **named volume** (`moodle-code`) for the web root, which does not live-sync with the host filesystem.
*   **Resolution:**
    *   The `docker-compose.yml` file was modified to replace the named volume with a **bind mount** for both the `moodle-nginx` and `moodle-php` services.
        *   **Changed:** `- moodle-code:/var/www/html`
        *   **To:** `- ./moodle:/var/www/html`
    *   All troubleshooting-related changes to `nginx/default.conf` were reverted to their original state.
    *   The Docker containers were re-created to apply the new volume configuration using the command: `docker compose up -d --force-recreate`. This resolved the issue.

---

#### Issue: Persistent "relation already exists" errors during database restore.
*   **Date:** 2025-09-22
*   **Found by:** bachir-AI
*   **Environment:** LCL-Windows
*   **Status:** Unresolved
*   **Symptoms:** 
    *   After dropping and creating the database, restoring from the SQL backup file consistently results in `ERROR: relation "mdl_..." already exists`. This occurs even after attempting a full Docker environment cleanup (stopping/removing all containers, removing all volumes, rebuilding environment).
*   **Diagnosis & Troubleshooting:** 
    *   The `DROP DATABASE` command appears insufficient to fully clear the database, leading to conflicts with `CREATE TABLE` commands in the backup. This suggests the issue might be within the SQL backup file itself (e.g., containing duplicate `CREATE TABLE` statements or other inconsistencies), or a deeper interaction with PostgreSQL's restore process that is not resolved by a simple database drop/create.

---

#### Issue: Docker volume management is unreliable on Windows.
*   **Date:** 2025-09-22
*   **Found by:** bachir-AI
*   **Environment:** LCL-Windows
*   **Status:** Unresolved
*   **Symptoms:** 
    *   `docker volume prune` and `docker volume rm` commands do not reliably remove volumes, even when containers are stopped. Volumes remain visible in Docker Desktop.
*   **Diagnosis & Troubleshooting:** 
    *   Docker daemon seems to hold onto volumes, preventing their removal. Requires manual verification with `docker volume ls` after any volume operation.