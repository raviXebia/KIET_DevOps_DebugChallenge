# KIET_DevOps_DebugChallenge

# DevOps Exam — Test Cases
### KIET Group of Institutions | Even Sem 2026

> **Instructions:** Read each test case carefully. Complete all tasks in sequence. Show your terminal output / screenshots as proof of completion wherever mentioned.


## Test Case 1 — Linux System Audit & User Management


**Scenario:** You are a Linux system administrator onboarding new employees at a tech company. Complete the following tasks on your system.

### Tasks

1. Check the identity of the currently logged-in user and display the username of the current session.
2. Check which users are currently logged into the system and what they are doing.
3. View the login history of the system.
4. Display complete system information, the hostname, and how long the system has been running.
5. Create two groups: `developers` and `qa`. Verify both groups were successfully created and display the complete list of groups on the system.
6. Create the following users and assign each to their respective group:

   | Employee | Department  |
   |----------|-------------|
   | aman     | developers  |
   | ritu     | developers  |
   | karan    | qa          |

7. Employee `ritu` has resigned. Ensure she has no active sessions, delete her account along with her home directory, and verify she no longer exists on the system.

### Expected Outcome

- System audit commands display current user, active sessions, login history, hostname, and uptime without errors.
- Groups `developers` and `qa` appear in the system group list.
- Users `aman` and `karan` are created and assigned to the correct groups (verified via `id` command).
- After deletion, any attempt to look up `ritu` returns: **"no such user"**.

---

## Test Case 2 — File Permission Configuration


**Scenario:** A web server requires a properly configured `index.html` file with specific ownership and access permissions.

### Tasks

1. Navigate to your home directory and create a file named `index.html`.
2. Add any HTML text content to the file.
3. Check the file's current default permissions.
4. Grant full permissions (read, write, execute) to all users on the file.
5. Change the file owner and group to `www-data`.
6. Change the file permissions to `755`.
7. Display the final permissions and ownership to verify the configuration.

### Expected Outcome

- File `index.html` exists in the home directory with content.
- After step 4, permissions show `rwxrwxrwx`.
- After step 5, owner and group both show `www-data`.
- Final `ls -l` output shows:
  ```
  -rwxr-xr-x 1 www-data www-data ... index.html
  ```

---

## Test Case 3 — Node.js Application Setup & Git Integration


**Scenario:** You are setting up a new Node.js REST API from scratch and publishing it to GitHub.

### Tasks

1. Initialize a new Node.js project and install the required dependencies: `express` and `@types/express` (as a dev dependency).
2. Create an `index.js` file that starts an Express server on port `8080` with the following endpoint:
   - `GET /health` — returns HTTP status `200` with the JSON response `{ "status": "OK" }`.
3. Initialize a Git repository in the project folder.
4. Create an initial commit with all project files.
5. Create a GitHub repository named `nodejs-api` and push your code to it.

### Expected Outcome

- Project folder contains `package.json`, `node_modules/`, and `index.js`.
- Running the server locally and calling `GET /health` returns:
  ```json
  { "status": "OK" }
  ```
- GitHub repository `nodejs-api` is publicly visible with at least one commit containing all project files.

---

## Test Case 4 — Docker & Containerization


**Scenario:** The Node.js API from Test Case 3 must be packaged into a Docker container for consistent deployment.

### Tasks

1. Write a `Dockerfile` in the project root that:
   - Uses `node:18-alpine` as the base image.
   - Sets `/app` as the working directory.
   - Copies and installs dependencies.
   - Exposes port `8080`.
   - Starts the application using `node index.js`.
2. Build a Docker image named `nodejs-api`.
3. Run the container and verify the `/health` endpoint responds correctly on `http://localhost:8080/health`.
4. Write a `docker-compose.yml` file to run the application as a background service on the same port.

### Expected Outcome

- `docker build` completes with no errors and the image appears in `docker images`.
- The container starts successfully and `GET http://localhost:8080/health` returns:
  ```json
  { "status": "OK" }
  ```
- `docker compose up -d` starts the service in detached mode with no errors.

---

## Test Case 5 — CI/CD Pipeline with GitHub Actions


**Scenario:** Your team requires an automated pipeline that runs on every code push to the `main` branch, installs dependencies, builds the Docker image, and simulates a deployment.

### Tasks

1. Inside your `nodejs-api` repository, create the directory `.github/workflows/`.
2. Create a workflow file `deploy.yml` inside that directory with the following behaviour:
   - **Trigger:** On every `push` to the `main` branch.
   - **Runner:** Ubuntu latest.
   - **Steps:**
     1. Check out the repository code.
     2. Set up Node.js version 18.
     3. Install project dependencies.
     4. Build the Docker image tagged as `nodejs-api`.
     5. Print the message: `Deploying application to server...`
3. Commit all changes with the message: **`"This is the new CI/CD pipeline for CA2"`** and push to `main`.
4. Navigate to the **Actions** tab on GitHub and confirm the pipeline ran successfully.

### Expected Outcome

- `.github/workflows/deploy.yml` is present in the repository.
- The GitHub Actions run triggered by the commit shows all 5 steps completing with a green checkmark.
- No step fails; the final step prints the deploy message in the logs.

---

## Test Case 6 — Code Quality Analysis with SonarQube


**Scenario:** Before any deployment, your team requires a static code quality check using SonarQube.

### Tasks

1. Start a SonarQube server locally using Docker on port `9000`. Wait until the dashboard is accessible at `http://localhost:9000`.
2. Log in with the default credentials and generate an authentication token.
3. Install the `sonarqube-scanner` CLI globally on your system.
4. Create a `sonar-project.properties` file in the project root with the following configuration:
   - **Project Key:** `nodejs-api`
   - **Project Name:** `NodeJS API`
   - **Host URL:** `http://localhost:9000`
   - **Login:** *(your generated token)*
   - **Sources:** `.` (current directory)
   - **Exclusions:** `node_modules/**`
5. Run the SonarQube scanner from the project directory.
6. Open the SonarQube dashboard and confirm the project appears with its analysis results.

### Expected Outcome

- SonarQube container is running and the dashboard loads at `http://localhost:9000`.
- Scanner runs without errors and exits with `EXECUTION SUCCESS`.
- Project `NodeJS API` appears on the SonarQube dashboard with a completed analysis report showing metrics (bugs, code smells, coverage).


