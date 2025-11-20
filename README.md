# Our Shelves

A reading tracker web application that allows users to search for books using the **Open Library API**, save them to their personal digital shelf, and manage their book collection.

## Team Members

**Sprint 1/2:**
- Alston
- Danny

**Sprint 3:**
- Kim
- Maddie

**Sprint 4:**
- Tav
- Tia

**Sprint 5:**
- Max
- Huma
---

## Project Description

**Our Shelves** lets users:
- Search for books by title using the [Open Library API](https://openlibrary.org/developers/api).
- Add books to their personal shelf stored in a **MySQL** database.
- View their saved books.
- Delete books from their library.
- Interact with a **React frontend** and **Express backend**, connected via REST API.

### Future Feature Goals
- Add personal notes to books.
- Track bookmarks / reading progress.
- Organize books into multiple shelves.
- Share shelves and notes with friends.
- Support light/dark theme customization.

---

## Tech Stack

| Layer             | Technology                     |
|--------------------|-----------------------------|
| Frontend           | React (Vite), React Router   |
| Backend            | Node.js, Express.js          |
| Database           | MySQL (`mysql2/promise`)     |
| External API       | Open Library API            |
| Packaging     | Docker            |
| Deployment         | Ubuntu Server         |

---

## Prerequisites

Make sure the following are installed:

- [Node.js v18+](https://nodejs.org/en/)
- [npm](https://www.npmjs.com/)
- [MySQL](https://dev.mysql.com/downloads/mysql/)
- [Docker](https://www.docker.com/get-started/)

---

## Environment Variables

### Inside project root `env`
```env
# db & api
MYSQL_USER=username
MYSQL_PASSWORD=superSecurePassword
MYSQL_DATABASE=my_favorite_db

# api
DB_PORT=3306
PORT=3000
HOST=localhost

# frontend
VITE_API_URL=http://${HOST}:${PORT}
```

> For production, replace `localhost` with your server IP or domain name.

---

## Local Development Setup

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/our-shelves.git
cd our-shelves
```

### 2. Install Dependencies
```bash
cd backend
npm install

cd ../frontend
npm install
```

### 3. Set Up `.env` File
- Create `.env` inside root directory.
- Copy and paste the environment variable structure shown above.

---

## Testing Strategy

This project includes unit tests for both the frontend and backend, integration tests, as well as end-to-end (E2E) tests.

### Backend - Unit Tests (Jest)

Purpose: Tests individual backend controllers in isolation. All database calls (db.js) and fetch requests are mocked, so no database or network connection is required.

How to Run:

```bash
cd backend
npm test
```


This command runs all files ending in .test.js found in the backend/__tests__ folder.

### Frontend - Unit Tests (Vitest & React Testing Library)

Purpose: Tests individual React components in a simulated browser environment. All fetch API calls are mocked in the setup file (frontend/src/setupTests.js).

How to Run:

```bash
cd frontend
npm test
```

This command will start Vitest in "watch mode," automatically re-running tests as you save changes.

### Coverage Reports

To generate a coverage report, cd into the respective directory (backend/frontend) and run the following command

```bash
npm test -- --coverage
```

## Backend - Integration Tests (Jest & Testcontainers)

Purpose: Tests the live application routes and controllers against a real, temporary database. This ensures all database queries, joins, and logic work as expected.

### Requirements: Docker must be installed and running on your system

How to Run:

```bash
cd backend
npm run test:integration
```

## End-to-End (E2E) Tests (Cypress)

Purpose: Simulates a real user journey in a real browser. It tests the complete application flow, from searching for a book to adding it to the library and deleting it.

How to Run (2 Steps):

1. Start the frontend dev server:

```bash
cd frontend
npm run dev
```

2. Open the Cypress runner:

### Important - Make sure to do this in a new terminal window

```bash
cd frontend
npx cypress open
```

This will open the Cypress app. From there, you can choose a browser and run the app.cy.js test suite.

### Generating Videos & Screenshots (Cypress Headless Mode)

Purpose: Runs the tests in the terminal without opening a popup window. **This mode is required to generate video recordings and error screenshots**.

How to Run:

1. Ensure the frontend dev server is running (if not already):

```bash
cd frontend
npm run dev
```

2. Open a new terminal:

```bash
cd frontend
npx cypress run
```
Output:

- Videos are saved to: frontend/cypress/videos
- Screenshots (on failure) are saved to: frontend/cypress/screenshots

## Running the Application (Local)

The backend, frontend, and MySQL server can all be built and ran with one command:
```bash
docker compose up -d
```

---

## Deployment Instructions (Ubuntu + Docker)

### 1. Update VM and Install Dependencies
**Updating:**
```bash
apt update
yes | sudo DEBIAN_FRONTEND=noninteractive apt-get -yqq upgrade
```

**Install Git**
```bash
apt install git
```

**Install Docker Engine With apt Repository**
Setup apt repo:
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Install docker package:
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Confirm docker is installed and running:
```bash
sudo systemctl status docker
```

**Install docker compose**
```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

### 2. Clone Repository
```bash
git clone https://github.com/your-username/our-shelves.git
cd our-shelves
```

### 3. Add .env to repository
```bash
nano .env
```
Inside nano text editor:

```env
# db & api
MYSQL_USER=username
MYSQL_PASSWORD=superSecurePassword
MYSQL_DATABASE=my_favorite_db

# api
DB_PORT=3306
PORT=3000
HOST=<VM IP>

# frontend
VITE_API_URL=http://${HOST}:${PORT}
```
Ctrl + o to save
Ctrl + x to exit

Check if .env exists:
```bash
ls -a
```
### 4. Build and Deploy Docker Images
The application can be built and deployed with one command now:
```bash
docker compose up -d
```

The frontend will be running on the VMs IP address defined in the env, on PORT 5173.
ex: http://0.0.0.0:5173  (replace 0.0.0.0 with VM IP)


---

## API Endpoints

| Method | Endpoint                        | Description                         |
|--------|-----------------------------------|-------------------------------------|
| GET    | `/books`                          | Fetch all saved books              |
| POST   | `/books`                          | Add a new book                     |
| DELETE | `/books/:id`                      | Delete a book by ID                |
| GET    | `/books/search/:bookName`         | Search books via Open Library API  |

---

## Useful Commands

**Updating Deployed App:**
```bash
git pull
docker compose up -build
```

**Stopping:**
```bash
docker compose down
```

---

## Troubleshooting Tips
- Ensure `.env` files are correctly configured
- Confirm no leading spaces in env variables
- Check firewall settings if deploying to a remote server (allow ports 3000 and 5173).

---

# Continuous Integration (GitHub Actions)

This project includes a full automated testing pipeline using **GitHub Actions**, ensuring that all parts of the application are validated before merging into protected branches.

The workflow file is located at:

```
.github/workflows/test.yml
```

## What the CI Pipeline Does

The CI runs automatically on:

- **Pushes** to `main` and `dev`
- **Pull requests** targeting `main` and `dev`

Each run executes the full test suite:

## 1. Backend Unit Tests (Jest)
- Runs:  
  ```sh
  cd backend && npm test
  ```
- Ensures all backend logic modules load and behave correctly.

## 2. Frontend Unit Tests (Vitest)
- Runs:
  ```sh
  cd frontend && npm test
  ```
- Validates React components and client-side logic.

## 3. Backend Integration Tests (Jest + Testcontainers)
- Spins up an ephemeral MySQL container in CI  
- Runs:
  ```sh
  cd backend && npm run test:integration
  ```

## 4. End-to-End Tests (Cypress)
- Starts full stack via Docker Compose:
  ```sh
  docker compose up -d
  ```
- Waits for the frontend to become available  
- Executes Cypress in headless mode against `http://localhost:5173`

---

# Merge Protection

If **any** test suite fails:

- The workflow fails  
- The pull request is blocked  
- The code cannot be merged into `main` or `dev`

This ensures only fully tested, working code reaches the protected branches.

---

# Viewing Test Results

You can view each CI run by navigating to:

**GitHub → Actions → CI - Run All Tests**

Each job exposes logs for:

- Dependency installation  
- Backend unit tests  
- Frontend unit tests  
- Integration tests  
- Docker startup  
- Cypress tests  

---

# Node Version Consistency

The CI pipeline uses:

**Node v20.19.0**

This matches the project’s development environment and ensures consistent module loading behavior across Windows 11 (local) and Ubuntu (CI).

---

## License
This project is for educational use as part of a student project at Green River College.
