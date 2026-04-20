🏗️ SteelQuantityCalculation

> **חישוב אוטומטי של כמויות ברזל מתוך תוכניות קונסטרוקציה (DWG/DXF) בעזרת מודלי ML**

[![Status](https://img.shields.io/badge/status-in%20development-yellow)](.)
[![Stack](https://img.shields.io/badge/stack-React%20%7C%20FastAPI%20%7C%20Python-blue)](.)
[![ML](https://img.shields.io/badge/ML-Classification%20%7C%20Clustering%20%7C%20Regression-green)](.)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](.)

---

## 📌 הבעיה

מהנדסי ביצוע וקבלני בנייה משקיעים **שעות עבודה ידנית** בחישוב כמויות ברזל (מוטות, קטרים, אורכים, משקלים) מתוך תוכניות קונסטרוקציה — תהליך איטי, מועד לטעויות, ומעכב הזמנות חומרים והגשת תוכניות.

## 💡 הפתרון

`SteelQuantityCalculation` מאפשרת להעלות קובץ DWG/DXF של תוכנית ברזל ולקבל תוך שניות:
- ✅ טבלת כמויות ברזל מסודרת (קוטר, סוג, אורך, כמות, משקל)
- ✅ קיבוץ לפי אלמנט מבני (קורה / תקרה / עמוד / סמ"ר)
- ✅ השוואה לטבלת חישוב קיימת (Ground Truth Validation)
- ✅ ייצוא ל-Excel ו-PDF לפועלים ולספקים

---

## 👥 קהל יעד

| משתמש | צורך עיקרי |
|--------|------------|
| **מהנדס ביצוע / קונסטרוקציה** | חישוב מהיר ומדויק של כמויות לתוכניות |
| **קבלן בנייה** | הזמנת חומרים ללא טעויות, הצעות מחיר מיידיות |

---

## 🚀 User Stories

### US-01 · העלאת תשריט וקבלת טבלת ברזל
> *"כמהנדס קונסטרוקציה, אני רוצה להעלות תשריט תקרה ולקבל טבלת ברזל מסודרת תוך שניות — כדי לחסוך שעות חישוב ידני ולהגיש תוכניות מדויקות."*

### US-02 · טבלה לפי חתך להזמנת חומרים
> *"כקבלן בנייה, אני רוצה לקבל טבלה עם קוטר, סוג, אורך וכמות ברזל לכל חתך — כדי להוציא הזמנת חומרים מיידית לספק ללא טעויות."*

### US-03 · ייצוא ל-Excel ול-PDF
> *"כמהנדס בפרויקט גדול, אני רוצה לייצא את טבלת הברזל ל-Excel ול-PDF — כדי להעביר אותה ישירות לפועלים באתר ולספק החומרים."*

---

## 🤖 מודלי ML

```
DWG/DXF File
     │
     ▼
┌─────────────────┐
│  DXF Parser     │  ← ezdxf (Python)
│  (geometry)     │
└────────┬────────┘
         │
    ┌────┴─────┬──────────┐
    ▼          ▼          ▼
┌────────┐ ┌────────┐ ┌────────┐
│Classify│ │Cluster │ │Regress │
│ סוג   │ │ חתך   │ │אורך/  │
│ רכיב  │ │ מבני  │ │ משקל  │
└────┬───┘ └───┬────┘ └───┬────┘
     └─────────┼──────────┘
               ▼
       ┌───────────────┐
       │  Steel Table  │
       │  (JSON/UI)    │
       └───────────────┘
```

| מודל | אלגוריתם | תפקיד |
|------|----------|--------|
| **Classification** | Random Forest / CNN | זיהוי סוג הרכיב: מוט / רשת / עניבה / לולאה / בריח |
| **Clustering** | K-Means / DBSCAN | קיבוץ רכיבים לפי אלמנט מבני (קורה, תקרה, עמוד) |
| **Regression** | XGBoost / Linear | חיזוי אורך ומשקל לפי גיאומטריה וקוטר |

---

## 🛠️ Tech Stack

### Frontend
| כלי | שימוש |
|-----|--------|
| **React 18 + TypeScript** | ממשק משתמש |
| **Vite** | Build tool |
| **Zustand** | ניהול state |
| **dxf-viewer** | תצוגת תוכנית DXF בדפדפן (WebGL) |
| **AG Grid / TanStack Table** | טבלת ברזל אינטראקטיבית |
| **Recharts** | גרפי סיכום |

### Backend
| כלי | שימוש |
|-----|--------|
| **FastAPI (Python 3.11)** | REST API |
| **ezdxf** | ניתוח קבצי DXF |
| **LibreDWG / ODA** | המרת DWG → DXF |
| **scikit-learn** | מודלי ML (Classification, Clustering, Regression) |
| **PyTorch** | מודלים מתקדמים (CNN לזיהוי גיאומטריה) |
| **openpyxl** | ייצוא Excel |
| **WeasyPrint** | ייצוא PDF עם תמיכת RTL |

### Infrastructure
| כלי | שימוש |
|-----|--------|
| **PostgreSQL** | מסד נתונים ראשי |
| **AWS S3 / MinIO** | אחסון קבצי DWG/DXF |
| **Docker** | containerization |
| **Redis** | queue לעיבוד קבצים כבדים |

---

## 📁 מבנה הפרויקט

```
steel-calc/
├── frontend/                  # React + TypeScript
│   ├── src/
│   │   ├── components/
│   │   │   ├── FileUpload/    # Drag & Drop + progress
│   │   │   ├── SteelTable/    # טבלה אינטראקטיבית
│   │   │   ├── DXFViewer/     # WebGL viewer
│   │   │   ├── ExportPanel/   # Excel / PDF export
│   │   │   └── Sidebar/       # ניהול פרויקטים
│   │   ├── pages/
│   │   │   ├── Dashboard.tsx
│   │   │   ├── Project.tsx
│   │   │   └── Upload.tsx
│   │   ├── store/             # Zustand state management
│   │   ├── api/               # Axios API calls
│   │   └── types/             # TypeScript interfaces
│   └── package.json
│
├── backend/                   # FastAPI
│   ├── api/
│   │   ├── routes/
│   │   │   ├── files.py       # Upload / download endpoints
│   │   │   ├── analysis.py    # ML pipeline trigger
│   │   │   └── export.py      # Excel / PDF generation
│   │   └── schemas/           # Pydantic models
│   ├── core/
│   │   ├── dxf_parser.py      # ezdxf wrapper
│   │   ├── dwg_converter.py   # DWG → DXF
│   │   └── steel_calc.py      # חישובי משקל וכמות
│   ├── ml/
│   │   ├── classifier.py      # Classification model
│   │   ├── clusterer.py       # Clustering model
│   │   └── regressor.py       # Regression model
│   ├── main.py
│   └── requirements.txt
│
├── ml_training/               # Notebooks לאימון
│   ├── data/                  # קבצי DXF לדוגמה + ground truth
│   ├── notebooks/
│   │   ├── 01_data_exploration.ipynb
│   │   ├── 02_classification.ipynb
│   │   ├── 03_clustering.ipynb
│   │   └── 04_regression.ipynb
│   └── models/                # Saved model weights
│
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## ⚡ הרצה מקומית

### דרישות מוקדמות
- Python 3.11+
- Node.js 20+
- Docker + Docker Compose (אופציונלי)

### Frontend
```bash
cd frontend
npm install
npm run dev
# → http://localhost:5173
```

### Backend
```bash
cd backend
python -m venv venv
venv\Scripts\activate          # Windows
pip install -r requirements.txt
uvicorn main:app --reload
# → http://localhost:8000
# Docs → http://localhost:8000/docs
```

### עם Docker
```bash
docker-compose up --build
```

---

## 📊 API Endpoints

| Method | Endpoint | תיאור |
|--------|----------|--------|
| `POST` | `/api/files/upload` | העלאת קובץ DWG/DXF |
| `GET` | `/api/files/{id}/status` | סטטוס עיבוד |
| `GET` | `/api/analysis/{id}` | תוצאות טבלת הברזל |
| `POST` | `/api/analysis/{id}/feedback` | תיקון ידני (feedback loop) |
| `GET` | `/api/export/{id}/excel` | ייצוא Excel |
| `GET` | `/api/export/{id}/pdf` | ייצוא PDF |
| `GET` | `/api/projects` | רשימת פרויקטים |
| `POST` | `/api/projects` | יצירת פרויקט חדש |

---

## 📋 Sprint Roadmap

```
Sprint 1 (NOW)     →  Frontend MVP + Mock Data
Sprint 2 (NEXT)    →  Backend + DXF Parser + ML Pipeline
Sprint 3           →  Real Export + DXF Viewer + Ground Truth
Sprint 4           →  ML Feedback Loop + Templates + Cloud
```

---

## 📐 נוסחאות חישוב

```
משקל מוט = אורך [מ'] × (π/4) × קוטר² [מ"מ²] × 7850 [ק"ג/מ³]

ווים לפי תקן IS 466:
  • קוטר ≤ 16 מ"מ  → hook = 6d
  • קוטר > 16 מ"מ  → hook = 12d

חפיפה מינימלית:
  • FeB400: 40d
  • FeB500: 50d
```

---

## 🎯 מדדי הצלחה (KPIs)

| מדד | יעד |
|-----|-----|
| דיוק Classification | ≥ 85% |
| שגיאת Regression (vs Ground Truth) | ≤ 5% |
| זמן עיבוד קובץ ≤ 10MB | ≤ 15 שניות |
| זמן ייצוא Excel/PDF | ≤ 5 שניות |

---

## 👤 תרומה לפרויקט

```bash
git checkout -b feature/my-feature
git commit -m "feat: add my feature"
git push origin feature/my-feature
# → פתח Pull Request
```

---

## 📄 רישיון

MIT License © 2026 SteelQuantityCalculation Team
