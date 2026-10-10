# ♻️ EcoTrack: MERN Smart Waste Management System

A full-stack **MERN (MongoDB, Express.js, React.js, Node.js)** smart waste management application designed to report, triage, assign, track, and verify community garbage collections in real time.

---

## 🎯 Objectives & Features Delivered

- ✅ **MERN Stack Architecture:** Decoupled React frontend (Vite) and Node.js/Express REST backend.
- ✅ **MongoDB CRUD Operations:** Full create, read, update, status-patch, and delete pipelines using Mongoose.
- ✅ **Express.js REST APIs:** Clean modular controllers, JSON endpoints, static file uploads, and centralized error handling.
- ✅ **React.js Interface:** Modern responsive UI with real-time KPI metrics, interactive map links, and timeline tracking.
- ✅ **Authentication & Role-Based Access Control (RBAC):**
  - **Citizen:** Report waste incidents, attach photos, pinpoint GPS coordinates, view live tracking status.
  - **Sanitation Worker:** View assigned route pickups, toggle "In Progress" dispatch, mark "Collected", and add resolution notes.
  - **Municipal Admin:** Full control room dashboard, assign workers, view waste type charts, update priority, remove invalid reports.
- ✅ **Rich Incident Record Management:**
  - **Location:** Street address, landmark, city, and GPS coordinates (Latitude & Longitude) with auto-detect button.
  - **Waste Type:** Plastic, Organic/Biodegradable, Dry Recyclable, E-Waste, Hazardous/Chemical, Construction Debris, Mixed Waste.
  - **Evidence Images:** Multi-part file upload support (Multer) or web image URLs with live image preview.
  - **Audit Timeline:** Historical event log tracing each transition (Reported ➔ Assigned ➔ In Progress ➔ Collected).

---

## 🏗️ Project Architecture

```
smart-waste-management/
├── package.json               # Root scripts (run client, server, seeder)
├── README.md                  # Comprehensive documentation
├── server/                    # Node.js + Express + MongoDB Backend
│   ├── package.json           # Backend dependencies (express, mongoose, bcryptjs, jwt, multer)
│   ├── .env                   # Environment variables (PORT, MONGO_URI, JWT_SECRET)
│   ├── server.js              # Express server entry point, CORS, routes mounting
│   ├── config/
│   │   └── db.js              # Mongoose database connection handler
│   ├── models/
│   │   ├── User.js            # User schema (Citizen, Worker, Admin)
│   │   └── WasteReport.js     # Waste report schema with coordinates, image, status, timeline
│   ├── controllers/
│   │   ├── authController.js  # Register, login, profile, worker listing
│   │   └── wasteReportController.js # CRUD endpoints, status patching, statistics aggregation
│   ├── routes/
│   │   ├── authRoutes.js      # /api/auth routes
│   │   └── wasteRoutes.js     # /api/waste-reports routes
│   ├── middleware/
│   │   ├── authMiddleware.js  # JWT authentication and RBAC checks
│   │   ├── uploadMiddleware.js# Multer photo upload handling
│   │   └── errorMiddleware.js # 404 and global error formatting
│   ├── uploads/               # Directory storing uploaded photos
│   └── seed.js                # Database seeder with sample accounts & reports
│
└── client/                    # React 18 + Vite Frontend
    ├── package.json           # Frontend dependencies (react, react-router-dom, vite)
    ├── vite.config.js         # Dev server with proxy to backend /api and /uploads
    ├── index.html             # HTML entry point with fonts & meta
    └── src/
        ├── main.jsx           # React DOM root
        ├── App.jsx            # React Router route tree
        ├── index.css          # Modern eco design system & stylesheet
        ├── context/
        │   └── AuthContext.jsx# Auth state and 1-click role switcher
        ├── services/
        │   └── api.js         # API client with Express REST proxy & offline fallback
        ├── components/
        │   ├── Navbar.jsx     # Navigation, status pill, role switcher
        │   ├── Footer.jsx     # Footer with architecture tags
        │   ├── StatusBadge.jsx# Color-coded status badge
        │   ├── WasteTypeBadge.jsx # Waste type categories & icons
        │   ├── StatCard.jsx   # KPI metric card
        │   ├── ReportCard.jsx # Waste incident card with map link
        │   ├── ReportDetailModal.jsx # Full timeline & map dialog
        │   └── StatusModal.jsx# Worker/Admin status update & assignment modal
        └── pages/
            ├── Home.jsx       # Landing page with KPIs, workflow guide, recent feed
            ├── ReportWaste.jsx# Form with GPS auto-detect and photo preview
            ├── TrackReports.jsx # Public tracker with search & filters (grid/table)
            ├── WorkerDashboard.jsx # Sanitation worker field task portal
            ├── AdminDashboard.jsx  # Municipal control room & analytics
            ├── Login.jsx      # Login page with 1-click demo credentials
            └── Register.jsx   # User registration with role selection
```

