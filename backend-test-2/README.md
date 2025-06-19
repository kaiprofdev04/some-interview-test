# 🔁 Trend Importer

Welcome! This take-home assignment is designed to evaluate your backend development skills in API integration, database design, and REST API implementation.

You will build a service that imports blog posts from a public WordPress API, stores them in a local database, and exposes them through your own API endpoints.

## 📌 Goal

Fetch blog posts from:

https://bergvik.se/wp-json/wp/v2/posts

Then:
- Store only the **first 10 posts** in a local database (PostgreSQL preferred, or another if you're more comfortable with it)
- Implement a sync endpoint that fetches more posts but only inserts **new ones**
- Implement a trends endpoint to retrieve posts from your database

## ✅ Requirements

### 1. Initial Data Load (Limit to 10 Posts)

- When setting up the project, insert only the **first 10 posts** returned by the WordPress API
- Extract and store the following fields:
  - `id` (or unique identifier like `link`)
  - `title`
  - `excerpt`
  - `link`
  - `date`
- Save them in a `posts` table in your database

Example schema:
```sql
CREATE TABLE posts (
  id TEXT PRIMARY KEY,
  title TEXT,
  excerpt TEXT,
  link TEXT,
  published_at TIMESTAMP,
  source TEXT
);
```

### 2. Sync Endpoint

`POST /api/sync`

This endpoint manually triggers a sync with the WordPress API.

- It should fetch **more than 10 posts** from the API (you can fetch the first 20, for example)
- For each fetched post:
  - Check if it already exists in your database (by `id` or `link`)
  - If it's **not yet stored**, insert it
  - If it **already exists**, skip it
- Return a response like:
```json
{
  "inserted": 4,
  "skipped": 16
}
```

### 3. Trends Endpoint

`GET /api/trends`

This endpoint should return posts **from your database**, not directly from the WordPress API.

Query Parameters:
- `limit=n` – Limit the number of results (default: 10)
- `search=keyword` – Filter posts where the keyword appears in the title or excerpt (case-insensitive)
- `since=YYYY-MM-DD` – Return posts published after the given date

Example:
```
GET /api/trends?limit=5&search=butik&since=2024-12-01
```

## 🔧 Tech Guidelines

- Use Node.js with TypeScript and build upon the existing Next.js project
- Use PostgreSQL or another database of your choice for persistent storage
- Code should be modular, clean, and well-commented
- Handle errors and edge cases gracefully

## ▶️ Getting Started

1. Clone the repo:
```bash
git clone https://github.com/kaiprofdev04/some-interview-test.git
```

2. Install dependencies:
```bash
npm install
```

3. Set up your environment:
- Connect to a local PostgreSQL (or your preferred database) instance
- Create the `posts` table using the schema above
- Fetch and insert the **first 10 posts** from the WordPress API as part of your initial setup logic

4. Run the server:
```bash
npm run dev
```

5. Sync more posts (e.g. up to 20 total from API):
```bash
curl -X POST http://localhost:3000/api/sync
```

6. Fetch stored posts:
```bash
curl http://localhost:3000/api/trends?limit=5&search=butik&since=2024-12-01
```

## 📦 Deliverables

Please submit to interviewer's email:
- A GitHub repo (public or private) containing:
  - Working implementation of:
    - `POST /api/sync`
    - `GET /api/trends`
  - All project files and code
  - A `README.md` with:
    - A brief description of your database schema (e.g. what columns you store)
    - Any assumptions or limitations

- A brief **screen recording (video)** where you demonstrate:
  - That your local database is working
  - That `/api/sync` successfully fetches posts and only inserts new ones
  - That `/api/trends` correctly filters and returns results using `limit`, `search`, and `since`
  - Optional: You may use Postman, browser, or curl to show API calls
  - **Explain your solution and your thought process**

## 📚 Notes

- No authentication is required
- You may use any libraries you're comfortable with
- Focus on good architecture, error handling, and performance

Good luck — we’re excited to see your solution! 🚀
