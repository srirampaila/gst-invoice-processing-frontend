# GST Invoice Processing — Frontend Dashboard

[![React](https://img.shields.io/badge/React-18.3.1-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-6.0-646CFF?logo=vite&logoColor=white)](https://vite.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.4-38B2AC?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Vercel Deployed](https://img.shields.io/badge/Deployed-Vercel-black?logo=vercel&logoColor=white)](https://micro-gst-saas-ivzvy9erb-sriram-paila.vercel.app)
[![License](https://img.shields.io/badge/License-Academic_Research-blue.svg)](#license)

> **Live Production Website:**  
> 🔗 [https://micro-gst-saas-ivzvy9erb-sriram-paila.vercel.app](https://micro-gst-saas-ivzvy9erb-sriram-paila.vercel.app)  
> *(Secondary alias: [https://micro-gst-saas-sriram-paila.vercel.app](https://micro-gst-saas-sriram-paila.vercel.app))*

---

## Related Repositories

| Repository | Description | Link |
| :--- | :--- | :--- |
| **Complete System** | Full-stack monorepo with backend, frontend, docs, and benchmarks | [![GitHub](https://img.shields.io/badge/GitHub-gst--invoice--processing--system-181717?logo=github)](https://github.com/srirampaila/gst-invoice-processing-system) |
| **Backend API** | FastAPI, OpenCV, Tesseract OCR, Ollama LLM, and Deterministic Engine | [![GitHub](https://img.shields.io/badge/GitHub-gst--invoice--processing--backend-181717?logo=github)](https://github.com/srirampaila/gst-invoice-processing-backend) |
| **Frontend UI** | React 18, TypeScript, Vite, Tailwind CSS SPA *(This Repository)* | [![GitHub](https://img.shields.io/badge/GitHub-gst--invoice--processing--frontend-181717?logo=github)](https://github.com/srirampaila/gst-invoice-processing-frontend) |

---

## Overview

This repository contains the standalone **React 18 + TypeScript + Vite + Tailwind CSS** frontend dashboard for the **AI-Powered GST Invoice Processing and GSTR-1 Draft Generation System**.

The user interface serves as the operator cockpit and visual inspection portal for accountants, tax practitioners, and auditors. It provides an intuitive, high-performance, single-page application (SPA) with zero external UI bloat, implementing real-time invoice processing, confidence inspection, human-in-the-loop review queues, and statutory GSTR-1 draft visualization.

---

## Key Features & UI Modules

### 1. Executive Overview (`ExecutiveOverview.tsx`)
- System health dashboard showing pipeline status, processing throughput, pass rates, and latency metrics.
- High-level KPIs: Invoices processed, GSTR-1 ready percentage, review queue volume, and tax discrepancy alerts.

### 2. Interactive Invoice Studio (`InvoiceStudio.tsx`)
- **Drag-and-Drop File Upload:** Supports PDF, PNG, JPG, and TIFF tax invoices with client-side mime validation.
- **Dynamic Sample Picker:** Allows instant testing of clean, noisy, skewed, multi-item, and synthetic invalid invoices without local file uploads.
- **Empty State UX:** Informative zero-state guidance preventing blind 404 network calls on cold startup.
- **Real-Time Processing Trigger:** Dispatches invoices through the full OCR $\rightarrow$ LLM $\rightarrow$ Validation $\rightarrow$ GSTR-1 pipeline with live progress telemetry.

### 3. Extraction Inspector (`ExtractionInspector.tsx`)
- Dual-pane side-by-side comparison between raw OCR text / tokens and canonical Pydantic v2 structured fields.
- Visual inspection of extracted vendor/buyer GSTINs, invoice numbers, tax dates, line items, and computed HSN summaries.

### 4. Statutory GST Validation View (`GSTValidationView.tsx`)
- Real-time audit view of 6 statutory rule engines:
  - `GSTIN-001`: 15-character statutory regex format and state code lookup.
  - `GSTIN-002`: Offline Luhn Modulo-36 checksum verification.
  - `TAX-001`: Inter-state (IGST) vs. Intra-state (CGST + SGST) tax classification consistency.
  - `ARITH-001`: Line-item quantity $\times$ unit price $=$ taxable value check.
  - `ARITH-002`: Line-item tax rate summation.
  - `TOTAL-001`: Mandatory invoice grand total reconciliation ($\le ₹0.05$ rounding tolerance).

### 5. Human-in-the-Loop Confidence Queue (`ConfidenceReviewView.tsx`)
- Visual queue for invoices routed by the confidence engine (`score < 0.85` or safety overrides triggered).
- Displays multi-factor confidence breakdown across 6 scoring dimensions.
- Operator actions: **Approve Draft**, **Reject Invoice**, or **Request Reprocessing** with audit logging and rationale persistence.

### 6. GSTR-1 Draft Preparation & Export View (`GSTR1DraftView.tsx`)
- Real-time statutory table routing:
  - **Table 4A (B2B):** Invoices with verified buyer GSTINs for input tax credit (ITC).
  - **Table 7 (B2CS):** Consumer and unregistered buyer invoices aggregated by POS and tax rate.
- One-click triggers for multi-sheet Excel workbook (`gstr1_draft.xlsx`) and CSV portal exports.

### 7. Performance & Architecture Dashboards
- `PerformanceProfilingView.tsx`: Latency breakdowns per pipeline stage and OCR error rate distributions.
- `TestingEvaluationView.tsx`: Automated test suite results and regression matrix.
- `ArchitectureView.tsx`: Interactive system diagrams and design specifications.

---

## Technology Stack

| Layer | Technology | Version | Purpose |
| :--- | :--- | :--- | :--- |
| **Framework** | React | `18.3.1` | Component lifecycle and reactive UI state |
| **Language** | TypeScript | `~5.6.2` | Compile-time strict typing and schema safety |
| **Build Tool** | Vite | `^6.0.1` | Ultra-fast HMR and optimized production bundling |
| **Styling** | Tailwind CSS | `^3.4.16` | Utility-first responsive design system |
| **Icons** | Lucide React | `^0.475.0` | Accessible, modern feather iconography |
| **Charts** | Recharts | `^2.15.0` | SVG-based performance and confidence visualizations |
| **Routing** | React Router DOM | `^6.28.0` | Client-side SPA navigation |
| **Deployment** | Vercel | Global CDN | Production hosting with edge rewrites |

---

## Directory Structure

```
gst-invoice-processing-frontend/
├── index.html                   # HTML5 shell with Google Inter font
├── package.json                 # Frontend dependencies & npm scripts
├── package-lock.json            # Deterministic dependency lockfile
├── postcss.config.js            # PostCSS configuration for Tailwind
├── tailwind.config.js           # Custom design tokens, colors & typography
├── tsconfig.json                # TypeScript project references
├── tsconfig.app.json            # Application TS compiler configuration
├── tsconfig.node.json           # Node/Vite build TS configuration
├── vercel.json                  # Vercel deployment rewrites & SPA routing
├── vite.config.ts               # Vite configuration with /api proxy
├── .env.example                 # Example environment variables
├── .gitignore                   # Ignored files (node_modules, dist, etc.)
└── src/
    ├── main.tsx                 # Application entrypoint
    ├── vite-env.d.ts            # Vite client environment type definitions
    ├── app/
    │   └── App.tsx              # Root component & navigation controller
    ├── components/
    │   ├── common/              # Reusable UI primitives (Button, Card, Badge, Modal, Tabs)
    │   └── layout/              # Layout shell (AppShell, Navbar, Footer)
    ├── features/
    │   ├── architecture/        # Architecture documentation view
    │   ├── evaluation/          # Benchmark & test evaluation views
    │   ├── extraction/          # OCR & LLM extraction inspector
    │   ├── gstr1/               # GSTR-1 draft tables & export triggers
    │   ├── invoices/            # Invoice upload, sample picker & studio
    │   ├── overview/            # System health & executive KPI dashboard
    │   ├── review/              # HITL confidence review queue
    │   └── validation/          # Statutory GST rule inspection
    ├── lib/
    │   ├── api.ts               # Resilient API client with fallback endpoints
    │   └── utils.ts             # Currency, date, and class merging helpers
    ├── styles/
    │   └── globals.css          # Tailwind directives and CSS variables
    └── types/
        └── index.ts             # TypeScript definitions for CanonicalInvoice & API
```

---

## Prerequisites

Before running the frontend locally, ensure you have:
- **Node.js:** `v18.0.0` or higher (tested on `v20.x` and `v22.x`)
- **npm:** `v9.0.0` or higher (or `pnpm` / `yarn`)

Verify your installation:
```bash
node -v
npm -v
```

---

## Installation & Local Setup

### 1. Clone the Repository
```bash
git clone https://github.com/srirampaila/gst-invoice-processing-frontend.git
cd gst-invoice-processing-frontend
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Configure Environment (Optional)
By default, the Vite dev server automatically proxies all `/api/*` calls to the local FastAPI backend at `http://127.0.0.1:8000`.

To point to a remote or deployed backend instead:
```bash
cp .env.example .env
```
Edit `.env`:
```env
VITE_API_BASE_URL=https://your-backend-api.onrender.com
```

### 4. Start Development Server
```bash
npm run dev
```
The application will start with Instant HMR at:
```
  VITE v6.0.1  ready in 180 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```
Open `http://localhost:5173` in your browser.

---

## Production Build & Vercel Deployment

### 1. Type-Check and Build Locally
```bash
npm run build
```
This runs `tsc -b` to enforce zero TypeScript errors and compiles the production bundle into `dist/`.
```bash
npm run preview
```
Previews the optimized production build at `http://localhost:4173`.

### 2. Deploy to Vercel

This repository includes a production-ready `vercel.json` preconfigured with:
1. **API Rewrites:** Routes `/api/:path*` to the configured backend URL.
2. **SPA Routing:** Rewrites all other paths to `/index.html` to prevent 404 errors on browser page reloads.

#### Deploy via Vercel CLI:
```bash
npm install -g vercel
vercel
```

#### Deploy via GitHub Integration:
1. Import `srirampaila/gst-invoice-processing-frontend` into [Vercel](https://vercel.com/new).
2. Framework Preset: **Vite**
3. Root Directory: `./`
4. Build Command: `npm run build`
5. Output Directory: `dist`
6. (Optional) Set Environment Variable: `VITE_API_BASE_URL` with your backend URL.
7. Click **Deploy**.

---

## License & Statutory Disclaimer

- **License:** Educational and Academic Research License.
- **Statutory Notice:** This software is an academic prototype and automated draft preparation assistant. It does not file returns autonomously with the Goods and Services Tax Network (GSTN), does not connect to live government tax filing APIs, and does not provide legal or financial tax advisory. All generated GSTR-1 drafts must undergo mandatory verification by an authorized signatory before submission on the GST portal.
