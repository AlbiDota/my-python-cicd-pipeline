# Calculator API - CI/CD Example

**CI:** [View CI Workflow](https://github.com/AlbiDota/my-python-cicd-pipeline/blob/main/.github/workflows/ci.yml)  
**CD:** [View CD Workflow](https://github.com/AlbiDota/my-python-cicd-pipeline/blob/main/.github/workflows/cd.yml)

A simple Flask-based calculator API demonstrating a complete CI/CD pipeline with GitHub Actions, Docker, and automated testing.
## TODO

### Add More Features
- [X] Power operation (x^y)
- [X] Square root function
- [X] Modulo operation
- [ ] Scientific calculator functions

### Improve Testing
- [ ] Integration tests
- [ ] API endpoint testing with pytest
- [ ] Load testing
- [ ] Security testing

### Enhance CI/CD
- [ ] Add staging environment
- [ ] Implement blue-green deployment
- [ ] Add manual approval gates
- [ ] Deploy to cloud (AWS, Azure, GCP)

### Code Quality
- [ ] Add type hints (mypy)
- [ ] Code formatting (black)
- [ ] Security scanning (bandit)
- [ ] Dependency updates (Dependabot)

### Monitoring & Observability
- [ ] Application logging
- [ ] Health check improvements
- [ ] Metrics collection (Prometheus)
- [ ] Error tracking (Sentry)

### Documentation
- [ ] API documentation (Swagger/OpenAPI)
- [ ] Architecture diagrams
- [ ] Deployment guides
- [ ] Contributing guidelines

## Features

- **REST API** with basic calculator operations (add, subtract, multiply, divide)
- **Automated Testing** with pytest and coverage reporting
- **Continuous Integration** with GitHub Actions
- **Continuous Deployment** to Docker Hub
- **Containerized** with Docker for easy deployment
- **Health Check** endpoint for monitoring

## API Endpoints

| Endpoint | Method | Description | Example |
|----------|--------|-------------|---------|
| `/` | GET | API information | `http://localhost:5000/` |
| `/health` | GET | Health check | `http://localhost:5000/health` |
| `/add/<a>/<b>` | GET | Add two numbers | `http://localhost:5000/add/5/3` |
| `/subtract/<a>/<b>` | GET | Subtract b from a | `http://localhost:5000/subtract/10/4` |
| `/multiply/<a>/<b>` | GET | Multiply two numbers | `http://localhost:5000/multiply/7/6` |
| `/divide/<a>/<b>` | GET | Divide a by b | `http://localhost:5000/divide/20/4` |
| `/power/<a>/<b>` | GET | Base to the power of exponent | `http://localhost:5000/power/4/2` |
| `/square/<a>/` | GET | Value a squared | `http://localhost:5000/square/4` |
| `/modulo/<a>/<b>` | GET | Modulo a of b | `http://localhost:5000/modulo/20/4` |


## Local Development

### Prerequisites

- Python 3.11+
- pip
- Virtual environment (recommended)

## Setup Instructions

### 1. Fork or Clone Repository

```bash
git clone https://github.com/peyman-t/python-cicd-example.git
cd python-cicd-example

# Remove original remote
git remote remove origin
```

### 2. Create Your GitHub Repository

1. Go to https://github.com/new
2. Name it `my-python-cicd-pipeline` (or any name you prefer)
3. Set to **Public**
4. **DO NOT** initialize with README (we have files already)
5. Click "Create repository"

### 3. Add Your Repository as Remote

```bash
# Add your repository as origin
git remote add origin https://github.com/YOUR_USERNAME/my-python-cicd-pipeline.git

# Verify
git remote -v
```

### 4. Create Docker Hub Account

1. Sign up at [hub.docker.com](https://hub.docker.com) (free tier is fine)
2. Remember your username (you'll need it for secrets)

### 5. Generate Docker Hub Access Token

1. Login to Docker Hub
2. Go to **Account Settings** → **Security**
3. Click **"New Access Token"**
4. **Description:** `GitHub Actions CI/CD`
5. **Permissions:** `Read, Write, Delete`
6. Click **"Generate"**
7. **IMPORTANT:** Copy the token immediately! It looks like `dckr_pat_...`
   - Save it somewhere safe
   - You won't see it again!

### 6. Add GitHub Secrets (CRITICAL!)

1. Go to your GitHub repository
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Under **"Repository secrets"** section (NOT Environment secrets):

**Add DOCKER_USERNAME:**
- Click "New repository secret"
- **Name:** `DOCKER_USERNAME` (exact spelling, all caps)
- **Secret:** Your Docker Hub username
- Click "Add secret"

**Add DOCKER_TOKEN:**
- Click "New repository secret" again
- **Name:** `DOCKER_TOKEN` (exact spelling, all caps)
- **Secret:** Paste your Docker Hub access token
- Click "Add secret"

**Verify:** You should now see both secrets listed under "Repository secrets"

### 7. Update README Badges (Optional)

Replace `YOUR_USERNAME` in the badge URLs at the top of this README with your actual GitHub username.

### 8. Test Locally First

```bash
# Install dependencies
pip install -r requirements.txt requirements-dev.txt

# Run tests
pytest tests/test_calculator.py -v

# Should show: 16 passed

# Test the app
python app/calculator.py
# Visit http://localhost:5000/health
```

### 9. Push to GitHub

```bash
# Stage all files
git add .

# Commit
git commit -m "Initial commit: Set up CI/CD pipeline"

# Push (this triggers the CI/CD pipeline!)
git push -u origin main
```

### 10. Watch the Pipeline!

1. Go to your GitHub repository
2. Click the **"Actions"** tab
3. Watch the CI workflow run (testing and linting)
4. Watch the CD workflow run (Docker build and push)
5. Check Docker Hub for your new image!

**Pipeline completes in ~5-8 minutes**


## CI/CD Pipeline

### Continuous Integration (CI)

**Triggers:** Push to `main` or Pull Request to `main`

**Pipeline Steps:**
1. [X] Checkout code
2. [X] Set up Python 3.11
3. [X] Install dependencies (with caching)
4. [X] Verify project structure
5. [X] Check test discovery
6. [X] Run linting with flake8
7. [X] Run tests with pytest (coverage enabled)
8. [X] Upload coverage report as artifact
9. [X] Display coverage summary

**Success Criteria:**
- All tests pass (16+ tests)
- No critical linting errors
- Coverage report generated

### Continuous Deployment (CD)

**Triggers:** Push to `main` (only after CI passes)

**Pipeline Steps:**
1. [X] Checkout code
2. [X] Set up Docker Buildx
3. [X] Login to Docker Hub (using secrets)
4. [X] Extract metadata and generate tags
5. [X] Build Docker image
6. [X] Push to Docker Hub with tags:
   - `latest` (for main branch)
   - `main-<commit-sha>` (specific version)
7. [X] Display deployment information

**Image Tags:**
- `YOUR_USERNAME/calculator-api:latest` - Most recent build
- `YOUR_USERNAME/calculator-api:main-abc1234` - Specific commit


## Project Structure

```
python-cicd-example/
├── .github/
│   └── workflows/
│       ├── ci.yml              # CI workflow configuration
│       └── cd.yml              # CD workflow configuration
├── app/
│   ├── __init__.py            # Package initialization
│   └── calculator.py          # Flask application
├── tests/
│   ├── __init__.py            # Test package initialization
│   └── test_calculator.py     # Unit tests (16+ tests)
├── .gitignore
├── Dockerfile                  # Docker image configuration
├── requirements.txt            # Production dependencies
├── requirements-dev.txt        # Development dependencies
└── README.md
```

## Testing the Deployed API

Once your Docker image is on Docker Hub:

```bash
# Pull your image
docker pull YOUR_USERNAME/calculator-api:latest

# Run it
docker run -p 5000:5000 YOUR_USERNAME/calculator-api:latest

# Test endpoints
curl http://localhost:5000/health
curl http://localhost:5000/add/10/5
curl http://localhost:5000/multiply/7/8
curl http://localhost:5000/divide/20/4
```

**Expected response (for add):**
```json
{
  "a": 10.0,
  "b": 5.0,
  "operation": "addition",
  "result": 15.0
}
```


## Learning Resources

- **GitHub Actions:** https://docs.github.com/en/actions
- **Docker:** https://docs.docker.com
- **Flask:** https://flask.palletsprojects.com
- **Pytest:** https://docs.pytest.org
- **CI/CD Best Practices:** https://www.atlassian.com/continuous-delivery

## License

This project is open source and available under the MIT License.

## Contributing

Contributions, issues, and feature requests are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Tips

- **Test locally first:** Always run `pytest` before pushing
- **Small commits:** Make incremental changes for easier debugging
- **Descriptive messages:** Write clear commit messages
- **Monitor Actions:** Check the Actions tab after each push
- **Read logs:** If something fails, read the complete log output

## Success Checklist

Before you're done, verify:

- [ ] Local tests pass: `pytest tests/test_calculator.py -v`
- [ ] Docker builds locally: `docker build -t test .`
- [ ] Secrets configured in GitHub (Repository secrets)
- [ ] CI workflow passes (green checkmark in Actions)
- [ ] CD workflow passes (image on Docker Hub)
- [ ] Can pull and run image: `docker pull YOUR_USERNAME/calculator-api:latest`
- [ ] API responds: `curl http://localhost:5000/health`

## Acknowledgments

This project demonstrates modern DevOps practices using:
- **GitHub Actions** for CI/CD automation
- **Docker** for containerization
- **Flask** for the web framework
- **Pytest** for testing

---

*Questions? Issues? Open an issue on GitHub!*