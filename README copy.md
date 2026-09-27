# Netflix Clone — Full Stack Setup

## Project Structure

```
netflix-clone/
├── public/
│   ├── index.html        ← your HTML
│   ├── style.css         ← your CSS
│   ├── script.js         ← frontend JS
│   └── assets/
│       ├── logo.svg
│       ├── bg.jpg
│       └── favicon.ico
├── server.js             ← Express backend
├── package.json
├── netflix.db            ← SQLite DB (auto-created on first run)
└── README.md
```

## Setup & Run

```bash
# 1. Install dependencies
npm install

# 2. Start the server
npm start

# 3. Open in browser
http://localhost:3000
```

For development with auto-reload:
```bash
npm run dev
```

---

## Database (SQLite)

The file `netflix.db` is created automatically on first run.

### Tables

**users**
| Column     | Type    | Description              |
|------------|---------|--------------------------|
| id         | INTEGER | Auto-increment PK        |
| email      | TEXT    | Unique user email        |
| password   | TEXT    | bcrypt hash (optional)   |
| plan       | TEXT    | Current plan             |
| created_at | TEXT    | Registration timestamp   |

**subscriptions**
| Column     | Type    | Description              |
|------------|---------|--------------------------|
| id         | INTEGER | Auto-increment PK        |
| user_id    | INTEGER | FK → users.id            |
| plan       | TEXT    | Plan name                |
| started_at | TEXT    | Subscription timestamp   |

### View data anytime
```bash
# Install sqlite3 CLI if needed
npx sqlite3 netflix.db

# Inside sqlite3:
.tables
SELECT * FROM users;
SELECT * FROM subscriptions;
.quit
```

---

## API Endpoints

| Method | Endpoint         | Description               |
|--------|-----------------|---------------------------|
| POST   | /api/register   | Register with email       |
| POST   | /api/signin     | Sign in (demo pw: netflix123) |
| POST   | /api/subscribe  | Choose a plan             |
| GET    | /api/plans      | List all plans + prices   |
| GET    | /api/languages  | Available languages       |
| GET    | /api/faq        | FAQ data                  |
| GET    | /api/users      | List all users (dev only) |

### Example requests

```bash
# Register
curl -X POST http://localhost:3000/api/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com"}'

# Sign in
curl -X POST http://localhost:3000/api/signin \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"netflix123"}'

# Subscribe
curl -X POST http://localhost:3000/api/subscribe \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","plan":"Premium"}'
```

---

## Upgrading to PostgreSQL (optional)

Replace `better-sqlite3` with `pg`:

```bash
npm install pg
npm uninstall better-sqlite3
```

Update connection in `server.js`:
```js
const { Pool } = require("pg");
const pool = new Pool({ connectionString: process.env.DATABASE_URL });
```

---

## Security Notes (before going live)
- Remove or protect `GET /api/users`
- Add JWT-based authentication
- Hash passwords with bcrypt on registration
- Use `helmet` middleware: `npm install helmet`
- Set up rate limiting: `npm install express-rate-limit`
