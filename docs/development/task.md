# Task: Hello World App

Now it's your turn. Build a small full-stack application from scratch by going through every role — Nika, Kristina, and Igor — exactly as described in the process above.

---

### What you'll build

A minimal web app with:

- A **"Hello" button** on the frontend
- A **FastAPI endpoint** on the backend that responds with `"Hello back!"`
- Proper folder structure and a feature branch workflow

### Project structure

```
hello-world-app/
├── Frontend/        # Next.js app
├── Backend/         # FastAPI (Python) server
└── Documentation/   # Notes, decisions, README
```

---

### Step 1 — Setup (both)

!!! warning "Python version check"
    Before you start, confirm which Python version is installed on your machine by running `python --version` or `python3 --version` in your terminal. Note the version — you will use this **default system Python** throughout the task. Do **not** create a virtual environment.

1. Create a new **private** repository on GitHub named `hello-world-app`.
2. Clone it locally.
3. Create the three top-level folders: `Frontend/`, `Backend/`, `Documentation/`.
4. Create and publish a feature branch named `feature/hello-button`.

---

### Step 2 — Nika's work (Frontend)

!!! info "Nika only starts the frontend server"
    For UI-only work you do **not** need the backend running.

1. **Set up the Next.js app** inside `Frontend/` — Nika is vibe coding, so she opens the project in **VScode or Cursor (AI Code Editor)** and prompts it directly instead of running CLI commands manually. A good prompt to get started:

    > *"Create a new Next.js project in the Frontend/ folder. Use TypeScript, Tailwind CSS and ESLint. Use the App Router. It's a Hello World app — the only thing on the page is a button labelled 'Hello'. The button doesn't do anything yet, no backend connection needed. Keep the folder structure clean and don't add any example components."*

    Review what Cursor generates before accepting — make sure the folder structure matches the project layout and no unwanted files are added.
2. **Start the frontend server** and confirm the app loads in the browser.
3. **Verify** the Hello button renders correctly in the browser.
4. **Commit and push** your changes to the `feature/hello-button` branch with a descriptive commit message.

---

### Step 3 — Kristina's work (Backend + integration)

!!! info "Kristina starts both servers"
    Backend and frontend must both be running for integration testing.

1. **Pull** Nika's latest changes from the `feature/hello-button` branch.
2. **Set up the FastAPI backend** inside `Backend/` — Kristina opens **VS Code** and uses **GitHub Copilot** to get it done. A good prompt:

    > *"Set up a Python backend in the Backend/ folder for a Hello World app. Use FastAPI. Use the default system Python — do not create a virtual environment. Install FastAPI and uvicorn and save the dependencies to a requirements.txt file. Add a single GET endpoint at /hello that returns the JSON message 'Hello back!'. Also configure CORS so the frontend running on localhost:3000 can call the API."*

    Review what Copilot generates, make sure the endpoint path and response match exactly, and that the virtual environment folder is covered by `.gitignore`.
3. **Connect the Hello button to the backend** — prompt Copilot again to wire up Nika's existing button so it calls the `/hello` endpoint and displays the response on screen:

    > *"Update the Hello button in the Next.js frontend so that when it's clicked it sends a GET request to the FastAPI /hello endpoint and shows the returned message below the button."*

4. **Start the backend server** in one terminal and the **frontend server** in a second terminal.
5. **Test the integration** — open the frontend in the browser, click the Hello button, and confirm `Hello back!` appears on screen.
6. **Add a README** inside `Documentation/` explaining how to run both servers locally.
7. **Commit and push** all changes to the `feature/hello-button` branch.

---

### Step 4 — Igor's work (Review & merge)

1. Open a **Pull Request** on GitHub from `feature/hello-button` → `main`.
2. Use the following PR checklist before approving:

    - [ ] Button renders and is styled correctly
    - [ ] Clicking the button calls the `/hello` endpoint and displays `Hello back!`
    - [ ] `requirements.txt` is present in `Backend/`
    - [ ] `Documentation/` contains setup instructions
    - [ ] No secrets or `.env` files committed

3. Leave at least one **review comment**, then **approve** and **Squash and merge** into `main`.

---

### Step 5 — Kristina's work (GCP deployment)

1. **Create a personal GCP account** at [console.cloud.google.com](https://console.cloud.google.com) if you don't already have one.
2. **Create a new GCP project** named `hello-world-app` in the console.
3. **Enable the Cloud Run API** and the **Artifact Registry API** for the project.
4. **Pull the merged code** from `main` to your local machine.
5. **Containerise the backend** — write a `Dockerfile` inside `Backend/` that installs dependencies and starts the FastAPI server.
6. **Build the container image** and push it to the project's Artifact Registry.
7. **Deploy the image to Cloud Run** — configure the service to allow unauthenticated requests and note the public URL assigned by GCP.
8. **Update the frontend** to point the Hello button's fetch call at the Cloud Run URL instead of `localhost`.
9. **Deploy the frontend** — you may use Vercel, GCP Cloud Run, or any static hosting of your choice.
10. **Test end-to-end** — open the deployed frontend URL in a browser, click Hello, and confirm `Hello back!` is returned from the live Cloud Run service.

---

### Done! ✓

You have completed one full feature cycle at Fintom8. You know how to:

- Set up and share a feature branch
- Build a Next.js frontend that calls a backend API
- Build a FastAPI endpoint with CORS enabled
- Open, review, and merge a Pull Request
- Deploy a containerised app to GCP Cloud Run
