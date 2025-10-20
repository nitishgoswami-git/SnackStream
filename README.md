# SnackStream

Small food-reels project with a Node/Express backend and a Vite + React frontend.

This repository contains two main folders:

- `backend/` — Express API server (MongoDB + Mongoose). Includes authentication for users and food partners, file uploads, likes and saves.
- `frontend/` — Vite + React single-page app that consumes the backend API and shows a short-form reels-like feed.

## Quick start

Requirements:

- Node.js (16+ recommended)
- npm
- A running MongoDB instance (local or cloud)

1. Clone the repo (or use the workspace files you already have).

2. Install and run the backend:

```bash
cd backend
npm install
# Development server with auto-reload
npm run dev
```

The backend uses `server.js` as the entrypoint and provides routes under `/api` (see API section).

3. Install and run the frontend:

```bash
cd frontend
npm install
# Start the Vite dev server
npm run dev
```

The frontend will run with Vite (default port 5173) and talk to the backend API.

## Project structure (top-level)

- backend/
  - server.js — Express server bootstrap
  - src/
    - controllers/ — request handlers
    - models/ — Mongoose models
    - routes/ — Express routes (auth, food, food-partner)
    - middlewares/ — auth middleware
    - services/ — helpers (e.g. storage)

- frontend/
  - src/ — React app components and pages
  - public/ — static assets
  - vite.config.js

## Important npm scripts

- Backend (`backend/package.json`):
  - `dev` — nodemon server.js (development)

- Frontend (`frontend/package.json`):
  - `dev` — vite (development server)
  - `build` — vite build (production bundle)
  - `preview` — vite preview

Use the exact scripts above when starting each part of the project.

## API (high-level)

Base API path: /api

Auth routes (public):

- POST /api/user/register — register a new user
- POST /api/user/login — login user
- GET /api/user/logout — logout user

- POST /api/food-partner/register — register a food partner
- POST /api/food-partner/login — login food partner
- GET /api/food-partner/logout — logout food partner

Food routes (requires auth):

- POST /api/food/ — create a food item (protected: food partner). Accepts multipart form data (file field named `mama`).
- GET /api/food/ — list food items (protected: authenticated user)
- POST /api/food/like — like a food item (protected: authenticated user)
- POST /api/food/save — save a food item (protected: authenticated user)
- GET /api/food/save — list saved food for the user (protected)

Food-partner routes:

- GET /api/food-partner/:id — get a food partner by id (protected: authenticated user)

Note: Authentication middleware lives in `backend/src/middlewares/auth.middleware.js` and the controllers are in `backend/src/controllers/`.

## Environment variables

The backend uses `dotenv`. Typical variables you may need:

- MONGODB_URI — MongoDB connection string
- JWT_SECRET — JSON Web Token secret
- IMAGEKIT_PUBLIC_KEY, IMAGEKIT_PRIVATE_KEY, IMAGEKIT_URL_ENDPOINT — if imagekit is used

Create a `.env` file in `backend/` with the required values before starting the server.

## Troubleshooting

- If ports collide, change the Vite dev server port in `frontend/package.json` scripts or set Vite env vars.
- If the frontend can't reach the backend, check the backend base URL used by the frontend (look for axios baseURL or environment variables in `frontend/src`).

## Testing / Verification

Manual smoke test:

1. Start backend (from `backend/`): `npm run dev`
2. Start frontend (from `frontend/`): `npm run dev`
3. Register a user and a food partner via the frontend or via curl/Postman using the auth endpoints.
4. As a food partner, create a food post with an image (field `mama`) and verify it appears in the user's feed.

## Notes and next steps

- Add a `README-frontend.md` and `README-backend.md` if you want more granular developer docs.
- Add tests and CI in the future.

---

// ...existing code...

## Screenshots

Below are example screenshots of the application. Put your images in `docs/screenshots/` and update filenames/captions as needed.

<p align="center">
  <img src="docs/screenshots/home.png" alt="Home screen" width="800"/>
  <br/>
  <em>Figure 1 — Home screen</em>
</p>

<p align="center">
  <img src="docs/screenshots/player.png" alt="Player screen" width="800"/>
  <br/>
  <em>Figure 2 — Player screen</em>
</p>

<p align="center">
  <img src="docs/screenshots/settings.png" alt="Settings" width="800"/>
  <br/>
  <em>Figure 3 — Settings</em>
</p>

// ...existing code...
