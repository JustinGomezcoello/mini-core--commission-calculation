# MiniCore - Commission Calculation System

MiniCore is a web application built with FastAPI (backend) and React + Material UI (frontend), which allows you to automatically calculate and visualize sales commissions for each salesperson within a date range.
It is designed with a decoupled MVC architecture, allows easy integration of new commission rules, and can be automatically deployed on [Render.com](https://render.com) as a Web Service (backend) and Static Site (frontend).

Explanatory video link: https://www.loom.com/share/a7ff3dc56ec546d5973d86b7ff839058?sid=76100809-3a23-4713-ac4c-de58cb375793

## System Architecture
### Backend (Python - FastAPI)

- Framework: FastAPI 0.103.0
- Server: Uvicorn
- Validation: Pydantic 2.4.0
- Database: In-memory simulation (for the scope of this project)

### Frontend (React - JavaScript)

- Framework: React 18
- UI Library: Material-UI (MUI)
- HTTP Client: Axios
- Date Handling: date-fns

```bash
miniCoreComisiones/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py                 # FastAPI entry point
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── comisiones.py       # Commission endpoints
│   │   │   ├── usuarios.py         # Salesperson endpoints
│   │   │   └── ventas.py           # Sales endpoints
│   │   ├── core/
│   │   │   ├── __init__.py
│   │   │   └── database.py         # In-memory database simulation
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   ├── usuario.py          # Salesperson model
│   │   │   ├── venta.py            # Sale model
│   │   │   └── comision.py         # Commission models
│   │   ├── schemas/
│   │   │   ├── __init__.py
│   │   │   └── responses.py        # Response schemas
│   │   └── services/
│   │       ├── __init__.py
│   │       └── comision_service.py # Business logic
│   └── requirements.txt
└── front-react/
    ├── public/
    │   ├── index.html
    │   └── _redirects            # For routing on Render
    ├── src/
    │   ├── App.js               # Main component
    │   ├── index.js             # Entry point
    │   ├── theme.js             # Material-UI configuration
    │   ├── components/
    │   │   ├── FiltroFechas.jsx
    │   │   ├── Layout.jsx
    │   │   ├── TablaComisiones.jsx
    │   │   └── TarjetasInformativas.jsx
    │   ├── pages/
    │   │   ├── Home.jsx         # Main dashboard
    │   │   ├── Usuarios.jsx     # Salesperson management
    │   │   └── Ventas.jsx       # Sales listing
    │   └── services/
    │       └── api.js           # Axios configuration
    └── package.json         
```

### 1. Clone the GitHub repository
Import the project into your preferred IDE from the [repository](https://github.com/JustinGomezcoello/mini-core--commission-calculation)

### 2. Backend (FastAPI)
```bash
cd backend
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

The backend will be available at: http://localhost:8000

### 3. Frontend (React)
```bash
cd front-react
npm install
npm start 
```

The frontend will be available at: http://localhost:3000

## Deployment on Render

**Backend - Web Service**

- Type: Web Service
- Branch: master
- Root Directory: *`empty`*

Build Command:
- Go to [Render Dashboard](https://dashboard.render.com/)
- Click on **"New Web Service"**
- Connect your GitHub account and select the project repository.

### 3. Configure the Service

- **Build Command**: *`pip install -r backend/requirements.txt`*
- **Start Command**:  
  ```bash
  python -m uvicorn backend.app.main:app --host 0.0.0.0 --port $PORT
    ```
- **Python Version**: *`3.11.7 (environment variable: PYTHON_VERSION)`*

Exposed at: [https://minicore-fastapi-react.onrender.com](https://mini-core-comisiones.onrender.com)


**Frontend - Static Site**

- Type: Static Site
- Root Directory: *`front-react`*
- **Build Command**:  *`npm ci && npm run build`*
- **Publish Directory**: *`build`*
- **Environment**: *`REACT_APP_API_URL: https://mini-core-comisiones.onrender.com`*

Exposed at: [https://minicore-fastapi-react-1.onrender.com](https://mini-core-comisiones-front.onrender.com)
 
## App Usage

Enter a date range in the frontend form.

When you click "Calculate Commissions", a POST request is sent to the backend:

The backend queries all sales within the range and calculates commissions according to the rules set in the hardcoded tables.

The frontend renders the table with the results.

