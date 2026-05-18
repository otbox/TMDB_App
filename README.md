# TMDB App

A full-stack movie discovery and rating application built with React on the frontend and Flask on the backend, consuming the [TMDB (The Movie Database) API](https://www.themoviedb.org/).

***

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                        Browser                          │
│              http://localhost:8080                       │
└──────────────────────┬──────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│               Frontend (React + Vite)                    │
│                  nginx:alpine — port 80                  │
│                                                         │
│  Pages: Home · RatedMovies                              │
│  Components: MovieGrid · MovieCard · MovieModal          │
│              StarRating · SearchBar · UserModal          │
│  Router: React Router v7                                │
│  HTTP Client: Axios                                     │
└──────────────────────┬──────────────────────────────────┘
                       │ REST API calls to :5000
┌──────────────────────▼──────────────────────────────────┐
│               Backend (Flask + Python 3.11)              │
│                   Gunicorn — port 5000                   │
│                                                         │
│  Routes:                                                │
│    POST   /users/register                               │
│    POST   /users/login                                  │
│    GET    /ratings                                      │
│    POST   /ratings                                      │
│    PUT    /ratings/<id>                                 │
│    DELETE /ratings/<id>                                 │
│                                                         │
│  Auth: JWT (Flask-JWT-Extended)                         │
│  ORM:  Flask-SQLAlchemy                                 │
│  CORS: Flask-Cors                                       │
└──────────────────────┬──────────────────────────────────┘
                       │ SQLite (persisted via Docker volume)
┌──────────────────────▼──────────────────────────────────┐
│              SQLite Database (db_data volume)            │
│              /app/instance/                              │
│                                                         │
│  Tables: User · Rating                                  │
└─────────────────────────────────────────────────────────┘
```

### Project Structure

```
TMDB_App/
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── requirements.txt
│   ├── run.py
│   └── app/
│       ├── __init__.py         # App factory, DB init, CORS config
│       ├── extensions.py       # SQLAlchemy + JWT instances
│       ├── models.py           # User and Rating models
│       └── routes/
│           ├── users.py        # Register / Login endpoints
│           └── ratings.py      # CRUD rating endpoints
└── frontend/
    ├── Dockerfile
    ├── package.json
    ├── vite.config.ts
    └── src/
        ├── pages/
        │   ├── Home.tsx
        │   └── RatedMovies.tsx
        └── components/
            ├── Layout/
            ├── SearchBar/
            ├── UserModal/
            └── MovieGrid/
                └── MovieCard/
                    └── StarRating/     # Reusable half-star rating component
```

***

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19, TypeScript, Vite 8, React Router v7 |
| Styling | CSS Modules |
| HTTP Client | Axios |
| Backend | Python 3.11, Flask 3.1 |
| Auth | JWT (Flask-JWT-Extended) |
| Database | SQLite via Flask-SQLAlchemy |
| Container Runtime | Docker + Docker Compose |
| Frontend Server | nginx:alpine |

***

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
- A free [TMDB API key](https://www.themoviedb.org/settings/api)

> **Note — IPv6 issue on Linux:** If Docker fails to pull images with a `network is unreachable` error on IPv6 addresses, add the following to `/etc/docker/daemon.json` and restart Docker:
> ```json
> { "dns": ["8.8.8.8", "1.1.1.1"], "ipv6": false }
> ```
> ```bash
> sudo systemctl restart docker
> ```

***

## Running with Docker (recommended)

### 1. Clone the repository

```bash
git clone https://github.com/otbox/TMDB_App.git
cd TMDB_App
```

### 2. Set the TMDB API key

The frontend reads the API key from an environment variable. Create a `.env` file inside `frontend/`:
```bash
echo 'VITE_TMDB_API_KEY="your_api_key_here"' > frontend/.env
```
A plain unquoted value also works in most cases:
```bash
echo "VITE_TMDB_API_KEY=your_api_key_here" > frontend/.env
```
### 3. Build and start all services

```bash
docker compose up --build
```

| Service | URL |
|---|---|
| Frontend | http://localhost:8080 |
| Backend API | http://localhost:5000 |

### 4. Stop all services

```bash
docker compose down
```

To also remove the database volume:

```bash
docker compose down -v
```

***

## Running Locally (without Docker)

### Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate       # Windows: venv\Scripts\activate
pip install -r requirements.txt
python run.py
# API available at http://localhost:5000
```

### Frontend

```bash
cd frontend
yarn install
yarn dev
# App available at http://localhost:5173
```

***

## API Reference

All protected routes require the `Authorization: Bearer <token>` header obtained from login.

### Auth

| Method | Endpoint | Body | Description |
|---|---|---|---|
| `POST` | `/users/register` | `{ username, password }` | Create a new account |
| `POST` | `/users/login` | `{ username, password }` | Returns a JWT token |

### Ratings

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| `GET` | `/ratings` | ✅ | Get all ratings for the logged-in user |
| `POST` | `/ratings` | ✅ | Rate a movie `{ movie_id, rating (0.5–5) }` |
| `PUT` | `/ratings/<id>` | ✅ | Update an existing rating |
| `DELETE` | `/ratings/<id>` | ✅ | Delete a rating |

***

## Environment Variables

| Variable | Location | Description |
|---|---|---|
| `VITE_TMDB_API_KEY` | `frontend/.env` | TMDB API key for movie search and details |
| `FLASK_ENV` | `docker-compose.yml` | Set to `production` in Docker |

***

## Features

- Search movies via the TMDB API
- View movie details in a modal
- Register and log in with JWT authentication
- Rate movies with a half-star precision picker (0.5 to 5 stars)
- View and manage all your rated movies on a dedicated page 
- Persistent ratings stored in SQLite via Docker volume