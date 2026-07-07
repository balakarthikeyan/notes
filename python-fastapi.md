# FastAPI

FastAPI endpoints to use Pydantic request models. client can send JSON bodies instead of query parameters. FastAPI’s `APIRouter` instead of creating multiple `FastAPI()` instances. 

## Why Use Pydantic?
- `Validation:` Pydantic automatically checks types (e.g., ensures  is a string).
- `Documentation:` Swagger UI () shows exactly what JSON body is expected.
- `Consistency:` Cleaner API design — clients always send JSON, not query strings.
- `Extensibility:` Easy to add more fields later without changing function signatures

# 🛠️ Project Setup 

```bash
pip install fastapi uvicorn sqlalchemy aiosqlite passlib[bcrypt] python-jose
```
 
# 📌 Explanation

- `fastapi` → your web framework.
- `uvicorn[standard]` → ASGI server to run FastAPI.
- `sqlalchemy[asyncio]` → ORM for database with async support.
- `aiosqlite` → async SQLite driver.
- `python-jose` → JWT handling.
- `passlib[bcrypt]` → password hashing and verification
- `httpx` → `httpx.AsyncClient` to call your endpoints and verify behavior.

# 📂 Project Structure

```bash
fastapi_app/ 
├── main.py 
├── models.py 
├── database.py 
├── auth.py 
```

# 🚀 Run the App 
`py -m <filename>`
`uvicorn fastapi_app.main:app --reload `

Open docs at: http://127.0.0.1:8000/docs

# ✅ Run Tests

pytest -v

---


```bash
py -m venv myenv
myenv\Scripts\activate
pip list
python -m pip install --upgrade pip
python -m pip --version
pip --version
pip list
pip install requests
pip install aiohttp
pip install --upgrade pip setuptools wheel
pip install fastapi uvicorn sqlalchemy aiosqlite passlib[bcrypt] python-jose 
pip install pytest black httpx pytest-asyncio

```

```python
from fastapi import FastAPI, Depends, HTTPException
```

- FastAPI → the web framework you’re using.
- Depends → lets you declare dependencies (like database sessions).
- HTTPException → used to raise errors with status codes (e.g., 404, 400).

```python
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.future import select
```

- AsyncSession → SQLAlchemy’s async database session class.
- select → SQLAlchemy’s modern query builder for selecting rows.

```python
from database import Base, engine, SessionLocal
from models import User, Item
from auth import verify_password, get_password_hash, create_access_token
```

- These are absolute imports from your own files:
- database.py → defines Base (ORM base class), engine (DB connection), and SessionLocal (session factory).
- models.py → defines your User and Item tables.
- auth.py → handles password hashing and JWT token creation.

```python
app = FastAPI()
```

- Creates the FastAPI application instance. This is what uvicorn runs.

---

If we face any issue in bcrypt
```bash
pip uninstall bcrypt py-bcrypt # uninstall
pip install --upgrade passlib[bcrypt] # upgrade
pip show bcrypt # verify
pip install bcrypt==3.2.0 # if need to use specific version

python -m pytest test_main.py
python -m pytest -v
pip install pydantic-settings
pip install python-dotenv
```

FastAPI needs the python-multipart package whenever you declare a dependency like OAuth2PasswordRequestForm (because that dependency parses application/x-www-form-urlencoded form data). Without it, FastAPI can’t handle the form submission and raises that error at startup.


✅ What You Learned 

FastAPI basics: endpoints, dependency injection 
Async DB queries: using AsyncSession with SQLAlchemy 
JWT authentication: secure login with tokens
CRUD operations: create, read, update, delete items 

Login → returns a JWT token. 
JWT token → must be passed in the Authorization header as Bearer <token>. 
Protected routes → use Depends(get_current_user) to ensure only authenticated users can access them. 
Ownership check → users can only update/delete their own items. 

	⚠️ Role validation:  accepts any string. Use  or an Enum for stricter validation.
• 	⚠️ Inactive users: You correctly check  in , but ensure all endpoints reject inactive users consistently.
• 	⚠️ Hardcoded secret key:  should be loaded from environment variables.
• 	⚠️ Single refresh token per user: Storing one refresh token in the  table limits multiple sessions. Consider a separate  table for scalability.
• 	⚠️ Error codes: Login failures return . Conventionally, they should return .

2. Example Usage
- /api/users/{user_id}/reactivate (PUT) → Admin‑only, restores a deactivated account.
- /api/users/me (DELETE) → Marks the current user inactive.
- /api/users/{user_id} (DELETE) → Admin-only, marks any user inactive.
- /api/users → list all users (admin only).
- /api/users/{user_id}/role → update roles (admin only).
- /api/users/me → self‑profile (any logged‑in user).
- /api/users/me/password → change own password (any logged‑in user).

- Get all logs by a specific admin:
/api/audit-logs?actor_id=1
- Get all logs affecting a specific user:
/api/audit-logs?target_id=5
- Get all role changes:
/api/audit-logs?action=role_update
- Get logs in a date range:
/api/audit-logs?start_date=2026-03-01&end_date=2026-03-15
- Get first 20 logs:
/api/audit-logs?limit=20&offset=0
- Get next 20 logs:
/api/audit-logs?limit=20&offset=20
- Filter + paginate:
/api/audit-logs?action=deactivate&limit=10&offset=30
- Newest first (default):
/api/audit-logs?limit=20&sort_order=desc
- Oldest first:
/api/audit-logs?limit=20&sort_order=asc
- Filter + sort + paginate:
/api/audit-logs?action=role_update&limit=10&offset=30&sort_order=asc
- Export as JSON (default):
/api/audit-logs/export
- Export as CSV:
/api/audit-logs/export?format=csv
- Filtered export:
/api/audit-logs/export?action=deactivate&start_date=2026-03-01&end_date=2026-03-15&format=csv
