# NeuroScanAI

React frontend, FastAPI backend, MySQL database, and local brain MRI analysis models.
For academic/research use.

## Run on another Windows PC

Install Python 3.10 (enable Add to PATH), Node.js 24.22.0 (includes npm), Git, and MySQL Server 8+. These match the source PC's Python/Node versions. React Router requires Node 20+.

1. Clone this repository and open PowerShell in its root folder.
2. Obtain the complete `backend/models` folder privately from the project owner, preserving all paths. Model weights are excluded from Git. Also copy `backend/model` if supplied. Without weights, analysis will fail.
3. To preserve existing accounts/reports, import the owner's private MySQL export into `tumer_db` with MySQL Workbench. Copy associated `backend/data`, `backend/uploads`, and root `uploads` folders if supplied. These contain private local data and are excluded from Git.
4. Create local environment files:

```powershell
Copy-Item backend\.env.example backend\.env
Copy-Item frontend\Tumer-Alzheimer-Detection\t-a-det\.env.example frontend\Tumer-Alzheimer-Detection\t-a-det\.env
notepad backend\.env
```

Set MySQL credentials for this PC and replace JWT_SECRET with a random secret.
For an empty installation, create the database first:

```sql
CREATE DATABASE tumer_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

5. Install dependencies from the repository root:

```powershell
python -m venv backend\venv
.\backend\venv\Scripts\python.exe -m pip install -r backend\requirements.txt
cd frontend\Tumer-Alzheimer-Detection\t-a-det
npm.cmd ci
cd ..\..\..
```

6. Initialize/update database tables:

```powershell
cd backend
.\venv\Scripts\python.exe create_tables.py
cd ..
```

This does not automatically create an administrator unless SUPERADMIN_EMAIL and SUPERADMIN_PASSWORD are configured. An imported database preserves its existing accounts.

7. Start both services:

```powershell
python start.py
```

Open http://localhost:3000. API documentation: http://localhost:8000/docs.
Keep the terminal open; press Ctrl+C to stop. The launcher clears processes on ports 3000 and 8000 before starting.
CPU inference is supported; CUDA is optional and requires a compatible PyTorch installation and driver.

## Future runs and updates

Run `python start.py` from the root. Use `git pull` to fetch code updates, then reinstall dependencies if requirements or lock files change.

## Excluded files

Secrets, virtual environments, node_modules, database exports, patient uploads/results, and model weights are intentionally excluded. Obtain required private assets directly from the owner. This repository alone does not reproduce existing database contents or include model weights.
