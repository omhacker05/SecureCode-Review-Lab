# 🔐 SecureCode Review Lab

**An Educational Application Security Audit & Remediation Platform**

![Python](https://img.shields.io/badge/Python-3.11+-blue)
![Flask](https://img.shields.io/badge/Flask-3.1.2-green)
![License](https://img.shields.io/badge/License-Educational-orange)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

SecureCode Review Lab is a **local-first Flask/SQLite application** designed for authorized source code security reviews and remediation. It provides an intuitive dashboard for managing security audits, tracking vulnerabilities, and monitoring remediation progress.

---

## ✨ Key Features

### 📊 Dashboard & Reporting
- Real-time security finding summary and severity distribution
- Interactive project management interface
- Comprehensive PDF and HTML audit reports
- Finding history and retest tracking

### 📁 Project Management
- Safe ZIP file import with validation
- Path traversal and decompression size checks
- File extension and upload limits
- Untrusted source code handling

### 🔍 Security Scanning
- **Built-in Python checks** for:
  - Dynamic code execution (`eval`, `exec`, `compile`)
  - SQL injection patterns
  - Unsafe pickle deserialization
  - Shell command execution
  - Weak cryptographic hashes
  - Hardcoded credentials
  - Disabled TLS verification
  - Debug configurations
  - Temporary file handling

- **Optional Bandit integration** for comprehensive Python security analysis
- Status tracking (pending, validated, false-positive, remediated)

### 📝 Findings Management
- Severity and confidence levels
- CWE classification
- Evidence tracking and remediation guidance
- Owner assignment and due dates
- Status history and audit trail
- Finding retest capability

### 🛠️ Technical Features
- SQLite database with foreign keys and integrity checks
- Parameterized SQL statements (SQL injection prevention)
- HTML output escaping
- Basic security headers
- RESTful health and project endpoints
- Deterministic unit tests

---

## 🚀 Quick Start

### Windows (Automated)
```powershell
# 1. Extract the ZIP
# 2. Open PowerShell in the SecureCode-Review-Lab folder
# 3. (Optional) Allow script execution for this session
Set-ExecutionPolicy -Scope Process Bypass

# 4. Run the automated setup script
.\RUN_WINDOWS.ps1

# 5. Open browser
Start-Process http://127.0.0.1:5000
```

### Manual Setup (Windows/Linux/Mac)
```bash
# 1. Clone or extract the repository
cd SecureCode-Review-Lab

# 2. Create virtual environment
python -m venv .venv

# 3. Activate virtual environment
# On Windows:
.\.venv\Scripts\Activate.ps1
# On Linux/Mac:
source .venv/bin/activate

# 4. Install dependencies
python -m pip install --upgrade pip
python -m pip install -r requirements.txt

# 5. (Optional) Install security scanners
python -m pip install bandit pip-audit

# 6. Initialize database
python -c "from app.web import create_app; create_app(); print('Database initialized')"

# 7. Run the application
python run.py

# 8. Access the application
# Open http://127.0.0.1:5000 in your browser
```

---

## 📦 Requirements

- **Python**: 3.11+ (3.12 recommended)
- **Core Dependencies**:
  - Flask 3.1.2 - Web framework
  - ReportLab 4.4.4 - PDF generation
  - pytest 8.4.2 - Testing framework

- **Optional Dependencies**:
  - Bandit - Python AST-based security scanner
  - pip-audit - Dependency vulnerability checker
  - Semgrep - Pattern-based SAST tool

---

## 🧪 Testing

Run the test suite:
```bash
# Activate virtual environment first
pytest -q

# Or with verbose output
pytest -v

# Run specific test file
pytest tests/test_app.py -v
```

The project includes deterministic unit tests for:
- Flask route handling
- Database operations
- Upload validation
- Scan functionality
- Report generation

---

## 📁 Project Structure

```
SecureCode-Review-Lab/
├── app/
│   ├── __init__.py           # Flask app factory
│   ├── web.py                # Main Flask routes & logic
│   ├── services/
│   │   ├── scanner.py        # Security scanning engine
│   │   └── reports.py        # PDF & HTML report generation
│   ├── templates/            # Server-rendered HTML pages
│   │   ├── base.html
│   │   ├── dashboard.html
│   │   ├── projects.html
│   │   ├── project_detail.html
│   │   ├── findings.html
│   │   ├── finding_detail.html
│   │   ├── remediation.html
│   │   ├── reports.html
│   │   ├── retests.html
│   │   ├── scan_detail.html
│   │   ├── history.html
│   │   ├── dependencies.html
│   │   ├── rules.html
│   │   ├── settings.html
│   │   └── help.html
│   └── static/
│       ├── css/              # Dashboard styling
│       └── js/               # Client-side functionality
├── docs/
│   ├── ARCHITECTURE.md       # System design & data model
│   ├── SECURITY_RULES.md     # Built-in rule documentation
│   ├── QA_REPORT.md          # Quality assurance notes
│   └── assets/               # Screenshots & diagrams
├── sample_projects/
│   ├── vulnerable_python/    # Example vulnerable code
│   ├── vulnerable_python.zip
│   ├── fixed_python/         # Fixed version of examples
│   ├── fixed_python.zip
│   └── README.md
├── tests/                    # Unit tests
│   ├── conftest.py
│   └── test_app.py
├── instance/                 # SQLite database (created at runtime)
├── reports/                  # Generated audit reports (PDF/HTML)
├── requirements.txt          # Python dependencies
├── run.py                    # Application entry point
├── INSTALL.md                # Detailed installation guide
└── RUN_WINDOWS.ps1          # Windows automated setup script
```

---

## 🔒 Security Considerations

### Important Limitations
⚠️ **This is an educational local application, NOT suitable for production deployment without significant modifications.**

### Design Principles
- **Local-Only Deployment**: Bound to `127.0.0.1` by default
- **Untrusted Input Handling**: Source code is never executed
- **Input Validation**: Strict ZIP upload checks (size, paths, entries)
- **Output Encoding**: HTML escaped in all reports
- **SQL Safety**: Parameterized statements throughout
- **No Authentication**: Not implemented (local-only assumption)
- **No CSRF Protection**: Not implemented (local-only assumption)

### Upload Limits
- Maximum compressed size: 10 MB
- Maximum decompressed size: 30 MB
- Maximum entries: 500
- Maximum file size: 1 MB per file
- Blocked patterns: Path traversal (`..`), system files

### Coverage Limitations
- **Python-focused**: Most comprehensive rules target Python
- **Limited for other languages**: JavaScript, TypeScript, Java, PHP can be imported but not fully scanned
- **Not exhaustive**: Built-in rules intentionally limited
- **No certification**: Results are potential findings, not guaranteed vulnerabilities

### Recommended Practices
✅ Use only on source code you are authorized to assess  
✅ Keep the app bound to 127.0.0.1  
✅ Run in isolated network environment  
✅ Review findings with security expertise  
✅ Use as complement to, not replacement for, professional code review  
✅ Implement additional scanners (Bandit, Semgrep) for comprehensive coverage  

---

## 🎯 Workflow Example

1. **Create Project** - Set up a new audit project
2. **Upload Source Code** - Import ZIP with Python/other code
3. **Run Scan** - Execute built-in and optional security checks
4. **Review Findings** - Examine vulnerabilities by severity
5. **Add Remediation** - Document fixes and assign owners
6. **Retest** - Verify fixes with follow-up scans
7. **Generate Report** - Create audit trail (PDF/HTML)

---

## 📊 Built-in Rules Coverage

| Category | Checks | Status |
|----------|--------|--------|
| Code Execution | eval, exec, compile | ✅ Implemented |
| Database | SQL patterns, string formatting | ✅ Implemented |
| Serialization | Pickle unsafe load | ✅ Implemented |
| OS Commands | shell exec, subprocess | ✅ Implemented |
| Cryptography | Weak hashes (MD5, SHA1) | ✅ Implemented |
| Credentials | Hardcoded secrets, API keys | ✅ Implemented |
| Network | TLS verification bypass | ✅ Implemented |
| Configuration | Debug flags, logging | ✅ Implemented |
| Temporary Files | Unsafe temp patterns | ✅ Implemented |
| Dependencies | Via pip-audit (optional) | ⚙️ Manual |
| Pattern Matching | Via Semgrep (optional) | ⚙️ Documented |

---

## 🔧 Configuration

### Environment Variables
Edit `.env` (copy from `.env.example`):
```env
FLASK_ENV=production
DEBUG=False
DATABASE_URL=sqlite:///instance/app.db
```

### Flask Configuration
Modify `run.py` to change:
- **Host**: Default `127.0.0.1` (change at your risk)
- **Port**: Default `5000`
- **Debug Mode**: Default `False` (keep disabled in production)

---

## 📚 Documentation

- **[INSTALL.md](INSTALL.md)** - Detailed installation instructions
- **[docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)** - System design, data model, and API details
- **[docs/SECURITY_RULES.md](docs/SECURITY_RULES.md)** - Complete rule documentation and CWE mappings
- **[docs/QA_REPORT.md](docs/QA_REPORT.md)** - Quality assurance testing results
- **[sample_projects/README.md](sample_projects/README.md)** - Educational example projects

---

## 🚨 Troubleshooting

### Port 5000 Already in Use
```powershell
# Find and stop the process (Windows)
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Or change port in run.py
app.run(host='127.0.0.1', port=5001, debug=False)
```

### Upload Rejected
Verify:
- ✓ ZIP file is valid
- ✓ File size < 10 MB (compressed)
- ✓ Decompressed size < 30 MB
- ✓ No paths with `..` (path traversal)
- ✓ Less than 500 entries
- ✓ No individual file > 1 MB
- ✓ Supported file extensions

### Python Not Found
```powershell
# Check Python installation
python --version
py --version

# Install from python.org if needed
# Remember to add Python to PATH
```

### Dependencies Won't Install
```powershell
# Upgrade pip
python -m pip install --upgrade pip

# Check internet connection
# Verify Python version (3.11+)
python --version

# Try installing one package at a time
python -m pip install Flask==3.1.2
```

### No Findings Detected
This doesn't mean the code is secure:
- Built-in rules have **limited coverage**
- Install **Bandit** for enhanced Python detection
- Use **Semgrep** for pattern-based scanning
- Combine with manual code review
- This tool is a **starting point**, not complete assurance

---

## 📖 Sample Projects

Two educational projects included:

### Vulnerable Python (`sample_projects/vulnerable_python/`)
Intentionally vulnerable code demonstrating:
- SQL injection
- Command injection
- Insecure deserialization
- Hardcoded credentials
- Weak cryptography

### Fixed Python (`sample_projects/fixed_python/`)
Corrected versions showing:
- Parameterized queries
- Safe subprocess usage
- Secure serialization
- Credential management
- Strong cryptography

---

## 🤝 Contributing

This is an educational project. For improvements:
1. Test thoroughly
2. Document changes
3. Include unit tests
4. Update relevant docs
5. Follow security best practices

---

## ⚖️ License

Educational use. Review project documentation for licensing details.

---

## 🎓 Educational Purpose

This tool is designed for:
- **Learning** application security concepts
- **Understanding** vulnerability patterns
- **Practicing** code review techniques
- **Educational labs** and training

Not intended for:
- Production deployments (without hardening)
- Automated vulnerability scanning (use dedicated tools)
- Compliance certifications
- Standalone security assessment

---

## 📞 Support

For issues or questions:
1. Check [INSTALL.md](INSTALL.md) troubleshooting section
2. Review [docs/](docs/) folder for detailed information
3. Check existing test cases for usage examples
4. Examine sample projects for reference implementations

---

## 🔐 Responsible Disclosure

Always:
- ✅ Use only on code you own or have authorization to audit
- ✅ Keep findings confidential until remediation
- ✅ Report responsibly to code owners
- ✅ Follow your organization's security policies

---

<div align="center">

**🛡️ Secure Code Review Starts Here** 🛡️

Built for learning, enhanced by practice.

[Documentation](docs/) • [Sample Projects](sample_projects/) • [Issues](../../issues) • [Releases](../../releases)

</div>
