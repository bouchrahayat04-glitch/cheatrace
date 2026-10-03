Cheatrace — ENSAJ Digital Trust Platform

Advanced forensic system for detecting academic fraud (AI & plagiarism) using CrewAI, Google Gemini (AI Studio), and n8n.

🚀 Features
Modern Dashboard: Responsive dark-mode interface built with TailwindCSS.
Role Management: Differentiated access for Administrators and Professors.
Forensic Pipeline:
OCR using Google Gemini 1.5 Flash for high-accuracy text extraction.
Analysis performed by specialized AI agents (NLP Expert, Forensic Judge).
n8n Integration: External workflow for asynchronous assignment processing.
Automated Reports: Generation of detailed Markdown reports with a risk score.
🛠️ Installation — From A to Z
1. Clone the Project
cd cheatrace
2. Create a Virtual Environment
python -m venv env

Activate on Windows:

.\env\Scripts\activate

Activate on Linux/Mac:

source env/bin/activate
3. Install Dependencies
pip install -r requirements.txt
4. Configuration (.env)

Create a .env file at the project root and configure your API key and database:

GEMINI_API_KEY="your_gemini_api_key_here"

DATABASE_URL="postgresql://postgres:your_password@localhost:5432/forensics_db"

SECRET_KEY="your_random_secret_key"
5. External Storage

Create the following directory on your C: drive for storing examination copies:

C:\Forensic_Storage\
⚙️ n8n Configuration

The platform delegates the analysis process to n8n.

1. Webhook URL

By default, the project uses:

http://localhost:5678/webhook/upload-copies
2. Callback

At the end of the workflow, your n8n workflow must send the results back through an HTTP Request node.

Method:

POST

URL:

http://localhost:5001/api/investigation/callback

JSON Body:

{
  "investigation_id": "{{ $node.Webhook.json.investigation_id }}",
  "status": "completed",
  "report_content": "{{ $node.CrewAI.json.output }}"
}
🏁 Running the Application

Simply run the automatic startup script:

python run.py

The script will create the database and default accounts on the first launch.

Access

URL:

http://localhost:5001

Admin account:

admin / admin123

Professor account:

prof_demo / prof123
📁 Project Architecture
cheatrace/
│
├── run.py
│   └── Startup script and database initialization
│
├── web/
│   └── Flask application
│       ├── Models
│       ├── Routes
│       └── Templates
│
├── crewai/
│   └── AI agent logic and OCR scripts
│
├── data/
│   └── Uploaded copies and generated reports
│
└── n8n/
    └── n8n workflow JSON for import
🧪 Testing

To verify the Flask ↔ n8n communication without running a real workflow:

python seed_test.py

This prepares an investigation in the database.

Then:

python test_n8n.py

This simulates an n8n response.

🎓 Project

Developed for ENSAJ — 2026