---

## ⚡ Quick Start Guide

### 1. Prerequisites
- **Node.js** (v18 or higher)
- **MongoDB** (Local instance on `mongodb://127.0.0.1:27017` or a MongoDB Atlas URI)

### 2. Setting Up Backend (`server`)

Open a terminal and navigate to the `server` folder:

```bash
cd server
npm install
```

Ensure your `.env` configuration is configured (default provided in `server/.env`):
```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://127.0.0.1:27017/smart_waste_db
JWT_SECRET=smart_waste_super_secret_jwt_key_2026_xyz
```

**(Optional) Seed Demo Data:**
Populate MongoDB with demo users and realistic garbage reports:
```bash
npm run seed
```

Start the backend server:
```bash
npm run dev
# or: npm start
```
The API server will run at: `http://localhost:5000`  
Health check: `http://localhost:5000/api/health`

---

### 3. Setting Up Frontend (`client`)

Open a second terminal and navigate to the `client` folder:

```bash
cd client
npm install
npm run dev
```

Open your browser at: **`http://localhost:5173`**

> **💡 Smart Offline / Demo Mode:**  
> The React client comes with a built-in mock fallback in `src/services/api.js`. If you test the frontend before starting MongoDB or the Express server, the application operates in **Demo Mode** with persistent local data so you can immediately interact with every feature (reporting, GPS detection, filtering, status transitions, role switching). Once the Express backend is running, it connects to MongoDB automatically!

---

## 🔑 Demo Login Credentials

The seeder creates 3 ready-to-test accounts (you can also use the 1-click role switcher in the Navbar):

| Role | Email | Password | Access Capabilities |
| :--- | :--- | :--- | :--- |
| **Municipal Admin** | `admin@wastewise.com` | `password123` | Full dashboard, assign workers, view analytics, delete reports |
| **Sanitation Worker** | `worker@wastewise.com` | `password123` | Field tasks, mark In Progress, mark Collected, upload cleanup notes |
| **Citizen Reporter** | `citizen@wastewise.com` | `password123` | Report waste, auto-detect GPS, track progress |

---

## 📡 REST API Reference

### Authentication (`/api/auth`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register new user account | Public |
| `POST` | `/api/auth/login` | Authenticate user & get JWT token | Public |
| `GET` | `/api/auth/me` | Fetch currently logged-in user profile | Bearer Token |
| `GET` | `/api/auth/workers` | Retrieve list of sanitation workers | Admin only |

### Waste Incident Reports (`/api/waste-reports`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/waste-reports` | Get reports (search, status, wasteType, priority filters) | Public |
| `GET` | `/api/waste-reports/stats/overview`| Summary metrics (total, pending, collected, type breakdown) | Public |
| `POST` | `/api/waste-reports` | Submit new waste incident (supports `multipart/form-data`) | Public / Auth |
| `GET` | `/api/waste-reports/:id` | Get single incident with full audit timeline | Public |
| `PUT` | `/api/waste-reports/:id` | Update report details (title, address, description) | Owner / Admin |
| `PATCH`| `/api/waste-reports/:id/status`| Update status (`Reported`, `Assigned`, `In Progress`, `Collected`), worker assignment, resolution notes | Worker / Admin |
| `DELETE`| `/api/waste-reports/:id` | Permanently delete report | Admin only |
| `GET` | `/api/waste-reports/user/my-reports` | List reports submitted by logged-in citizen | Authenticated |
| `GET` | `/api/waste-reports/worker/assigned` | List tasks assigned to logged-in sanitation worker | Worker / Admin |

---

## 🧪 Demonstration Steps

1. **Submit a Report as Citizen:**
   - Go to **Report Garbage** (`/report`).
   - Click **"📍 Use My GPS Location"** to automatically grab current coordinates.
   - Select waste category (e.g. *Plastic Waste* or *E-Waste*), set priority to *High*.
   - Upload a photo or use the sample image preview.
   - Click **Submit Garbage Report**.
2. **Track the Incident:**
   - Go to **Live Tracker** (`/track`).
   - Use search or filter by status (*Reported*) to locate the new report.
   - Click **Details & Timeline** to inspect the audit log.
3. **Dispatch & Pickup as Sanitation Worker:**
   - Switch role to **Worker (Marcus)** via the Navbar.
   - Navigate to **Worker Tasks** (`/worker`).
   - Click **Mark In Progress** when dispatching the truck.
   - Click **Complete Collection**, write resolution notes (e.g. *"Cleaned 200kg of plastic and disinfected site"*), and click **Save & Update**.
4. **Inspect Municipal Command Center as Admin:**
   - Switch role to **Admin (Sarah)** via the Navbar.
   - Navigate to **Admin Portal** (`/admin`).
   - Observe live updated KPIs, resolution rate %, and waste distribution graphs.
