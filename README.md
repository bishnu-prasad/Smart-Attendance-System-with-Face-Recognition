
# Smart Attendance System with Face Recognition

**AI-powered attendance automation** — a full-stack project that detects & recognizes faces in real time and marks attendance in a backend database. Built to demonstrate real-time image processing, REST APIs, and a clean React dashboard.

> 🔎 Repo: `Smart-Attendance-System-with-Face-Recognition`  
> Tech stack: **Python (Django)**, **face_recognition / OpenCV**, **React.js**, **SQL (SQLite/MySQL)**

---

## 🚀 Project Overview

This project automates classroom attendance using face recognition.

- **Real-time face detection & recognition** from camera/video.
- **Django backend** for user/auth management and attendance records (REST APIs).
- **React frontend** dashboard for admins/teachers to view and export attendance.
- **Face training scripts** to add new students (build face encodings).

---

## ⭐ Features

- Real-time webcam face detection & recognition
- Register / train new students from images
- Store attendance logs in SQL database
- React dashboard: view/search/export attendance reports
- Simple REST APIs (Django REST Framework) for integration

---

## 🧰 Tech Stack

- Backend: Python, Django, Django REST Framework
- Face Recognition: `face_recognition` (uses dlib) and OpenCV
- Frontend: React.js (Create React App / Vite)
- Database: SQLite (default) / MySQL / PostgreSQL (optional)
- Deployment: Docker (optional)

---

## 📁 Repository Structure (example)

```

Smart-Attendance-System-with-Face-Recognition/
│
├─ backend/                   # Django project
│  ├─ manage.py
│  ├─ attendance\_app/
│  │  ├─ models.py
│  │  ├─ views.py
│  │  ├─ serializers.py
│  │  ├─ urls.py
│  └─ requirements.txt
│
├─ frontend/                  # React dashboard
│  ├─ package.json
│  └─ src/
│     ├─ components/
│     └─ App.js
│
├─ face\_recognition/          # scripts for training & recognition
│  ├─ train\_faces.py
│  ├─ recognize\_faces.py
│  └─ utils.py
│
├─ dataset/                   # folders with student images (used for training)
│  ├─ student\_name1/
│  │  ├─ img1.jpg
│  │  └─ img2.jpg
│  └─ student\_name2/
│
└─ README.md

````

---

## ⚙️ Prerequisites

- Python 3.8+  
- Node.js & npm (for frontend)  
- `pip` and virtualenv (recommended)  
- A webcam (for real-time demo)  
- For `face_recognition`:
  - `cmake`, `boost` & compiler toolchain (Linux/Mac), or Visual Studio Build Tools (Windows)
  - dlib (installed as dependency of `face_recognition`)

> If you want to avoid `dlib` installation issues, you can use OpenCV LBPH recognizer (less accurate but easier to install). See *Troubleshooting* below.

---

## 🔧 Backend Setup (Django)

1. Create & activate a Python virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate      # macOS / Linux
   venv\Scripts\activate         # Windows
````

2. Install backend dependencies:

   ```bash
   cd backend
   pip install -r requirements.txt
   ```

   Example `backend/requirements.txt`:

   ```
   Django>=4.2
   djangorestframework
   Pillow
   face_recognition
   opencv-python
   numpy
   ```

3. Configure DB (SQLite default). For MySQL/Postgres, update `backend/settings.py` `DATABASES` section.

4. Run migrations and create a superuser:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   python manage.py createsuperuser
   ```

5. Start Django server:

   ```bash
   python manage.py runserver
   ```

   Backend APIs available at `http://127.0.0.1:8000/api/`

---

## 🧠 Face Data: Training & Recognition

### Prepare dataset

Create `dataset/` and inside create a folder per student, e.g.:

```
dataset/
  ├─ john_doe/
  │   ├─ 1.jpg
  │   └─ 2.jpg
  └─ alice_s/
      ├─ 1.jpg
```

### Train encodings

Run the training script to compute and save encodings:

```bash
cd face_recognition
python train_faces.py
```

This produces a `face_encodings.pkl` (or similar) that `recognize_faces.py` uses.

### Real-time recognition

Run the recognition script (it will use your webcam):

```bash
python recognize_faces.py
```

When a face is recognized, the script can call your Django API endpoint (e.g. `POST /api/attendance/`) to mark the student present.

> In `recognize_faces.py` look for the section where you can add an API POST request (using `requests` library) to push attendance to the backend.

---

## 💻 Frontend (React) Setup

1. Install dependencies and start:

   ```bash
   cd frontend
   npm install
   npm start
   ```
2. The dashboard will open at `http://localhost:3000/`. Ensure it fetches data from your Django backend (`http://127.0.0.1:8000/api/attendance/`).

---

## 🔌 Example API Endpoints (Django)

* `GET /api/students/` — list students
* `GET /api/attendance/` — list attendance records
* `POST /api/attendance/` — add attendance (payload: `{ "student": <id>, "date": "YYYY-MM-DD", "status": "Present" }`)

Adjust paths based on your `attendance_app/urls.py` routing.

---

## 📸 Screenshots

Add screenshots to `README` (place images in `docs/` or `screenshots/` in repo):

```markdown
![Dashboard](screenshots/dashboard.png)
![Recognition Demo](screenshots/recognize.png)
```

---

## 🐛 Troubleshooting & Tips

### face\_recognition / dlib install errors

* On **Ubuntu**:

  ```bash
  sudo apt update
  sudo apt install build-essential cmake libopenblas-dev liblapack-dev libx11-dev libgtk-3-dev
  pip install dlib
  pip install face_recognition
  ```
* On **Windows**:

  * Install Visual Studio Build Tools
  * Consider using prebuilt wheels for `dlib` or use conda:

    ```bash
    conda create -n face python=3.8
    conda activate face
    conda install -c conda-forge dlib
    pip install face_recognition
    ```
* If `dlib` is too painful, use OpenCV LBPH face recognizer as fallback.

### Webcam not detected

* Make sure no other app uses the webcam.
* Try `cv2.VideoCapture(0)` vs `cv2.VideoCapture(1)` (different indices).

### API not marking attendance

* Ensure the recognition script uses the correct backend URL and that CORS is allowed on Django (install and configure `django-cors-headers` if React runs on different port).

---

## 🔒 Privacy & Ethics

* This project uses face recognition — only test with consented participants.
* Do not deploy to public-facing systems without clear privacy policies and legal compliance.

---

## ✅ Future Improvements (Ideas)

* Improve model accuracy & lighting robustness
* Bulk image upload & training via dashboard
* Auto-scheduler for multiple classes & time slots
* Deploy backend + frontend on cloud (Heroku/AWS) and store encodings in cloud storage
* Add role-based access (teacher/admin) and audit logs

---

## 📜 License

This project is released under the **MIT License** — see `LICENSE` for details.

---

## 🙋 Author

**Bishnuprasad Tripathy**

* GitHub: [https://github.com/bishnu-prasad](https://github.com/bishnu-prasad)
* Email: *[bishnuprasadtripathy.work@gmail.com](mailto:bishnuprasadtripathy.work@gmail.com)*

---
