## Troubleshooting

### CI Fails: "collected 0 items"

**Problem:** Pytest can't find tests

**Solution:**
```bash
# Verify test file name (must use underscore!)
ls tests/
# Should show: test_calculator.py (NOT test-calculator.py)

# Verify __init__.py files exist
ls app/__init__.py tests/__init__.py

# Create if missing
touch app/__init__.py tests/__init__.py

# Test locally
pytest tests/test_calculator.py -v
```

### CD Fails: "Username and password required"

**Problem:** Docker Hub secrets not configured correctly

**Solution:**
1. Go to Settings → Secrets and variables → Actions
2. Verify secrets are under **"Repository secrets"** (NOT "Environment secrets")
3. Both `DOCKER_USERNAME` and `DOCKER_TOKEN` must exist
4. If wrong, delete and re-add them correctly
5. Re-run the failed workflow

### Tests Pass Locally but Fail in CI

**Problem:** Missing files or environment differences

**Solution:**
```bash
# Check what's committed
git ls-files tests/

# Should include:
# tests/__init__.py
# tests/test_calculator.py

# Ensure Python version matches
python --version  # Should be 3.11+

# Run exact CI command
pytest tests/test_calculator.py -v --cov=app
```

### Docker Container Won't Start

**Problem:** Port conflict or missing dependencies

**Solution:**
```bash
# Check if port 5000 is in use
# Windows PowerShell:
Get-NetTCPConnection -LocalPort 5000

# Kill process using port 5000 if needed
# Or use different port:
docker run -p 8080:5000 YOUR_USERNAME/calculator-api:latest
```

### Can't Push to GitHub

**Problem:** Authentication failure

**Solution:**
```bash
# Use Personal Access Token (not password)
# 1. Go to: GitHub Settings → Developer settings
# 2. Personal access tokens → Tokens (classic)
# 3. Generate new token with 'repo' scope
# 4. Use token as password when pushing
```
